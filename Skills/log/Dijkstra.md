# Dijkstra (다익스트라)

## 알고리즘 원리

1. 출발 노드를 설정한다.
2. 최단 거리 테이블을 초기화한다.
3. **방문하지 않은 노드 중에서 최단 거리가 가장 짧은 노드를 선택한다.**
4. **해당 노드를 거쳐 다른 노드로 가는 비용을 계산하여 최단 거리 테이블을 갱신한다.**
5. 위 과정에서 3과 4번을 반복한다.

- 최단 거리를 계산
- 음의 무게를 가진 간선에 대해 작동하지 않는다

## Java 구현

```java
import java.util.*;

class Node implements Comparable<Node> {

    public final int index;

    public final int distance;

    public Node(int index, int distance) {
        this.index = index;
        this.distance = distance;
    }

    @Override
    public int compareTo(Node o) {
        return Integer.compare(this.distance, o.distance);
    }
}

public class Main {

    // 그래프의 각 i 번째 리스트에는 다른 노드로 향하는 노드들이 들어있음
    public static int[] dijkstra(List<List<Node>> graph, int start) {
        int[] costs = new int[graph.size() + 1];
        Arrays.fill(costs, Integer.MAX_VALUE);

        PriorityQueue<Node> pq = new PriorityQueue<>();
        pq.offer(new Node(start, 0));

        while (!pq.isEmpty()) {
            Node cur = pq.poll();
            if (costs[cur.index] <= node.getDistance()) {
                continue;
            }

            for (Node next : graph.get(curIdx)) {
                int nextCost = costs[cur.index] + next.distance;
                if (nextCost >= costs[next.index]) {
                    continue;
                }
                pq.offer(new Node(next.index, nextCost));
            }
        }

        return costs;
    }
}
```

## 의사 코드

```
function Dijkstra(Graph, source):
    dist[source] ← 0    // 초기화

    create vertex set Q

    for each vertex v in Graph:
        if v ≠ source
            dist[v] ← INFINITY  //소스에서 v까지의 아직 모르는 길이
        prev[v] ← UNDEFINED // v의 이전 노드

        Q.add_with_priority(v, dist[v])


    while Q is not empty:   // 메인 루프
        u ← Q.extract_min()  // 최고의 꼭짓점을 제거하고 반환한다
        for each neighbor v of u:   // Q에 여전히 남아 있는 v에 대해서만
            alt ← dist[u] + length(u, v)
            if alt < dist[v]
                dist[v] ← alt
                prev[v] ← u
                Q.decrease_priority(v, alt)
    return dist, prev
```

```java
import java.util.*;

class Node implements Comparable<Node> {

    private final int index;

    private final int distance;

    public Node(int index, int distance) {
        this.index = index;
        this.distance = distance;
    }

    public int getIndex() {
        return index;
    }

    public int getDistance() {
        return distance;
    }

    @Override
    public int compareTo(Node o) {
        return this.distance - o.distance;
    }
}

public class Main {

    // 그래프의 각 i 번째 리스트에는 다른 노드로 향하는 노드들이 들어있음
    public static int[] dijkstra(List<List<Node>> graph, int start) {
        int[] d = new int[graph.size() + 1];
        Arrays.fill(d, Integer.MAX_VALUE);
        PriorityQueue<Node> pq = new PriorityQueue<>();
        pq.offer(new Node(start, 0));
        d[start] = 0;

        while (!pq.isEmpty()) {
            Node node = pq.poll();
            int curIdx = node.getIndex();
            if (d[curIdx] < node.getDistance()) {
                continue;
            }

            for (int i = 0; i < graph.get(curIdx).size(); i++) {
                int cost = d[curIdx] + graph.get(curIdx).get(i).getDistance();
                int nextIdx = graph.get(curIdx).get(i).getIndex();
                if (cost < d[nextIdx]) {
                    d[nextIdx] = cost;
                    pq.offer(new Node(nextIdx, cost));
                }
            }
        }

        return d;
    }

    /**
    * 위 다익스트라 메서드가 메모리 초과 날 때 아래 메서드를 사용하니 해결되었음 
    * 다익스트라 메서드 내에서 visited 배열을 사용하지 않을 경우
    * Node 의 distance (weight) 가 같은 경우에 대해서 중복이 허용됨
    * * cost 배열만 사용해서도 위 문제를 해결할 수 있는데 cost 비교를 <, > 로 하지 않고
    * * <=, => 로 하면 같을 때는 방문했을 때로 보기 떄문에 중복을 방지할 수 있음
    */
    public static int[] dijkstra_UsingVisitedArr(List<List<Node>> graph, int start) {
        int[] cost = new int[graph.size()];
        Arrays.fill(cost, Integer.MAX_VALUE);
        cost[start] = 0;

        PriorityQueue<Node> pq = new PriorityQueue<>();
        pq.offer(new Node(start, 0));

        boolean[] visited = new boolean[graph.size()];

        while (!pq.isEmpty()) {
            Node node = pq.poll();
            int curV = node.getTo();
            int curCost = node.getWeight();
            if (visited[curV]) {
                continue;
            }
            visited[curV] = true;

            for (Node nextNode : graph.get(curV)) {
                int nextV = nextNode.getTo();
                int nextCost = curCost + nextNode.getWeight();
                if (nextCost > cost[nextV]) {
                    continue;
                }
                cost[nextV] = nextCost;
                pq.offer(new Node(nextV, nextCost));
            }
        }

        return cost;
    }
}
```

## leetcode 743

```python
import collections
import heapq
from typing import List


class Solution:
    def networkDelayTime(self, times: List[List[int]], N: int, K: int) -> int:
        graph = collections.defaultdict(list)
        # 그래프 인접 리스트 구성
        for u, v, w in times:
            graph[u].append((v, w))

        # 큐 변수: [(소요 시간, 정점)]
        Q = [(0, K)]
        dist = collections.defaultdict(int)

        # 우선 순위 큐 최소값 기준으로 정점까지 최단 경로 삽입
        # 기존 다익스트라에서 dist를 무한대로 설정하고 우선순위를
        # 조정하는 것이 파이썬의 heap으로 불가능 하므로 아래와 같이 해결
        # 우선순위 큐에 기존에 정점이 추가되는 것과 달리 간선이 추가된다.
        while Q:
            time, node = heapq.heappop(Q)
            if node not in dist:
                dist[node] = time
                for v, w in graph[node]:
                    alt = time + w
                    heapq.heappush(Q, (alt, v))

        # 모든 노드 최단 경로 존재 여부 판별
        if len(dist) == N:
            return max(dist.values())
        return -1
```
