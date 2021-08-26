---
toc : true
title : "[rocksDB] Compaction priority option in LCS"
category : 
    - rocksDB
---
rocksDB의 기본 compaction 알고리즘인 `Leveled compaction`에는 총 4가지의 `Compaction priority option`이 존재합니다. 옵션에 대해 알아보기에 앞서 Leveled compaction이 어떻게 동작하는지 알아보겠습니다.

### Leveled compaction workload

![leveled compaction](https://raw.githubusercontent.com/facebook/rocksdb/gh-pages-old/pictures/pre_l1_compaction.png)

L0에서 컴팩션이 일어난후에 L1의 타겟 사이즈가 위와같이 초과되면 컴팩션이 트리거 되면서 L1에서 하나의 파일$($SSTable)을 선택하게 되고, 해당 파일의 key range와 겹치는 L2의 모든 파일들을 합병정렬 시켜서 L2에 정렬된 상태로 재배치 시키는 과정이 일어나게 됩니다.

![leveled compaction](https://raw.githubusercontent.com/facebook/rocksdb/gh-pages-old/pictures/post_l1_compaction.png)

L1에서 컴팩션 후보 파일을 선정할때, 어떤 파일을 선정하느냐에 따라 컴팩션 횟수와 쓰기증폭 값이 달라지며 선정 기준을 결정하는 옵션이 바로 `Compaction priority option`입니다.

- kByCompensatedSize $($0x0)
- kOldestLargestSeqFirst $($0x1)
- kOldestSmallestSeqFirst $($0x2)
- kMinOverlappingRatio $($default) $($0x3)

이러한 4가지 옵션별로 어떤 특징이있고 같은 환경에서 어떤 성능차이를 보이는지 db_bench를 통해 실험해 보았습니다.

---

### kByCompensatedSize

기존 leveled 컴팩션에서는 하나의 파일에 delete 마커가 많이 포함되어 있으면 별 의미없는 delete 마커까지 읽어야 하기 때문에 파일의 데이터를 훑는데 쓸데없이 많은 시간이 소요될 수 있다는 단점이 존재했습니다.

이 `kByCompensatedSize` 옵션은 삭제 횟수가 삽입횟수보다 많은 파일을 선택하여 컴팩션을 진행합니다.

때문에 delete 키들을 가능한 빨리 마지막 레벨로 컴팩션 해주어 디스크 공간이 더 빨리 회수되므로 앞서 설명한 문제를 해결하고 공간 효율성에 장점을 보인다고 할 수 있습니다.

![kill_error](/assets/images/rocksDB/compensatedSize.png) 

<br>

![kill_error](/assets/images/rocksDB/compensatedSize2.png) 

### kOldestLargestSeqFirst 

`kOldestLargestSeqFirst` 기법의 경우, 최신 업데이트가 가장 오래된 파일을 선택합니다. 

coldest range를 먼저 컴팩션 해줌으로써 hot ranges는 기존 상위 레벨에 머물게 되고 
hot ranges에 해당되는 값이 들어왔을때 단순히 hot range에 키값을 덮어 쓸수 있게됩니다. 

이 덕분에 해당 레벨의 target size가 초과되지 않게되고 전체적인 컴팩션 횟수를 줄일 수 있게 됩니다.

때문에 특정한 부분 키값들만 활발하게 업데이트가 되고 다른 key range는 cold한 케이스에 좋은 성능을 발휘 하는 기법입니다.

예를 들어서 DB에 10~20인 키값만이 빈번하게 업데이트가 되고 다른 값들은 거의 업데이트가 되지 않는다는 가정에서 level 1의 target size가 20일때, 

10~20의 key range를 가능한 level 1에 위치하도록 해서 level 0 -> level 1으로 컴팩션이 발생할때 단순히 level 1에 키값을 덮어 씀으로써 level 1의 target size를 초과하지 않도록 할 수 있습니다.

![kill_error](/assets/images/rocksDB/oldestLargestSeqFirst.png) 


### kOldestSmallestSeqFirst

`kOldestSmallestSeqFirst` 기법의 경우, 업데이트가 가장 오래전에 된, 즉 시퀀스 넘버가 가장 작은 파일을 컴팩션 파일로 고릅니다. 선택된 파일은 키가 일정하게 분포되어있다는 가정하에서 key range의 밀도가 가장 높게 형성 되어있습니다.

key range의 밀도가 높을 수록 컴팩션 과정에서 하위 레벨과 겹치는 key range가 적어지기 때문에 쓰기 증폭을 최적화 할 수 있다는 장점이 존재합니다. 

때문에 kOldestSmallestSeqFirst 옵션은 쓰기가 키 공간에 균일하게 분포되어 있는 상황에서 쓰기증폭을 줄이는데 효율적인 알고리즘이라고 할 수 있습니다.

![kill_error](/assets/images/rocksDB/oldestSmallestSeqFirst.png) 

### kMinOverlappingRatio $($Default)

마지막 옵션은 현재 compaction priority의 디폴트 옵션값인 kMinOverlappingRatio 입니다. 다음 레벨에서 중첩되는 크기와 해당 파일의 크기의 비율을 기준으로 가장 작은값을 컴팩션 파일로 선정하는 기법입니다.

kByCompensatedSize의 쓰기증폭을 30% 개선시켜줍니다.

![kill_error](/assets/images/rocksDB/minOverlappingRatio.png) 



