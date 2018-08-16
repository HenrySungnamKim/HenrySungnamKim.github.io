---
layout: post
title:  "Memory Management, 운영체제 강의 정리 8강"
date:   2018-08-16
excerpt: "Memory management"
tag:
- OS
- 운영체제
- 강의 정리
- Memory Management
- logical address
- physical address
- contiguous allocation
- Noncontiguous Allocation
- address binding
- paging
- page table
- segment

comments: true
---
* auto-gen TOC:
{:toc}


# 8강. Memory Management

## 메모리란?

주소를 통해 접근하는 객체로 Main Memory는 주 기억장치를 의미한다.

## 주소의 종류

- Logical Address(Virtual Address)

  프로세스마다 독립적으로 갖는 공간. 0번지부터 시작한다.

  CPU가 보는 주소이다.

- Physical Address

  메모리에 실제로 올라가는 위치

- Symbolic Address

  프로그래밍을 할 때 변수를 지정하면 변수의 이름을 통해 값에 접근할 수 있는 방법.

  그 변수를 Symbolic Address라고 한다.

  변수가 컴파일되어 숫자 주소가 만들어지고 물리적 메모리와 매핑되는것을 **주소바인딩**이라고 한다.

## 주소 바인딩. Address Binding

- 주소바인딩이란?

  Data의 주소를 결정하는 것, 물리적 메모리의 어느 부분에 적재할 지 결정하는 것.

- 언제 바인딩 되는가? 3가지

  바인딩 되는 순간에 따라 세가지로 분류한다.

  - Compile time binding

    컴파일 할 때 바인딩이 이루어진다.

    물리적인 메모리가 많이 비었어도 이미 주소가 결정되어 변경할 수 없는 비효율적인 방법.

    변경할 수 없으므로 컴파일 된 코드를 absolute code라고 한다.

  - Load time binding

    프로그램이 실행 되었을 때 바인딩이 이루어진다. 컴파일러가 재배치 가능한 코드라 해서 relocatable code라고 한다.

  - Execution time binding(Runtime binding)

    현재 사용하는 방법. 프로그램이 실행된 후에도 주소를 변경할 수 있다.

    CPU가 주소를 참조할 때마다 새로 binding 상태를 점검해야 한다.

    하드웨어적 지원이 필요하다.(base and limit registers, MMU)

- MMU(Memory Management Unit)

  <img width="400" alt="screenshot2018-08-13at10-404369c9-46f2-4d14-8669-5b943cbff6b0 00 54am" src="https://user-images.githubusercontent.com/37807838/44186821-97bff800-a156-11e8-9e80-58bf8eb20dfb.png">

  Logical Address → Physical Address로 매핑해주는 하드웨어 장치

  아래 두가지 레지스터를 이용해서 주소를 변환한다.

  - Relocation Register(Base Register)

    물리적 메모리 주소의 시작 값

  - Limit Register

    주소가 존재할 수 있는 범위. 프로그램의 크기

    악의적인 목적으로 프로그램의 크기를 벗어난 요청을 할 수 있다.

    trap을 두어 addressing error를 발생시켜 CPU제어권을 운영체제에게 넘긴다.

  - MMU Scheme

    사용자 프로세스가 CPU에서 수행되며 생성해내는 모든 주소값에 대해 Relaocation Register의 값을 더해준다.

  - User Program

    Logical Address 만을 다루며 Physical Address는 볼 수도 없고 알 필요없다.

## Dynamic Loading

프로세스 전체를 메모리에 미리 다 올리는 것이 아니라 해당 루틴이 불려질 때 마다 메모리에 Load하는 것

- Memory Utilization을 향상
- 가끔씩 사용되는 많은 양의 코드 일 경우 유용하다
- **운영체제의 특별한 지원 없이** 프로그램 자체에서 구현 가능

  OS는 라이브러리를 통해 지원 가능하다.

## Overlays

메모리에 실제 필요한 정보만을 올린다. Dynamic Loading과 같다. but 메모리가 작던 초창기 시스템에서 수작업으로 프로그래머가 구현

- 프로세스의 크기가 메모리보다 클 때 유용하다
- 프로그래밍이 매우 복잡하다.

