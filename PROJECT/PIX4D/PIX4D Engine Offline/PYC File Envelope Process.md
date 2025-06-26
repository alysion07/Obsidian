
Thales의 '**Envelope' Tool** 을 사용하여 Python code(.pyc) 난독화 진행   
  

1. Thales 라이선스 매니져를 통해 전달된 H/W info를 기반하여 **H/W Key 생성**
2. PIX4Dengine pyc 파일과 python310.dll를 **페어링 하여 암호화 및 난독화 진행**   

![[Pasted image 20250626085805.png| H/W key와 함께 Envelope 기능을 사용하여 암호화]]

![[Pasted image 20250626090135.png|Thales의 H/W Info 추출 상황]]


3. 암호화된 Interpreter에서만 난독화된 .pyc 구동 가능   
  - 생성된 **H/W Key**를 통해 Thales 라이선스 **검증을 통과해야 pyc파일 사용 가능**   
  - 검증 실패시 import , call 불가능 (ex. 등록되지 않은 H/W key, 라이선스 만료 등)  

![[Pasted image 20250626085820.png| 암호화에 사용되었던 H/W Key를 사용해 암호화된 Python으로만 엔진 구동 가능]]

- 암호화된 Interpreter에서의 난독화되지 않은 일반 .pyc 파일에 대한 사용은 전혀 문제 없음  
- 난독화된 pyc의 경우는 decompile 불가능