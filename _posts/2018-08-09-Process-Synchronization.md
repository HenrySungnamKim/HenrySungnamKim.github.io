---
layout: post
title:  "Process Synchronization, 운영체제 강의 정리 5강"
date:   2018-08-09
excerpt: "Process Synchronization에 대해 알아보자."
tag:
- OS
- 운영체제
- 강의 정리
- Process
- Synchronization
comments: true
---
* auto-gen TOC:
{:toc}

# 6강. Process Synchronization

강의 링크 : [https://core.ewha.ac.kr/publicview/C0101020140401134252676046?vmode=f](https://core.ewha.ac.kr/publicview/C0101020140401134252676046?vmode=f)

# 프로세스 동기화, Process Synchronization = 병행제어, Concurrency Control

## Race Condition

여러 객체가 하나의 데이터를 동시에 접근하려고 하는 상태를 말한다.

- 데이터의 최종 연산 결과는 마지막에 다룬 프로세스에 따라 달라진다.

  공유 데이터(Shared Data)의 동시접근(Consistency Access)은 데이터의 불일치(Inconsistency) 문제를 발생시킬 수 있다. 일관성(Consistency) 유지를 위해서 협력 프로세스 간의 실행 순서를 정해주는 메커니즘 필요

- 어떤 경우에 데이터 불일치가 발생하는가? 상황별 해결방법은?
  - 커널 수행 중 인터럽트가 발생한 경우

    중요한 루틴을 처리할 때는 인터럽트가 들어와도 인터럽트 처리를 미루고 작업을 마친 뒤 인터럽트 처리

  - 프로세스가 시스템 콜을 해서 커널 모드일 때 Context Switch가 발생한 경우

    프로세스가 커널 모드로 수행 중일 때는 CPU를 Preempt(선점) 하지 않게 한다. 커널 모드에서 사용자 모드로 복귀할 때 Preemt한다.

  - Multi Processor에서 Shared Memory 내의 커널 데이터에 접근한 경우

    한 프로세서가 공유 메모리에 접근할 때 데이터에 lock을 걸고 메모리에서 작업이 끝나면 unlock한다.

    ✔︎ 무조건 접근을 제한하면 효율에 문제가 생긴다. 따라서 Critical Section(공유 데이터에 접근하는 코드)에 특별한 방법을 적용해야 한다.

- Critical Section 문제를 해결하기 위한 조건
  - Mutual Exclusion (상호 제외)

    한 프로세스가 Critical Section부분을 수행 중이면 다른 모든 프로세스는 Critical Section에 들어가면 안된다.

  - Progress

    아무도 Critical Section에 있지 않다면 Critical Section에 들어가고자 하는 프로세스를 들어가게 해주어야 한다.

  - Bounded Waiting

    한 프로세스가 Critical Section에 들어가려고 요청한 후 부터 그 요청이 허용될 때까지 다른 프로세스들이 Critical Section에 들어가는 횟수에 한계가 있어야 한다.

- Critical Section 문제를 해결하는 알고리즘
  - Algorithm 1 : Synchronization Variable인 turn을 사용한다.

        //Process P_0
        do{
        	while(turn!=0);
        	critical section
        	turn=1;
        	remainder section
        	}while(1);

    프로세스 P0가 turn변수가 0이 아니게 될 때 까지 대기한다. 다른 프로세스에서 turn변수를 0으로 바꾸어 주면 critical section에서 일을 처리하게 되고 다시 turn변수를 1로 바꾸어 다른 프로세스가 일을 하게 한다.

    ✔︎ 이 방법은 조건 2번인 progress를 만족시키지 못한다. 둘 중 한 프로세스가 다른 프로세스에 비해 critical section을 자주 수행해야 한다면 좋은 방법이 아니게 된다.

  - Algorithm 2 : 각 프로세스마다 고유한 flag 변수를 이용한다.

        //Process P_i
        do{
        	flag[i]=true;
        	while(flag[j]);
        	critical section
        	flag[i]=false;
        	remainder section
        }while(1);

    프로세스는 flag를 true로 바꾼다. 다른 프로세스의 플래그를 체크하고 true값이 없다면 critical section의 일을 처리한다.

    ✔︎ 이 방법도 2번 progress를 만족시키지 못한다. 작업중에 CPU preempt를 당하면 true인 flag가 두개가 되어 아무도 critical section에 못가게 될 것이기 때문.

  - Algorithm 3 : Peterson's Algorithm. 위 두가지 방식을 합쳐놓은 방식이다.

        //Process P_i
        do{
        	flag[i]=true;
        	turn=j;
        	while(flag[j]&&turn==j);
        	critical section
        	flag[i]=false;
        	remainder section
        }while(1);


    두 프로세스가 동시에 접근하려 할 때, trun 값으로 critical section에 접근한다.

    ✔︎ 이 알고리즘은 Busy Waiting(Spin lock)의 문제가 있다. CPU와 메모리를 사용하면서 대기하기 때문에 비효율적인 방법이다.

- 하드웨어 적인 해결 방법. Synchronization Hardware
  - test & modify
- Semaphores

  앞의 알고리즘들을 추상화 시킨다. 두가지 타입으로 나누는데

  - counting semaphore

    rescue counting에 사용한다.

  - binary semaphore

    0과 1의 값을 갖는다.

  semaphore은 두가지의 atomic 연산에 의해서만 접근이 가능하도록 한다.

  - P(s)

        P(s)
        	while(s<=0) do no-op;
        	s--;

    자원을 할당받지 못하면 while문에서 루프를 돌며 기다린다. lock

  - V(s)

    자원을 반납한다. unlock

        V(s)
        	s++;

  이 방법도 busy waiting은 발생한다. 그래서 해결책으로 **Block & Wakeup**방식을 도입한다.

- Block & Wakeup

  자원이 없을 때 루프를 돌며 기다리는 것이 아니라 프로세스가 sleep되는 것과 비슷하게 blocked상태에서 기다리게 된다.

      P(s):
      s.value--;
      if(s.value<=0){
      	add this process s.L;
      	block();
      }

      V(s):
      s.Value ++;
      if(s.value<=0){
      	remove a process P from s.L;
      	wakeup(P);
      }

  이 방식은 Overhead가 발생한다. critical section이 짧은 경우에는 busy waiting(spin lock) 방식을 이용해도 되지만 긴 경우에는 block & wakeup방식이 적절하다.
