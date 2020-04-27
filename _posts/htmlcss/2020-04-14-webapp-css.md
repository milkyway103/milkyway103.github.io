---
layout: post
title: 웹앱 과정 - CSS
categories : htmlcss
tags : CSS
comments : true
---

CSS -> 화면 구성이나 표현 형식 등 디자인 부분을 HTML 대신 CSS에 위임함으로써 문서 구조와 문저 표현을 분리할 수 있다 -> 확장성, 재사용성

```CSS
h1
  {
    color: blue;
    background: gray;
  }
```

### 선택자
- `*` : 전체 선택자 <- 별로 사용하지 않는다. 모든 태그에 동일한 스타일을 적용하는 경우는 거의 없다.
- 태그명 : 태그 선택자
- .클래스명 : 클래스 선택자 -> Jquery에서도 똑같이 사용한다.
- #아이디명 : 아이디 선택자
- 계층 선택자
    - 선택자 > 태그명 : 자식 선택자
    - 선택자 + 태그명 : 형제(근접후행) 선택자
    - 선택자 ~ 태그명 : 형제(후행) 선택자
- 상태 선택자 : 특성 상태 조건을 충족하는 선택자
    - 링크선택자
        - 선택자:link
        - 선택자:visited
    - 반응선택자
        - 선택자:hover (마우스 포인터가 올라가 있는 상태)
        - 선택자:active (클릭하고 있는 중)
    - 부정선택자
        - 선택자:not

```CSS
/* 태그 선택자 */
p { background-color: blue; border: red; }
/* 복수 태그 선택자 */
h1, h3, p { background-color: yellow; }
/* 아이디 선택자 */
#id1 { border: solid red; }
/* 클래스 선택자 */
.class1 { border: solid purple; }
```

- 실무에서는 일반적으로 아이디와 클래스를 둘 다 쓰고, 이름을 동일하게 해 준다. <- Jquery에서 둘 다 쓰기 때문에.

### 스타일 적용의 우선 순위
1. 인라인 방식
2. 내부 방식
3. 외부 방식
4. 임포트 방식

제일 좋은 건 외부 방식을 주로 쓰고 꼭 필요한 경우에 인라인 방식을 추가로 사용하는 것

1. 아이디 선택자
2. 클래스 선택자
3. 계층 선택자
4. 태그 선택자

보통 가장 우선순위가 높은 것을 맨 밑에 쓴다.

### CSS 속성값의 단위
- 상대 단위 : `%`, `em`, `ex`, `px`
- `px`, `%`, `em` 등이 가장 많이 쓰인다.

### 박스 모델
- 엘리먼트들을 박스로 간주한다 -> 박스 모델

<center>
   <figure>
   <img src="/assets/post-img/htmlcss/box_model.png" alt="views">
   <figcaption>box model</figcaption>
   </figure>
</center>

- `display` 속성에서 `none`으로 하여 엘리먼트를 숨길 수 있다.
