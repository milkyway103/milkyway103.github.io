---
layout: post
title: 초보자를 위한 장고 뿌시기 8 Include로 HTML 소스 관리하기
categories : django
tags : django python
comments : true
---

> goormedu의 [초보자를 위한 장고 뿌시기](https://edu.goorm.io/learn/lecture/16377/%EC%B4%88%EB%B3%B4%EC%9E%90%EB%A5%BC-%EC%9C%84%ED%95%9C-%EC%9E%A5%EA%B3%A0-django-%EB%BF%8C%EC%8B%9C%EA%B8%B0)를 보고 공부한 것을 정리합니다.

웹페이지에는 재사용되는 요소들이 많다. 대표적으로 navigation bar가 있다. bootstrap에서 nav 중 하나를 base.html의 block 상단에 붙여넣기하면 다음과 같은 화면을 볼 수 있다.

<center>
   <figure>
   <img src="/assets/post-img/django/destroydjango08-image1.png" alt="views">
   <figcaption>main 화면</figcaption>
   </figure>
</center>
<center>
   <figure>
   <img src="/assets/post-img/django/destroydjango08-image2.png" alt="views">
   <figcaption>novels 화면</figcaption>
   </figure>
</center>

이는 분명히 편리하지만, (하나의 html에만 코드를 추가해줘도 된다는 점에서) 어떤 페이지에는 navbar가 필요하고 어떤 페이지는 그렇지 않은 경우, base.html을 각각 만들어줘야 하는 문제가 생긴다.

여기에서는 공유하는 html 파일을 share 폴더 안에 넣어주고, 앞에 `_`를 붙여서 이름지어준다.

<center>
   <figure>
   <img src="/assets/post-img/django/destroydjango08-image3.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

<center>
   <figure>
   <img src="/assets/post-img/django/destroydjango08-image4.png" alt="views">
   <figcaption>이제 특정 페이지에서만 navbar를 볼 수 있다.</figcaption>
   </figure>
</center>

이렇게 하면 코드 관리 효율성이 좋아진다.
navbar를 변경할 필요성이 있을 때 `_navbar` 폴더만 변경하면 되기 때문이다. 만약 이 코드가 여러 html에 산발적으로 쓰여져 있다면 코드의 중복성 때문에 일일이 찾아서 수정변경해야 하는 번거로움이 생긴다.
