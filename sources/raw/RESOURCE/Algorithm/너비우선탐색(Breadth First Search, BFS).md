  
**너비 우선 탐색**은 트리나 그래프를 방문 또는 탐색하는 방법이다. 탐색 방법은 다음과 같다.

1. 루트에서 시작한다.
    
2. 자식 노드들을 [1]에 저장한다
    
3. [1]에 저장된 노드들을 차례로 방문한다. 또한 각각의 자식들을 [2]에 저장한다.
    
4. [2]에 저장된 노드들을 차례로 방문한다. 또한 각각의 자식들을 [3]에 저장한다.
    
5. 위의 과정을 반복한다.
    
6. 모든 노드를 방문하면 탐색을 마친다.

![[7W7lG80nMCAyXxq_AReWiFGOU1wddJWoUyZxHqyR5u17jWcZRjWXHjAPErzJQuLd7bgFnQZPWWllNyOkOQiY9w.gif]]


## 특징

[[깊이우선탐색 (DFS, Depth First Search)|DFS]]와 가장 큰 차이로, 여러 갈래 중 무한한 길이를 가지는 경로가 존재하고, 탐색 목표가 다른 경로에 존재하는 경우, BFS의 경우는 모든 경로를 동시에 진행하기 때문에 탐색이 가능하다. DFS의 경우는 무한한 길이를 가진 경로에서 탈출할 수 없음

또한 BFS에서는 연결되는모든 길을 한번씩 탐색하기 때문에 가중치가 없는(혹은 같은) 그래프에서는 시작점에서 끝점까지의 최단 경로를 알아 낼 수 있다.

## 구현 

BFS는 재귀 호출(Recursion Call)을 이용하여 소스 코드로 구현하는 DFS와는 달리, 자료 구조 [Queue](https://namu.wiki/w/%ED%81%90\(%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0\) "큐(자료구조)")를 사용하는 경우가 일반적이다. 배열에서 사용하는 경우, 방향 데이터를 이용해 배열의 시작점에서 범위를 넓혀 가면서 탐색하는 것이다.  


```cpp title:'BFS Example'
const int MAX = 100'001;

queue<int> q;
bool visited[MAX]; // 방문 배열. visited[node] = true이면 node는 방문이 끝난 상태이다.

void bfs(const vector<int> graph[], int source) { // graph는 인접 리스트, source는 시작 노드
    // source 방문
    q.push(source);
    visited[source] = true;

    while(!q.empty()) {
        // 큐에서 노드를 하나 빼 온다. 이 노드를 current라고 하자.
        int current = q.front();
        q.pop();

        for(int next: graph[current]) { // current의 인접 노드들을 확인한다. 이 각각의 노드를 next라고 하자.
            if(!visited[next]) { // 만일 next에 방문하지 않았다면
                // next 방문
                q.push(next);
                visited[next] = true;
            }
        }
    }
}
```