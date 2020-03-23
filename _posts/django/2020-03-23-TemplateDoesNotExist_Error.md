---
layout: post
title: TemplateDoesNotExist 에러
categories : django
tags : django python bugreport TemplateDoesNotExist
comments : true
---

### 경위

Django에서 templates 폴더 아래에 html을 생성하고 테스트하는 도중 에러가 발생하였다.

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
 'novels']
Installed Middleware:
['django.middleware.security.SecurityMiddleware',
 'django.contrib.sessions.middleware.SessionMiddleware',
 'django.middleware.common.CommonMiddleware',
 'django.middleware.csrf.CsrfViewMiddleware',
 'django.contrib.auth.middleware.AuthenticationMiddleware',
 'django.contrib.messages.middleware.MessageMiddleware',
 'django.middleware.clickjacking.XFrameOptionsMiddleware']

Template loader postmortem
Django tried loading these templates, in this order:

Using engine django:
    * django.template.loaders.filesystem.Loader: /Users/eunha/PycharmProjects/koreagojeon/templates/home/main.html (Source does not exist)
    * django.template.loaders.app_directories.Loader: /Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/contrib/admin/templates/home/main.html (Source does not exist)
    * django.template.loaders.app_directories.Loader: /Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/contrib/auth/templates/home/main.html (Source does not exist)
    * django.template.loaders.app_directories.Loader: /Users/eunha/PycharmProjects/koreagojeon/novels/templates/home/main.html (Source does not exist)



Traceback (most recent call last):
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
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/template/loader.py", line 61, in render_to_string
    template = get_template(template_name, using=using)
  File "/Users/eunha/PycharmProjects/koreagojeon/venv/lib/python3.7/site-packages/django/template/loader.py", line 19, in get_template
    raise TemplateDoesNotExist(template_name, chain=chain)

Exception Type: TemplateDoesNotExist at /
Exception Value: home/main.html

```

-> 지정한 경로에 해당 template 파일이 존재하지 않는다! 경로를 잘못 지정해 주었거나 하는 문제일 것이라고 생각했다.

### 문제 원인

`settings.py` 파일의 `INSTALLED_APPS` 부분에 app이 추가되어 있지 않아서 app을 인식하지 못해서, 당연히 app 하위에 있는 templates 폴더도 인식하지 못한 것이었다.

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'home',
    'novels',
]
```

추가해주니 정상적으로 작동하였다.

이 에러의 다른 가능한 원인으로는 폴더 이름을 잘못 생성했기 때문일 수도 있다. 가장 흔하게는 `templates`의 `s`를 빼고 `template`라고 쓰는 실수인 듯하다. (처음에는 나도 이것 때문인 줄 알았다.) 가장 상위 폴더>app폴더>templates>app이름>html파일이라는 경로를 반드시 지킬 것!

## 참고
- [템플릿 불러오기 에러](https://programmers.co.kr/learn/questions/105)
- [Django Template Does Not Exist](https://angel-jinsu.tistory.com/5)
