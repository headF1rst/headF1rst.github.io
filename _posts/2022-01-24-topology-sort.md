---
toc: true
title: "위상 정렬[파이썬 코드]"
category:
  - Algorithm
---
선후 관계가 존재하는 그래프상에서, 위상 정렬을 수행하여 모든 선후 관계를 지키는 **전체 순서**를 계산할 수 있다.

스타크래프트를 예로 들자면 마린을 생성하는 베럭을 먼저 지어야만 탱크를 뽑을수 있는 팩토리를 건설 할 수 있다. 

하지만 탱크를 생성하기 위해서는 가스 자원이 필요하기 때문에 팩토리 이전에 정제소를 짓는것이 권장된다.

![스크린샷 2022-01-24 오후 3.38.02.png](https://i.imgur.com/POcn8rp.png)

안정적으로 게임을 운영하기 위해서는 베럭 → 정제소 → 팩토리 순으로 건물을 지어야만 한다.

위상 정렬 알고리즘은 베럭 → 정제소 → 팩토리 순으로 지어지도록 **선후 관계를 보장**한다.

각각을 노드라고 가정하였을때, 팩토리 노드는 진입차수가 2인것을 확인할 수 있다.

**진입 차수**: 특정한 노드로 들어오는 간선의 개수

## 위상 정렬 수행과정

1. 진입차수가 0인 노드를 큐에 넣는다.
2. 큐가 빌 때까지 다음의 과정을 반복한다.
    1. 큐에서 원소를 꺼내 해당 노드에서 출발하는 간선을 그래프에서 제거한다.
    2. 새롭게 진입차수가 0이 된 노드를 큐에 넣는다.

만약 큐에서 원소가 V번$($정점의 개수) 추출되기 전에 큐가 비어버리면 사이클이 발생한 것이다.

다음 그래프를 예로 들어서 설명해 보겠다.

![스크린샷 2022-01-25 오전 2.17.27.png](https://i.imgur.com/vwR1vhk.png)

2, 3번 노드는 1번 노드를 먼저 방문해야지만 방문이 가능하고 4번 노드는 2번과 3번 노드를 둘다 방문한 후에 도착할 수 있다.

**step 1.**

![스크린샷 2022-01-25 오전 2.23.22.png](https://i.imgur.com/IwuB9kZ.png)

가장먼저 진입차수가 0인 노드를 큐에 넣는다. 

현재 노드 1의 진입차수만 0이기 때문에 큐에 노드 1만 삽입한다.

**step 2.**

![스크린샷 2022-01-25 오전 2.42.42.png](https://i.imgur.com/fp98feY.png)

큐에 들어있는 노드 1을 꺼낸 다음 노드 1과 연결되어 있는 모든 간선들을 제거한다.

이제 노드 2와 노드 3의 진입차수가 0이 되었고, 진입차수가 0인 노드 2, 3을 큐에 삽입한다.

**step 3.**

![스크린샷 2022-01-25 오전 2.34.13.png](https://i.imgur.com/j6gC987.png)

그 다음 큐에 들어있는 노드 2를 꺼낸 다음, 노드 2와 연결되어있는 간선들을 모두 제거한다.

새롭게 진입차수가 0이 되는 노드가 없으므로 그냥 넘어간다.

**step 4.**

![스크린샷 2022-01-25 오전 2.36.46.png](https://i.imgur.com/l1vrSbC.png)

큐에 있는 노드 3을 꺼낸 다음 노드 3과 연결되어있는 간선을 제거한다.

노드 4의 진입차수가 0이 되었으므로 노드 4를 큐에 삽입한다.

**step 5.**

![스크린샷 2022-01-25 오전 2.37.44.png](https://i.imgur.com/6ZOOku0.png)

큐에 들어있는 노드 4를 꺼낸다. 

큐가 비었으므로 모든 과정을 종료한다.

위의 과정을 수행하는 동안 큐에서 빠져나간 노드를 순서대로 출력하면 위상 정렬의 수행 결과가 된다.

`1 → 2 → 3 → 4` or `1 → 3 → 2 → 4`

다음과 같이 한 단계에서 큐에 새롭게 들어가는 노드가 2개 이상인 경우가 존재하면, 여러 가지의 답이 존재하게 된다.

## 위상 정렬 파이썬 코드

```python
from collections import deque

# 노드의 개수와 간선의 개수 입력
v, e = map(int, input().split())
# 모든 노드의 진입차수 0으로 초기화
indegree = [0] * (v + 1)
# 각 노드에 연결된 간선 정보를 담기 위한 연결 리스트 초기화
G = [[] for _ in range(v + 1)]

# 방향 그래프의 모든 간선 정보를 입력
for _ in range(e):
    a, b = map(int, input().split())
    G[a].append(b) # 정점 a에서 b로 가는 간선
    indegree[b] += 1 # b노드로 들어오는 진입차수 1 증가

''' 위상정렬 시작 '''
def topology_sort():
    result = [] # 알고리즘 수행 결과를 저장
    q = deque()

    # 진입 차수가 0인 노드를 큐에 삽입하고 시작
    for i in range(1, v + 1):
        if indegree[i] == 0:
            q.append(i)

    while q:
        now = q.pop() # 큐에서 다음 노드 꺼내기
        result.append(now)
        # 해당 노드와 연결된 노드들의 진입차수에서 1 빼기
        for i in G[now]:
            indegree[i] -= 1
            # 새롭게 진입차수가 0이 되는 노드를 큐에 삽입
            if indegree[i] == 0:
                q.append(i)

    # 위상 정렬 수행 결과 출력
    for i in result:
        print(i, end=' ')

topology_sort()
```

## 위상 정렬 시간복잡도

위상 정렬은 모든 노드를 차례대로 확인해 가면서 해당 노드에서 출발하는 간선을 차례대로 제거한다.

따라서 O$($V + E) 시간 복잡도를 갖게 된다.