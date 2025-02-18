#DFS #depth #search

## Overview

그래프 순회 방식의 일종. 
[[너비우선탐색(Breadth First Search, BFS)]]과 비교되어 나온다.

---
## Detail

![[Pasted image 20250206104029.png]]

Tree나 Graph에서 한 루트로 탐색하다가 더 이상 나아갈 곳이 없는 경우 다시 돌아가 다른 루트를 탐색하는 방식. 대표적으로 [^1]백트래킹에  사용함.

---
## 장점

- 현 경로상의 노드들만 기억하면 되므로 저장 공간의 수요가 비교적 적다.
- 목표 노드가 깊은 단계에 있는 경우 해를 빠르게 구할 수 있다.
---
## 단점

- 해가 없는 경로에 깊이 빠질 가능성이 있다. 
- 얻어진 해가 최단 경로가 된다는 보장이 없다.
	- **목표에 이르는 경로가 다수**인 문제에 대해 깊이 우선 탐색은 해에 다다르면 탐색을 끝내버리므로, 이때 얻어진 해는 **최적이 아닐 수 있음**을 의미한다.
---

## 예제 

```cpp title:"DFS example 1"
const int MAX = 100'001;

bool visited[MAX]; // 방문 배열. visited[node] = true이면 node는 방문이 끝난 상태이다.

void dfs(const vector<int> graph[], int current) { // graph는 인접 리스트, current는 현재 노드
    visited[current] = true; // current 방문

    for(int next: graph[current]) { // current의 인접 노드를 확인한다. 이 노드를 next라고 하자.
        if(!visited[next]) { // 만일 next에 방문하지 않았다면
            dfs(graph, next); // next 방문
        }
    }
}
```


```cpp title:"use Recursive"

function dfs(node, visited = new Set()) {
  if (!node || visited.has(node)) return;
  
  console.log(node.value); // 현재 노드 방문
  visited.add(node);

  for (let child of node.children) {
    dfs(child, visited); // 재귀 호출
  }
}
```

- 노드를 방문할 때마다 `visited` 집합(Set)에 추가하여 중복 방문을 방지함.
- 재귀를 사용해 자식 노드를 탐색하는 구조.
---

```cpp title:' use Stack'
function dfsIterative(root) {
  let stack = [root];  // 스택을 사용하여 DFS 구현
  let visited = new Set();

  while (stack.length > 0) {
    let node = stack.pop(); // 스택에서 노드 가져오기
    if (!node || visited.has(node)) continue;

    console.log(node.value); // 방문한 노드 출력
    visited.add(node);

    // 자식 노드를 스택에 추가 (반대로 추가하면 DFS 순서 유지)
    for (let i = node.children.length - 1; i >= 0; i--) {
      stack.push(node.children[i]);
    }
  }
}
```

- 스택을 사용하여 DFS를 반복문으로 구현.
- `pop()`을 사용하여 스택의 마지막 요소(가장 깊이 있는 노드)부터 탐색.

---

[^1]: 백트래킹은 **현재 상태에서 다음상태로 가는 모든 경우의 수를 찾아서 이 모든 경우의수가 더 이상 유망하지 않다고 판단되면 이전의 상태로 돌아가는 것을 말한다**. 여기서 더 이상 탐색할 필요가 없는 상태를 제외하는 것을 가지치기(pruning)라고도 한다.
