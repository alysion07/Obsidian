`<script>` 태그는 HTML 문서에서 JavaScript 코드를 포함하거나 외부 JavaScript 파일을 로드하는 데 사용됩니다. 다음은 `<script>` 태그의 주요 속성과 사용 방법에 대한 상세한 설명입니다.

### `<script>` 태그의 주요 속성

1. **`src`**:
   - 외부 JavaScript 파일의 URL을 지정합니다.
   - 예: `<script src="path/to/your/script.js"></script>`

2. **`type`**:
   - 스크립트의 MIME 타입을 지정합니다. 기본값은 `text/javascript`입니다.
   - ES6 모듈을 사용할 때는 `type="module"`을 지정합니다.
   - 예: `<script type="module" src="path/to/your/module.js"></script>`

3. **`async`**:
   - 스크립트를 비동기적으로 로드합니다. 스크립트가 로드되는 동안 HTML 파싱이 계속 진행됩니다.
   - 스크립트가 로드되면 즉시 실행됩니다.
   - 예: `<script src="path/to/your/script.js" async></script>`

4. **`defer`**:
   - 스크립트를 비동기적으로 로드하지만, HTML 파싱이 완료된 후에 스크립트를 실행합니다.
   - `defer` 속성은 `src` 속성과 함께 사용해야 합니다.
   - 예: `<script src="path/to/your/script.js" defer></script>`

5. **`integrity`**:
   - SRI(Subresource Integrity) 해시를 사용하여 로드된 스크립트의 무결성을 확인합니다.
   - 예: `<script src="path/to/your/script.js" integrity="sha384-..."></script>`

6. **`crossorigin`**:
   - CORS(Cross-Origin Resource Sharing) 설정을 지정합니다.
   - 예: `<script src="path/to/your/script.js" crossorigin="anonymous"></script>`

### `<script>` 태그의 사용 예제

#### 1. 내부 JavaScript 코드 포함
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Internal Script Example</title>
</head>
<body>
  <script>
    console.log('Hello, world!');
  </script>
</body>
</html>
```

#### 2. 외부 JavaScript 파일 로드
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>External Script Example</title>
  <script src="path/to/your/script.js"></script>
</head>
<body>
</body>
</html>
```

#### 3. 비동기적으로 스크립트 로드 (`async`)
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Async Script Example</title>
  <script src="path/to/your/script.js" async></script>
</head>
<body>
</body>
</html>
```

#### 4. HTML 파싱 후 스크립트 실행 (`defer`)
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Defer Script Example</title>
  <script src="path/to/your/script.js" defer></script>
</head>
<body>
</body>
</html>
```

#### 5. ES6 모듈 사용
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Module Script Example</title>
  <script type="module" src="path/to/your/module.js"></script>
</head>
<body>
</body>
</html>
```

#### 6. SRI(Subresource Integrity) 및 CORS 설정
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>SRI and CORS Example</title>
  <script
    src="https://cdnjs.cloudflare.com/ajax/libs/gl-matrix/2.8.1/gl-matrix-min.js"
    integrity="sha512-zhHQR0/H5SEBL3Wn6yYSaTTZej12z0hVZKOv3TwCUXT1z5qeqGcXJLLrbERYRScEDDpYIJhPC1fk31gqR783iQ=="
    crossorigin="anonymous"
    defer
  ></script>
</head>
<body>
</body>
</html>
```

### 요약
- `<script>` 태그는 HTML 문서에서 JavaScript 코드를 포함하거나 외부 JavaScript 파일을 로드하는 데 사용됩니다.
- 주요 속성으로는 `src`, `type`, `async`, `defer`, `integrity`, `crossorigin` 등이 있습니다.
- `<script>` 태그를 사용하여 JavaScript 코드를 포함하거나 외부 파일을 로드할 때, 각 속성의 특성과 사용 방법을 이해하면 더 효율적으로 스크립트를 관리할 수 있습니다.