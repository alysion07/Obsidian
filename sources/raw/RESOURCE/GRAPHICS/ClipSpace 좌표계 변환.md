
## Pixel -> ClipSpace 

```cpp title:'Vertex Shader' hl:25,28,31, 33
// an attribute is an input (in) to a vertex shader.
// It will receive data from a buffer
in vec2 a_position;

// Used to pass in the resolution of the canvas
uniform vec2 u_resolution;

// translation to add to position
uniform vec2 u_translation;

// rotation values
uniform vec2 u_rotation;

void main() {
    //Rotate the position
    vec2 rotatedPosition = vec2(
        a_position.x * u_rotation.y + a_position.y * u_rotation.x,
        a_position.y * u_rotation.y - a_position.x * u_rotation.x
    );

  // Add in the translation
  vec2 position = rotatedPosition + u_translation;

  // convert the position from pixels to 0.0 to 1.0
  vec2 zeroToOne = position / u_resolution;

  // convert from 0->1 to 0->2
  vec2 zeroToTwo = zeroToOne * 2.0;

  // convert from 0->2 to -1->+1 (clipspace)
  vec2 clipSpace = zeroToTwo - 1.0;

  gl_Position = vec4(clipSpace * vec2(1, -1), 0, 1);
}
```

이 버텍스 셰이더에서 zeroToOne, zeroToTwo, clipSpace를 단계별로 계산하는 이유는 픽셀 좌표계에서 WebGL의 클립 공간 좌표계로 변환하는 과정을 명확하게 보여주기 위함입니다. 각 단계의 의미와 필요성을 설명해 드리겠습니다:

1. zeroToOne = position / u_resolution;
   
   목적: 픽셀 좌표를 0.0에서 1.0 사이의 값으로 정규화합니다.

   이유:
   - 원래 좌표는 픽셀 단위입니다 (예: 800x600 해상도에서 (400, 300)).
   - 이를 해상도로 나누면 상대적인 위치를 얻게 됩니다 (0.0에서 1.0 사이).
   - 이는 다양한 화면 크기에 대해 일관된 결과를 제공합니다.

2. zeroToTwo = zeroToOne * 2.0;
   
   목적: 0.0에서 1.0 사이의 값을 0.0에서 2.0 사이의 값으로 확장합니다.

   이유:
   - 다음 단계에서 -1.0에서 1.0 사이의 클립 공간으로 변환하기 위한 중간 단계입니다.
   - 0에서 2로 확장함으로써, 다음 단계에서 간단히 1을 빼서 원하는 범위를 얻을 수 있습니다.

3. clipSpace = zeroToTwo - 1.0;
   
   목적: 0.0에서 2.0 사이의 값을 -1.0에서 1.0 사이의 클립 공간 좌표로 변환합니다.

   이유:
   - WebGL의 클립 공간은 x, y, z 각 축에 대해 -1.0에서 1.0 사이입니다.
   - 이 범위를 벗어나는 모든 것은 화면에 렌더링되지 않습니다.
   - 2에서 1을 빼면 정확히 -1에서 1 사이의 범위를 얻을 수 있습니다.

4. gl_Position = vec4(clipSpace * vec2(1, -1), 0, 1);
   
   목적: 최종적으로 클립 공간 좌표를 설정하고, y축을 뒤집습니다.

   이유:
   - vec2(1, -1)을 곱하는 것은 y축을 뒤집는 역할을 합니다.
   - 이는 WebGL의 좌표계(위가 +y)와 일반적인 2D 그래픽스 좌표계(아래가 +y)의 차이를 조정합니다.
   - z 좌표는 0(2D이므로), w는 1(원근 나눗셈을 위해)로 설정합니다.

이렇게 단계를 나누는 것은 코드의 가독성과 유지보수성을 높이며, 각 변환 단계를 명확하게 이해하고 필요시 수정할 수 있게 해줍니다. 실제 성능 측면에서는 이 모든 계산이 하나의 수식으로 합쳐질 수 있지만, 교육적 목적이나 디버깅을 위해 이렇게 분리하여 작성하는 것이 유용할 수 있습니다.