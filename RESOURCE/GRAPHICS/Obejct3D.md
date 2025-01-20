![[Pasted image 20250103141037.png| **Object3D**가 장면에 배치되기 위한 속성]]

Three.js 대부분의 객체에 대한 **기본 클래스** 
3D 공간에서 객체를 조작하기 위한 일련의 속성 및 메서드를 제공함

[.add](https://threejs.org/docs/index.html#api/ko/core/Object3D.add "Object3D.add")( object ) 메서드를 사용해 객체를 그룹핑해서 오브젝트를 자식으로 추가하는 데에 사용할 수도 있지만, [Group](https://threejs.org/docs/index.html#api/en/objects/Group "Group")를 사용하는 것이 더 낫습니다.

### .[getWorldPosition](https://threejs.org/docs/index.html#api/ko/core/Object3D.getWorldPosition) ( target : [Vector3](https://threejs.org/docs/index.html#api/en/math/Vector3) ) : [Vector3](https://threejs.org/docs/index.html#api/en/math/Vector3)

[target](https://threejs.org/docs/index.html#api/en/math/Vector3 "Vector3") — 결과값은 이 Vector3에 복제됩니다.  
  
객체의 월드 스페이스에서의 벡터를 리턴합니다.