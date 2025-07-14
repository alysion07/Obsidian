# Config JSON 설정 문서

이 문서는 `config.json` 가이드 문서입니다.  
*작성자 : js.park@e8ight.co.kr*

---

## 프로젝트 기본 설정

### `version`  
- 설명: 설정 파일 버전
- 형식: `string`  
- 예시: `"0.1"`

### `project_name`  
- 설명: 프로젝트 명칭. 리포트에 출력
- 형식: `string`  
- 예시: `"Awesome project"`
- 기본값 : `"Awesome project"`

### `processing`  
- 설명: 사용될 처리 파이프라인. 
- 형식: `string`  
- 선택지 : 
    > `"CATCH"` - CATCH  
    > `"NADIR"` - NADIR  
    > `"OBLIQUE"` - OBLIQUE
- 예시 : `"CATCH"` 

---

## 하드웨어 리소스 설정 (`hw_resources`)

### `use_gpu`  
- 설명: GPU 사용 여부. <span style="color:gray">현재 우리가 사용할 AWS는 GPU를 사용하지 않기 때문에 false로 고정</span>
- 형식: `boolean`  
- 기본값: `false`

### `max_ram_ratio`  
- 설명: 사용 가능한 최대 RAM 비율  
- 형식: `float`
- 범위 : 0.0 ~ 1.0 
- 기본값: `0.75`

---

## 좌표계 설정 (`projection`)

### `horizontal_epsg`  
- 설명: 수평 좌표계 EPSG 코드  
- 형식: `int`
- 예시: `2056`

### `vertical_epsg`  
- 설명: 수직 좌표계 EPSG 코드  
- 형식: `int`
- 예시: `5728`

### `geoid` (optional)  
- 설명: 지오이드 모델  
- 형식: `string`
- 기본값: `"None"`

 참조 링크 : <https://developer.pix4d.com/server/2.9.0/supported_geoid_models.html>

---

## 카메라 캘리브레이션 설정 (`calibration_settings`)

### `templates` *(중요)*  
- 설명: 캘리브레이션 템플릿  
- 형식: `string`
- 선택지: 
    > `"LARGE"` - 대규모 장면의 항공 일반 영상 촬영과 함께 사용하도록 최적화  
    > `"FLAT"` - 평평하고 질감이 낮을 수 있는 장면의 항공 천저(Nadir) 데이터 세트에 최적화  
    > `"MAPS_3D"` - 그리드 비행 계획에서 이미지 중복이 많은 작은(500개 미만의 이미지) 항공 천저(Nadir) 또는 경사(Oblique) 데이터 세트에 최적화  
    > `"MODELS_3D"` - 이미지 중복이 많은 항공 경사(Oblique) 또는 지상파(Terrestrial) 데이터 세트에 최적화  
    > `"CATCH"` - PIX4Dcatch에서 캡처한 데이터 세트용으로 특별히 설계  
- 기본값 : 
    > `"LARGE"` - Nadir, Oblique  
    > `"CATCH"` - Catch  
    

### `use_detail_settings` *(중요)*  
- 설명: 상세 설정 사용 여부. `true`일 때, 아래 자세한 설정 값 사용   
- 형식: `boolean`  
- 기본값: `false`

### `image_scale`  
- 설명: 특징이 계산되는 이미지 축척. 배율은 이미지의 초기 크기에 대한 비율  
- 형식: `float`
- 범위: `0.5 ~ 2.0`  
    > `0.5` - 40Mpx 이상의 RGB 이미지  
    > `0.5 - 1.0` - 12 - 40Mpx의 RGB 이미지  
    > `1.0 - 2.0` - 낮은 해상도의 이미지 (멀티 스펙트럼, 모바일 캡쳐 등)   
- 기본값: `1.0`

### `matching_algorithm`  
- 설명: 매칭 알고리즘
- 형식: `string`
- 선택지
    > `"Standard"` - 기본값이며 대부분의 캡처 사례에 적합  
    > `"GeometricallyVerified"` - 느리지만 더 강력한 매칭 전략. 이 옵션을 선택하면 기하학적으로 일치하지 않는 일치 항목이 삭제. 이 옵션은 프로젝트 전반에 걸쳐 유사한 기능이 많이 있는 경우(예: 농경지의 식물 행, 건물 정면의 창 모서리 등)에 유용  
