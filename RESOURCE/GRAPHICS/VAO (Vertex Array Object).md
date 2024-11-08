---
ref: https://rvalueref.tistory.com/6
tags:
  - vertexAttribPointer
  - vao
  - GL
  - bind
  - Buffer
---

## Overview 

#GL(Graphics Library)의 API는 크게 세 종류로 나누어져 있음

- GPU에 **데이터를 전송**
- 라이브러리의 **설정을 변경**
- GPU에 **작업을 명령**

3가지 종류의 API를 사용하여 보통 다음과 같은 순서로 작업이 이루어집니다.

1. 화면에 표시할 물체의 **데이터(3D 모델, 이미지 등)를 GPU에 전송**한다
2. GPU에 담긴 여러 데이터 중 **어떤 데이터를 사용할지 설정**한다
3. 화면에 **그린다**.

여기에서 **1, 2번 작업과 3번 작업이 나누어져 있다**는 것이 핵심입니다. 코드로 그림을 그린다고 할 때, 보통의 경우 `draw(image)` 형태의 함수를 상상한다. 그러나, 이런 식으로 설계할 경우 먼저 CPU에서 데이터를 GPU에 **전송**하고, 다음으로 GPU가 그림을 화면에 표시합니다. `draw` 콜이 여러 번 반복되면 CPU와 GPU가 서로의 작업을 기다리게 되어 **병목 현상이 발생**합니다.

![[Pasted image 20240710120507.png]]

이러한 사태를 방지하기 위해 데이터를 전송하는 API와 실제로 그리는 작업을 명령하는 API를 **분리**했습니다. 데이터 전송은 필요한 경우에 비동기적으로 진행하고, 빠르게 돌아가야 하는 render loop가 계속해서 작업을 명령할 수 있도록 설계하는 것이 바람직합니다.

  
VAO를 사용하는 것과 그냥 매번 Vertex Buffer와 Layout(즉 glVertexAttribPointer를 매번 호출)을 지정해주는 것중에 뭐가 더 좋을까? 상황에 따라 다르지만, 대부분의 상황에서 VAO를 사용하는 게 편의성과 성능면에서 우월하다. 실제로 VAO를 사용하는 것이 OpenGL이 권장하는 방식이기도 하다.

### VAO 저장 항목 
- `glEnableVertexAttribArray` `glDisableVertexAttribArray` 함수의 호출
- `glVertexAttribPointer` 함수를 통한 Vertex 속성의 구성
- `glVertexAttribPointer` 함수를 통해 Vertex 속성과 연결된 Vertex Buffer Objects(VBOs)
![test caption]()

### Bind

```js
void gl.bindBuffer(GLenum target, WebGLBuffer buffer)
```

앞서 설명한 API의 종류 중 “어떤 데이터를 사용할지” 설정하는 함수들이 있다고 했는데, 어떤 데이터를 bind한다는 뜻은 GPU가 그 데이터를 사용하도록 설정한다는 뜻

`target`은 bind할 Buffer Object를 어떻게 사용할지 지정합니다. (예시: `gl.ARRAY_BUFFER`)


### BufferData 

```js
void gl.bufferData(GLenum target, ArrayBuffer data, GLenum usage)
```


`data`에 들어 있는 데이터를 현재 bind된 Buffer Object로 전송.
`target`은 앞서 설명한 `bindBuffer` 함수와 동일합니다. `usage`는 GPU에게 이 데이터를 어떻게 사용할 것이라는 힌트를 주는 것인데, 힌트일 뿐이라 꼭 그렇게 사용해야 하는 것은 아닙니다.  

*ex. “이 BO에 단 한 번만 데이터를 쓰고(STATIC), 데이터를 쓰기만 하고 읽지는 않을 것(DRAW)”라는 의미를 가진 `gl.STATIC_DRAW`로 설정합니다.*



---

## 동작 원리 


#### **VAO 만들기**

이렇게 데이터를 전달하는 방식으로 알 수 있듯이, VBO는 꼭짓점 데이터를 단순한 바이트 배열 또는 메모리 한 뭉태기로 볼 뿐, **내부 구조에 대해서는 알지 못합니다.** 따라서 VBO의 데이터를 OpenGL에서 **해석**하고 **사용**하기 위해서는 **VAO를 사용**해야 합니다.

