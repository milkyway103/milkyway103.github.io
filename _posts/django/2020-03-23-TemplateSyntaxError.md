---
layout: post
title: TemplateSyntaxError
categories : django
tags : django python TemplateSyntaxError bugreport
comments : true
---

### 경위

프로젝트에 static 폴더를 생성해 테스트 중 에러 발생

### error log

```
Environment:


Request Method: GET
Request URL: http://127.0.0.1:8000/

Django Version: 3.0.4
Python Version: 3.7.3
Installed Applications:
['django.contrib.admin',
 'django.contrib.auth',
 'django.contrib.contenttypes',
 'django.contrib.sessions',
 'django.contrib.messages',
 'django.contrib.staticfiles',
 'home',
 'novels']
Installed Middleware:
['django.middleware.security.SecurityMiddleware',
 'django.contrib.sessions.middleware.SessionMiddleware',
 'django.middleware.common.CommonMiddleware',
 'django.middleware.csrf.CsrfViewMiddleware',
 'django.contrib.auth.middleware.AuthenticationMiddleware',
 'django.contrib.messages.middleware.MessageMiddleware',
 'django.middleware.clickjacking.XFrameOptionsMiddleware']


Template error:
In template /Users/eunha/PycharmProjects/koreagojeon/koreagojeon/templates/base.html, error at line 3
   'staticfiles' is not a registered tag library. Must be one of:
admin_list
admin_modify
admin_urls
cache
i18n
l10n
log
static
tz
   1 : <!doctype html>
   2 :
   3 :  {% load staticfiles %}
   4 :
   5 : <html lang="ko">
   6 :   <head>
   7 :     <!-- Required meta tags -->
   8 :     <meta charset="utf-8">
   9 :     <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
   10 :     <title>한국고전문학의 세계</title>
   11 :   </head>
   12 :   <body>
   13 :     {% block content %}


Traceback (most recent call last):
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/template/defaulttags.py", line 1021, in find_library
    return parser.libraries[name]

During handling of the above exception ('staticfiles'), another exception occurred:
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/core/handlers/exception.py", line 34, in inner
    response = get_response(request)
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/core/handlers/base.py", line 115, in _get_response
    response = self.process_exception_by_middleware(e, request)
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/core/handlers/base.py", line 113, in _get_response
    response = wrapped_callback(request, *callback_args, **callback_kwargs)
  File "/Users/eunha/PycharmProjects/koreagojeon/home/views.py", line 4, in main
    return render(request, 'home/main.html')
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/shortcuts.py", line 19, in render
    content = loader.render_to_string(template_name, context, request, using=using)
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/template/loader.py", line 62, in render_to_string
    return template.render(context, request)
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/template/backends/django.py", line 61, in render
    return self.template.render(context)
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/template/base.py", line 171, in render
    return self._render(context)
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/template/base.py", line 163, in _render
    return self.nodelist.render(context)
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/template/base.py", line 936, in render
    bit = node.render_annotated(context)
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/template/base.py", line 903, in render_annotated
    return self.render(context)
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/template/loader_tags.py", line 127, in render
    compiled_parent = self.get_parent(context)
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/template/loader_tags.py", line 124, in get_parent
    return self.find_template(parent, context)
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/template/loader_tags.py", line 104, in find_template
    template_name, skip=history,
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/template/engine.py", line 125, in find_template
    template = loader.get_template(name, skip=skip)
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/template/loaders/base.py", line 30, in get_template
    contents, origin, origin.template_name, self.engine,
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/template/base.py", line 156, in __init__
    self.nodelist = self.compile_nodelist()
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/template/base.py", line 194, in compile_nodelist
    return parser.parse()
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/template/base.py", line 477, in parse
    raise self.error(token, e)
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/template/base.py", line 475, in parse
    compiled_result = compile_func(self, token)
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/template/defaulttags.py", line 1078, in load
    lib = find_library(parser, name)
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/template/defaulttags.py", line 1025, in find_library
    name, "\n".join(sorted(parser.libraries)),

Exception Type: TemplateSyntaxError at /
Exception Value: 'staticfiles' is not a registered tag library. Must be one of:
admin_list
admin_modify
admin_urls
cache
i18n
l10n
log
static
tz

```

-> staticfiles가 등록된 tag library에 없다...즉 잘못된 문법 사용

### 해결 방법

```python
{% load staticfiles %}
```

가 아니라

```python
{% load static %}
```

라고 적어야 한다.

load staticfiles는 이전 버전의 Django에서 사용되었으나, 2016년 이후부터 동작하지 않는다.

이제는 사용할 수 없게 되긴 했지만, 둘의 차이는 이러했다.

`static` 태그는 `STATIC_ROOT`에 저장된 static 파일들을 link하라는 의미이고, `staticfiles` 태그는 주어진 path에 있는 모든 URL을 생성하기 위해 `STATICFILES_STORAGE` 저장소를 사용한다. 이것은 파일을 배치할 때 로컬 스토리지를 사용하지 않는 경우에 유용하다.

`static` 태그의 공식문서에는 만약 static 파일들을 배치하기 위해 클라우드 서비스를 사용하는 것과 같은 경우에 `staticfiles`가 유용하다고 쓰여 있다.

그러나 `static`이 `staticfiles`보다 좀더 명확하다.

## 참고
- [공식 Django 문서](https://docs.djangoproject.com/en/2.2/ref/templates/builtins/#static)
- [Static 파일](http://pythonstudy.xyz/python/article/314-Static-%ED%8C%8C%EC%9D%BC) <-이 링크는 이전 버전의 Django를 다루고 있다. `load staticfiles`는 더 이상 동작하지 않는다.
- [Django Static 파일](https://velog.io/@ground4ekd/django-static)
- [What is the difference between {% load staticfiles %} and {% load static %}](https://stackoverflow.com/questions/24238496/what-is-the-difference-between-load-staticfiles-and-load-static)
