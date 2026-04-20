#script #html

`<script>` 태그를 이용하면 자바스크립트 프로그램을 HTML 문서 대부분의 위치에 삽입할 수 있다.   `<script>` 태그엔 자바스크립트 코드 작성되며, 브라우저는 이 태그를 만나면 안의 코드를 자동으로 처리한다.

### 외부 스크립트

코드의 양이 많은 경우 파일로 소분하여 저장할 수 있다. 이렇게 분해해놓은 파일은 `src` 속성을 사용해 HTML에 삽입한다.

```html
<script src="/path/to/script.js"></script>
```

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.11/lodash.js"></script>
```

> [!NOTE]
 >
> HTML 안에 직접 스크립트를 작성하는 방식은 대개 스크립트가 아주 간단할 때만 사용한다. 스크립트가 길어지면 별개의 분리된 파일로 만들어 저장하는 것이 좋음.
> 
> 스크립트를 별도의 파일에 작성하면 브라우저가 스크립트를 다운받아 [캐시(cache)](https://en.wikipedia.org/wiki/Web_cache)에 저장하기 때문에, 성능상의 이점이 있다.
> 
> 여러 페이지에서 동일한 스크립트를 사용하는 경우, 브라우저는 페이지가 바뀔 때마다 스크립트를 새로 다운 받지 않고 캐시로부터 스크립트를 가져와 사용한다. 스크립트 파일을 한 번만 다운 받으면 된다
> 
> 이를 통해 트래픽이 절약되고 웹 페이지의 실제 속도가 향상될 수 있다.

>[!WARNING]
>`src`속성이 있으면 태그 내부의 코드는 무시된다. 
> `<script>` 태그는 `src` 속성과 내부 코드를 동시에 가지지 못한다. 
> 다음 코드는 실행되지 않는다.
> 
 
```html
<script src="file.js">  
alert(1); // src 속성이 사용되었으므로 이 코드는 무시됩니다. 
</script> 
```


> 따라서 `<script src="…">`로 외부 파일을 연결할지 아니면 `<script>` 태그 내에 코드를 작성 할지를 선택해야 한다.
> 
> 위의 예시는 스크립트 두 개로 분리하면 정상적으로 실행됩니다.

```html 
<script src="file.js"></script>
<script>
  alert(1);
</script>
```