![[Pasted image 20240709155757.png|center|400]]

VAO에는 **속성 (attribute)**(왼쪽)과 **VBO 바인딩 (VBO binding)**(오른쪽) 두 종류의 슬롯이 있습니다. '속성 (attribute)'은 꼭짓점의 속성을 의미하는 것으로, 0번 속성은 위치, 1번 속성은 색상.. 과 같은 식으로 할당할 수 있습니다.

각 VBO 바인딩 슬롯에는 VBO를 연결(바인딩)할 수 있습니다. 각 속성에는 최대 1개의 VBO 바인딩을 연결하고, 그 VBO 바인딩에 연결된 VBO를 어떻게 해석해야 하는지에 대한 **형식 (format)** 정보를 설정할 수 있습니다.


#### **VBO Binding**

VBO를 VAO의 슬롯에 **바인딩** 하기 위해 사용하는 함수 
- `glVertexArrayVertexBuffer(GLuint vaobj, GLuint bindingindex, GLuint buffer, GLintptr offset, GLsizei stride)`

- `vaobj` : VAO의 번호
- `bindingindex` : 몇 번째 VBO 바인딩 슬롯에 바인딩 할 것인지 
- `buffer`: 바인딩 할 VBO의 번호(주소 번호)
- `offset`: (시작 위치) VBO의 몇 번째 바이트부터 사용할 것인지
- `stride`: ("보폭") 는 꼭짓점 n의 데이터와 꼭짓점 n+1의 데이터 사이의 간격, 즉 VBO의 size 

![[Pasted image 20240709155210.png|center]]
vertex buffer object의 구조

우리는 `my_vao`의 0번째 바인딩 슬롯에 `my_vbo`를 바인딩할 것이고, `Vertex vertices[6]` 배열을 그대로 VBO에 복사했으니 `offset`은 **0**, `stride`는 ==Vertex==**구조체의 크기**

```cpp title:'Binding VBO to VAO' 
// VAO의 0번 VBO로 my_vbo를 바인딩할 것임
const int MY_VBO_BINDING = 0;

// my_vbo를 my_vao의 0번 VBO binding에 바인딩하기
glVertexArrayVertexBuffer(
	my_vao, MY_VBO_BINDING, my_vbo, 0, sizeof(Vertex));
```

위 코드를 실행한 후의 VAO 의 상태

![[Pasted image 20240709155742.png]]

0번 (`MY_VBO_BINDING`번) VBO 바인딩이 활성화 (**초록색**) 되고, 그곳에 `my_vbo`가 연결되었습니다.



#### **Attribute & VBO 연결**

0번 속성에는 위치 (`position`), \1번 속성에는 색상 (`color`)를 연결

속성과 VBO 바인딩을 연결하기 전, 우선 속성을 **활성화 (enable)** 해야 합니다. 활성화는 `glEnableVertexArrayAttrib()` 함수를 사용. 두 번째 인자에 몇 번째 속성을 활성화할 것 인지를 넘겨줍니다.

##### **Attributes Enable** 

```cpp
// 0번 attribute로 position을 설정할 것임 
const int POSITION_ATTRIBUTE_INDEX = 0; 
// my_vao의 0번 attribute 활성화
glEnableVertexArrayAttrib(my_vao, POSITION_ATTRIBUTE_INDEX);
```

이렇게 하면 아래와 같이 **0**(`POSITION_ATTRIBUTE_INDEX`) Attributes가 활성화 됨
![[Pasted image 20240709162729.png|center]]


##### **Attribute & VBO 연결** 

Attribute와 VBO 바인딩을 연결하려면 `glVertexArrayAttribBinding()` 함수를 사용.
두 번째 인자로 Attribute의 index, 세 번째 인자로 VBO Binding index를 넘겨준다.

```cpp
// my_vao의 0번 attribute로 my_vbo의 데이터를 바인딩 
glVertexArrayAttribBinding(my_vao, POSITION_ATTRIBUTE_INDEX, MY_VBO_BINDING);
```

코드 실행 결과
![[Pasted image 20240709162936.png|center]]

