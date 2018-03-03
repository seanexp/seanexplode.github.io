---
layout: post
title: Vim에서 파이썬 프로젝트 code navigation하기
date: 2018-03-03
description: Vim과 ctags를 이용하여 IDE에서처럼 code navigation하는 방법
tags: [Vim, ]
---

Jetbrains의 pycharm같은 IDE를 사용하다보면 **Go-to-definition** 기능을 사용할 수 있다. 코드를 읽다가 그 함수 혹은 클래스가 어떻게 동작하는지 모르겠거나 잊어버렸을 때 유용하게 사용할 수 있다. 그러나 Vim을 사용하다보면 이 기능을 이용하기 쉽지가 않다. 구글에 _vim go to definition_이라고 검색하면 `gd`나 `gD`를 볼 수 있다. 이 명령들은 local definition 이나 global definition을 보여주는 명령들로써 다른 파일에 존재하는 함수나 클래스 혹은 변수의 definition을 보여줄 수는 없다.

그러면 다른 파일에 존재하는 함수나 클래스의 definition을 보려면 어떻게 해야 할까? 아쉽게도 이 기능은 Vim만으로는 할 수 없고, [ctags](https://ko.wikipedia.org/wiki/Ctags) 라는 프로그래밍 도구를 이용해야 한다. [Vim Tips Wiki](http://vim.wikia.com/wiki/Browsing_programs_with_tags) 를 보면 Vim은 사용자가 찾고 싶은 단어들고 그 단어들의 위치를 열거하는 tags 파일을 사용한다. 각각의 찾고 싶은 단어들은 **tag**라고 한다. 예를 들면, 각 함수의 이름이나 전역 변수는 tag가 될 수 있다.

## ctags 설치하기

Mac OSX를 사용하고 있다면 다음과 같이 *homebrew*를 이용하여 ctags를 설치한다.

{% highlight bash %}
$ brew install ctags
{% endhighlight %}

만약 linux를 사용하고 있다면 ```apt-get```이나 ```yum```으로 설치할 수 있다.

## ctags 이용하여 tag파일 생성하기

## code navigation

이제 원하는 함수 혹은 클래스 위에 커서를 놓고 ```ctrl + ]```를 누른다. 그러면 definition으로 이동한 것을 볼 수 있다. 원래의 코드로 이동하고 싶다면 ```ctrl + t```를 누르면 된다. 원하는 함수나 클래스 위에 커서를 이동하기도 귀찮다면 *command mode*에서 ```:tags <원하는 함수/클래스 이름>```을 입력하면 된다.

그런데 이 기능은 원래의 코드와 definition을 같이 볼 수 없다는 단점이 있다. 둘을 같이 보려면 어떻게 해야 할까? [Vim Tips Wiki](http://vim.wikia.com/wiki/Browsing_programs_with_tags) 에서 그 답을 확인할 수 있다. 원하는 함수나 클래스 위에 커서를 올려놓고 ```ctrl + w + ]```를 누르거나 *command mode*에서 ```:ptag <원하는 함수/클래스 이름>```을 입력하면 된다.
