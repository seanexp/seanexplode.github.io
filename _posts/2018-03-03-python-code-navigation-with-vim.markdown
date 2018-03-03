---
layout: post
title: Vim에서 파이썬 프로젝트 코드 네비게이션하기
date: 2018-03-03
description: Vim과 ctags를 이용하여 IDE에서처럼 코드 네비게이션하는 방법
tags: [Vim, ]
---

Jetbrains의 pycharm같은 IDE를 사용하다보면 **Go-to-definition** 기능을 사용할 수 있다. 코드를 읽다가 그 함수 혹은 클래스가 어떻게 동작하는지 모르겠거나 잊어버렸을 때 유용하게 사용할 수 있다. 그러나 *Vim*을 사용하다보면 이 기능을 이용하기 쉽지가 않다. 구글에 _vim go to definition_이라고 검색하면 `gd`나 `gD`를 볼 수 있다. 이 명령들은 local definition 이나 global definition을 보여주는 명령들로써 다른 파일에 존재하는 함수나 클래스 혹은 변수의 definition을 보여줄 수는 없다.

그러면 다른 파일에 존재하는 함수나 클래스의 definition을 보려면 어떻게 해야 할까? 아쉽게도 이 기능은 *Vim*만으로는 할 수 없고, [ctags](https://ko.wikipedia.org/wiki/Ctags) 라는 프로그래밍 도구를 이용해야 한다. [Vim Tips Wiki](http://vim.wikia.com/wiki/Browsing_programs_with_tags) 를 보면 *Vim*은 사용자가 찾고 싶은 단어들고 그 단어들의 위치를 열거하는 tags 파일을 사용한다. 각각의 찾고 싶은 단어들은 **tag**라고 한다. 예를 들면, 각 함수의 이름이나 전역 변수는 tag가 될 수 있다.

## ctags 설치하기

Mac OSX를 사용하고 있다면 다음과 같이 *homebrew*를 이용하여 *ctags*를 설치한다.

{% highlight bash %}
$ brew install ctags
{% endhighlight %}

만약 linux를 사용하고 있다면 ```apt-get```이나 ```yum```으로 설치할 수 있다.

## ctags 이용하여 tag파일 생성하기

파이썬 프로젝트를 *Vim*을 이용하여 작업하면서 편하게 코드 네비게이션하고 싶다면 이 섹션을 주의하며 읽어야 한다. 이 섹션은 [rampart81님의 글](https://rampart81.github.io/post/python-ctags/)에 강하게 영향을 받았음을 밝힌다. 

보통의 *ctags* 관련 글을 읽으면 다음과 같이 tag 파일을 생성하는 것을 볼 수 있다.

{% highlight bash %}
$ ctags -R .
{% endhighlight %}

그러나 파이썬 프로젝트에서 이렇게 tag 파일을 만들면 약간의 불편함이 생긴다. 이 [포스트](http://www.held.org.il/blog/2011/02/configuring-ctags-for-python-and-vim/)에서도 언급하고 있듯 **import line**을 definition으로 인식한다. 따라서 다음과 같이 다른 옵션들을 넣어주면서 이 문제를 해결해야 한다.

{% highlight bash %}
$ ctags -R --fields=+l --languages=python --python-kinds=-iv -f /.tags ./
{% endhighlight %}

ctags의 매뉴얼을 보면 다음과 같이 옵션의 효과들을 볼 수 있다.

* -R 옵션은 디렉토리 내의 모든 코드의 태그 파일을 recursive 하게 만든다.
* --fields 옵션은 tag파일의 엔트리에 포함되는 필드를 확정한다. l 플래그는 태그가 속한 파일의 언어를 엔트리에 추가한다.
* --languages 옵션은 너무나 명확하니 패스. --languages 옵션은 comma-separated 리스트를 받을 수 있다. 이 옵션이 없으면 뒤에 나오는 --python-kinds 옵션을 쓸 수 없는 것 같다.
* --python-kinds 옵션은 --<LANG>-kinds 옵션에서 살펴볼 수 있다. 특정 언어로 작성된 파일에서 만들어진 태그들이 어떤 종류여야 하는지를 설정하는 옵션이다. ```-i``` 옵션은 import line을 신경쓰지 않는 옵션, ```-v``` 옵션은 변수를 신경 쓰지 않는 옵션이다.
* -f 옵션은 -f 옵션 바로 뒤에 오는 인자를 tag 파일의 이름으로 설정한다.

그러나 위의 옵션만으로 tag 파일을 생성하는 것은 또다른 문제를 야기한다. 개발자가 직접 짠 코드 내에서는 잘 이동하지만 pip로 설치한 모듈이나 python standard library의 코드로는 이동하지 못한다. 어떻게 하면 이 문제를 해결할 수 있을까? 물론 python standard library 및 pip로 설치한 모듈의 path를 파악한 후 추가해주는 방법이 있지만 이 방법은 너무나 귀찮고 파이썬에서 가상환경을 이용했을 때 path가 달라진다는 문제점이 있다. 이 문제를 해결하기 위해서 다음과 같이 파이썬 코드를 커맨드 라인에서 직접 실행시켜 문제를 해결한다.

{% highlight bash %}
$ ctags -R --fields=+l --languages=python --python-kinds=-iv -f ./tags . $(python -c "import os, sys; print(' '.join('{}'.format(d) for d in sys.path if os.path.isdir(d)))")
{% endhighlight %}

[rampart81](https://rampart81.github.io/post/python-ctags/) 님은 다음과 같이 alias를 지정해서 사용하신다고 한다. 나도 역시 이와 같은 alias를 사용한다.

{% highlight bash %}
$ alias python_ctags="ctags -R --fields=+l --languages=python --python-kinds=-iv -f ./tags . $(python -c "import os, sys; print(' '.join('{}'.format(d) for d in sys.path if os.path.isdir(d)))")"
{% endhighlight %}

```~/.bash_profile``` 혹은 ```~/.bashrc``` 에 추가해서 사용하면 될 듯하다.

## code navigation

이제 원하는 함수 혹은 클래스 위에 커서를 놓고 ```ctrl + ]```를 누른다. 그러면 definition으로 이동한 것을 볼 수 있다. 원래의 코드로 이동하고 싶다면 ```ctrl + t```를 누르면 된다. 원하는 함수나 클래스 위에 커서를 이동하기도 귀찮다면 *command mode*에서 ```:tags <원하는 함수/클래스 이름>```을 입력하면 된다.

그런데 이 기능은 원래의 코드와 definition을 같이 볼 수 없다는 단점이 있다. 둘을 같이 보려면 어떻게 해야 할까? [Vim Tips Wiki](http://vim.wikia.com/wiki/Browsing_programs_with_tags) 에서 그 답을 확인할 수 있다. 원하는 함수나 클래스 위에 커서를 올려놓고 ```ctrl + w + ]```를 누르거나 *command mode*에서 ```:ptag <원하는 함수/클래스 이름>```을 입력하면 된다.
