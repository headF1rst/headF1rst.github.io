---
toc : true
title : "[rocksDB] Compaction priority option in LCS"
category : 
    - rocksDB
---
Level compaction에서는 compaction 과정에서 어떤 sstable을 선택하여 하위 level과 합병해 줄지 선택 할 수있는 4가지 기본 알고리즘이 존재합니다.

- kOldestSmallestSeqFirst
- kOldestLargestSeqFirst 
- kByCompensatedSize
- kMinOverlappingRatio (default)

이번 스터디에서는 이러한 4가지 옵션별로 어떤 특징이있고 같은 환경에서 어떤 성능차이를 보이는지 db_bench를 통해 실험해 보았습니다.

### kOldestSmallestSeqFirst
컴팩션 과정에서 key range의 밀도가 낮은 파일을 후보로 선택하는 것은 파일을 다음 레벨로 이동시키는데 많은 비용을 들게 하며 더 많은 쓰기작업을 요구합니다.

때문에 밀도가 높은 파일을 선택하여야 컴팩션 과정에서 하위 레벨과 겹치는 key range가 적어지기 때문에 쓰기 증폭을 개선할 수 있습니다.

`kOldestSmallestSeqFirst` 기법의 경우, 업데이트가 가장 오래전에 된, 즉 시퀀스 넘버가 가장 작은 파일을 컴팩션 파일로 고릅니다. 이렇게 선택된 파일은 키가 일정하게 분포되어있다는 가정하에서 key range의 밀도가 가장 높게 형성 되어있습니다.

쓰기가 키 공간에 균일하게 분포되어 있는 상황에서 쓰기증폭을 줄이는데 효율적인 알고리즘이라고 할 수 있습니다.

### kOldestLargestSeqFirst 
특정한 부분 키값들만 활발하게 업데이트가 되고 다른 key range는 cold한 케이스에 좋은 성능을 발휘 하는 기법입니다,

이러한 경우에서는 hot key range를 컴팩션 후보로 선택하지 않음으로서 하위 레벨로 내리지 않고 현 레벨에 유지되도록 해주는 것만으로도 쓰기 증폭과 공간 증폭을 줄일수 있습니다.

예를 들어서 DB에 10~20인 키값만이 빈번하게 업데이트가 되고 다른 값들은 거의 업데이트가 되지 않는다는 가정에서 level 1의 target size가 20일때, 

10~20의 key range를 가능한 level 1에 위치하도록 해서 level 0 -> level 1으로 컴팩션이 발생할때 단순히 level 1에 키값을 덮어 씀으로써 level 1의 target size를 초과하지 않도록 합니다.

`kOldestLargestSeqFirst` 기법의 경우, 최신 업데이트가 가장 오래된 파일을 선택합니다. $($coldest range) coldest range를 먼저 컴팩션 해줌으로써 hot ranges는 기존 레벨에 머물게 되고 전체적인 컴팩션 횟수를 줄일 수 있게 됩니다.

작은 범위의 기존 키를 덮어쓰는 경우의 최적화에 적합한 기법이라고 할 수 있습니다.

### kByCompensatedSize

하나의 파일에 delete 마커가 많이 포함되어 있으면 별 의미없는 delete 마커까지 읽어야 해서 해당 파일의 데이터를 훑는데 쓸데없이 많은 시간이 소요될 수 있습니다.

따라서 delete 키들을 가능한 빨리 마지막 레벨로 컴팩션 하는것이 디스크 공간이 더 빨리 회수되므로 공간 효율성에 좋습니다.

과거 rocksDB의 디폴트 옵션이었던 `kByCompensatedSize`는 파일에 삭제횟수가 삽입횟수보다 많아질 수록 컴팩션될 우선순위를 높이는 방식을 사용여 공간 효율성을 개선합니다.

### kMinOverlappingRatio (Default)

다음 레벨에서 중첩되는 크기와 해당 파일의 크기의 비율을 기준으로 가장 작은값을 컴팩션 파일로 선정하는 기법입니다.

현재 rocksDB의 디폴트 옵션입니다.


