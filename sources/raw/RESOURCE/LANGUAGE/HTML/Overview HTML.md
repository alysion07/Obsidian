


[HTML](https://developer.mozilla.org/ko/docs/Glossary/HTML) (Hypertext Markup Language, 하이퍼 텍스트 마크업 언어)는 프로그래밍 언어는 아니고, 우리가 보는 웹페이지가 어떻게 구조화되어 있는지 브라우저로 하여금 알 수 있도록 하는 마크업 언어입니다. 이는 개발자로 하여금 복잡하게도 간단하게도 프로그래밍 할 수 있습니다. 

HTML은 [elements](https://developer.mozilla.org/ko/docs/Glossary/Element)로 구성되어 있으며, 이들은 적절한 방법으로 나타내고 실행하기 위해 각 컨텐츠의 여러 부분들을 감싸고 마크업 합니다. [tags](https://developer.mozilla.org/ko/docs/Glossary/Tag) 는 웹 상의 다른 페이지로 이동하게 하는 하이퍼링크 내용들을 생성하거나, 단어를 강조하는 등의 역할을 합니다.

```
My cat is very grumpy
```


```html title:'HTML 언어로 표현'
<p>My cat is very grumpy</p>
```

> [!NOTE]
> **참고:** HTML 요소는 대소문자를 구분하지 않습니다. 그 말은 [`<title>`](https://developer.mozilla.org/ko/docs/Web/HTML/Element/title) 요소는 `<title>`, `<TITLE>`, `<Title>`, `<TiTlE>` 들과 같이 사용할 수 있습니다. 하지만 대게는 가독성과 기타 이유때문에 소문자로 작성합니다.

## HTML 요소(Element)의 구조

1. **여는 태그(Opening tag):** 이것은 요소의 이름과(이 경우 p), 열고 닫는 꺽쇠 괄호로 구성됩니다. 요소가 시작(이 경우 단락의 시작 부분)부터 효과가 적용되기 시작합니다.
2. **닫는 태그(Closing tag):** 이것은 요소의 이름 앞에 슬래시(/)가 있는것을 제외하면 여는 태그(opening tag)와 같습니다. 이것은 요소의 끝(이 경우 단락의 끝 부분)에 위치합니다. 닫는 태그를 적어주지 않는 것은 흔한 초심자의 오류이며, 이것은 이상한 결과를 낳게됩니다.
3. **내용(Content):** 요소의 내용이며, 이 경우 단순한 텍스트이다.
4. **요소(Element):** 여는 태그, 닫는 태그, 내용을 통틀어 요소(element)라고한다.

![[Pasted image 20240830090840.png]]