## Swapping

프로세스를 일시적으로 메모리에서 Backing store(Swap area)로 쫓아내는 것

- Swap in / Swap out

  일반적으로 중기 스케쥴러(swapper)에 의해 swap out시킬 프로세스 선정

  우선 순위 기반 CPU 스케쥴링 알고리즘

  - 우선순위가 낮은 프로세스를 swap-out시킨다.
  - 우선순위가 높은 프로세스를 메모리에 Load한다.

  Compile time binding 혹은 Load time binding에서는 원래 메모리 위치로 swap-in 해야 한다. 비 효율적이다.

  → 그래서 Execution time binding을 사용하면 빈 메모리 영역 아무 곳에나 올릴 수 있다.

  Swap time은 대부분 transfer time이다. 헤드가 움직이면서 위치를 찾는 seek time이 일반적으로 길지만 메모리의 양이 많은 Swap 시스템에서는 swap의 양에 비례해서 길어지는 transfer time이 중요하게 된다.

## Dynamic Linking

Linking이란, 프로그램을 컴파일하고 링크를 통해 프로그램을 실행시킨다. 흩어진 프로그램 파일을 묶어주는 것이다.

Dynamic Linking은 Linking을 실행 시간(Execution time)까지 미루는 기법이다.

- Static Linking

  라이브러리가 프로그램의 실행 파일 코드에 포함된다

  실행 파일의 크기가 커진다.

  동일한 라이브러리를 각각의 프로세스가 메모리에 올리므로 메모리의 낭비가 있다.

- Dynamic Linking

  라이브러리가 실행 시 연결된다.

  라이브러리 호출 부분에 라이브러리 루틴의 위치를 찾기 위한 stub이라는 작은 코드를
   둔다.

  라이브러리가 이미 메모리에 있다면, 그 루틴의 주소로 가고 없으면 디스크에서 읽어 온다.

  운영체제의 도움이 필요하다.

## Allocation of Physical Memory

메모리는 일반적으로 두 영역으로 나누어 사용한다.

- OS 상주 영역

  interrupt vector와 함께 낮은 주소 영역을 사용한다.

