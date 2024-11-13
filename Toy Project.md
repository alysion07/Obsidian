
## Matrix Multiply(MVP, SRT)

#### Camera Matrix
- **Model**  ->  **View** -> **Projection** 순서로 행렬을 곱해야 함 
- 코드 상에서는 연산자 우선순위에 의해 오른쪽에서부터 곱셈이 실행되므로, 아래와 같은 순서로 작성해야 한다.

```js
// Camera Matrix 곱 순서 
matrix = projectionMatrix * viewMatirx * modelMatrix ;
```

#### Model Matrix
- **Scale** -> **Rotate** -> **Translate** 순서로 행렬 곱함

```js
Translate 
```





## Near, Far

Frustum의 Near, Far의 간극이 **10^6** 이상 차이나면 Z-Fighting이 심해지며 좋지 않은 현상을 자주 목격하게 됨.