- 기본값: `"Standard"`

### `use_rig_matching`  
- 설명: 일치시킬 때 리그 인스턴스에 있는 모든 카메라의 일치를 결합. 리그 다중 스펙트럼 캡처 및 `pipeline = LowTexturePlanar` 와 `matching_algorithm = GeometricallyVerified`에만 관련있음
- 형식: `boolean`  
- 기본값: `false`

### `min_matches`  
- 설명: 보정에서 고려할 이미지 쌍당 최소 일치에 대한 임계값을 설정합니다. 쌍의 일치 항목 수가 이 임계값보다 작으면 쌍이 완전히 삭제  
- 형식: `int`  
- 기본값: `20`

### `min_image_distance`  
- 설명: 일치 또는 호모그래피 추정을 위한 최소 거리(xy-평면). 대부분의 경우 가장 가까운 이미지를 일치에서 제거하는 것은 의미가 없으므로 권장 값은 `0.0`  
- 형식: `float`
- 기본값: `0.0`

### `pipeline`  
- 설명: 캘리브레이션 파이프라인  
- 형식: `string`
- 선택지: 
    > `"Standard"` - (기본값) 표준 보정 파이프라인  
    > `"Scalable"` - 대규모 및 복도 캡처를 위한 보정 파이프라인  
    > `"LowTexturePlanar"` - 정확한 지리적 위치와 평면과 같은 장면 및 리그 카메라의 동질적이거나 반복적인 콘텐츠를 포함한 항공 천저 이미지를 위해 설계된 캘리브레이션 파이프라인  
    > `"TrustedLocationOrientation"` - 정확한 상대 위치 및 관성 측정(IMU) 데이터가 있는 프로젝트를 위해 설계된 캘리브레이션 파이프라인. 모든 이미지에는 위치 및 방향에 대한 정보가 포함되어야 함  
- 기본값: 
    > `"TrustedLocationOrientation"` - Catch  
    > `"Standard"` - Nadir, Oblique

### `calibration_int_param_opt` 
- 설명: 내부 카메라 매개변수에 대한 최적화 옵션. 매개변수 : 초점거리, 왜곡 등 센서관련  
- 형식: `string`
- 선택지: 
    > `"NoOptim"` - 내부 카메라 매개변수를 최적화하지 않음. 대형 카메라에 이미 보정된 경우 유용  
    > `"Leading"` - 가장 중요한 내부 카메라 매개변수만 최적화. 롤링 셔터 속도가 느린 카메라를 처리하는 데 권장  
    > `"All"` - (기본값) 모든 내부 카메라 매개변수(해당되는 경우 롤링 셔터 포함)를 최적화  
    > `"AllPrior"` - 모든 내부 카메라 매개변수(해당되는 경우 롤링 셔터 포함)를 최적화하지만 최적의 내부 매개변수가 초기 값에 가깝게 되도록 설정  
- 기본값: `"All"`


### `calibration_ext_param_opt`  
- 설명: 외부 카메라 매개변수에 대한 최적화 옵션. 매개변수 : 위치 및 방향  
- 형식: `string`
- 선택지: 
    > `"Motion"` - 회전과 방향을 최적화하지만 롤링 셔터는 최적화하지 않음  
    > `"All"` - (기본값) 회전 및 위치를 최적화하며, 카메라 모델이 선형 롤링 셔터 모델을 따르는 경우 선형 롤링 셔터를 최적화  
- 기본값: `"All"`

### `oblique`
- 설명: 비행 계획의 종류  
- 형식: `boolean`
- 기본값: 
    > `true` - Oblique, Catch  
    > `false` - Nadir

### `rematch`
- 설명: 리매치시, 초기 처리의 첫 번째 부분 이후에 더 많은 일치 항목이 추가됨  
- 형식: `boolean`
- 기본값: `false`

### `use_gcp_srs`
- 설명: 장면 참조 프레임 정의를 파생하기 위해 투영된 카메라 대신 GCP에 정의된 SRS를 사용  
- 형식: `boolean`
- 기본값: `false`

### `use_itps`
- 설명: 구조적 선 교차점에서 접합점을 생성하여 장면을 보정  
- 형식: `boolean`
- 기본값: `false`

