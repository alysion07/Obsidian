---
title: Algorithm (알고리즘)
type: concept
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-RESOURCE.md
tags:
  - 알고리즘
  - 자료구조
  - CS
---

# Algorithm (알고리즘)

## Big O Notation (빅오 표기법)

알고리즘의 성능을 나타내는 수학적 표기법으로, 입력 크기 `n`에 따라 실행 시간이나 공간 사용량이 어떻게 증가하는지를 설명한다. 컴퓨터/언어/환경에 관계없이 알고리즘 자체의 효율성을 평가할 수 있다.

### 주요 Big O 종류

| Big O 표기 | 이름 | 설명 |
|---|:---:|:---|
| `O(1)` | 상수 시간 | 입력 크기와 무관하게 항상 같은 시간 |
| `O(log n)` | 로그 시간 | 이진 탐색처럼 반씩 줄어드는 경우 |
| `O(n)` | 선형 시간 | 입력 크기만큼 반복하는 경우 |
| `O(n log n)` | 로그-선형 시간 | 병합 정렬, 퀵 정렬 평균 시간 등 |
| `O(n^2)` | 이차 시간 | 중첩 반복문 (예: 버블 정렬) |
| `O(2^n)` | 지수 시간 | 모든 경우 탐색 (예: 완전 탐색) |
| `O(n!)` | 팩토리얼 시간 | 순열 등 모든 경우의 조합 탐색 |

### 핵심 개념

- 일반적으로 **Worst case**(최악의 경우)를 기준으로 분석
- Big O는 상수, 저차항을 무시 (예: `O(3n + 10)` = `O(n)`)
- 반복문이 중첩이면 곱하고, 분리되면 더한다

### 자료구조별 접근 비용

| 자료구조 | 접근 시간 |
|---|---|
| `std::vector` | O(1) (인덱스로 접근) |
| `std::map` | O(log n) (레드블랙 트리 기반) |
| `std::unordered_map` | O(1) 평균, O(n) 최악 |
| `std::set` | O(log n) |
| `std::priority_queue` | 삽입/삭제 O(log n) |

---

## DFS (깊이 우선 탐색, Depth First Search)

그래프 순회 방식의 일종으로, Tree나 Graph에서 한 루트로 탐색하다가 더 이상 나아갈 곳이 없는 경우 다시 돌아가 다른 루트를 탐색하는 방식이다. 백트래킹에 대표적으로 사용된다.

### 장점
- 현 경로상의 노드들만 기억하면 되므로 저장 공간의 수요가 비교적 적다
- 목표 노드가 깊은 단계에 있는 경우 해를 빠르게 구할 수 있다

### 단점
- 해가 없는 경로에 깊이 빠질 가능성이 있다
- 얻어진 해가 최단 경로가 된다는 보장이 없다

### 구현 방식
- **재귀(Recursion)**: 함수 호출 스택을 활용한 자연스러운 구현
- **스택(Stack)**: 명시적 스택 자료구조를 사용한 반복문 구현

```cpp
// DFS - 재귀 방식
void dfs(const vector<int> graph[], int current) {
    visited[current] = true;
    for(int next: graph[current]) {
        if(!visited[next]) {
            dfs(graph, next);
        }
    }
}
```

---

## BFS (너비 우선 탐색, Breadth First Search)

트리나 그래프를 방문 또는 탐색하는 방법으로, 루트에서 시작하여 인접한 노드를 먼저 탐색하고, 그 다음 레벨의 노드들을 순차적으로 탐색한다.

### 특징
- DFS와 달리, 무한한 길이의 경로가 존재해도 다른 경로의 탐색 목표를 찾을 수 있다
- 가중치가 없는(또는 같은) 그래프에서 **최단 경로**를 보장한다
- 자료구조 **Queue**를 사용하여 구현하는 것이 일반적

```cpp
// BFS - Queue 방식
void bfs(const vector<int> graph[], int source) {
    q.push(source);
    visited[source] = true;
    while(!q.empty()) {
        int current = q.front();
        q.pop();
        for(int next: graph[current]) {
            if(!visited[next]) {
                q.push(next);
                visited[next] = true;
            }
        }
    }
}
```

---

## 관련 문서

- [[Computer-Engineering]] - 컴퓨터 공학 개념 모음
