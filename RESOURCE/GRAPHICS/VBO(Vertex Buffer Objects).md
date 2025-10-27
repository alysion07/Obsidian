 #VBO 
 VBO는 많은 양의 정점들을 GPU 메모리 상에 저장할 수 있습니다. 이러한 버퍼 객체를 사용하면 대량의 데이터를 한꺼번에 그래픽 카드로 전송할 수 있다는 장점이 있습니다. CPU에서 그래픽 카드로 데이터를 전송하는 것은 **비교적 느립**니다. 그래서 **가능한 많은 데이터를 한 번에 보내야 합니다.** 데이터가 그래픽 카드의 메모리에 할당되기만 하면 vertex shader는 거의 즉각적으로 빠르게 정점들에 접근할 수 있습니다.
 
OpenGL의 모든 객체처럼 이 버퍼도 버퍼에 맞는 **고유한 ID**를 가지고 있습니다. 그래서 우리는 glGenBuffers 함수를 사용하여 버퍼 ID를 생성할 수 있습니다.

```C
unsigned int VBO; 
glGenBuffers(1, &VBO);
```
 
 OpenGL은 많은 유형의 버퍼 객체를 가지고 있으며 vertex buffer object의 버퍼 유형은 `GL_ARRAY_BUFFER`입니다. OpenGL은 버퍼 유형이 다른 여러가지 버퍼를 바인딩할 수 있습니다. 우리는 새롭게 생성된 버퍼를 `glBindBuffer` 함수를 사용하여 `GL_ARRAY_BUFFER`로 바인딩할 수 있습니다.
 
```C
glBindBuffer(GL_ARRAY_BUFFER, VBO);  
```

  그 시점부터 우리가 호출하는 모든 버퍼(`GL_ARRAY_BUFFER`를 타겟으로 하는)는 현재 바인딩 된 버퍼(VBO)를 사용하게 됩니다. 그런 다음 우리는 `glBufferData` 함수를 호출할 수 있습니다. 이 함수는 미리 정의된 정점 데이터를 버퍼의 메모리에 복사합니다.
  
```C
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
```

  `glBufferData` 함수는 사용자가 정의한 데이터를 현재 바인딩된 버퍼에 복사하는 기능을 수행합니다. 
  1. 첫 번째 파라미터는 우리가 데이터를 복사하여 집어넣을 버퍼의 유형(현재 바인딩된 vertex buffer object `GL_ARRAY_BUFFER`)을 받습니다.
  2. 두 번째 파라미터는 버퍼에 저장할 데이터의 크기(바이트 단위)를 받습니다. 간단히 `sizeof` 키워드를 써도 충분합니다.
  3. 세 번째 파라미터는 우리가 보낼 실제 데이터를 받습니다.
  4. 네 번째 파라미터는 그래픽 카드가 주어진 데이터를 관리하는 방법을 받습니다. 이 것은 3가지의 종류가 있습니다.
	  - GL_STATIC_DRAW: 데이터가 거의 변하지 않습니다.
	  - GL_DYNAMIC_DRAW: 데이터가 자주 변경됩니다.
	  - GL_STREAM_DRAW: 데이터가 그려질때마다 변경됩니다.

삼각형의 위치 데이터는 모든 렌더링 호출 때마다 변하지 않고 항상 같으므로 `GL_STATIC_DRAW`가 가장 알맞습니다. 예를 들어 자주 바뀔 수 있는 데이터가 들어있는 버퍼일 경우 `GL_DYNAMIC_DRAW`, `GL_STREAM_DRAW`로 설정하면 그래픽 카드가 빠르게 쓸 수 있는 메모리에 데이터를 저장합니다.

현재로서는 정점 데이터를 그래픽 카드의 메모리에 저장했습니다. 이 메모리는 VBO라고도 불리는 *vertex buffer object*가 관리하게 됩니다.