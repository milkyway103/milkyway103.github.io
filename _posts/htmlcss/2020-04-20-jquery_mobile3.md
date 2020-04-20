---
layout: post
title: 웹앱 과정 - 제이쿼리 모바일 (3)
categories : html&css
tags : jQuerymobile
comments : true
---

# 제이쿼리 모바일 버튼과 리스트뷰
## 버튼
- 버튼 : 보통 사용자 의사를 전달하거나 요청을 위해 사용
- 모바일 페이지에서는 **페이지 간 이동 수단** 으로 주로 사용

```HTML
<!-- <a> 태그 버튼 -->
<a href="#" data-role="button">버튼 제목</a>

<!-- <button> 태그 버튼 -->
<button>버튼 제목</button>

<!-- <input> 태그 버튼 -->
<input type="button" value="버튼 제목">
```

태그와 명세 방법은 다르지만 생성된 버튼의 모양은 모두 같다.
default 모양은 둥근 모서리와 테두리 음영 효과, 크기는 화면 전체의 너비나 문자열 길이에 비례하고 버튼 문자열은 가운데 정렬

## 기본 버튼 종류
### 링크 버튼 (`<a>`)
- 버튼처럼 보여지는 링크
- 가장 많이 사용됨
- `<a>` 태그가 기존 링크 문자열 형태로 표시될 경우 손으로 터치하기 불편하여 작은 화면의 모바일 장치에는 부적합
    -> 제이쿼리 모바일이 `<a>` 태그를 버튼으로 변환함
- `data-inline="true"` : 버튼 너비를 문자열(아이콘 포함) 크기에 맞춤
- `data-min="true"` : 버튼 높이와 글자 크기를 기본보다 작게 표시
- `data-corners="false"` : 버튼 모서리를 직각으로 표시
- `data-shadow="false"` : 버튼 밑의 음영 효과를 표시하지 않음
- 콘텐츠 영역에서는 반드시 `data-role="button"` 설정을 해 줘야 한다.

### 폼 버튼
- 입력, 수정 등 폼과의 상호 작용 혹은 완성된 폼의 제출을 위해 사용

```HTML
<button type="버튼 유형">버튼 제목</button>
<input type="버튼 유형" value="버튼 제목">
```

- type 속성값이 `submin`, `reset`, `button`에 해당할 경우 모바일 버튼으로 자동 변환된다.
- `<button>` 태그의 경우도 type 속성을 명세하는 것이 바람직
- `data-role="none"` : 변환 전의 기존 버튼 모양으로 표시됨
- `<button>` 태그에 의해 생성된 버튼은 주로 자바스크립트의 실행을 호출하는 역할을 위해 사용

### 아이콘 버튼
- 버튼을 생성하는 마크업 안에 `data-icon` 속성을 추가
- 제이쿼리 모바일에서 40개의 표준 아이콘을 제공

```HTML
<button type="버튼 유형" data-icon="아이콘이름">버튼 제목</button>
<input type="버튼 유형" value="버튼 제목" data-icon="아이콘이름">
```

- `data-role="none"` 속성값을 설정하면 고전적인 버튼 모양에 아이콘은 표시되지 않는다.
- `data-iconpos="notext"` : 문자열 없이 아이콘만으로 버튼 생성

```HTML
<button type="버튼 유형" data-icon="아이콘이름" data-iconpos="notext">버튼 제목</button>
<input type="버튼 유형" value="버튼 제목" data-icon="아이콘이름" data-iconpos="notext">
```

- `data-iconpos="top|bottom|left|right"` : 버튼 내 아이콘의 위치 변경
- `data-icon` : 사용자 정의 아이콘 추가. 추가하는 아이콘의 이름은 CSS로 미리 정의되어야 한다. 이때 추가하는 CSS 클래스의 이름은 반드시 `.ui-icon-` 접두어 뒤에 `data-icon` 속성값으로 설정하는 식별자, 즉 사용자 정의 아이콘 이름을 붙여서 명세

```HTML
<style>
  .ui-icon-my-icon:after{
    background: url(buttonicon1.png); /* url(아이콘파일 경로명) */
    background-size: 18px 18px; /* 아이콘이미지크기 */
  }
</style>

<button data-icon="my-icon">버튼 제목</button>
```

> 서용자 정의 아이콘을 적용하려면 png 형식의 아이콘 이미지 파일을 준비. 18*18 픽셀의 흰색(또는 투명)에 알파 투명도가 적용된 PNG-8 형식으로 크기를 약간 작게 만든다. 선명한 아이콘을 원하면 36*36 크기의 이미지를 사용하여 `background-size` 속성값을 18px 18px로 준다.