---
## 컨트롤 포인트 (`control_points`)

### `use_auto_gcp`
- 설명: GCP 자동 감지 알고리즘 설정 여부. GCP 위치가 불확실 할 때 사용  
- 형식: `boolean`
- 기본값: `false`

### `xy_uncertainty`  
- 설명: 절대 수평 이미지 지리 참조 불확실성  
- 형식: `float`
- 기본값: `5.0`

### `z_uncertainty`  
- 설명: 절대 수직 이미지 지리 참조 불확실성  
- 형식: `float`
- 기본값: `10.0`

---


## 캘리브레이션 재최적화 설정 (`calibration_reopt_settings`)
 설정 항목은 `calibration_settings`와 유사. GCP의 데이터가 부족하거나 다른 위치정보가 부족할 때 사용됨.

### `calibration_int_param_opt` 
- 설명: 내부 카메라 매개변수에 대한 최적화 옵션. 매개변수 : 초점거리, 왜곡 등 센서관련  
- 형식: `string`
- 선택지: 
    > `"NoOptim"` - 내부 카메라 매개변수를 최적화하지 않음. 대형 카메라에 이미 보정된 경우 유용  
    > `"Leading"` - 가장 중요한 내부 카메라 매개변수만 최적화. 롤링 셔터 속도가 느린 카메라를 처리하는 데 권장  
    > `"All"` - (기본값) 모든 내부 카메라 매개변수(해당되는 경우 롤링 셔터 포함)를 최적화  
    > `"AllPrior"` - 모든 내부 카메라 매개변수(해당되는 경우 롤링 셔터 포함)를 최적화하지만 최적의 내부 매개변수가 초기 값에 가깝게 되도록 설정  
- 기본값: `"All"`


### `calibration_ext_param_opt`  
- 설명: 외부 카메라 매개변수에 대한 최적화 옵션. 매개변수 : 위치 및 방향  
- 형식: `string`
- 선택지: 
    > `"Motion"` - 회전과 방향을 최적화하지만 롤링 셔터는 최적화하지 않음  
    > `"All"` - (기본값) 회전 및 위치를 최적화하며, 카메라 모델이 선형 롤링 셔터 모델을 따르는 경우 선형 롤링 셔터를 최적화  
- 기본값: `"All"`

### `use_optimized_internals`  
- 설명: 보정된 장면에서 최적화된 내부를 사용할지 또는 초기 카메라의 내부를 사용할지 여부를 지정  
- 형식: `boolean`
- 기본값: `true`

---

## 밀집도 설정 (`dense_settings`)

### `templates` *(중요)*  
- 설명: 밀집도 템플릿  
- 형식: `string`
- 선택지: 
    > `"NADIR"` - 항공 천저(Nadir) 이미지 캡처에 최적화  
    > `"OBLIQUE"` - 항공 경사(Oblique) 또는 지상의 이미지 캡처에 최적화  
- 기본값 : 
    > `"NADIR"` - Nadir  
    > `"OBLIQUE"` - Oblique, Catch  

### `use_detail_settings` *(중요)*  
- 설명: 상세 설정 사용 여부. `true`일 때, 아래 자세한 설정 값 사용   
- 형식: `boolean`  
- 기본값: `false`

### `image_scale`  
- 설명: 이미지 스케일은 처리에 사용 될 최고 해상도 이미지의 다운샘플링 계수를 정의  
- 형식: `int`
- 범위: `0, 1, 2, 3`  
    > `0` - 원본 이미지 크기를 사용. 절반 크기 이미지에 비해 결과를 크게 향상시키지는 않음  
    > `1` - (기본값) 절반 크기(1/2) 이미지를 사용  
    > `2` - 쿼터 크기(1/4) 이미지를 사용   
    > `3` - 1/8 이미지를 사용  
- 기본값: `1`


### `point_density`  
- 설명: 동일한 카메라에서 재구성된 두 점이 가질 수 있는 이미지 평면의 최소 거리를 픽셀 단위로 정의  
- 형식: `int`
- 범위: `1, 2, 3`  
    > `1` - 고밀도. 3D 포인트는 모든 image_scale 픽셀에 대해 계산  
    > `2` - (기본값) 최적의 밀도. 3D 포인트는 4/image_scale 픽셀마다 계산  
    > `3` - 저밀도. 3D 포인트는 16/image_scale 픽셀마다 계산  
