#ARM 은  **Advanced RISC Machine**의 약자로 ARM Holdings가 개발한 컴퓨터 프로세서 제품군과 기본 기술을 나타냄. 

ARM 프로세서는 RISC(Reduced Instruction Set Computing) 아키텍처를 기반으로 함.
CISC 아키텍처보다 빠르고 효율적으로 작동할 수 있도록 더 적은 수의 컴퓨터 명령어 유형을 수행하도록 설계됨

 
 ## ARM Architecture의 주요 기능 및 이점
 1. 효율성 및 성능

	 - 전력 효율성으로 유명. 배터리 수명과 열 방출이 중요한 Mobile 장치 및 Embeded 시스템에 이상적
	 - RISC Architecture는 명령어 세트를 단순화하여 실행 속도를 높이고, 리소스를 효율적으로 사용
	 
 2.  다양성 
	 - SmartPhone, Tablet, Embeded 시스템, IoT, Server 등 다양한 Device에 사용 
	 - 저전력 및 고성능 어플리케이션에 모두 사용


## 명령어 세트 아키텍처(ISA):

ARM 프로세서는 단일 주기로 실행될 수 있는 단순화된 명령어 세트를 사용합니다. 이는 실행하는 데 여러 사이클이 필요할 수 있는 다른 아키텍처의 복잡한 명령어 세트와 대조됩니다.
이러한 단순성으로 인해 와트당 더 높은 성능이 가능하며 이는 배터리로 작동되는 장치에 중요한 이점입니다.

## 파이프라인 아키텍처 ([[Pipeline Architecture]])

ARM 프로세서는 성능 향상을 위해 파이프라인 아키텍처를 사용하는 경우가 많습니다. 파이프라이닝을 사용하면 여러 실행 단계에서 여러 명령을 동시에 처리할 수 있습니다.
## Thumb 및 Thumb-2 명령어 세트:

ARM은 코드 밀도를 향상시키기 위해 Thumb 명령어 세트를 도입했습니다. Thumb 명령어는 32비트 ARM 명령어에 비해 폭이 16비트이므로 코드를 더 간결하게 만들 수 있습니다.
Thumb-2는 추가 32비트 명령어로 Thumb 명령어 세트를 확장하여 성능과 코드 밀도 간의 균형을 제공합니다.
 
 
 #아키텍처 #architecture #arm #RISC #CISC