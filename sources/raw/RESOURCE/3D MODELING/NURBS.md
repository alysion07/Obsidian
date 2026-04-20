**Non-Uniform Rational B-Splines**
![[Pasted image 20240903155003.png]]
 3D 형상을 수학적으로 표현하는 데 적합
 Polygon Model은 평활도가 부족하기 때문에 최종 설계에 사용되지 않는다. 

## Why NURBS are used

- **Flexibility** to create sculptural shapes
- **Tension** to keep surfaces smooth and taut
- **Alignment** to create smooth, invisible joins

## NURBS, Polygon 비교

폴리곤 모델링은 전통적으로 캐릭터 애니메이션 모델링과 게임 모델링에 사용되어 왔는데, 특히 다음과 같은 장점이 있습니다.

- 표면 디테일 만들기(캐릭터 얼굴의 주름, 초목, 바위)
- 빠른 렌더링 계산(폴리곤 모델은 이미 테셀레이션되어 있음)

![[Pasted image 20240903155015.png]]

다각형 모델은 부드러운 모양에 가까운 평평한 패싯(Facet)의 집합입니다. 이는 렌더링 또는 프로토타이핑에는 적합하지만(실제로 NURBS 모델은 이러한 목적을 위해 다각형 표현으로 변환됨) **프로덕션에는 적합하지 않습니다.**

최종 제품용 툴링을 만드는 CNC 공작 기계는 정확하고 부드러운 **NURBS** 데이터를 기반으로 작동합니다.