- 기본값: `2`

### `min_no_match`  
- 설명: 3D 포인트가 포인트 클라우드에 보관하기 위해 이미지에 가져야 하는 유효한 재투영의 최소 수(포인트를 재구성하는 데 필요한 최소 일치 수). 값이 높을수록 노이즈가 줄어들 수 있지만 계산된 3D 점의 수도 줄어듦.  
- 형식: `int`
- 범위: `2, 3, 4, 5, 6`  
    > `2` - 각 3D 포인트는 최소 2개의 이미지에서 올바르게 재투영. 작은 프로젝트에 권장  
    > `3` - (기본값) 각 3D 포인트는 최소 3개의 이미지에서 올바르게 재투영  
    > `4` - 각 3D 포인트는 최소 4개의 이미지에서 올바르게 재투영  
    > `5` - 각 3D 포인트는 최소 5개의 이미지에서 올바르게 재투영  겹침이 매우 많은 경사 이미지 프로젝트에 권장
    > `6` - 각 3D 포인트는 최소 6개의 이미지에서 올바르게 재투영. 겹침이 매우 많은 경사 이미지 프로젝트에 권장.  
- 기본값: `3`

### `window_size`  
- 설명: 원본 이미지의 조밀한 점을 일치시키는 데 사용되는 정사각형 그리드의 크기(픽셀)  
- 형식: `int`
- 범위: `7, 9`  
    > `7` - 7x7 픽셀 그리드를 사용. NADIR 템플릿에 권장  
    > `9` - 9x9 픽셀 그리드를 사용. 원본 이미지에서 밀집된 점을 보다 정확하게 배치하는 데 유용. OBLIQUE 템플릿에 권장  
- 기본값: `7`

### `multi_scale` 
- 설명: `true`(기본값)로 설정하면 알고리즘은 매개 변수로 선택한 해상도 외에 동일한 이미지의 더 낮은 해상도 `image_scale` 사용. 경우에 따라 노이즈가 증가하는 대신 완성도가 향상   
- 형식: `boolean`  
- 기본값: `true`

### `limit_depth` 
- 설명: 점이 재구성되는 깊이를 제한하여 배경 객체를 재구성하지 않도록 하므로 객체의 3D 모델에 유용. Oblique 프로젝트에서 사용.   
- 형식: `boolean`  
- 기본값: 
    > `true` - Oblique, Catch  
    > `false` - Nadir

### `regularized_multiscale` 
- 설명: 균일성이 실패한 경우에만 더 낮은 해상도의 패치를 사용. 이 옵션은 `multi_scale` 비활성화된 경우 적용되지 않음   
- 형식: `boolean`  
- 기본값: `false`

### `min_image_size`  
- 설명: `multi_scale` 활성화된 상태에서 밀도를 높일 때 사용되는 최소 이미지 크기. 이미지가 사용될 가장 큰 규모를 결정. 기본값(512)을 사용하는 것을 권장  
- 형식: `int`
- 기본값: `512`

### `depth_limit_percentile`  
- 설명: 깊이 값을 제한하는데 사용. `limit_depth`가 true일 때만 적용  
- 형식: `float`
- 기본값: `0.949999988079071`

### `uniformity_threshold`  
- 설명: 점을 생성하는 데 필요한 최소 텍스처 콘텐츠에 대한 임계값. 기본값 사용하는 것을 권장  
- 형식: `float`
- 범위: `[0, 1]`  
- 기본값: `0.03125` (1/32)

### `compute_depth_maps` 
- 설명: 별도의 깊이맵을 계산. 메쉬 생성 단계에서 얇은 구조물의 3D메쉬를 생성할 때만 사용.    
- 형식: `boolean`  
- 기본값: `false`


---

## Mesh 설정 (`mesh_settings`)

### `templates`  
- 설명: 일반적으로 사용되는 이미지 캡처 유형에 대한 사전 정의된 설정  
- 형식: `string`
- 선택지: 
    > `"LARGE"` - 대규모 장면 및 항공 천저(Nadir) 이미지 캡처에 최적화  
    > `"SMALL"` - 작은 장면과 공중 경사(Oblique) 또는 지상파(terrestrial) 이미지 캡처에 최적화  
    > `"TOWER"` - 타워와 같은 구조에 최적화  
