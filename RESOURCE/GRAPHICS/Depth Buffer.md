
- ==Z-Buffer==라고도 불린다. 
- 깊이 픽셀로 이루어진 직사각형 깊이 픽셀은 우리가 **정점 쉐이더**에서 반환하는 **Z값**을 기반으로 계산됩니다.
- X와 Y값에 대해 클립 공간으로 변환해야 하는 것처럼, Z값도 클립 공간( **-1 ~ +1** 사이값)으로 변환

어떤 픽셀 뒤쪽에 있는 픽셀은 그려지지 않는다.
그리기 전 깊이 버퍼 초기화 필요

```js title:"WebGL에서의 사용"
gl.enable(gl.DEPTH_TEST);
```

```js hl:3-4
function drawScene() {
    ...
    // canvas와 깊이 버퍼를 clear
    gl.clear(gl.COLOR_BUFFER_BIT | gl.DEPTH_BUFFER_BIT);
    ...
```


---

## Three.js 에서

- 3차원 객체를 카메라를 통해 좌표로 변환 시켜 화면상에 렌더링 될 때 해당 3차원 객체를 구성하는 각 픽셀의 깊이값을 **0 ~ 1** 사이의 값으로 **normalize한 값이 저장된 Buffer**
- 0에 가까울수록 카메라에 가까운 Object 객체의 픽셀임

![[Pasted image 20250106094652.png]]

> [!NOTE]
> Z-Buffer 의 주용도는 멀리 있는 객체의 픽셀이 **가까이 있는 픽셀을 덮어쓰여지지 않도록 하기 위함**
