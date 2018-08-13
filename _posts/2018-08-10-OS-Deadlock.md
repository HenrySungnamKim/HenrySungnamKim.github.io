---
layout: post
title:  "Deadlock, 운영체제 강의 정리 6강"
date:   2018-08-10
excerpt: "Deadlock의 발생과 처리"
tag:
- OS
- 운영체제
- 강의 정리
- Deadlock
comments: true
---
* auto-gen TOC:
{:toc}

# 교착상태 , Deadlock

강의 링크 : [https://core.ewha.ac.kr/publicview/C0101020140411151510275738?vmode=f](https://core.ewha.ac.kr/publicview/C0101020140411151510275738?vmode=f)

## Deadlock Porblem

일련의 프로세스들이 서로가 가진 자원을 기다리며 blocked인 상태

- Resource의 개념

  하드웨어와 소프트웨어를 포괄하는 개념이다. I/O space, CPU cycle, Memory Space, Semaphore등이 있다. 프로세스가 Resource를 사용하는 절차는

  - Request 자원 요청
  - Allocate 자원 할당
  - Use 자원 사용
  - Release 자원 반납
- Deadlock이란.

  P1(a),P2(b)인 상태, 프로세스 P1, P2가 각각 자원 a, b를 할당 받았을 때 P1(b), P2(a)를 요청하는 상태이다.

## Deadlock발생의 4가지 조건

- Mutual Exclusion 상호배제

  매 순간 하나의 프로세스만이 자원을 사용한다.

- No preemption 비선점

  프로세스는 자원을 스스로 내어놓을 뿐 강제로 빼앗기지 않는다.

- Hold and wait 보유대기

  자원을 가진 프로세스가 다른 자원을 기다릴 때 보유자원을 놓지 않고 계속 가지고 있는다.

- Circular wait 순환대기

  자원을 기다리는 프로세스 간에 사이클이 형성되어야 한다.

## Deadlock의 처리 방법

Prevention과 Avoidance는 예방하는 방법. Detection and Recovery와 Ignorance는 그대로 두는 방법이다.

- Deadlock Prevention

  자원 할당 시 Deadlock의 4가지 발생 조건 중 하나가 만족되지 않도록 하는 방법.

  - Mutual Exclusion, 상호배제를 차단하면

    자원을 공유하는 방법. 공유해선 안되는 자원에는 사용 불가능하다.

  - No Preemption, 비선점을 차단하면

    프로세스가 자원을 요청해야 하는 경우, 이미 보유하고 있던 자원이 선점된다. 모든 필요한 자원을 얻을 수 있을 때, 프로세스가 다시 시작된다. CPU나 메모리처럼 프로세스의 State를 쉽게 저장하고 복구할 수 있는 자원에서만 사용 가능한 방법

  - Hold and Wait, 보유대기를 차단하면

    자원을 요청할 때 다른 어떠한 자원도 갖고 있지 않게 만든다.

    방법 1 : 프로세스 시작 시 모든 필요한 자원을 할당.

    방법 2: 다른 자원이 필요한 경우 보유하고 있던 모든 자원을 반환 시킨다.

  - Circular wait, 순환대기를 차단하면

    모든 자원 유형에 할당 순서를 정해서 정해진 순서대로 자원을 할당한다. ex)3번 자원을 을 얻기 위해서는 1,2를 보유한 상태여야 한다.

- Deadlock Avoidance

  자원 요청에 대한 부가적인 정보를 이용해서 자원 할당이 deadlock에서 안전한 지를 동적으로 조사해서 **안전한 경우**에만 할당한다.

  안전한 경우란, Safe State; 시스템 state가 원래의 state로 돌아올 수 있는 sequence가 존재하는 경우를 말한다.

  가장 단순하고 일반적인 모델은 프로세스 별로 필요로 하는 자원 별 최대 사용량을 미리 선언하도록 하는 방법이 있다.

  **avoidance 알고리즘 2가지**

  - Single instance per resource types

    ✔︎ Resource Allocation Graph Algorithm 사용

<img width="443" alt="screenshot2018-08-10at10-bf9f6cd5-0b06-4cde-b5da-8277f3c80f09 34 39am" src="https://user-images.githubusercontent.com/37807838/44011136-c3b2208a-9ef1-11e8-9801-1ea9890621b4.png">


    이 방법은 Claim edge를 이용한다. 점선은 프로세스가 자원을 미래에 요청할 수 있음을 의미한다. 실제로 자원을 요청하면 점선은 실선으로 바뀐다. 실선으로 cycle이 생기지 않는 경우에만 요청한 자원을 할당한다.

    cycle 생성 여부 조사 시 프로세스 n개가 있을 때, O(n^2)의 시간을 소요한다.

  - Multiple instance per resource types

    ✔︎ Banker's Algorithm 사용

    프로세스가 평생 사용할 자원들을 미리 선언한다. 현재 할당된 자원의 양(Allocation), 최대로 할당 가능한 양(Max)을 계산하고 두 데이터를  통해 추가로 할당해 줄 수 있는 자원의 양(Need)을 계산한다.

    `Need = Max - Allocation`

    현재 가용할 수 있는 자원의 양(Available) 데이터를 이용해서  Need > Available 이면 할당해 주지 않는다. Need에 있는 양을 최악의 상황으로 생각해서 추가 요청한 정도가 Need보다 낮아도 할당해 주지 않는다. 낮은 값을 먼저 요청하면 나중에 또 요청할 수 있고 Deadlock이 발생할 수 있기 때문이다. 이때는 요청할 자원의 양이 커져서 Need의 값이 될 때까지 대기해야 한다.

- Deadlock Detection and Recovery

  데드락을 탐색하고 그 상황을 복구하는 방법이다.

  - Process Termination

    프로세스 종료

  - Resource Preemption

    비용을 최소화 할 victim process를 선정하고 자원을 뺏어서 데드락을 해제한다.

    Starvation 문제 발생. victim 프로세스가 극단적으로 한 프로세스에만 해당 된다면, 그 프로세스는 계속해서 자원을 빼앗기게 된다.

- Deadlock Ignorance

  무시하는 방법. 현재 시스템에서 채택. Deadlock은 잘 발생하지 않기 때문에 방지하는데 오버헤드를 들이지 않는 것이 효율적