- 기본값 : `"SMALL"`

---

## 텍스처 설정 (`texture_settings`)

### `templates`  
- 설명: 다양한 품질의 형상 재구성 및 이미지 캡처를 위한 사전 정의된 설정  
- 형식: `string`
- 선택지: 
    > `"STANDARD"` - 잘 재구성된 형상과 이미지 캡처에 최적화. 폐색이나 움직이는 물체가 거의 없을 때 사용.  
    > `"DEGHOST"` - 불완전한 형상과 많은 폐색 또는 이동 물체가 있는 이미지 캡처에 최적화  
- 기본값 : `"STANDARD"`

---

## DTM 설정 (`dtm_settings`)

### `rigidity`  
- 설명: 포인트 클라우드의 베이스 위에 놓인 시뮬레이션된 지면 모델의 장력 <span style="color:gray">(Tension of simulated cloth overlying the base of the point cloud)</span>  
- 형식: `string`
- 선택지: 
    > `"Low"`  
    > `"Medium"`   
    > `"High"`   
- 기본값 : `"Medium"`

### `filter_threshold`  
- 설명: 지형 분류를 위한 컷오프 임계값  
- 형식: `float`
- 기본값: `0.5`

### `cloth_sampling_distance`  
- 설명: 포인트 클라우드의 베이스 위에 있는 시뮬레이션된 지면 모델델을 파생하는 데 사용되는 포인트 간 샘플링 거리  
- 형식: `float`
- 범위: `[1.0, 1.5]`  
- 기본값: `1.0`

---

## 파일 패턴 (`file_patterns`)

### `image_pattern`  
- 정규 표현식으로 이미지 파일 필터링  
- 예시: `"^(?!._).*.jp.*g$"` → `.jpg`, `.jpeg` 포함, 숨김 파일 제외

---

## 하늘 마스킹 (`skyseg`)

이미지로부터 하늘, 물을 마스킹으로 식별할 때 사용. Oblique에서만 사용

### `mode`
- 설명: 세분화 모드를 선택  
- 형식: `string`
- 선택지: 
    > `"FULL"` - 전체 세분화 모드  
    > `"FAST"` - 빠른 세분화 모드. 물 세그먼트에서는 권장하지 않음 
- 기본값 : `"FULL"`

### `masking_type`
- 설명: 세분화 모드를 선택  
- 형식: `string`
- 선택지: 
    > `"SKY"` - 하늘 세그먼트를 식별  
    > `"WATER"` - 물 세그먼트를 식별  
    > `"SKY_WATER"` - 하늘과 물 세그먼트를 식별   
- 기본값 : `"SKY"`

---

## 정사영상 설정 (`ortho_settings`)

### `fill_occlusion_holes` 
- 설명: 오클루젼 구멍(카메라에 캡처되지 않은 영역)을 가장 가까운 이미지의 픽셀로 채움.  
- 형식: `boolean`  
- 기본값: `true`

### `blend_ratio`  
- 설명: 이미지 패치의 경계에서 블렌딩할 영역의 크기를 결정하는 계수. `0.0`은 블렌딩이 없음을 의미하며, 이로 인해 경계가 굵어짐. `1.0`은 가장 가까운 두 이미지의 완전한 혼합을 의미. *0.1 - 0.2의 값을 권장*.  
- 형식: `float`
- 범위: `[0.0, 1.0]`  
- 기본값: `0.1`

### `pipeline`
- 설명: 정사영상 생성에 사용할 알고리즘 파이프라인의 유형.  
- 형식: `string`
- 선택지: 
    > `"FAST"` - 속도 지향 알고리즘  
    > `"FULL"` - 품질 지향 알고리즘  
    > `"DEGHOST"` - 움직이는 물체의 제거를 목표로 하는 알고리즘. `FULL`의 확장형  
- 기본값 :
    > `"FAST"` - Nadir  
    > `"FULL"` - Oblique, Catch  

### `capture_pattern`
- 설명: 이미지 캡처에 사용되는 사진 유형  
- 형식: `string`
- 선택지: 
    > `"NADIR"`  
    > `"OBLIQUE"`  
