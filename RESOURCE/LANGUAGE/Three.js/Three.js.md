![[Pasted image 20250102152517.png]]


Geometry

###  지오메트리의 내부 속성 데이터

![[Pasted image 20250102153422.png]]


## Transform




## [[Obejct3D]]




#### Matrix 
**Scale > Rotation > Translation** 순으로 적용 올바르게 적용됨 변환 행렬은 교환법칙을 성립하지 않기 때문.

```ts title:"pos를 먼저 설정해도 이상없이 적용됨"
box.position.set(0, 2, 0,);  
box.rotation.x = degToRad(45);  
box.scale.set(2, 2, 2);
```

위 코드 처럼 `geometry` 내부 함수를 사용해 변환하는 경우는 **Three.js**에서 내부적으로 SRT 순서로 처리해주기 때문에 순서에 상관없이 설정할 수 있다.

###### 부모 자식 관계

![[Pasted image 20250103152714.png|부모 객체의 변환 영향을 받음]]

```ts hl:5,10
const material = new THREE.MeshStandardMaterial();  
const geoParent = new THREE.BoxGeometry(2, 2, 2);  
const parent = new THREE.Mesh(geoParent, material);  
parent.position.y = 2;  
parent.rotation.z = degToRad(45);  
  
const geoChild = new THREE.BoxGeometry(1, 1, 1);  
const child = new THREE.Mesh(geoChild, material);  
child.position.x = 3;  
child.rotation.y = degToRad(45);  
  
parent.add(child);  // 자식으로 설정
this.scene.add(parent);
```

## SceneGraph

![[Pasted image 20250103152504.png]]



## Material 

![[Pasted image 20250103163222.png]]

#### Line 
###### LineSegments 
- 매개변수로 전달 받은 Geometry Buffer를 2개씩 끊어 라인의 시작점, 끝점으로 인식하여 라인을 Draw 해준다.


![[Pasted image 20250106092630.png|LineSegment example|350]]

###### LineLoop
-  Geometry Buffer의 첫번째 인덱스와 마지막 인덱스를 이어 하나의 이어진 선응로 Draw 해준다.


![[Pasted image 20250103163236.png]]

#### Mesh 
###### MeshBasicMaterial 
- 매개변수로 전달된 `color` 값 그대로 Mesh를 표현해 준다. 
- 사용 가능한 `Attribute`
	- `visible`:  가시화 여부
	- `transparent`: opacity 속성 사용 여부  
	- `opacity`: 투명도 (0 ~ 1)
	- `depthTest`: `Z`값 비교 연산 사용 여부
	- `depthWrite`:  `Z`값 저장 여부,  
- `side`: Culling 속성


![[Pasted image 20250106112336.png]]
- MeshLambertMaterial
- MeshPyshicsMaterial

###### MeshToonMaterial
- 젤다의 전설에서 사용된 머터리얼 효과
- **HDRi 광원은 사용불가함** 
- mipmap이 반드시 설정되어야함 

```ts hl:4,5 ar:3
private setupModels() {  
    const textureLoader = new THREE.TextureLoader();  
    const toonTexture = textureLoader.load("./toon.png");  
    toonTexture.minFilter = THREE.NearestFilter;  
    toonTexture.magFilter = THREE.NearestFilter;  
  
    const material = new THREE.MeshToonMaterial({  
        gradientMap: toonTexture,  
    });
}
```


![[Pasted image 20250106113628.png| **4x1** Texture 필요]]
 ![[Pasted image 20250106113738.png|texture를 활용한 ToonMaterial]]
### MeshStandardMaterial
- `displacementMap `: HeightMap을 사용해 geometry의 좌표를 변형시켜 입체감을 얻으려는 목적으로 사용됨
- `




# Light 

![[Pasted image 20250106170127.png]]

- **Directional Light, Point Light, Spot Light**는 그림자를 생성할 수 있는 광원임
### AmbientLight
- `scene`에 있는 모든 `Object`를 전체적으로 균등하게 비추는 광원
- Shading 없는 단순한 색상으로 렌더링 된다.
- 세기를 약하게 (0.1)하여 그려지지 않는 물체가 없도록 하는 용도로 사용

HemisphereLight
`AmbientLight`와 달리 두 개의 색상값 을 가짐
- 하나는 위에서 하나는 아래서 비치는 광원의 색상 값
