---
layout: single
title: "[Java] 비선형 자료구조"
categories: 
  - CS
---

# Graph (그래프)

그래프는 **정점(Vertex)**과 **간선(Edge)**으로 이루어진 자료구조로,  
객체 간의 **다대다 관계**나 **복잡한 연결성**을 표현할 수 있다.

$$ G = (V, E) $$  
- **V**: 정점(Vertex)의 집합  
- **E**: 간선(Edge)의 집합

---

## 1. 그래프 기본 용어

| 용어 | 설명 |
|------|------|
| **정점(Vertex)** | 데이터를 저장하는 단위 |
| **간선(Edge)** | 두 정점을 연결하는 선 |
| **인접 정점 (adjacent vertex)** | 간선으로 직접 연결된 정점 |
| **차수 (degree)** | 정점에 연결된 간선의 수 |
| **진입 차수 (in-degree)** | 방향 그래프에서 들어오는 간선 수 |
| **진출 차수 (out-degree)** | 방향 그래프에서 나가는 간선 수 |
| **경로 (path)** | 정점들을 간선으로 연결한 연속적인 경로 |
| **단순 경로 (simple path)** | 정점이 반복되지 않는 경로 |
| **사이클 (cycle)** | 단순 경로의 시작과 끝이 동일한 경우 |

---

## 2. 그래프의 종류

### 무방향 그래프 (Undirected Graph)
- 간선 `(A, B)`는 A↔B 양방향 이동 가능  
- 정점 A와 B가 연결되어 있으면 `graph[A].contains(B)`이고 `graph[B].contains(A)`도 true

### 방향 그래프 (Directed Graph)
- 간선 `<A, B>`는 A → B 한 방향만 가능  
- `graph[A].contains(B)`는 true지만, `graph[B].contains(A)`는 false일 수 있음

### 가중치 그래프 (Weighted Graph)
- 각 간선에 가중치(비용)가 부여된 그래프  
- 네트워크, 지도, 경로 탐색 등에 사용

### 완전 그래프 (Complete Graph)
- 모든 정점이 서로 연결된 그래프  
- 무방향 완전 그래프의 간선 수:  
  $$ \frac{n(n-1)}{2} $$

### 오일러 경로 (Eulerian Tour)
- 모든 간선을 한 번씩만 통과하고, 시작 정점으로 되돌아오는 경로  
- 모든 정점의 차수가 짝수일 때 존재함

---

## 3. 그래프 표현 방식

### 3.1 인접 리스트 (Adjacency List)

- 각 정점에 연결된 정점 목록을 리스트로 표현  
- 메모리 효율적이며, 간선이 적은 희소 그래프에 적합

```java
import java.util.*;

public class AdjacencyListGraph {
    public static void main(String[] args) {
        int V = 5;
        ArrayList<ArrayList<Integer>> graph = new ArrayList<>();

        for (int i = 0; i <= V; i++) {
            graph.add(new ArrayList<>());
        }

        graph.get(1).add(2);
        graph.get(2).add(1);
        graph.get(1).add(3);
        graph.get(3).add(1);
        graph.get(2).add(4);
        graph.get(4).add(2);

        System.out.println(graph);
    }
}
```
---
### 3.2 인접 행렬 (Adjacency Matrix)

- `V x V` 크기의 2차원 배열로 그래프 표현
- `matrix[i][j] = 1`이면 i에서 j로의 간선이 존재함
- 연결 여부 확인이 빠르지만, 메모리 사용량이 많음
- **밀집 그래프 (Dense Graph)** 에 적합

#### 특징

- 두 노드 간 연결 확인: O(1)
- 연결된 노드 전체 확인: O(V)
- 메모리 사용량: O(V²)
- 간선이 적을 경우 공간 낭비 심함

#### 자바 예시

```java
public class AdjacencyMatrixGraph {
    public static void main(String[] args) {
        int V = 5;
        int[][] matrix = new int[V + 1][V + 1];

        // 간선 추가 (방향 그래프)
        matrix[1][2] = 1;
        matrix[2][3] = 1;
        matrix[4][1] = 1;

        // 간선 존재 확인
        if (matrix[1][2] == 1) {
            System.out.println("1 → 2 간선 존재");
        }
    }
}
---

## 4. 그래프 순회 (Traversal)

그래프의 정점을 하나씩 탐색하는 방법에는 다음 두 가지가 있다:

- **DFS (Depth-First Search)**: 깊이 우선 탐색  
- **BFS (Breadth-First Search)**: 너비 우선 탐색

---

### 4.1 깊이 우선 탐색 (DFS)

- 가능한 한 **깊게** 들어가서 탐색  
- 더 이상 탐색할 곳이 없으면 **되돌아가며(backtracking)** 진행  
- **재귀** 또는 **스택**으로 구현 가능  
- **연결 요소 찾기, 조합/순열 문제**에 유리

#### 예시:

```java
static void dfs(int node, boolean[] visited, ArrayList<ArrayList<Integer>> graph) {
    visited[node] = true;
    System.out.print(node + " ");

    for (int next : graph.get(node)) {
        if (!visited[next]) {
            dfs(next, visited, graph);
        }
    }
}
```
---
### 4.2 너비 우선 탐색 (BFS)

- 시작 정점에서 가까운 정점부터 **넓게** 탐색  
- **Queue(큐)** 자료구조를 사용하여 구현  
- **최단 거리 탐색**, **단계적 확장** 문제에 유리

#### BFS 특징

- 방문 순서가 정점 번호 순이 아님에 주의
- DFS보다 구현이 직관적
- 무방향 그래프에서는 방문 여부만 체크하면 됨
- 방향 그래프에서는 간선 방향을 고려해야 함

#### 예시

```java
import java.util.*;

static void bfs(int start, ArrayList<ArrayList<Integer>> graph) {
    Queue<Integer> queue = new LinkedList<>();
    boolean[] visited = new boolean[graph.size()];

    queue.offer(start);
    visited[start] = true;

    while (!queue.isEmpty()) {
        int current = queue.poll();
        System.out.print(current + " ");

        for (int next : graph.get(current)) {
            if (!visited[next]) {
                visited[next] = true;
                queue.offer(next);
            }
        }
    }
}
```