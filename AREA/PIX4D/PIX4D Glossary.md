Oblique: 비스듬한, 경사진 촬영 이미지 
Nadir: 천저 이미지, 상공에서 정확하게 90도로 지면을 찍은 촬영 이미지 
Point Cloud Library(PCL): 
Point Cloud Tile set(pnts):


**Calibration** 

## Tie points
GCP및 MTP는 사진 측량 프로젝트의 절대 및 상대 정확도를 개선하는데 사용됨.
체크포인트는 품질 평가에 사용된다.

**Ground Control Points (GCPs)**:  일반적으로 GCP 좌표는 매우 정확하며 **RTK/PPK GNSS** 수신기 또는 토탈 스테이션을 사용하여 측정된다. GCP는 재구성의 정확성을 평가하기 위해 프로젝트를 정확하게 지리 참조하는 데 사용

**Manual Tie Points (MTPs)**: 이미지에서 사용자가 표시(클릭)한 피쳐에 해당하는 3D 점. 재구성 정확도를 평가하고 개선하는데 사용
**CheckPoint(CKP)** : 체크포인트는 모델의 절대 정확도를 평가하는 데 사용되며 프로젝트를 지리적으로 참조하는 데는 사용되지 않습니다.