- 기본값 : 
    > `"NADIR"` - Nadir  
    > `"OBLIQUE"` - Oblique, Catch  

### `pan_sharpening` 
- 설명:  팬 샤프닝을 활성화합니다(팬크로매틱 밴드 필요).  
- 형식: `boolean`  
- 기본값: `false`

#### 참고사항
- `FAST`/`OBLIQUE`의 조합은 좋은 결과를 보장하지 않음. (권장하지 않음)  
- `FULL`/`NADIR`의 조합도 작동하지만 `FAST`/`NADIR`의 조합을 더 권장함

#### 선택 가이드
- 나디어 촬영(카메라가 아래를 향함) → `FAST`/`NADIR`  
- 고정익 나디어 비행 → `FAST`/`NADIR` 또는 불안정한 공기(바람, 난기류)에서는 대체로 `FULL`/`NADIR`  
- 이중 격자 비행(카메라가 10도 이상 기울어진 경우) → `FULL`/`OBLIQUE`  
- 나디어 촬영과 혼합된 이중 격자 비행(사선 촬영) → `FULL`/`OBLIQUE`  
- 원형 촬영(원 또는 여러 개의 원) → `FULL`/`OBLIQUE`  
- 타워(어떤 비행 플랜이든) → `FULL`/`OBLIQUE`  
- Catch → `FULL`/`OBLIQUE`  
- 지형을 따라 일정 고도로 비행(카메라가 아래를 향함) → `FAST`/`NADIR` 또는 대체로 `FULL`/`NADIR`  
- 지형을 따라 이중 격자 비행, 카메라가 풍경 쪽으로 기울어짐 → `FULL`/`OBLIQUE`  
- 지형을 따라 단일 격자 비행, 카메라가 풍경 쪽 또는 각도를 가진 나디어 → `FULL`/`OBLIQUE`  
- 다중 스펙트럼(항상 나디어) → `FAST`/`NADIR` + Triangulation() DSM 방식 (M3M/P4M용)  
- 단독 열화상 촬영 - 사용 불가  
- 움직이는 물체, 잡동사니(크레인 카메라, 도로, 건설 현장의 사람들 등) → `DEGHOST`  

---

## GeoTIFF 설정 (`geotiff`)

### `cog` 
- 설명:  GDAL을 활용하여 cog(Cloud Optimized GeoTIFF)를 생성. `merged`가 `true`일때만 적용  
- 형식: `boolean`  
- 기본값: `false`

### `tiling` 
- 설명:  타일 저장 여부  
- 형식: `boolean`  
- 기본값: `false`

### `merged` 
- 설명:  하나로 병합(with tile) 여부  
- 형식: `boolean`  
- 기본값: `true`

### `compression` 
- 설명: `LZW` 압축 여부  
- 형식: `boolean`  
- 기본값: `true`

---

## LAS 설정 (`LAS`)
포인트 클라우드 객체를 LAS/LAZ 파일로 직렬화. 형식(LAS/LAZ)은 `.las` 또는 `.laz`(대/소문자 구분 안 함)와 일치하는 경우 파일 확장명에서 결정되며, 그렇지 않으면 `compress` 매개변수에 의해 판별

### `las_version`
- 설명: las 버전  
- 형식: `string`
- 선택지: 
    > `"V1_2"` - las 1.2 출력  
    > `"V1_4"` - las 1.4 출력 
- 기본값 : `"V1_2"`

### `compress` 
- 설명: `True`/`False`인 경우 각각 출력 형식을 `LAZ`/`LAS`로 설정. `output_path` 확장에서 형식을 유추할 수 있는 경우에는 효과가 없음음  
- 형식: `boolean`  
- 기본값: `false`

---

## 로깅 설정 (`logging`)

### `level`  
- 설명: 로그 남기는 레벨 설정  
- 형식: `string`
- 선택지: 
    > `"CRITICAL"` - 50  
    > `"FATAL"` -  CRITICAL  
    > `"ERROR"` - 40  
    > `"WARNING"` - 30  
    > `"WARN"` - WARNING  
    > `"INFO"` - 20  
    > `"DEBUG"` - 10  
    > `"NOTSET"` - 0  
- 기본값 : `"INFO"`


 