#### VBO Format 
 
 0번 속성에서 `my_vbo`를 사용하라는 명령은 내렸지만, **VAO에서 아직도 `my_vbo`의 Format을 모르고 있기 때문**입니다.

 ##### **glVertexArrayAttribFormat** 

 - `glVertexArrayAttribFormat(GLuint vaobj, GLuint attribindex, GLint size, GLenum type, GLboolean normalized, GLuint relativeoffset)` 
 - Attribute와 VBO Bindings를 연결

|    ==타입== | ==**이름**==     | ==**뜻**==                                                                                             |
| --------: | :------------- | ----------------------------------------------------------------------------------------------------- |
|    GLuint | vaobj          | VAO의 번호                                                                                               |
|    GLuint | attribindex    | 몇 번째 VAO 속성의 포맷을 설정할 것인지.                                                                             |
|     GLint | size           | 이 속성에 꼭짓점 하나에 값이 몇 개 있는지를 나타냄. 1, 2, 3, 4 중의 하나여야 함.  <br>예를 들어 2차원 위치 데이터 (x, y)라면 2, RGB 색상 데이터라면 3 |
|    GLenum | type           | 이 속성의 값의 타입 (GL_FLOAT, GL_DOUBLE 등)                                                                   |
| GLboolean | normalized     | GL_FALSE로 통일                                                                                          |
|    GLuint | relativeoffset | 각 꼭짓점 데이터의 시작점에서 몇 바이트를 들어가야 속성 값이 나오는지를 나타냄.                                                         |


---

```cpp title:'1번 Attribute에 color 설정'
// 1번 attribute로 color를 설정할 것임
const int COLOR_ATTRIBUTE_INDEX = 1;
// my_vao의 1번 attribute 활성화
glEnableVertexArrayAttrib(my_vao, COLOR_ATTRIBUTE_INDEX);
// my_vao의 1번 attribute로 my_vbo의 데이터를 바인딩
glVertexArrayAttribBinding(my_vao, COLOR_ATTRIBUTE_INDEX, MY_VBO_BINDING);
// my_vao의 1번 attribute에 바인딩된 vbo (my_vbo)를 어떻게 해석할지(format)를 설정
glVertexArrayAttribFormat(my_vao,                     // VAO 번호
                            COLOR_ATTRIBUTE_INDEX,    // VAO attribute 번호
                            3,                        // 벡터의 원소 개수
                            GL_FLOAT,                 // 원소의 타입
                            GL_FALSE,                 // normalized
                            offsetof(Vertex, color)); // Vertex 내의 color 데이터의 위치
```

#### VAO 설정 완료
![[Pasted image 20240709172744.png|center]]

