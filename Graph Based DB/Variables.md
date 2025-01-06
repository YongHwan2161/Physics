# `Core`
```c
uchar** Core;
```
- RAM에 올려진 node data들의 메모리상 위치를 가리키는 pointer들의 배열을 가리키는 pointer이다(이중 pointer). 
- 특정 index의 node data를 얻기 위해서는 먼저 Core에 이 node data가 저장되어 있는지 탐색한 다음 있으면 그 데이터를 가져오면 되고, 없다면, binary file에서 읽어들여야 한다. 이를 관리하기 위해서는 어떤 index의 node data가 현재 Core에 저장되어 있는지에 대한 상태를 추적할 수 있어야 한다. 이는 index와 Core 상에서의 위치(배열의 몇 번째에 해당 index의 node data를 가리키는 pointer가 저장되어 있는지)를 mapping시켜 주는 변수가 하나 더 필요하다. 이 변수는 binary file상에서 node data의 offset에 대한 정보를 저장하는 변수와는 다른 역할을 한다. 
- 
# `initValues`
- node를 새로 생성할 때 초기화되는 값을 미리 전역변수로 설정해 놓는다. 
```c
uchar initValues[16] = {
    4,  0,     // data size (2^4 = 16 bytes)
    1,  0,     // number of channels (1)
    8,  0, 0, 0,   // offset for channel 0 (starts at byte 8)
    0,  0,     // number of axes (0)
    0,  0, 0, 0, 0, 0    // remaining bytes initialized to 0
};
```

# `CoreMap`
- node index가 RAM에 올려져 있는지, Core array에서의 해당 위치, binary file에서의 offset 정보를 저장하는 구조체이다. 
- CoreMap 구조체에서의 index는 node data의 index와 동일하기 때문에, 구조체 내에 별도의 index를 저장할 필요는 없다. 
- 프로그램 실행시 초기화 과정에서 map.bin file에 있는 node data offset 정보도 CoreMap에 모두 저장해 놓고 필요할 때 이용할 수 있어야 한다. 필요할 때마다 map.bin 파일을 읽어서 데이터를 활용하는 방식보다 미리 CoreMap에 모두 올려놓는 것이 더 효율적이다. map.bin data는 크기가 크지 않기 때문에 RAM에 모두 올려도 부담이 적다. 
- CoreMap을 초기화하는 함수: [[Functions#`init_core_mapping`|init_core_mapping]]
```c
// Core status tracking
typedef struct {
    int core_position;   // Position in Core array (-1 if not loaded)
    int is_loaded;      // 1 if loaded in RAM, 0 if not
    long file_offset;   // Offset position in data.bin
} NodeMapping;

NodeMapping* CoreMap;
```

# `FreeSpace`
- free space를 관리하는 구조체이다. 
- free blocks의 개수를 나타내는 `count`, free blocks 정보가 저장된 array의 포인터가 저장된 blocks, 삭제된 node의 index가 기록된 array의 포인터인 `free_indices`, 삭제된 index의 개수를 저장하는 index_count
- `FreeBlock`은 block size를 power of 2로 나타낸 `size`와 data.bin에서의 offset을 저장하는 구조체이다. 
```c
typedef struct {
    uint size;          // Size of free block (power of 2)
    long offset;        // Offset in data.bin
} FreeBlock;
  
typedef struct {
    uint count;         // Number of free blocks
    FreeBlock* blocks;  // Array of free blocks
    uint* free_indices; // Array of deleted node indices
    uint index_count;   // Number of free indices
} FreeSpace;
```