### 이미지 버튼
- 이미지만으로 버튼을 만들 수 있다.
```HTML
<a href="#" data-role="button"> <img src="이미지파일경로명"> </a>
```
### 버튼 그룹
- 여백 없이 버튼들을 모아서 표시
    -> 의미적으로 밀접하게 관련된 버튼들을 그룹화할 때 사용
- `data-role="controlegroup"`을 갖는 `<div>` 태그로 버튼들을 감싸면 된다.
- `data-inline="true"` 속성을 갖는 버튼이 여러 개 있을 때 버튼 그룹으로 그룹화하는 것이 좋다.
- 버튼들은 기본적으로 세로 방향으로 결합해서 표시됨
- `data-type="horizontal"` : 버튼들을 가로 방향으로 나열

### 버튼 테마
- 제이쿼리 모바일에서 제공하는 스타일 기본 테마는 `a`, `b` 2가지
- `data-theme="버튼테마이름"` 속성값으로 다른 테마 적용 가능
- 보통 콘텐츠 컨테이너에 `data-theme` 속성값을 설정하는 것이 바람직

## 리스트뷰
- 제이쿼리 : 모바일 페이지들은 대부분 리스트뷰 기반의 레이아웃
- 리스트뷰(listview) : 여러 리스트들, 즉 목록들을 모아 간결한 형태로 화면에 표시
- 기본 리스트뷰(basic listview)
    - 세로로 나열
    - 항목 정보만을 목록으로 제공
    - '읽기전용 리스트뷰'
    - 보통 비순서 리스트 태그 `<ul>` 안에 `data-role="listview"` 속성값을 준다.
    - 순서 리스트 태그 `<ol>` 에도 가능
    - 각 목록 항목 버튼들의 너비는 페이지 전체 너비와 동일
    - 디폴트 테마는 `a`

```HTML
<ul data-role="listview">
  <li>항목 이름</li>
  ...
</ul>
```

- 연결 리스트뷰(linked listview)
    - 항목 선택이 가능
    - 기본 리스트뷰의 각 항목에 `<a>`를 추가하면 연결 리스트뷰가 된다.
    - 특정 페이지 이동을 안내하거나 다른 하위 리스트뷰를 연결하기 위해 사용
    - 기본 리스트뷰의 각 항목에 오른쪽 화살표가 추가된 모양
    - 링크 버튼의 간격도 조금 넓어지면서 '오른쪽 화살표' 아이콘이 오른쪽에 자동으로 추가됨
    - 연결 리스트뷰 항목의 내용이 길 경우, 자동으로 내용을 자르고 생략 기호(...)를 추가해 준다.

```HTML
<ul data-role="listview">
  <li><a href="#">항목이름</a></li>
  ...
</ul>
```

- 순서 리스트뷰
    - 자동으로 항목마다 번호가 앞에 표시됨
    - `<ol>` 태그 안에 `data-role="listview"` 속성을 주면 된다.
    - 순위나 순번들을 갖는 항목에 적용

```HTML
<ol data-role="listview">
  <li>항목 이름</li>
  ...
</ol>
```

- 인셋 리스트뷰(inset listview)
    - 콘텐츠 영역 안에 둥근 사각형 모양의 내부 공간을 형성하고 그 안에 리스트뷰를 표시
    - `<ul>` 안에 `data-inset="true"` 속성값을 주어 사용
    - 기본 리스트뷰는 여백이 없어 보기에 좋지 않고 페이지에 다른 내용들을 함께 표시할 경우 조화를 이루기가 쉽지 않다.
    - 인셋 리스트뷰는 화면의 가로, 세로 길이에 비례하여 상하좌우에 적정 여백과 둥근 모서리 모양을 가진다.

- 분할 리스트뷰(split listview)
    - 각 항목 영역을 분할하여 둘 이상의 링크를 통해 여러 페이지와의 연동이 가능
    - 분할된 항목 영역은 독립된 분할 버튼으로서 동작
    - 각 링크 항목은 수직선으로 구분됨
    - 첫 번째 링크는 주 항목으로 문자열 버튼으로 표시
    - 두 번째 링크는 보조 항목으로 간주하며 아이콘 버튼만 표시
    - 예) 첫 번째 문자열 버튼은 상세 정보를 열람, 두 번째 아이콘 버튼은 수정 편집을 위해 선택하도록 구성 가능
    - 두 번째 버튼의 기본 아이콘은 오른쪽 화살표 모양
    - `<ul>` 태그 안의 `data-split-icon`속성에 원하는 아이콘 이름을 명시하여 변경 가능
    - 기본적으로 테마 `a`가 적용됨
    - `data-split-theme` : 테마 수정 가능

