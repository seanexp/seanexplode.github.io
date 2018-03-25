---
layout: post
title: jupyter notebook 에 virtualenv 커널 추가하기
date: 2018-03-25
description: 가상환경의 이점을 jupyter notebook에서도 누릴 수 있도록 커널을 추가한다.
tags: [jupyter notebook, ]
---

## 가상환경 (virtualenv)

다른 이들의 코드를 돌려봐야 하는 경우가 종종 생긴다. 오픈 소스 A와 오픈 소스 B를 동시에 돌려보고 싶은 경우를 생각해보자. 오픈 소스 A는 python2.7, tensorflow1.3 로 만들어지고 오픈 소스 B는 python3.6, tensorflow1.5 로 만들어졌다. 이런 경우에는 참 난감하다. 어느 버전의 파이썬을 쓸지, 어느 버전의 tensowflow를 설치해야 할지 모르기 때문이다. **virtualenv (가상환경)** 은 이런 상황을 해결하는데 있어 정말 유용한 도구이다. [The Hitchhiker's Guide to Python](http://docs.python-guide.org/en/latest/dev/virtualenvs/) 에 다음과 같은 설명이 있다.

> **virtualenv**는 독립적인 파이썬 환경을 만들어주는 도구이다. **virtualenv**는 파이썬 프로젝트가 필요로 하는 패키지를 사용할 수 있도록 필요한 실행파일을 포함하는 폴더를 만든다.

그러니까, virtualenv는 오픈 소스 A와 오픈 소스 B에 각각 독립적인 환경을 만들어주는 도구라고 생각하면 될 것 같다.

## 가상환경 만들기

앞으로의 글에서는 여러분이 [Anaconda](https://www.anaconda.com/download/)를 설치했다고 가정하고 설명한다.

**conda** 명령어를 사용하여 가상환경을 만드는 것은 아주 간단하다. 

{% highlight bash %}
$ conda create --name myenv
{% endhighlight %}

이제 *myenv*라는 이름의 virtualenv가 생성되었다. virtualenv의 파이썬 버전을 설정하고 싶다면 다음과 같이 만들 수 있다.

{% highlight bash %}
$ conda create --name myenv python=3.5
{% endhighlight %}

그러면 파이썬 버전이 3.5인 *myenv*라는 이름의 virtualenv가 생성된다. 만들어진 virtualenv에 들어가기 위한 방법은 다음과 같다.

{% highlight bash %}
$ source activate myenv
{% endhighlight %}

*myenv* 대신 자신의 virtualenv 이름을 쓰면 된다. 만들어진 virtualenv에 들어가는 것을 virtualenv를 활성화(activate)했다고 표현한다. virtualenv를 활성화하면 아래 사진과 같이 shell에 맨 왼쪽에 virtualenv의 이름이 괄호로 감싸져있는 것을 확인할 수 있다. 

{% include image.html url="/assets/img/add-virtualenv-kernel/virtualenv-name.png" %}

virtualenv에서 빠져나오기 위해서는

{% highlight bash %}
$ source deactivate
{% endhighlight %}

라고 치면 된다.

## 가상환경 커널 추가하기

이렇게 만든 virtualenv를 **jupyter notebook**에서 활용하려면 추가적인 작업을 해주어야 한다. 먼저, virtualenv를 활성화한다.

{% highlight bash %}
$ source activate myenv
{% endhighlight %}

그 다음 ```ipykernel```을 설치한다.

{% highlight bash %}
$ pip install ipykernel
{% endhighlight %}

그리고 나서는 커널을 스스로 설치하는 스크립트를 실행해준다.

{% highlight bash %}
$ python -m ipykernel install --user --name=myenv
{% endhighlight %}

역시 마찬가지로 *myenv* 대신에 자신의 virtualenv의 이름을 넣어주면 된다. 다음과 같이 virtualenv의 커널이 생긴 것을 확인할 수 있다.

{% include image.html url="/assets/img/add-virtualenv-kernel/check.png" description="가상환경과 같은 이름의 커널이 생긴 것을 확인할 수 있다." %}