```cpp title:'전체 코드'
#include <iostream>

#define GLFW_INCLUDE_NONE
#include <GLFW/glfw3.h>
#include <glad/glad.h>

// 2차원 (위치)벡터를 담을 수 있는 구조체
struct Vec2d {
    float x;
    float y;
};

// 색상 정보를 담을 수 있는 구조체
struct Color {
    float r;
    float g;
    float b;
};

// 꼭짓점 정보를 담을 수 있는 구조체
struct Vertex {
    Vec2d position;
    Color color;
};

int main() {

    // GLFW 초기화
    if (!glfwInit()) {
        std::cerr << "GLFW Initialization Failed!\n";
        return -1;
    }

    // OpenGL 4.5 Core Profile 버전 설정
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 4);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 5);
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);

    // 창 띄우기
    GLFWwindow* window = glfwCreateWindow(640, 480, "My Title", NULL, NULL);
    if (!window) {
        std::cerr << "Create Window Failed!\n";
        glfwTerminate();
        return -1;
    }

    // 창에 OpenGL Context 설정하기
    glfwMakeContextCurrent(window);
    gladLoadGL();

    // V-Sync 설정하기 (Screen Tearing 방지)
    glfwSwapInterval(1);    

    // 삼각형 데이터
    const Color pink_color = {1.0f, 0.765f, 0.765f};
    const Color green_color = {0.545f, 1.0f, 0.345f};

    const Vertex vertices[6] = {
        // 핑크색 삼각형
        Vertex{Vec2d{2.0f, 8.0f}, pink_color},
        Vertex{Vec2d{1.0f, 5.0f}, pink_color},
        Vertex{Vec2d{5.0f, 6.0f}, pink_color},

        // 연두색 삼각형
        Vertex{Vec2d{4.0f, 4.0f}, green_color},
        Vertex{Vec2d{6.0f, 1.0f}, green_color},
        Vertex{Vec2d{8.0f, 3.0f}, green_color},
    };

    // VBO 생성
    GLuint my_vbo;
    glCreateBuffers(1, &my_vbo);

    // VBO에 데이터 전달
    glNamedBufferData(my_vbo, sizeof(vertices), vertices, GL_STATIC_DRAW);

    // VAO 생성
    GLuint my_vao;
    glCreateVertexArrays(1, &my_vao);

    // VAO의 0번 VBO로 my_vbo를 바인딩할 것임
    const int MY_VBO_BINDING = 0;

    // my_vbo를 my_vao의 0번 VBO binding에 바인딩하기
    glVertexArrayVertexBuffer(my_vao, MY_VBO_BINDING, my_vbo, 0, sizeof(Vertex));

    // 0번 attribute로 position을 설정할 것임
    const int POSITION_ATTRIBUTE_INDEX = 0;

    // my_vao의 0번 attribute 활성화
    glEnableVertexArrayAttrib(my_vao, POSITION_ATTRIBUTE_INDEX);

    // my_vao의 0번 attribute로 my_vbo의 데이터를 바인딩
    glVertexArrayAttribBinding(my_vao, POSITION_ATTRIBUTE_INDEX, MY_VBO_BINDING);

    // my_vao의 0번 attribute에 바인딩된 vbo (my_vbo)를 어떻게 해석할지(format)를 설정
    glVertexArrayAttribFormat(my_vao,                       // VAO 번호
                              POSITION_ATTRIBUTE_INDEX,     // VAO attribute 번호
                              2,                            // 벡터의 원소 개수
                              GL_FLOAT,                     // 원소의 타입
                              GL_FALSE,                     // normalized
                              offsetof(Vertex, position));  // Vertex 구조체 내의 position 데이터의 위치

    // 1번 attribute로 color를 설정할 것임
    const int COLOR_ATTRIBUTE_INDEX = 1;

    // my_vao의 1번 attribute 활성화
    glEnableVertexArrayAttrib(my_vao, COLOR_ATTRIBUTE_INDEX);

    // my_vao의 1번 attribute로 my_vbo의 데이터를 바인딩
    glVertexArrayAttribBinding(my_vao, COLOR_ATTRIBUTE_INDEX, MY_VBO_BINDING);

    // my_vao의 1번 attribute에 바인딩된 vbo (my_vbo)를 어떻게 해석할지(format)를 설정
    glVertexArrayAttribFormat(my_vao,                       // VAO 번호
                              COLOR_ATTRIBUTE_INDEX,        // VAO attribute 번호
                              3,                            // 벡터의 원소 개수
                              GL_FLOAT,                     // 원소의 타입
                              GL_FALSE,                     // normalized
                              offsetof(Vertex, color));     // Vertex 구조체 내의 color 데이터의 위치

    // 렌더 루프
    while (!glfwWindowShouldClose(window)) {
        // 화면 노란색으로 채우기
        GLfloat color[4] = {1.0, 1.0, 0.0, 1.0};
        glClearNamedFramebufferfv(0, GL_COLOR, 0, &color[0]);

        // Front Buffer와 Back Buffer Swap하기
        glfwSwapBuffers(window);

        // 사용자 입력 받아오기
        glfwPollEvents();
    }

    // VAO와 VBO 리소스 해제
    glDeleteVertexArrays(1, &my_vao);
    glDeleteBuffers(1, &my_vbo);

    // 창 리소스 해제
    glfwDestroyWindow(window);

    // GLFW 리소스 해제
    glfwTerminate();
    return 0;
}
```



---


## 사용 목적

VAO의 사용 목적은 Vertex buffer가 여러 개 있을 때 각 버퍼마다 Layout(Attribute를 지정해서 버퍼 내에 어떤 정보가 어디에 얼만큼 위치하는지 지정해두는 것)을 개별적으로 지정해둘 필요가 없게 하는 것이다.

즉 Binding된 Vertex Buffer가 변경될 때마다 매번 그 버퍼가 가진 Attribute들에 대해 glVertexAttribPointer()를 호출할 필요가 없다는 의미다.

```cpp
glBindBuffer(buffer1);
glVertexAttribPointer(...);
...

glBindBuffer(buffer2);
glEnableVertexAttribArray(...);
```