```HTML
<li><a href="#">분할 항목 이름1</a><a href="#">분할 항목 이름2</a></li>
```

## 리스트뷰 고급
### 카운트 버블(count bubble)
- 리스트뷰 항목의 둥근 거품 방울 모양의 화면 요소
- 전체 하위 항목이나 새로운 항목의 개수, 항목 내용을 강조하고자 할 경우 유용
- 리스트뷰의 일반 항목이나 리스트 분리자에 모두 표시 가능
- 기본적으로 테마 `c`

```HTML
<li><a href="#">상위 항목 이름<span class="ul-li-count">5</span></a></li>
<li><a href="#">상위 항목 이름<span class="ul-li-count">new</span></a></li>
```

### 리스트 구분자(list devider)
- 항목들을 그룹으로 분류하고 각 그룹별로 분류명을 표시
- 항목 그룹에 대한 헤더와 같은 역할

```HTML
<li data-role="listdivider">구분자 제목</li>
```

### 썹네일 리스트뷰
- 리스트뷰에서 썸네일 이미지를 항목 왼쪽에 추가 가능
> 썸네일 이미지 : 데이터 전송의 효율성을 위해 변환된 작은 크기의 이미지

- 리스트뷰의 각 항목 내용으로 `<img>` 태그를 추가하면 된다.
- `<img>` 태그는 각 리스트 항목의 문자열보다 먼저 명세되어야 한다.

```HTML
<li><a href="#"><img src="썸네일 이미지파일 경로명">항목이름</a></li>
```
- 설정하는 썸네일 이미지의 기본 크기는 80*80 픽셀의 정사각형 모양
- 기본 크기보다 더 커도 리스트뷰에서는 자동으로 80*80 픽셀 크기로 조정되는데 이때, 이미지의 가로 크기를 기준으로 크기가 축소된다.
- 기본 크기보다 작은 이미지는 원본 크기 그대로 표시된다.
- 이미지 크기를 강제로 80*80 픽셀로 지정할 수 있다.

```HTML
<ul data-role="listview">
  <li>
    <a href="#">
      <img src="listimg.png" style="max-width: 80px; max-height:80px;">리스
      트1
    </a>
  </li>
</ul>
```

### 아이콘 리스트뷰
- `<img>` 태그에 `ul-li-icon` 클래스 속성을 추가하면 썸네일 이미지 대신 크기가 더 작은 이미지를 아이콘처럼 추가 가능
- 각 항목 왼쪽에 아이콘을 작게 표시

### 여러 줄 리스트뷰
- 표시 내용의 중요도에 따라 강조할 내용은 `<h>` 제목 태그로, 나머지 내용은 `<p>` 문단 태그를 사용하여 일정한 형식으로 여러 줄에 표시 가능

```HTML
<li>
  <h3>제목</h3><p>내용1</p><p>내용2</p>
  <p class="ul-li-aside">추가설명</p>
</li>
```

- `ul-li-aside` 클래스 속서을 설정하여 항목 오른쪽에 날짜와 같은 추가 정보 표시 가능

### 검색 필터 리스트(search filter list)
- 제이쿼리 모바일은 검색 필터 리스트를 제공한다.
- 항목들의 맨 위쪽에 검색 필터 상자를 추가할 수 있다.
- 검색어를 입력하면 키워드 검색 방식으로 일치하는 항목만 걸러서 보여준다.
- `<ul>` 태그 안에 `data-filter="true"` 속성을 설정
- `data-filter-placeholder="..."` : 검색 필터 상자에 기본적으로 표시되는 초기 문자열

 ```HTML
<ul data-role="listview" data-filter="true" data-filter-placeholder="검색 키워드를 입력하세요">
 ```

> `<a>` 링크 태그는 링크 기능을 제거하고 사용하기도 한다. 이 경우 `href="#"` 라고 기입하는데, 이를 빈 링크(empty link)라고 하며 클릭 이벤트 발생시 불필요한 페이지 전환을 방지하기 위해 주로 사용한다.

## 제이쿼리 모바일 위젯
### 페이지 탭(page tap)
- 같은 페이지 공간 안에 여러 개의 탭 페이지를 생성하여 각 탭 페이지를 선택적으로 브라우징하는 인터페이스
- 모바일 화면의 좁은 페이지 공간을 효율적으로 활용하는 방법 중 하나
- 보통 현재 페이지와 의미적으로 연관된 복잡하고 다양한 정보들을 제공할 때 사용
- 각각의 탭 컨테이너 상위에 `data-role="tabs"` 속성을 갖는 `<div>` 태그를 설정하여 사용 가능

