

FrameBuffer는 화면에 그려질 화면 전체에 대한 정보를 담는 공간, 메모리입니다. 해상도라고 아시죠? 1024*768 또는 800*480 등등... 그 해상도에 그려질 화면에 대한 정보를 담습니다. 



1초에 몇 번 바뀌는지의 수를 **주사율(Refresh Rate)** 이라고 부르고, 단위는 **Hz (헤르츠)** 를 씁니다. 그래픽카드는 #FrameBuffer 라는 메모리 영역을 읽어 화면을 띄웁니다.


### Basic
```js title:'framebuffer example'

```

Framebuffer를 bind 한 후에는 `gl.viewport`를 호출해주어야 한다.

---

## 더블 버퍼링 (Double Buffering)

#DoubleBuffering 은 지금 화면에 표시되어야 하는, 혹은 표시되고 있는 프런트 버퍼(Front Buffer)와 현재 그리고 있는 **미완성된 백 버퍼**(Back Buffer) 두 개의 FrameBuffer를 사용하는 방식. Back Buffer에 프레임을 다 그리면, Front Buffer와 빠르게 Swap(바꿔치기)함.

`glfwSwapBuffers()` 함수 사용

```cpp title:'glfwSwapBuffers'
while (!glfwWindowShouldClose(window)) { 
// TODO: 화면에 뭔가 그리기 

// Front Buffer와 Back Buffer Swap하기 
glfwSwapBuffers(window); 

glfwPollEvents(); }
```

---

## 스크린 테어링 (Screen Tearing)

디스플레이는 위에서 아래로 스캔하듯이 화면이 업데이트 됨. 
Front Buffer와 BackBuffer를 Swap하는 도중 디스플레이에 전송되어 서로 다른 프레임이 표시되는 현상

![[Pasted image 20240710134655.jpg]]

이 현상의 해결 방법은 디스플레이 업데이트 직후, 다음 업데이트 전 사이에 빠르게 버퍼를 Swap하는 것. Render Loop가 시작하기 전에 **glfwSwapInterval** 호출하여 사용 가능.

### **glfwSwapInterval**(int **interval**)
 
`glfwSwapBuffers()`가 호출이 되었을 때 디스플레이 업데이트를 `interval`번 기다렸다가 Swap함. 보통 **1**보다 큰 값은 사용하지 않음

---

## 수직 동기화

이 기능은 다른 말로 **V-Sync** (Vertical Sync, 수직 동기화)라고도 합니다. V-Sync를 사용하게 되면 **FPS**가 디스플레이의 주사율로 고정되게 됩니다.



#V-Sync #수직 #동기화 #스크린 #테어링 #더블 #버퍼링
#Double #Buffering