#정규분포 #가우시안 #스플래팅 

참고: [3D Gaussian Splatting 완벽 분석 (feat. CUDA Rasterizer)](https://velog.io/@gjghks950/3D-Gaussian-Splatting-%EC%99%84%EB%B2%BD-%EB%B6%84%EC%84%9D-feat.-CUDA-Rasterizer)
참고: [[논문 리뷰] 3D Gaussian Splatting (SIGGRAPH 2023) : 랜더링 속도/퀄리티 개선](https://xoft.tistory.com/51)
## Gaussian or Normal 이란?

- 자연 현상을 묘사할 때, 다음과 같이 **종(bell) 모양의 함수 형태**를 띄면 이를 **가우시안**(Gaussian)이라 칭함

![[Pasted image 20250225103850.png]]



- 가우시안 블러링을 사용하여 블러효과를 구현하는 경우 평균값 블러를 사용할 때보다 

####  평균값 필터링의 단점
- 필터링 대상 위치에서 가까이 있는 픽셀과 멀리 있는 픽셀이 모두 같은 가중치를 사용하여 평균을 계산
- 멀리있는 픽셀의 영향을 많이 받을 수 있음
- **가우시안 필터는 멀리 있는 픽셀들의 영향이 적음**


![[Pasted image 20250225104314.png| 좌: 평균값 블러 우: 가우시안 블러 (바둑판의 선이 조금 더 선명)]]


[[NeRF(Neural Radiance Fields)]]와는 다른 개념의 3D 렌더링 
