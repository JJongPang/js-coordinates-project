# Coordinates-Project

## 구현할 기능 목록
+ Browser client screen에 mouse move시  mouse 포인터 기준으로 target이미지 이동, 해당 좌표 x, y축 출력

## 결과물
![coordinates](https://user-images.githubusercontent.com/68219486/91557826-28e61e00-e970-11ea-8732-9ebe3e38097c.JPG)
![moving](https://user-images.githubusercontent.com/68219486/91557830-2a174b00-e970-11ea-87e9-2aeaf0d7bf10.JPG)

#### Project URL: https://jjongpang.github.io/js-coordinates-project/

<br />

## 성능 개선
### Critical rendering path
- requests/response -> loading -> scripting -> rendering -> layout -> painting
- Construction = DOM, CSSOM, RenderTree(간략한 DOM 즉 Tag남용, wrapper class남용 등...)
- Opertaion = layout, *paint, composition
- paint = 비트맵 형태로 데이터로 변환하여 레이어 단계를 만들어서 레이어별로 paint 준비.

```Javascript
//기존 코드
const horozontal = document.querySelector('.horozontal');
const vertical = document.querySelector('.vertical');
const target = document.querySelector('.target');
const tag = document.querySelector('.tag');

document.addEventListener('mousemove', (event) => {
    const x = event.clientX;
    const y = event.clientY;

    vertical.style.left = `${x}px`;
    horozontal.style.top = `${y}px`;

    target.style.top = `${y}px`;
    target.style.left = `${x}px`;

    tag.style.top = `${y}px`;
    tag.style.left = `${x}px`;

    tag.innerHTML = `${x}px, ${y}px`;
});
```

```Javascript
//개선 코드
const horizontal = document.querySelector('.horizontal');
const vertical = document.querySelector('.vertical');
const target = document.querySelector(".target");
const tag = document.querySelector('.tag');

addEventListener('load', () => {
    const targetRect = target.getBoundingClientRect();
    const targetHalfWidth = targetRect.width / 2;
    const targetHalfHeight = targetRect.height / 2;    

document.addEventListener('mousemove', (event) => {
    const x = event.clientX;
    const y = event.clientY;

    vertical.style.transform = `translateX(${x}px)`
    horizontal.style.transform = `translateY(${y}px)`
    target.style.transform = `translate(${x - targetHalfWidth}px, ${y - targetHalfHeight}px)`
    tag.style.transform = `translate(${x + 20}px, ${y + 20}px)`
    tag.innerHTML = `${x}px, ${y}px`;
   });
});
```
##### 마우스 move시 transform을 이용하여 paint 없이 composition 발생 으로 인해 성능 개선 확인

## 참조
#### css triggers: http://csstriggers.com/
