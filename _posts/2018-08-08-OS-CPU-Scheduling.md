---
layout: post
title:  "CPU Scheduling, 운영체제 강의 정리 5강"
date:   2018-08-08
excerpt: "CPU 스케쥴링"
tag:
- OS
- 운영체제
- 강의 정리
- CPU Scheduling
comments: true
---
* auto-gen TOC:
{:toc}

# 5강. CPU Scheduling

강의 링크 : [https://core.ewha.ac.kr/publicview/C0101020140328151311578473?vmode=f](https://core.ewha.ac.kr/publicview/C0101020140328151311578473?vmode=f)

# CPU 스케쥴링은 무엇인가?

CPU를 얻고자 ready queue에서 기다리는 프로세스들에게 효율성 있고 공평하게 CPU를 할당하는 방법. CPU가 일을 수행하는 시간을 CPU burst time, I/O 요청 응답을 기다리는 시간을 I/O burst time이라고 한다.

## CPU 스케쥴링이 필요한 이유

I/O 작업이 빈번한 작업(I/O bounded job), 그렇지 않은 작업(CPU bounded job)이 있다. 공평하고 효율성 있는 작업도 중요하지만 사용자와 직접 상호작용 하는 I/O bounded job을 적절하게 잘 처리해 주는 것이 중요하다.

## CPU 스케쥴링의 분류

- Preemtive(선점형)
  - time interrupt : 시간을 할당하고 만료되면 CPU를 강제로 빼앗고 다른 프로세스에 넘긴다.
  - device controller의 I/O 완료 interrupt : I/O 작업을 마치면 해당 프로세스를 ready queue에 대기시켜 준다.
- NonPreemtive(비 선점형)
  - I/O interrupt : I/O 요청이 들어오면 프로세스에게 CPU를 넘겨준다.
  - Terminate : 프로세스가 일을 모두 마치면 다른 프로세스에게 CPU를 넘겨준다.

## CPU 스케쥴링의 성능척도 Criteria

- CPU utilization (이용도) : CPU가 놀지 않고 일한 시간의 비율
- Throughput (처리량) : 얼마나 많은 일을 했는지
- Turnaround time (반환 시간) : CPU를 쓰러 queue에 들어와서 쓰고 나갈 때 까지 걸린 시간
- Waiting time (대기 시간) : ready queue에서 기다린 시간
- Response time (응답 시간) : queue에 들어와서 처음으로 CPU를 얻을 때 까지 걸린 시간

## 어떤 스케쥴링이 좋을까?

판단하는 기준이 여러가지 있다. 위의 sheduling criteria로 판단하는 방법, 시스템 입장의 성능 척도, 프로세스 입장에서의 성능 척도.

시스템의 입장에서는 CPU가 놀지 않고 최대한 많이 일을 하면 CPU 스케쥴링 성능이 높을 것이고, 프로세스 입장에서는 '자신에게 얼마나 CPU를 할당 해 주는가' 가 CPU 스케쥴링 성능이 높을 것이다.

## Scheduling Algorithms (Single Core)

- FCFS (First Come First Served)

  선착순 처리. 비 선점형이다. CPU를 길게 쓰는 프로세스가 있다면 효율이 아주 나빠진다. (Convoy effect)

- SJF (Shortest Job First)

  burst time이 짧은 프로세스 부터 우선 처리한다.

- SRTF(Shortest Remaining Time First), SJF 보완

  SJF는 시간 소요가 짧은 작업을 먼저 처리한다. 기본적으로 비 선점형 방식이다. 이 점에서 발생되는 문제를 해결하기 이해 preemtive속성을 추가했는데, 현재 수행 중인 프로세스의 남은 burst time보다 짧은 프로세스가 도착하면 현재 수행중인 프로세스에게서 CPU를 빼앗아 주게 된다. 이렇게 되면 전체적으로 프로세스 수행 시간이 짧아지게 된다. 이 특성이 추가된 SJF를 SRTF(Shortest Remaining Time First)라고 한다. SRTF는 프로세스들에게 평균 최소 대기시간을 보장한다는 점에서  Optimal(적절한)하다. 하지만 새 프로세스가 도착할 때 마다 스케쥴링을 새로 해주어야한다.

  - 문제점. Starvation

    효율성을 추구하는 점에서 바람직한 방법이지만 특정 프로세스가 지나치게 차별받는 문제가 존재한다. 스케쥴링을 계속하므로  cpu burst time을 측정할 수 없다.

- Priority Scheduling

  프로세스마다 우선순위를 정하고 우선순위가 높은 프로세스에게 CPU를 선 할당하는 방식. 더 높은 우선순위의 프로세스가 들어오면 뺏아서 넘기므로 Preemtive하다. SJF 또한 CPU burst time을 기준으로 잡은 우선순위 스케쥴링이라 볼 수 있다.

  - 문제점. starvation

    해결하기 위해서 aging기법을 도입한다. as time progress increase the priority of the process로 우선순위가 낮은 프로세스라도 기다릴 수록 우선순위를 높여주는 방식이다.

- RR (Round Robin)

  현대적인 CPU 스케쥴링 방식. 각 프로세스는 동일한 크기의 할당 시간을 갖는다. 할당 시간이 끝나면 프로세스는 CPU를 반환하고 readyqueue의 맨 뒷 줄로 가서 대기한다.

  - 장점 : Response time이 짧아진다. 사용자에게 매우 빠른 것처럼 느껴진다. 공정한 스케쥴링.
  - 문제점 : 하지만 할당시간이 너무 커지게 되면 FCFS와 같은 방식으로 처리되어 문제가 발생한다. 할당시간이 너무 짧아지면 정말 공정하겠지만 Context Switch로 Overhead가 발생한다.

  라운드 로빈이 가능한 이유는 프로세스마다 Context가 있고 그 상태를 저장할 수 있기 때문이다. 하지만 진행상황을 모두 저장하고 다른 프로세스로 넘긴다는 것은 상당이 overhead가 큰 작업이다. CPU burst time이 랜덤한 프로세스가 많을 때 좋은 방법이다.

- Multi Level Queue

  Ready queue를 여러 개로 분할한다. 프로세스는 어느 queue에 가서 대기해야 할까? 각 queue는 내부에 독립적인 스케쥴링 알고리즘을 갖는다. 사용자와 interactive한 작업은 우선순위를 높게, 그렇지 않다면 낮게 설정한다. 우선순위가 고정된 Fixed Priority Scheduling방법이다.

  1단계에서 대기 상태인 프로세스가 있다면 1단계에서 대기하고 있는 프로세스에게 CPU를 할당해 주고 1단계에 아무 프로세스가 없어야 2단계에서 대기하는 프로세스에게 CPU를 할당한다.

  - 문제점. Starvation

    해결책으로 Time Slice를 사용한다. 각 queue에 적절한 비율로 CPU 시간을 할당하는 방법이다.

- Multi Level Feedback Queue

  프로세스가 다른 queue로 이동이 가능해졌다. 처음 들어온 프로세스는 우선순위가 가장 높은 queue에 보낸다. 각 queue마다 할당 시간(quantum)이 있는데, 하위 queue로 내려갈 수록 값이 커진다. 각 단계에서 수행을 못하면 할당시간이 긴 queue로 이동하게 된다. CPU burst time이 짧은 프로세스를 먼저 완료하는데 집중한 알고리즘이다.

## Scheduling Algorithms (Multi Core)

- Homogeneous Processor Scheduling

  queue에 한 줄로 세워서 프로세스가 알아서 꺼내가게 한다. 반드시 특정 프로세서에서 수행되어야 하는 경우는 문제가 복잡하다.

- Load Sharing

  일부 프로세서에 job이 몰리지 않도록 적절히 부하를 공유하는 매커니즘이다. 별개의 queue를 두는 방법이 있고 공동 queue를 사용하는 방법이 있다.

- Symmetric Multi Processor

  각 프로세서가 각자 알아서 스케쥴링을 결정한다. 모든 프로세서가 동등한 위치에 존재한다.

- Asymmetric Multi Processor

  하나의 프로세서가 시스템 데이터의 접근과 공유를 책임지고 나머지는 따르는 방식

## Real Time Scheduling

- Hard Real Time Sharing

  정해진 시간안에 반드시 수행이 완료되도록 하는 스케쥴링 방식이다.

- Soft Real Time Sharing

  일반 프로세스에 비해 높은 Priority를 갖게 해서 알고리즘을 수행한다.

## Thread Scheduling

- Local Scheduling

  User Level Thread의 경우, 사용자 수준의 Thread Library에 의해 어떤 Thread를 스케쥴 할지 결정한다.

- Global Scheduling

  Kernel Level Thread의 경우, 일반 프로세스와 마찬가지로 커널의 단기 스케쥴러가 어떤 Thread를 스케쥴 할 지 결정

## Algorithm Evaluation

- Queueing Models

  확률 분포로 Process Arrival Rate, Service Rate등을 통해 각종 Performance Index값을 계산한다.

- Implementation & Measurement

  실제 시스템에 알고리즘을 구현해서 실제 작업에 대해 성능을 측정하고 비교한다.

- Simulation

  알고리즘을 모의 프로그램을 작성하고 trace(input data)를 입력해서 비교한다.
