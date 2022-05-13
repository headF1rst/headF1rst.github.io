---
toc : true
title : "[Programmers] 징검다리 건너기 파이썬 풀이"
category :
    - 오답노트
---

## 문제링크
[2019 카카오 개발자 겨울 인턴십 > 징검다리 건너기](https://programmers.co.kr/learn/courses/30/lessons/64062)

## 문제 풀이

- 친구들은 디딤돌을 하나씩 건너다가 0을 만나면 그 다음 디딤돌로 점프해서 넘어간다
- 점프는 최대 k 거리 만큼 뛸수 있다.
- stones 배열 각 원소는 1 ~ 200,000,000 이하 이다. 따라서 다리를 건널 수 있는 최대 
친구수 또한 1 ~ 200,000,000명 이다.
- 이진 탐색을 사용하여 징검다리를 건널 수 있는 최대 인원 수를 mid로 가정하고 검증한다
- 검증은 점프가 k 이하로 가능하면 mid 가 작은 경우이고 k 이상이면 건너지 못하기 때문에 
mid를 크게 가정한 경우이다.

## Python3 코드

``` python
def solution(stones, k):
    left = 1
    right = 200000000
    
    while left <= right:
        mid = (left + right) // 2
        cnt = 0
        tmp = stones.copy()
        
        for t in tmp:
            if t - mid <= 0:
                cnt += 1
            else:
                cnt = 0
            
            if cnt >= k:
                break
        
        if cnt >= k:
            right = mid - 1
        else:
            left = mid + 1
            
    return left
```