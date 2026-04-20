## 개념

밉맵은 미리 계산되고 최적화된 일련의 텍스처이며, 각 텍스처는 원본 텍스처를 점진적으로 낮은 해상도로 표현합니다. 다양한 거리와 각도로 표면에 텍스처를 적용할 때 렌더링 속도를 높이고 [[RESOURCE/IT Glossary/Artifact|아티팩트]]를 줄이는 데 사용됩니다.

![[Pasted image 20250106135754.png]]


크기가 다른 텍스처를 폴리곤에 입혀서 그리려는 경우, 폴리곤의 크기에 맞추어 텍스쳐 픽셀의 평균값을 내는 작업은 GPU에서 너무 느린 작업임

- GPU 에서는 **밉맵(Mipmap)** 을 사용하여 문제 해결 
- 밉맵은 점차적으로 작아지는(**1/4씩 크기가 줆**) 이미지의 집합
- 일반적으로 작은 크기의 텍스처는 이전 레벨로부터 **쌍선형 보간(Bilinear interpolation)** 하여 만들어짐 

![[Pasted image 20240726095204.png|center]]
<center>16X16 픽셀 텍스처에 대한 밉맵</center>
##### `gl.generateMipmap`

가장 큰 레벨을 보고 가장 작은 밉맵을 만들어 줌. 작은 레벨별 밉맵을 직접 입력 가능 
- WebGL2에서는 텍스처들이 **'TextureComplete'** 이여야  함. 아닐 경우 렌더링 안됨
- 가장 쉬운 방법은 `gl.generateMipmap` 호출
##### Teture Filtering
- `NEAREST`: 가장 **큰** 밉맵에서 **1**개의 픽셀을 선택
- `LINEAR`: 가장 **큰** 밉맵에서 **4**개 픽셀을 선택하고 블렌딩
- `NEAREST_MIPMAP_NEAREST`: 가장 적절한 밉맵을 선택, **1**개의 픽셀을 선택
- `LINEAR_MIPMAP_NEAREST`: 가장 적절한 밉맵을 선택하고 **4**개 픽셀을 블렌딩
- `NEAREST_MIPMAP_LINEAR`: 가장 적절한 두 개의 밉맵을 선택, 각 **1**개씩 픽셀 블렌딩
- `LINEAR_MIPMAP_LINEAR`: 가장 **적절한 두 개**의 밉맵을 선택, 각 **4**개씩의 픽셀 블렌딩

필터링 모드를 설정하기 위해서는 아래처럼 `gl.texParameter`를 호출

```CPP
gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MIN_FILTER, gl.LINEAR_MIPMAP_LINEAR);
gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MAG_FILTER, gl.LINEAR);
```

- `TEXTURE_MIN_FILTER`: 그리려고 하는 크기가 가장 큰 밉맵보다 작을 경우
- `TEXTURE_MAG_FILTER`: 그리려고 하는 크기가 가장 큰 밉맵보다 큰 경우 
- `TEXTURE_MAG_FILTER`: `NEAREST`와 `LINEAR`만 설정 가능
##### 밉맵의 중요성
`NEAREST`, `LINEAR`을 사용하면 가장 큰 이미지로부터 픽셀값을 선택하기 때문에 Flickering 현상이 많이 발생함 

아래 이미지는 설정된 필터링에 따라 어떤 밉맵이 선택되는지 보여준다

<center>NEAREST, LINEAR, NEAREST_MIPMAP_NEAREST</center>
![[Pasted image 20240726102325.png|center|400]]
<center>LIN_MIP_NEAR, NEAR_MIP_LIN, LIN_MIP_LIN (약어)</center>
![[Pasted image 20240726102335.png|center|400]]

##### `LINEAR_MIPMAP_LINEAR`이외의 옵션이 필요한 이유
- LINEAR_MIPMAP_LINEAR 가장 **느림**  (텍스처 1개당 8개의 픽셀 읽음)
- 특정한 효과 기대 -  레트로_한 픽셀화 효과를 얻기 위해서는 `NEAREST`를 사용해야만 함
- 작은 크기 밉맵을 생성하기 위해 **33% 더 많은 메모리 공간을 필요**로 함.가장 큰 밉맵만 사용한다면 그냥 `NEAREST`나 `LINEAR`를 사용(생성 x)

##### Texture Complete
1. 필터링 방법을 유저가 설정하여 **가장 큰 밉맵**만 사용하는 경우 즉, `TEXTURE_MIN_FILTER`를 `LINEAR` 또는 `NEAREST`로 설정한 경우.
2. 밉맵 사용시 그 크기가 적절(?)하고 1X1 크기까지 **모든 밉맵을 제공**하는 경우

#generateMipmap #mipmap
 
## Filter

### 종류 
###### NearestFilter 
- 가장 가까운 **픽셀 1개 취득**

![[Pasted image 20250106140031.png]]

###### LinearFilter
- 가장 가까운 **픽셀 4개 취득 후 선형 보간**

![[Pasted image 20250106140224.png]]

###### **Nearest**Mimmap**Linear**Filter
1. Mipmap 이미지 2장 선택 후,
2. 가장 가까운 픽셀 1개 취득 
3. 가중치 평균 


![[Pasted image 20250106135919.png|600]]

###### **Nearest**Mipmap**Nearest**Filter 
1. 크기가 가장 비슷한 Mipmap 이미지 **1개** 선택 
2. **가장 가까운 픽셀 1개 취득** 

![[Pasted image 20250106140239.png]]

###### **Linear**Mipmap**Nearst**Filter

1. 크기가 가장 비슷한 Mipmap 이미지 **1개** 선택
2. 가장 가까운 **픽셀 4개 취득 후 선형 보간**
 ![[Pasted image 20250106140250.png]]
 ###### **Linear**Mipmap**Linear**Filter
 1. 크기가 비슷한 Mipmap 이미지 **2장** 선택
 2. 각 이미지 별, 가장 가까운 **픽셀 4개 취득 후 선형보간**
 3. 가중치 평균으로 **최종 색상값** 추출

![[Pasted image 20250106140301.png]]



### minFilter 

![[Pasted image 20250106140326.png]]

 ---
