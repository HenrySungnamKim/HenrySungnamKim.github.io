---
layout: post
title:  "Process Management, 운영체제 강의 정리 4강"
date:   2018-08-07
excerpt: "Process의 생성과 종료 "
tag:
- OS
- 운영체제
- 강의 정리
- Process
comments: true
---
* auto-gen TOC:
{:toc}

# 4강. Process Management

강의 링크 : [https://core.ewha.ac.kr/publicview/C0101020140321144554159683?vmode=f](https://core.ewha.ac.kr/publicview/C0101020140321144554159683?vmode=f)

# Process Management

## 프로세스 생성, Process Creation

- 부모 프로세스가 자식 프로세스를 생성하여 트리구조를 형성한다.
- 프로세스는 자원을 필요로 한다.

  운영체제에게 자원을  받는다.

  부모와 자원을 공유한다.

- 자원의 공유
  1. 부모와 자식이 모든 자원을 공유하는 모델
  2. 일부를 공유하는 모델
  3. 전혀 공유하지 않는 모델
- 수행 (Execution)

  부모와 자식은 공존하며 수행되는 모델

  자식이 종료(termiate)될 때까지 부모가 기다리는(wait) 모델

- 주소 공간(Address space)

  자식 프로세스는 부모 프로세스의 공간(Process Context)을 복사한다. (binary and OS data)

  자식은 그 공간에 새로운 프로그램을 올린다.

- 유닉스의 예

  fork() 시스템 콜이 새로운 프로세스를 생성한다.

  - 부모를 그대로 복사한다. (OS data except PID and binary)
  - 주소 공간 할당

  fork 다음에 이어지는 exec() 시스템 콜을 통해 새로운 프로그램을 메모리에 올린다.

프로세스의 생성은 시스템 콜을 통해 생성되므로 운영체제에 의해서만 생성 가능하다.

## fork()

fork 명령어를 통해서 자식 프로세스를 생성하기 의한 시스템 콜 요청을 한다.

운영체제가 이를 받아들여 자식 프로세스를 생성하면 부모 프로세스와 같은 코드가 생성된다. 'fork가 부모 프로세스에 포함되어 있어서 무한히 자식 프로세스를 생성하게 되는 것이 아닌가?' 하고 생각할 수 있지만 부모 프로세스로 부터 PC(프로그램 카운터)를 복사해 왔기 때문에 fork를 완료한 시점부터 자식 프로세스는 프로세스를 수행한다.

프로세스는 ProcessID값인 PID가 있어서 프로세스 별로 구분한다. 부모는 PID값이 양수, 자식 프로세스는 0이다.

## exec()
```c++
    int main(){
    int pid = fork();
    if (pid==0){//자식 프로세스라면,
    	printf("\n Hello, I am child. Now I'll run date \n");
    	execIp("/bin/date","/bin/date",(char*)0);
    }
    else if(pid>0)//부모 프로세스라면,
    	printf("\n Hello, I am parent\n");
    }
```
fork()이후에 자식 프로세스가 생성 되었다. exec()를 보면 내부에 있는 위치에 존재하는 프로그램(/bin/date)으로 자식 프로세스를 덮어쓰게 된다.

## wait()

프로세스 A가 wait() 시스템 콜을 요청하면, 프로세스 A의 자식 프로세스가 종료 될 때까지 프로세스 A를 Sleep시킨다. (blocked)

자식 프로세스가 종료되면 커널은 Sleep중이던 프로세스 A를 깨운다. (ready)

## exit()

프로세스를 종료시키는 시스템 콜이다. C언어 상에서 사용자가 입력하지 않아도 중괄호를 닫을 때 컴파일러가 자동으로 삽입한다.

## 프로세스 종료, Process Termination

- exit() 자발적 종료. 프로세스가 마지막 명령을 수행하고 운영체제에게 이를 알려준다.

  자식이 부모에게 Output data를 보냄.

  프로세스의 각종 자원들이 운영체제에게 반납된다.

- abort() 강제적 종료. 부모 프로세스가 자식의 수행을 종료시킨다.

  자식이 할당 자원의 한계치를 넘어설 때

  자식에게 할당된 태스크가 더 이상 필요하지 않을 때

  부모가 종료(exit)되는 경우

  - 운영체제는 부모 프로세스가 종료되는 경우, 자식이 더 이상 수행되도록 두지 않는다.
  - 단계적인 종료

  외부에서 사람이 키보드로 kill, break를 입력해서 종료시키는 경우

## 프로세스 간 협력

- 독립적 프로세스 (Independent Process)

  프로세스는 각자의 주소공간을 갖고 수행되므로 원칙적으로 다른 프로세스에게 영향을 끼치지 못한다.

- 협력 프로세스 (Cooperating Process)

  경우에 따라 협력을 해야만 효율이 높아지는 경우가 생긴다. 프로세스 협력 매커니즘(IPC)을 통해 하나의 프로세스가 다른 프로세스의 수행에 영향을 끼친다.

- 프로세스 협력 매커니즘 (IPC : InterProcessCommunication)

  메시지를 전달하는 방법

  - message passing : 커널을 통해 메시지 전달

  주소 공간을 공유하는 방법

  - shared memory : 서로 다른 프로세스 간에도 일부 주소 공간을 공유하게 한다.
  - thread : thread는 사실상 하나의 프로세스 이기 때문에 프로세스간 협력으로 보기 어렵지만 동일한 프로세스를 구성하는 thread간에는 주소공간을 공유하므로 협력이 가능하다.