이런식으로 할 필요가 없어진다는 의미다.

참고: [glVertexAttribPointer needed everytime glBindBuffer is called?](https://stackoverflow.com/questions/7617668/glvertexattribpointer-needed-everytime-glbindbuffer-is-called)
참고:  [https://stackoverflow.com/a/57258564/14134221](https://stackoverflow.com/a/57258564/14134221)
참고:  [What are Vertex Array Objects?](https://stackoverflow.com/questions/11821336/what-are-vertex-array-objects)

  
쉽게 말해, 점들의 좌표값 4개를 저장하고 있는, 사각형을 나타내는 어떤 버퍼가 있다고 가정해보자.  
이 사각형 버퍼가 두 종류가 있다면, 즉 사각형 A와 사각형 B가 있다면,  
우리는 사각형A와 사각형B에 개별적으로 레이아웃을 지정해주어야 할 것이다. (즉 A 버퍼는 꼭짓점 4개가 float으로 들어있고, B 버퍼에도 꼭짓점 4개가 float으로 들어있다는 걸 개별적으로 두번 지정해주어야 한다)  
그러나 VAO를 사용하면 이 과정을 줄일 수 있다.


---


## 전체 코드 

```cpp
 /* 
 VAO를 생성하고 바인딩한다
위에서 OPENGL_CORE_PROFILE을 사용중이기 때문이다. 만약 여기서 VAO를 바인딩해주지 않는다면 그동안 사용해왔던 코드인 glEnableVertexAttribArray(0);를 호출할 때 에러가 발생한다.
glEnableVertexAttribArray(0);는 기본적으로 opengl이 만들어준 0번 VAO에 AttribArray를 연결해준다는 의미인데, CORE_PROFILE에서는 0번 VAO를 만들어주지 않기 때문이다.
*/
unsigned int vao;
// VAO 1개를 uint vao id로 만들어준다.
GLCall( glGenVertexArrays(1, &vao) );
// 이렇게 만든 VAO를 bind해준다.
GLCall( glBindVertexArray(vao) ); 

// Vertex buffer (혹은 VBO)를 생성한다
unsigned int buffer;
GLCall( glGenBuffers(1, &buffer) );
GLCall( glBindBuffer(GL_ARRAY_BUFFER, buffer) );
GLCall( glBufferData(GL_ARRAY_BUFFER, 4 * 2 * sizeof(float), positions, GL_STATIC_DRAW) ); // define vertex layout (레이아웃, 즉 vertex buffer의 Attrib 들을 설정한다)

/*
이 아랫줄 코드가 굉장히 중요하다.
이 코드가 Vertex Array와 Vertex Buffer을 연결해주는 역할을 한다.
glVertexAttribPointer의 첫 아규먼트인 0은 현재 바인딩된 Vertex Array의 0번 인덱스에 해당 Attrib Pointer가 가리키는 Vertex Buffer을 연결한다는 의미이다. 즉 만약 또 다른 Attrib Pointer을 연결해주려면 GLCall( glVertexAttribPointer(1, ... , Attrib pointer); 형식으로 작성하면 된다. 
*/
        GLCall( glVertexAttribPointer(0, 2, GL_FLOAT, GL_FALSE, 2 * sizeof(float), 0) );// This links the attrib pointer wih the buffer at index 0 in the vertex array object      
        GLCall( glEnableVertexAttribArray(0) ); //https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=jungwan82&logNo=20108365931
```

 참고: [Vertex Array Pointer](  https://www.khronos.org/registry/OpenGL-Refpages/gl4/html/glVertexAttribPointer.xhtml)
 참고: [Concept behind OpenGL's 'Bind' functions - Stack Overflow](https://stackoverflow.com/questions/9758884/concept-behind-opengls-bind-functions)


간단히 말해, OpenGL에게 지금 선택한 오브젝트가 무엇이다 라고 전해주는 것이다.  
포토샵 같은걸 쓸 때 마우스로 그림을 그리면 현재 선택된 레이어에 그려지는 것처럼, 미리 무언가를 선택한 이후(OpenGL에서는 버퍼나 셰이더 등 다양한 게 될 수 있다) 행위를 해야 그 선택한 대상에 행위가 처리된다. (선택한 레이어에 그림이 그려진다)

