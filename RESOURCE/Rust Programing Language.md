
[Rust 번역](https://doc.rust-kr.org/title-page.html)

#### 소유권
- Rust가 채택한 메모리 관리 방식.  
- 소유권이라는 시스템을 만들고, 컴파일러가 컴파일 중에 검사할 여러 규칙을 정해 메모리를 관리. 
- 이 규칙 중 하나라도 위반하면 프로그램은 컴파일 되지 않음.
- 소유권은 프로그램 실행 속도에 영향을 주지 않음.
##### Stack Area & Heap Area 
###### Stack
후입선출 방식(Last in First Out). 값이 들어온 순서대로 저장하고, 역순으로 제거
스택에 **저장되는 데이터는 모두 명확하고 크기가 정해져 있어야 함**.
컴파일 타임에 크기를 알 수 없거나, 크기가 변경될 수 있는 데이터는 스택 대신 **힙**에 저장되어야 합니다.

###### Heap
데이터를 힙에 넣을 때 먼저 저장할 공간이 있는지 운영체제에 물어봅니다. 그러면 메모리 할당자는 커다란 힙 영역 안에서 어떤 빈 지점을 찾고, 이 지점은 사용 중이라고 표시한 뒤 해당 지점을 가리키는 _포인터 (pointer)_ 를 우리한테 반환합니다. 이 과정을 _힙 공간 할당 (allocating on the heap)_, 줄여서 _할당 (allocation)_ 이라 합니다 (스택에 값을 Push하는 것은 할당이라 부르지 않습니다).

 포인터는 크기가 정해져 있어 스택에 저장할 수 있으나, 포인터가 가리키는 실제 데이터를 사용하고자 할 때는 포인터를 참조해 해당 포인터가 가리키는 위치로 이동하는 과정을 거쳐야 합니다. 힙 구조는 레스토랑에서 자리에 앉는 과정으로 비교할 수 있습니다. 레스토랑에 입장하면, 직원에게 인원수를 알립니다. 그러면 직원은 인원수에 맞는 빈 테이블을 찾아 안내하겠죠. 이후에 온 일행이 우리 테이블을 찾을 땐 직원에게 물어 안내 받을 겁니다.
 
스택 영역은 데이터에 접근하는 방식상 힙 영역보다 속도가 빠릅니다. 메모리 할당자가 새로운 데이터를 저장할 공간을 찾을 필요가 없이 항상 스택의 가장 위에 데이터를 저장하면 되기 때문이죠. 반면에 힙에 공간을 할당하는 작업은 좀 더 많은 작업을 요구하는데, 메모리 할당자가 데이터를 저장하기 충분한 공간을 먼저 찾고 다음 할당을 위한 준비를 위해 예약을 수행해야 하기 때문입니다.