---
layout: post
title: 웹앱 과정 - 모바일 웹
categories : htmlcss
tags : viewport
comments : true
---

## 뷰포트(Viewport)
- 보여지는 창문이라는 뜻
- 화면에 보여지는 요소
1. 레이아웃 뷰포트 (layout viewport) : 장치의 전체 화면 해상도에 해당하는 영역
2. 비주얼 뷰포트 (visual viewport) : 전체 화면 중 현재 화면에 보이는 영역

- 스마트폰은 모바일 브라우저 창의 크기가 화면 최대 크기로 고정됨 -> 비주얼 뷰포트 = 레이아웃 뷰포트
- 각 스마트폰 브라우저별로 실제보다 훨씬 큰 크기의 뷰포트가 설정되어 있다.
    <- 모바일에 최적화되지 않은 페이지도 모두 볼 수 있도록
- 이 뷰포트를 기준으로 글자 크기를 표시 -> 글자 크기가 작아진다.

### 뷰포트 메타 태그
- `<meta>` 태그를 이용하여 뷰포트 정보를 제공
- 화면 해상도에 따른 최대, 최소 너비를 지정할 수 있게 한다.

- `width` : 뷰포트의 너비
    예) 480px, device-width
- 'height' : 뷰포트의 높이
    예) 800px, device-height
- `initial-scale` : 초기 화면 확대 비율
    예) 1.0
- `minimun-scale` : 화면 축소 최대 비율
    예) 0.5
- 'maximum-scale' : 화면 확대 최대 비율
    예) 2.0
- 'user-scalable' : 사용자의 확대 혹은 축소 가능 여부
    예) yes(default), no

뷰포트 선언의 예
```HTML
<meta name="viewport" content="width=device-width; initial-scale=1.0; minimum-scale=1.0; maximum-scale=1.0; user-scalable=no"/>
```

일반적인 뷰포트 선언
```HTML
<meta name="viewport" content="width=device-width; initial-scale=1.0"/>
```
