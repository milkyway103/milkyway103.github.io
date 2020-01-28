---
layout: post
title: 초보자를 위한 장고 뿌시기 6 링크 타고 다른 페이지 넘어가기
categories : django
tags : django python
comments : true
---

> goormedu의 [초보자를 위한 장고 뿌시기](https://edu.goorm.io/learn/lecture/16377/%EC%B4%88%EB%B3%B4%EC%9E%90%EB%A5%BC-%EC%9C%84%ED%95%9C-%EC%9E%A5%EA%B3%A0-django-%EB%BF%8C%EC%8B%9C%EA%B8%B0)를 보고 공부한 것을 정리합니다.

장고에서는 뼈대가 되는 html을 만들어 두고 재활용하는 것이 가능하다.
그러기 위해서는 우선 `settings.py`에서 templates의 경로를 잡아 주어야 한다.

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'MyFirstDjango', 'templates')],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

이 폴더 (MyFirstDjango) 내의 templates 경로를 읽어올 수 있게 해 준다.

그리고 templates 폴더 내에 `base.html`을 만들고 아래와 같이 부트스트랩의 기본 템플릿을 가져와준다.

```html
{% raw %}
<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">

    <title>My First Django</title>
  </head>
  <body>
    {% block content %}
    {% endblock %}
    <!-- Optional JavaScript -->
    <!-- jQuery first, then Popper.js, then Bootstrap JS -->
    <script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>
  </body>
</html>
{% endraw %}
```

내부에 만들어놓은 `block`을 통해 이 base.html을 재사용하는 것이 가능해진다.
main.html을 아래와 같이 바꾸고 base.html을 `extends` 하게 되면

```html
{% raw %}
{% extends 'base.html' %}

{% block content %}
<h1>
    Hello, Django!
</h1>

<a href='{% url "lovely:first" %}'>first</a>
<a href='{% url "lovely:second" %}'>second</a>
<a href='{% url "lovely:third" %}'>third</a>
{% endblock %}
{% endraw %}
```

![img](/assets/post-img/django/destroydjango07-img02.png)

이애 더하여, static을 지정해줄 수 있다.
static이란 정적 파일로, 프로젝트 안에 있으면서 계속 재사용할 수 있는 css, js, 이미지, 폰트 등을 뜻한다.
ststic 역시 다음과 같이 경로를 지정해 주어야 한다.

```python
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'MyFirstDjango', 'static')
]
```

그 후 MyFirstDjango 폴더 안에 static 폴더를 만들어 주는데, 이 때 프로젝트 폴더 자체가 아닌 프로젝트 폴더 아래의 MyFirstDjango 폴더 아래에 static 폴더를 생성해야 한다. 이로 인한 실수를 해서 30분 정도 헤맸다....
static 아래에 font, css, js 등의 폴더를 생성해준다.
그리고 css 폴더 아래에 `project.css` 라는 css 파일을 생성해주고, 아래와 같이 커스텀한다.

```CSS
h1 {
    font-size: 50px;
}
```
{% raw %}
이를 사용하기 위해서는 `base.html`의 가장 하단에 `{% load static %}`라고 쓰고, 이 스타일 시트를 불러와 주면 된다.
{% endraw %}

```html
{% raw %}
{% load static %}
<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
      <link rel="stylesheet" href="{% static 'css/project.css' %}">

    <title>My First Django</title>
  </head>
  <body>
    {% block content %}
    {% endblock %}
    <!-- Optional JavaScript -->
    <!-- jQuery first, then Popper.js, then Bootstrap JS -->
    <script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>
  </body>
</html>
{% endraw %}
```

그리고 main 페이지를 들어가보면

![img](/assets/post-img/django/destroydjango07-img01.png)

물론 main 페이지 이외에 다른 페이지에도 extends 하여 base.html을 적용할 수 있다. 그리고 여러 개의 base.html을 만들어 각각 다른 뼈대를 적용할 수도 있다.
