---
layout: post
title: 초보자를 위한 장고 뿌시기 2 MTV 패턴
categories : django
tags : django python
comments : true
---

> goormedu의 [초보자를 위한 장고 뿌시기](https://edu.goorm.io/learn/lecture/16377/%EC%B4%88%EB%B3%B4%EC%9E%90%EB%A5%BC-%EC%9C%84%ED%95%9C-%EC%9E%A5%EA%B3%A0-django-%EB%BF%8C%EC%8B%9C%EA%B8%B0)를 보고 공부한 것을 정리합니다.

MTV 패턴
========

-	일반적으로 MVC 패턴이라고 부르는 것과 의미 자체는 같음
-	view -> template
-	controller -> view

---

-	모델(Model) : 데이터베이스에 저장되는 데이터를 가지고 오거나 저장, 수정
-	템플릿(Template) : 사용자에게 보여지는 부분
-	뷰(View) : 실질적으로 프로그램 로직이 동작하여 데이터를 가져오고 적절하게 가공하여 그 결과를 템플릿에 전달하는 역할 / 모델과 템플릿 사이에서 중계자 역할

---

-	클라이언트 : 행위의 주체
-	뷰 : 요청에 대한 응답을 하는 곳 / 서버같은 역할
-	뷰는 어떤 주방같은 곳
-	클라이언트는 뷰에게 요청(request)
-	뷰는 클라이언트에게 응답(response)
-	url 접속 -> 11번가 서버로 request -> 11번가 홈페이지 보임 -> 11번가 서버의 response(html로 이루어진 화면)
-	즉, 요청을 보내고 응답을 받는 구조
-	템플릿 : 뷰에서 response로 쓰이는 html 등등
-	view는 render를 통해 template을 response로 클라이언트에게 보여줌
-	모델 <- 정말 중요
-	모델은 추상적 개념
-	파이썬에서의 클래스
-	붕어빵을 찍어내는 것과 같은 modeling을 통해 만들어짐
-	database를 만들기 위한 설계
-	Database : 실제로 데이터를 저장하는 곳
-	11번가의 여러 상품들 - 상품이름, 가격, 이미지 등
-	product에는 이름, 가격, 이미지가 있다는 설계
-	그 설계를 통해 만들어진 것 -> 모델
-	객체 : 상품 모델 (설계) 를 통해 찍어낸 상품 하나하나
-	데이터베이스에서는 SQL 사용
-	파이썬과 SQL은 다른 언어이므로 통역사가 필요 -> ORM
-	ORM : Object Relational Mapping
-	모델링 과정을 통해 모델을 만들고 이를 통해 찍어낸 각 객체들을 데이터베이스에 실질적으로 저장
-	이들을 서버의 데이터베이스에서 불러와서 화면에 띄워준다!

---

-	웹서비스를 하나 만든다고 가정
-	먼저 모델링부터
-	User라는 모델을 만든다
-	-> Username, Email, Password, Bdate의 컬럼(필드, 애트리뷰트)
-	하나의 객체를 생성한다
-	-> 성원, tjddnjss@domain.com, 1111, 910101
-	모델링한 유저 모델을 실제 데이터베이스에도 저장해야 함
-	migration이라는 주문서를 만들고 이 주문서를 migrate라는 과정을 통해 데이터베이스에 User Table 생성
-	column = 속성
-	모델은 붕어빵 틀처럼 객체들을 찍어내기 위해 만드는 것
-	Post (Title, Content)
-	쇼핑몰을 만든다고 가정
-	User(Username, Email, Password, Bdate, CreatedAt, UpdatedAt)
-	Product(Name, Price, CreatedAt, UpdatedAt)
-	이렇게 설계한 것들이 모델이고 모델을 통해 객체를 찍어낸다
-	Review(Content, CreatedAt, UpdatedAt)
-	이 정보들을 실질적으로 데이터베이스에 저장하고
-	그 데이터베이스에서 정보를 불러와서 사용자 화면에 띄워줌

-	CreatedAt, UpdatedAt : 객체가 언제 생성되고 언제 수정됐는지에 대한 정보는 중요함 -> 꼭 가지고 있어야 함

-	모델링의 각각의 컬럼이 어떤 데이터 타입을 가지고 있는지 중요

-	Username : String

-	Email : String

-	Bdate : date

	---

-	Relation : 상호간의 관계

-	리뷰 <- 어떤 사용자가 어떤 상품에 대해 썼는지 <- 외래키를 통해 가져옴

-	관계까지 설정하는 것이 모델링

	---

-	장고에서 모델링을 할 수 있다!

	---

### 처리과정

-	클라이언트의 요청(request)

-	url conf 모듈을 이용하여 url을 분석

-	해당 url에 대한 처리를 담당하는 view 결정

-	view는 로직을 실행하면서 데이터베이스 처리가 필요한 것은 모델을 통해 처리하고 결과를 반환하여 템플릿을 활용해 클라이언트에 전송할 html 파일 생성

-	view는 최종결과로서 html 파일을 클라이언트에게 응답(response)

-	11번가 링크 누름 -> 11번가 서버로 request를 보냄 -> 모델을 거쳐 데이터베이스로부터 ORM을 통해 상품 오브젝트들을 가져와서 템플릿에 데이터들을 뿌려준 뒤 응답으로 이 화면을 사용자에게 띄워준다.

### References

-	[Django의 개발 방식 MTV 패턴](https://revidream.tistory.com/16)
