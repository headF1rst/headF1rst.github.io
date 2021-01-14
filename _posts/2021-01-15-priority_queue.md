---
toc : true
title : "[STL] 우선 순위 큐 priority_queue"
category : 
    - TIL
    - STL
---
우선 순위 큐 `(priority queue)`는 데이터의 삽입은 일반적인 큐와 같지만 삭제는 우선 순위가 가장 높은 것을 먼저 삭제하는 특징이 있습니다. 삽입된 데이터는 트리구조인 `히프 (heap)`를 사용하여 주어진 조건에 맞는 우선 순위 순으로 루트$($top)에서 부터 아래로 정렬됩니다.