```HTML
<div data-role="tabs">
  <div data-role="header">
    <ul>
      <li><a href="#tab1">탭1</a></li>
      <li><a href="#tab2">탭2</a></li>
      <li><a href="#tab3">탭3</a></li>
    </ul>
  </div>
  <div id="tab1">...</div>
  <div id="tab2">...</div>
  <div id="tab3">...</div>
</div>
```

- 위에서 tab1, tab2, tab3 아이디를 가지는 탭 컨테이너들은 페이지 탭 설정에 의해 같은 공간을 공유한다.

### 컬럼 토글 테이블 (column toggle table)
- 좁은 공간을 위해 테이블의 컬럼을 선택적으로 표기하거나 숨길 수 있는 테이블
- 숨겨진 컬럼들은 표시를 원할 경우, 컬럼 팝업 메뉴를 통해 선택함으로써 표시된다.
- 화면 폭에 비해 테이블 컬럼들의 수가 너무 많은 경우, 각 컬럼들에 대해 우선 순위를 부여함으로써 표시되는 테이블 컬럼 항목들을 제한한다.
- `<table>` 태그에 `data-role="table"`, `data-mode="columntoggle"` 속성값을 설정한다.
- `id` 속성도 추가해주는데, 컬럼 선택 팝업 메뉴와 테이블을 연결시키기 위해 필요
- `class="ui-responsive"` : 다양한 화면 폭에 따라 허용되는 범위만큼 자동으로 표시되는 테이블 컬럼들을 조절
- `class="table-strip"` : 테이블의 인접한 행들을 쉽게 구별할 수 있도록 줄무늬 형태로 표시
- `<th>` 태그 안에 `data-priority` 속성값을 설정함으로써 가장 낮은 1부터 6까지 표시되는 컬럼들의 우선 순위를 지정할 수 있다. `data-priority` 속성값이 지정된 테이블 헤더 컬럼들은 컬럼 선택 메뉴에 표시된다. 생략하면 화면 폭에 상관없이 항상 고정적으로 표시되며, 컬럼 선택 메뉴에 표시되지 않는다.

```HTML
<div data-role="page">
  <div data-role="header">
    <h1>header 영역</h1>
  </div>
  <div data-role="content">
    <table class="ui-responsive table-strip" id="my-table1" data-mode="columntoggle" data-role="table">
      <thead>
        <tr>
          <th data-priority="2">학년</th>
          <th>이름</th>
          <th data-priority="3">성별</th>
          <th data-priority="1">성적</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>1</th>
          <td>홍길동</td>
          <td>남</td>
          <td>94</td>
        </tr>
        <tr>
          <th>4</th>
          <td>신나라</td>
          <td>여</td>
          <td>77</td>
        </tr>
        <tr>
          <th>3</th>
          <td>홍미미</td>
          <td>여</td>
          <td>88</td>
        </tr>
      </tbody>
    </table>
  </div>
  <div data-role="footer" data-position="fixed">
    <h4>footer 영역</h4>
  </div>
</div>
```

### 숨김 패널(hidden panel)
- 숨김 패널 안에 메뉴나 폼 등을 숨겨 놓았다가 필요할 때 페이지를 전환하지 않고도 제공 가능
- 컨테이너에 `data-role="panel"` 속성 설정
- `data-display` : 패널의 표시 방식을 지정, `overlay` 속성값은 페이지 내용 위에 패널이 펼쳐지는 형태로 표시
- 링크 버튼에 `data-rel="close"` 속성을 추가하면 숨김 패널 안에 종료 버튼이 추가된다.
- `data-position` : 화면 안에서의 숨김 패널의 위치를 설정. 기본값은 `left` 보통 화면의 왼쪽 가장자리로부터 숨겨진 패널 내용이 표시됨
- 숨김 패널 컨테이너는 페이지 컨테이너 안에는 포함하되 헤더, 컨텐츠, 푸터 컨테이너 안에 포함시키지는 않는다.

```HTML
<div data-role="page">
  <div data-role="panel" id="panel1" data-display="overlay" data-theme="b">
    <form>
      <h3>로그인 패널</h3>
      <label for="name">사용자명:</label>
      <input name="name" id="name" type="text" data-mini="true" data-clear-btn="true">
      <label for="password">비밀번호:</label>
      <input name="password" id="password" type="password" value="" data-mini="true" data-clear-btn="true">
      <div class="ui-grid-a">
        <div class="ui-block-a"><a href="#" data-role="button" data-rel="close">취소</a></div>
        <div class="ui-block-b"><a href="#" data-role="button" data-rel="close">저장</a></div>
      </div>
    </form>
  </div>
</div>
```
