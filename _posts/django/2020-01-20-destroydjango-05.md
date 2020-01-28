---
layout: post
title: 초보자를 위한 장고 뿌시기 5 HTML 띄우기
categories : django
tags : django python
comments : true
---

> goormedu의 [초보자를 위한 장고 뿌시기](https://edu.goorm.io/learn/lecture/16377/%EC%B4%88%EB%B3%B4%EC%9E%90%EB%A5%BC-%EC%9C%84%ED%95%9C-%EC%9E%A5%EA%B3%A0-django-%EB%BF%8C%EC%8B%9C%EA%B8%B0)를 보고 공부한 것을 정리합니다.

View, Template
View : 클라이언트의 요청 처리 및 응답을 주는 역할
Template : HTML과 같이 사용자에게 보여주는 역할

1. 경로를 만들어준다.
naver.com은 도메인
/(slash)가 들어가면 경로
즉, url = domain + 경로
저번에 서버를 실행시키고/admin이라고 했을 때 그 뒤의 `/admin`이 경로가 되었던 것
`프로젝트 이름 폴더 > urls.py의
urlpatterns`에 `list` 형태로 경로들이 들어 있다.
2. 그 경로에 해당하는 view를 만들어준다.
그 경로에 가면 어떤 서버로 가라!
`urls.py` : **장고에서 경로를 생성하고 어디로 갈지 알려주는 네비게이션 역할**
3. 해당 View에서 요청을 처리하여 응답을 한다.

장고에서 views.py에 코딩을 할 때 두 가지 방법 가능
1. 함수로 view 짜기 <- 여기서는 주로 이 방식 사용
2. 클래스로 view 짜기

**실습!**

1. `urls.py`의 urlpatterns에 path를 추가해준다!
```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', 어떤-곳으로?),
]
```
2. posts 폴더의 `views.py`에서 `main`이라는 함수를 정의해준다.
```python
from django.shortcuts import render

def main(request):
    return render(request, 어떤 html을 띄워줄건지 <- html 경로)
```
- render : html을 response로 뿌려줄 때 사용하는 함수
3. posts 폴더에 templates > posts 폴더를 생성하고, 그 안에 `main.html` 파일을 생성한다.
- 왜 templates 안에 app 이름과 똑같은 posts 폴더를 만들까?
-> render 함수가 html 등의 파일을 가져올 때 templates 폴더에서 가져온다.
만약 다른 앱(폴더)에 똑같은 이름의 다른 html이 있다면 의도치 않은 html을 가져올 가능성을 방지하기 위해서!
- html의 이름을 `main.html`로 한 까닭은 함수의 이름이 main이기 때문에 알아보기 쉽도록!
```HTML
<h1>
    Hello, Django!
</h1>
```
4. main 함수의 render를 html 경로로 바꾸어 준다.
```python
def main(request):
    # request에 대해 posts/main.html을 render해줘!
    return render(request, 'posts/main.html')
```
5. urls.py의 경로를 main 함수로 바꾸어 준다.
```python
from django.contrib import admin
from django.urls import path
from posts.views import main

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', main), # 콤마 꼭!
    # -> 아무 경로도 없을 때 posts > views > main 함수로 가라!
]
```
- 콤마 반드시!
- 아무 경로도 없을 때 posts > views > main 함수로 가라!
- 그럼 main 함수에서는 `posts/main.html`을 렌더링한 결과를 뿌려준다.

![](/_assets/post-img/rendering-html.png)
