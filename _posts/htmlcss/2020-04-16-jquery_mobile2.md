---
layout: post
title: 웹앱 과정 - 제이쿼리 모바일 (2)
categories : html&css
tags : jQuerymobile
comments : true
---

# 제이쿼리 모바일 헤더바와 푸터바
## 헤더바
###툴바
- 다른 페이지로의 이동이나 페이지 관련 처리 기능을 모아 놓은 인터페이스로 링크, 버튼, 텍스트 등으로 구성된 바 형태
- 제이쿼리 모바일은 헤더 영역의 <mark>헤더바</mark>, 푸터 영역의 <mark>푸터바</mark>를 제공한다.
    페이지의 콘텐츠 영역과는 다른 색상으로 구별되어 표시된다.

```HTML
<!-- 헤더바 생성 -->
<div data-role="header"></div>
<!-- 푸터바 생성 -->
<div data-role="footer"></div>
```

### 헤더 문자열
- 헤더 영역은 왼쪽 영역, 제목 문자열 영역, 오른쪽 영역으로 나누어진다.
- 제목 문자열 영역의 공간은 제한됨 -> 문자열이 길어지면 자동으로 생략 기호(...)가 추가된다.
- 주로 페이지 제목을 표시
- `<h1>`~`<h6>` 제목 태그 중 선택 가능. 제이쿼리 모바일에서 화면 표현상의 차이는 없다. 관습적으로 `<h1>` 사용

```HTML
<div data-role="header"><h1>헤더 문자열</h1></div>
```

### 헤더 버튼
- 좌우에 버튼이 최대 두 개까지 표시된다.
- 헤더 컨테이너 영역 안에서 `<a>` 태그는 버튼으로 자동 변환되어 표시된다.
- 첫 번째 링크는 왼쪽 버튼으로, 두 번째 링크는 오른쪽 버튼이 된다.
- 버튼의 너비는 문자열 길이에 따라 다르다.

```HTML
<div data-role="header"><a href="#">헤더 버튼</a></div>
```

### 뒤로가기 버튼
- `data-add-back-btn="true"` : 헤더 영역에 뒤로가기(back)버튼이 자동으로 표시된다.
    첫 번째 페이지에는 적용되지 않고 두 번째 이후 페이지부터 동작한다.
    클릭하면 바로 이전 페이지로 이동하며 이동할 때의 이전 화면 전환과 반대 방향의 애니메이션 효과
- `data-add-back-btn-text="뒤로가기"` : 버튼의 문자열 변경
- `data-backbtn="false"` : 뒤로가기 버튼의 자동 생성을 원하지 않을 때

### 오른쪽 버튼
- `<a>` 태그 버튼이 첫 번째 버튼이면 기본적으로 헤더 영역의 왼쪽 버튼으로 표시됨
- `class="ui-btn-right"` 속성을 `<a>` 태그에 추가하면 첫 번째 버튼을 오른쪽에 표시할 수 있다.
- `class="ui-btn-left"` : 두 번째 버튼을 왼쪽에 표시
- `data-add-back-btn` 속성값을 설정해도 헤더바에 직접 링크 버튼을 추가하면 자동 뒤로가기 버튼이 표시되지 않음

## 푸터바
- 모바일 페이지의 마지막 엘리먼트
- 보통 콘텐츠 영역 밑, 페이지의 가장 아래쪽에 위치

### 푸터바 문자열 추가
- `<h1>`~`<h6>` 까지의 제목 태그를 사용할 수 있다.
    헤더바에 비해 중요도 낮음 -> 보통 `<h4>`~`<h6>` 태그 사용
```HTML
<div data-role="footer"><h4>푸터 텍스트</h4></div>
```

### 푸터바 버튼 추가
- 푸터 컨테이너 안의 `<a>` 태그도 버튼으로 자동 변환된다.
- 왼쪽부터 하나씩 인라인 방식으로 추가됨
- 여러 개의 버튼 표시 가능

```HTML
<div data-rold="footer"><a href="#">푸터 버튼</a></div>
```
- 이때 버튼 주위에 기본 여백이 설정되지 않음
- 여백이 필요하다면 `ui-bar` 속성을 설정 : 약간의 여백을 두고 버튼을 연속으로 배열
```HTML
<div data-rold="footer"><a href="#" class="ui-bar">푸터 버튼</a></div>
```

### 푸터바 그룹 버튼 추가
- `data-role="controlgroup"` : 버튼 사이에 여백을 전혀 두지 않음
- 버튼을 그룹으로 모아 표시 -> 버튼들 사이의 공간과 둥근 모서리가 모두 사라짐
- `data-type="horizontal"` : 버튼들을 수평으로 나열 default는 `vertical` (수직)

## 네비게이션바와 툴바
- = 메뉴바, 탭바

### 네비게이션바
- 순서 없는 리스트 항목들을 하나의 '수평 버튼 바' 모양으로 만들어주는 버튼
- 헤더바와 푸터바 모두 추가 가능
- 헤더바에서 3개 이상의 버튼이 필요할 경우 네비게이션바를 사용한다.
- 보통 푸터바에 많이 배치
    모바일 기기를 손에 쥐고 쉽게 메뉴를 선택하도록

```HTML
<!-- 네비게이션바 선언 -->
<div data-role="navbar">
  <ul>
    <li><a href="#">page1</a></li>
    <li><a href="#">page2</a></li>
    <li><a href="#">page3</a></li>
  </ul>
</div>
```

- `<li>` 태그의 개수만큼 네비게이션바 공간이 같은 크기로 분할됨
- 항목이 많을수록 네비게이션바 안의 버튼 너비는 줄어듬
- 보통 한 줄에 1~5개의 버튼을 생성
- 6개 이상이면 한 줄에 버튼 두 개씩 여러 줄에 걸쳐 표시됨
    스마트폰에서 화면 너비의 1/5 영역이 터치를 위한 최소한의 크기로 간주됨
- `class="ui-btn-active"` : 여러 버튼들 중 하나가 이미 선택되었음을 활성화하여 표시

### 툴바 위치 설정
- `data-position="fixed"` : 헤더바와 푸터 컨테이너에 설정하면 헤더바와 푸터바의 표시 위치를 화면 맨 위와 맨 밑으로 고정 가능
    스크롤이 완료되면 현 화면의 맨 위와 맨 아래에 헤더바와 푸터바가 표시됨
- `data-fullscreen="true"` : 헤더바와 푸터 컨테이너에 설정
    content 영역을 눌러 헤더바와 푸터바를 모바일 페이지에서 사라지게 하고 필요할 때만 화면에 동적으로 표시하도록 할 수 있다.
    `data-position="fixed"` 속성과 반드시 함께 사용해야 한다.
    화면을 터치하여 헤더바와 푸터바의 표시와 숨김 두 가지 상태를 반복 가능
- 페이지 이동 시에도 화면 하단에 같은 푸터바를 설정할 수 있다.
  푸터 컨테이너의 `data-id` 속성값을 같게 설정
  -> 지속형 푸터바
  `ui-state-persist` 클래스 속성을 추가하면 네비게이션바 안의 선택된 버튼도 그대로 활성 상태를 유지할 수 있다.