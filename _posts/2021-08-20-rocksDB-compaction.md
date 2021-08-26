---
toc : true
title : "[rocksDB] Compaction in rocksDB"
category : 
    - rocksDB
---
Compaction 알고리즘은 disk 상의 invalid한 데이터를 삭제하여 공간을 확보하는데 중요한 역할을 합니다.

Li 에서 compaction이 발생하면 Li와 Li+1의 중복되는 key값을 갖는 모든 sstable은 compaction에 참여하게 됩니다.

rocksDB에서는 총 3가지 $($`Leveled`, `Universal`, `FIFO`) compaction 스타일을 제공하고 있습니다.

### 1. Leveled Compaction $($default)

스토리지는 아래로 내려갈수록 커지는 트리 구조의 여러 level들로 구성되어 있습니다.

각 level에는 SSTable들이 존재하며 최상위 L0를 제외한 Li에서의 sstable에는 중복되는 key값이 존재하지 않습니다.

L0는 메모리의 쓰기 버퍼 $($memtable)에서 방금 flush된 파일들을 저장하며 그 외의 level은 하나의 정렬된 데이터의 연속입니다.

*Levled compaction workload*

leveled compaction은 memtable의 flush로 인해, Li의 sstable 저장공간이 부족하게 되면 발생합니다.

1) Li에서 sstable을 선택합니다. 이때 선택되는 sstable은 다양한 정책에 따라 달라지게 됩니다. <br> `round-robin`, `oldest`, `least overlapped` ... 

2) Li 와 Li+1에 중첩되는 sstable을 읽어들입니다.

3) 읽어들인 sstable을 합병정렬 시킵니다.

4) 합병정렬된 key-value pair를 Li+1에 씁니다.




