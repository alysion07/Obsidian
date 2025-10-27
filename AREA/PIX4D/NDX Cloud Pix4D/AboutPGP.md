## **PGP 관련 자료**

### **PGP 계산**
- PGP = Processed PigaPixel = 1,000 Megapixels.
- Example : 200장 * 20MP = 4,000MP = 4 PGP


### **PGP 소모 타이밍**
- Calibration (Processing)을 수행하면 그 뒤 어떤 행위나 프로세싱을 하더라도 동일한 PGP를 소모함
- Example : Calibration 이후 Ortho, DSM, DTM, Mesh 생성 및 export PGP == Calibration 이후 Ortho만 생성 및 export.
- Calibration 이후 Ortho, DSM, Mesh 생성이 실패하더라도 Calibration을 수행했기 때문에 PGP소모
![Mesh 생성 실패했으나 PGP는 소모함](money_losing.png)

Mesh 생성 실패했으나 PGP는 소모함


### **PGP당 예상 시간**
- 사진에서는 0.346 PGP가 소모되었고, Calibration에서는 48초가 소모
- 또 다른 예시에서 0.356 PGP를 소모 했을 때 Calibration에서 53초 소모. (GPU를 사용하지 않았을 때 77초)
- GPU를 사용했을 때 약 1 PGP에서 약 144초 소모 (Calibration에서만). 하지만 Calibration 세팅마다 계산영역이 다르기 때문에 케이스마다 학습해야함. >> 단정짓기 어려움



![안보이겟지~](AWS_Benchmark_2024.png)

2024 공식 벤치마크에서도 영역당 계산시간이 일정하게 분배되지 않음

