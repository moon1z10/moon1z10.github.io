---
title:  "[javascript] Counter-Up. 지정된 숫자까지 숫자를 증가시켜주는 애니메이션 적용하기"
excerpt: "javascript를 통해 지정된 숫자까지 증가되는 애니메이션을 적용시켜보자."

toc: true
toc_sticky: true
toc_label : "페이지 목차"

categories:
  - Development
tags:
  - javascript
last_modified_at: 2020-03-05T15:00:00-00:00
---
------------

숫자가 0에서 지정된 숫자까지 증가되서 멈춰지는 애니메이션 효과를 주고 싶을 때가 있다.
주로 현황판(Dashboard)에서 많이 사용되는 애니메이션으로, 쉽게 코딩이 적용 가능하다.

1. 일단 [GitHub](https://github.com/bfintal/Counter-Up)에 공개된 소스와 demo를 보자.

2. 해당 저장소의 `jquery.counterup.js` 또는 `jquery.counterup.min.js` 를 다운로드 받아 본인의 알맞은 js위치에 저장한다.

3. 본인의 HTML에 아래 코드를 삽입시킨다.
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/waypoints/4.0.1/jquery.waypoints.min.js"></script>
<script src="js/jquery.counterup.min.js"></script>
```

- 첫번재 script는 waypoints 외부 라이브러리를 참조하는 것으로, 스크롤링 할 때 해당 위치에서 실행시킬 수 있도록 구현해주는 스크립트이다.

> GitHub에 있는 url과 다르다. 필자는 jquery 3.4.1버전을 사용하는데, 버전 충돌이 나서 waypoints 최신 버전으로 수정하였다.

- 두번째는 저장된 위치를 알맞게 수정하자.

4. Counter-Up 시키고 싶은 곳에 counter class를 추가해준다.

```html
<span class="counter">1,234,567.00</span>
<span>$</span><span class="counter">1.99</span>
<span class="counter">12345</span>
```

5. jquery를 이용해서 body나 head안에 아래 코드 중 아무거나 집어 넣는다.

```javascript
$('.counter').counterUp();
```

또는

```javascript
$('.counter').counterUp({
    delay: 10,
    time: 1000
});
```

> delay : 숫자를 증가시키고 다음 번 증가에 대해서 얼마나 지연을 줄건지 milliseconds
time : 마지막 숫자까지 총 애니메이션 길이

------------