- 사용자 프로세스 영역

  높은 주소 영역을 사용한다.

  - Contiguous Allocation

    각각의 프로세스가 메모리의 연속적인 공간에 적재되도록 한다.

    앞의 Dynamic Reloaction에 나왔던 기법

    - Fixed Partition Allocation

      사용자가 들어갈 프로그램 영역을 미리 나누어 놓는 것

      대부분의 경우 프로세스의 크기가 파티션보다 크거나 작다. 딱 들어 맞지 않는다. 그럴 때마다 아래의 조각들이 발생한다.

      - 외부조각, External fragmentation

        프로그램 크기보다 분할의 크기가 **작은** 경우

        아무 프로그램에도 배정되지 않았지만 프로그램이 올라갈 수 없는 작은 분할

      - 내부조각, Internal fragmentation

        프로그램 크기보다 분할의 크기가 **큰** 경우

        하나의 분할 내부에서 발생하는 사용되지 않는 메모리 조각

        특정 프로그램에 배정 되었지만 사용되지 않는 공간

      비효율 적인 방법이라 아래의 Variable Partition Allocation방식을 도입했다.

    - Variable Partition Allocation

      요청한 프로세스의 크기만큼 **순서대로** 할당 해준다.

      프로세스의 시작과 종료는 먼저 요청을 하고 먼저 할당을 받았다고 해서 먼저 반환하는 것이 아니다. 이때문에 가변적으로 할당해 줬던 메모리 군데 군데에 빈 공간(hole)이 생기게 된다.

      - Hole

        가용 메모리 공간으로 여러 곳에 흩어져 존재한다. 운영체제는 할당 공간과 가용 공간(hole)정보를 유지한다.

      - Dynamic Allocation Problem

        Size가 n인 프로세스를 할당할 가장 적절한 hole을 찾는 문제

        **First Fit**

        Size가 n인 것을 찾기 시작해서 가장 먼저 찾아지는 hole에 할당하는 방법. n보다 크기만 하면 할당해 준다.

        **Best Fit**

        Size가 n이상인 hole중 가장 작은 hole을 찾아 할당하는 방법. 모든 hole을 검색해야 하므로 시간이 소요되고 아주 작은 hole들이 계속해서 생성되는 문제가 있다.

        **Worst Fit**

        Size가 n인 hole중 가장 큰 hole에 할당한다. 역시 모든 hole을 검색해야 한다. 상대적으로 아주 큰 hole들이 생성된다.

        ✔︎ First fit, Best fit이 Worst fit보다 속도와 공간 이용률 측면에서 효과적인 방법이다.

      - Compaction, Hole을 한 군데로 모으는 방법

        외부 조각,External Fragmetation을 해결하는 한가지 방법

        사용중인 메모리 영역을 한 군데로 몰고 hole들을 다른 한 곳으로 몰아 큰 block을 만든다

        매우 비용이 많이들며, 최소한의 메모리 이동으로 compaction하는 아주 복잡한 방법이다.

        Compaction은 프로세스 주소가 실행시간에 동적으로 재배치 가능한 경우에만 수행될 수 있다.

  - Noncontiguous Allocation

    하나의 프로세스가 메모리의 여러 영역에 분산되어 올라갈 수 있다.

    - **Paging 기법**

      프로세스의 가상 메모리를 같은 크기의 페이지로 자르는 것

      Paging 기법을 사용하면 hole이 생기지 않지만 불연속으로 할당하게 되면 (일부는 backing storage, 일부는 physical memory) 주소 변환이 복잡해 진다.

      → Page 별로 주소변환을 해줄 방법이 필요해 졌고, page table을 생성했다. logical address는 table의 index, table index의 value값은 physical address를 갖는다.

      <img width="400" alt="screenshot2018-08-14at9-2ab298aa-a4bd-49c4-9cfa-b746de29f2a6 16 55am" src="https://user-images.githubusercontent.com/37807838/44186823-98f12500-a156-11e8-915a-c3919f3a0a22.png">

      paging

      외부 조각은 발생하지 않지만 내부 조각은 발생 가능하다. (프로세스 크기가 페이지의 배수가 되라는 법이 없으므로)

      **테이블은 어디에 저장 될까?**

      프로그램의 주소 공간이 무수히 많은 페이지로 분할되고, table도 또한 무수히 많은 index, value 즉, Entry 생성이 필요해진다. 용량이 커지기 때문에 레지스터, 캐시 메모리에 들어가지 못하게 된다. 메모리(메인)에 들어가게 된다. 그래서 메모리에 접근하기 위해 페이지 테이블에 접근을 하고 다시 물리적 메모리로 접근하는 방식이 되었다. 메모리에 **두 번** 접근하게 된 것이다.

      → 더 느려지는 결과 발생

      이 문제를 해결하기 위해서 레지스터를 사용한다.

      page-table base register가 메모리 상의 page table의 주소값을 갖는다.

      page-table length register가 table 크기를 보관한다.

      TLB(Translation Look aside Buffer)를 사용하는데 이것은 일종의 캐시 메모리이고 주소 변환을 한다. TLB는 page table의 일부를 저장한다.

      - CPU에서 논리적 주소를 통해 요청 → TLB점검
      - 요청한 주소에 해당 내용 존재(TLB hit) → 주소 변환
      - 해당 내용 존재 X (TLB miss) → page table로 접근 → 주소 변환
      - TLB 사용의 문제점

        TLB 전체를 검색해야 한다. O(n)의 시간 복잡도 발생

      - TLB 문제점 해결

         Associative register를 이용해서 해결. 이 레지스터는 병렬로 검색(Parallel search)이 가능하다. →time complexity가 O(1)이 됨.

      컴퓨터의 발전으로 지원할 수 있는 프로그램의 크기가 커졌다. 주소 공간이 커져서 page table들의 크기도 비약적으로 커졌다. 프로세스마다 존재하는 page table을 모두 메모리에 올리기엔 한계가 있다. 그래서 page table을 새로운 page로 인식하는 방식을 사용한다. page안에 주소 체계를 하나 더 두는 것이다.

    - **페이지 테이블 기법**
      - Two Level Page Table

        inner/outer page table을 통해 최종적으로 메모리에 접근한다. 실제로 사용하는 페이지에 대해서만 inner table이 형성된다.

      - Multi Level Paging and Performance

        다 단계 page table을 통해 주소 공간 사용을 절약한다. 각 단계의 페이지 테이블이 메모리에 존재하므로 logical address의 physhical address 변환에 더 많은 메모리 접근이 필요하다. 따라서 TLB의 역할이 중요해진다.

      **페이지 테이블 구성**

      - Valid/Invalid Bit in a Page Table

        사용되지 않는 페이지들은 Invalid로 표시된다. 물리적인 메모리에는 존재하지 않고 swap area에 존재하거나 프로세스가 그 주소 부분을 아예 사용하지 않는 경우를 의미한다.

      - Memory Protection

        Page table의 각 entry마다 다음의 bit를 둔다.

        - Protection bit

          Page에 대한 접근 권한(read/write/read-only)

          코드 영역 : 사용자의 수정을 막기위해 read-only

          데이터, 스택 영역 : read/write

        - Valid - invalid bit

          **valid**는 해당 주소의 frame에 그 프로세스를 구성하는 유효한 내용이 있음을 뜻한다. (접근 허용)

          **invalid**는 해당 주소의 frame에 유효한 내용이 없음을 뜻한다. (접근 불가)

      - Inverted Page Table

        <img width="400" alt="screenshot2018-08-16at10-760b980e-22fa-4776-b66b-b8c54480ccff 35 47am" src="https://user-images.githubusercontent.com/37807838/44186826-9abae880-a156-11e8-96a3-19b063ba2812.png">

        공간 오버헤드가 큰 페이지 테이블 기법의 단점을 보완하기 위해 나왔다. 페이지 테이블의 엔트리가 프로세스의 페이지 만큼 존재하는 것이 아니라, 물리적 메모리의 페이지 프레임 수 만큼 존재하게 된다. 테이블 전체를 탐색해야 하는 단점이 있고, 값비싼 associative register를 사용해 해결한다.

      - Shared page
        - Shared code(Pure code)

        여러 프로세스가 공유하는 코드를 하나의 물리적 메모리에 올리는 기법. read-only로 세팅한다. 모든 프로세스의 동일한 logical address에 위치해야 한다.

        - Private code and data

        각 프로세스들이 독자적으로 가져야 할 영역, 메모리에 독자적으로 올린다. private하며 logical address space 중 어느 곳에 와도 무방하다.

    - **Segmentation 기법**

      프로그램의 주소 공간을 의미있는 단위로 자르는 것이다.

      - process(code, data, stack)을 code segment, data segment, stack segment로 나눈다. 더 작게 프로그램을 구성하는 함수 하나 하나를 segment로 지정할 수 있고 크게 프로그램 전체를 하나의 segment로 지정할 수 있다.
      - 분할된 조각들의 크기가 균일하지 않기 때문에 contiguous allocation때 발생했던 문제들이 그대로 발생할 가능성이 있다.
      - segment에는 local address(segment number, offset)가 지정되며 이를 segment table이라 한다. segment 각각의 길이가 동일하지 않을 수 있어서 table에는 offset이라는 개념으로 table의 length를 갖고있다. paging에서는 offset값이 결국 페이지 크기였지만 segment 기법은 offset값이 결정되어야만 한다는 단점이 존재한다.
      - paging기법 처럼 두 레지스터와 함께 동작한다.

        Segment-Table Base Register(STBR), Segment-Table Length Register(STLR)

      - 의미 단위로 나눔으로써 생기는 장점이 있다. Segment 별로 Protection bit이 존재하여 code, stack, data 별로 접근 권한을 설정할 수 있다. Segment는 page처럼 무수히 많이 존재하지 않기 때문에 table의 크기가 상대적으로 작다.
    - **Paged Segmentation 기법**

      Paging 기법과 Segmentation 기법의 장점을 혼합하여 만든 방식이다. Segment가 페이지로 잘리게 된다.

      메모리에 적재될 때 Page단위로 적재함으로써 segmetation기법의 hole발생하는 문제를 해결하였고 segment단위로 수행하면서 paging 기법의 공유,보안,접근 권한 문제를 해결했다.
