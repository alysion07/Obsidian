---
Reference URL: https://medium.com/stayfolio-tech/react%EA%B0%80-0-016%EC%B4%88%EB%A7%88%EB%8B%A4-%ED%95%98%EB%8A%94-%EC%9D%BC-feat-fiber-1b9c3839675a
---
#reconciliation #fiber

## Reconciliation
Reconciliation(재조정)이란, React 에서 매우 중요한개념 중 하나로 UI에서 변경 사항이 발생하면 **DOM과 Virtual DOM을 비교하여 변경사항을 식별하고 업데이트하는 과정**을 의미한다.


재조정과 렌더링은 각각 `reconciler`모듈과 `renderer`모듈로 분리되어 실행

---

## ## Fiber Reconciler

`증분 렌더링(Incremental Rendering)`를 구현하기 위해서는 작업을 일시정지하고 나중에 다시 시작할 수 있어야 했다. 또 이전에 완료된 작업을 재사용하거나 필요하지 않은 경우 중단 할 수 있어야 했다. 그래서 도입된게 `Fiber`이다.

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="eYMXJPN" data-pen-title="fiber reconciliation animation" data-user="ejilee" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/ejilee/pen/eYMXJPN">
  fiber reconciliation animation</a> by Yeji Lee (Bailey) (<a href="https://codepen.io/ejilee">@ejilee</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://public.codepenassets.com/embed/index.js"></script>



![[image.gif]]



#### 렌더 단계(Render Phase)

Fiber는`current` 와 `workInProgress` 두가지 트리를 가지고 있다.

`current` 트리는 현재 화면에 있는 것으로 React가 변경 하면 안정성을 보장 할 수 없기 때문에 복사본인 `workInProgress` 트리를 만들어서 작업하게 된다.

Render Phase는 비동기적으로 동작하며 두 fiber 트리를 비교하고 변경된 Effect들을 수집하는 작업을 한다. 리액트 scheduler로 인해 허용되는 시간 동안 작업하고 user input, animation 같은 더 급한 작업이 있다면 해당 작업에게 메인 스레드를 양보한다.

> Effect는 Dom을 변경하거나 특정 생명 주기 메서드를 호출하는 것 같은 활동이다. 이러한 활동은 부작용(Side Effect)라고 부르며 다른 구성요소에 영향을 미칠 수 있으며 렌더링 중에는 실행할 수 없다.  
> ( 예외는 존재한다. 렌더링 메소드 자체나 shouldComponentUpadte 메소드는 렌더링 단계 중에 호출된다. )

위 과정에서 React가 언제라도 `workInProgress` 트리 내부 변경사항을 버릴 수 있기 때문에 Dom 또는 componentDidMount 같은 생명 주기 메서드에 대한 변경사항은 렌더링 단계중에서 실행 할 수 없다.

#### 커밋 단계(Commit Phase)

커밋 단계에서는 렌더 단계에서 수집한 Effect와 변경된 정보를 가지고 있는 Fiber를 통해 Effect를 실행하고 Dom에 적용하는 단계를 거친다. 이 단계는 동기적으로 한번에 이루어지기 때문에 일시정지하거나 취소할 수 없다.

Commit 후에는 `workInProgress` 트리가 현재(current)의 트리가 된다.


![[image 2.gif]]