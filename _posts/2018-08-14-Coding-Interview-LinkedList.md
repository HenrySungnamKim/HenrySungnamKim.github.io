---
layout: post
title:  "Coding Interview 4.Linked List"
date:   2018-08-14
excerpt: "코딩인터뷰 4장 연결 리스트"
tag:
- 코딩 인터뷰
- 연결 리스트
- Linked List
comments: true
---
* auto-gen TOC:
{:toc}

# 연결 리스트 정리

> 도서 코딩 인터뷰 퀘스천을 읽고 정리한 내용입니다.

## 연결 리스트란 무엇인가?

데이터 집합을 저장할 때 사용되는 자료 구조.

- 연속된 요소들은 포인터로 연결된다.
- 마지막 요소는 Null을 가리킨다.
- 프로그램 실행 중에 크기가 줄거나 늘 수 있다.
- 필요한 만큼 만들 수 있다.
- 메모리 공간을 낭비하지 않는다.

## 연결 리스트의 추상 함수

- Insert : 리스트에 요소 추가
- Delete : 지정된 위치의 요소 제거
- Delete List : 모든 요소 제거
- Count : 리스트 내 모든 요소의 수를 반환

## 연결 리스트와 배열의 차이점

둘 다 데이터의 집합을 저장하는데 사용된다.

- 배열
  - 배열의 요소에 접근하는데 일정한 시간이 필요하다

    배열은 전체 요소들을 유지하기 위해 하나의 메모리 블록이 할당 된다. 요소들에 대한 접근은 인덱스를 사용해서 고정된 시간 내에 할 수 있다. 여기서 접근에 고정된 시간이 걸리는 이유는 배열의 시작에서 요소까지 거리를 구하기 위해 곱셈 연산이 사용된다. 요소의 데이터 형의 크기를 구한 후 요소의 인덱스를 곱하여 결과를 배열의 시작 주소에 더해 해당 요소의 주소를 찾아낸다. 곱셈과 덧셈이 한번씩 수행되는데 이 때문에 고정된 시간이 걸리게 된다.

  - 장점

    간단하고 사용하기 쉽다. 요소에 접근하는 시간이 빠르다.

  - 단점

    크기가 고정되어 있다. 한 블록에 한 할당을 가져서 시작에 배열을 할당할 경우, 종종 전체 배열에 대한 메모리를 얻을 수 없는 경우가 생김

    위치 지정 삽입이 복잡하다. 요소를 끼워 넣으려면 기존의 요소를 밀어야 한다. 원하는 위치가 앞 쪽 일수록 기존에 존재하는 요소들을 많이 밀어야 하므로 비용이 커진다.

  - 동적 배열(Dynamic Arrays, Growable Array, Resizable Array, Dynamic Table Array List)

    동적 배열은 배열의 요소를 추가하거나 삭제할 수 있는 임의 접근, 가변 길이 리스트 데이터 구조이다.

    동적 배열을 구현 하려면 처음에 고정된 크기의 배열에서 시작하고 배열이 다 차면 원래 배열보다 두 배 크기의 새로운 배열을 만들고, 배열의 요소들의 수가 배열 크기의 절반보다 작으면 배열의 크기를 절반으로 줄인다.

- 연결 리스트
  - 장점

    연결 리스트는 일정 시간 내에 확장될 수 있다. 배열은 시간이 많이 소요된다.

  - 단점

    **접근 시간**

    배열은 임의 접근 방식이어서 어떤 요소에 접근해도 O(1)의 time complexity를 갖는다. 연결 리스트는 최악의 경우 O(n)의 time complexity를 갖는다. 배열은 요소들이 연속된 메모리 위치에 있기 때문에 중앙처리장치 캐싱 방법에 장점이 된다.

    **데이터 정렬/탐색의 오버헤드(추가 처리 요구 시간)**

    연결 리스트는 마지막 요소를 삭제할 때 마지막 이전 요소가 null을 참조하도록 변경해야 한다. 이런 추가적인 참조 포인터가 있다는 면에서 그만큼 메모리를 낭비하게 된다.

## 단일 연결 리스트. Single Linked List

일반적으로 연결 리스트는 단일 연결 리스트를 말한다. 다음 요소를 가리키는 포인터를 가진 노드들로 구성된다.

- 리스트의 기본 동작

  리스트 탐색

  리스트 내 아이템 삽입

  리스트에서 아이템을 삭제

- 연결 리스트 탐색

  Head 포인터가 리스트의 첫 번째 노드를 가리킨다고 할 때, 다음을 수행한다.

  포인터를 따라간다. 탐색된 노드의 값을 표시한다. 다음 포인터가 null일 때 탐색을 종료한다.

- 단일 연결 리스트의 노드 삽입 3가지 경우

  **head 앞에 새로운 노드 삽입**

  하나의 next 포인터만 변경된다.

  - 새로운 노드의 next 포인터가 현재 head를 가리키도록 한다.
  - head 포인터가 새로운 노드를 가리키도록 변경한다.

  **마지막 위치에 새로운 노드 삽입**

  두개의 next 포인터가 변경된다.

  - 새로운 노드의 next 포인터가 null을 가리킨다.
  - 마지막 노드의 next 포인터가 새로운 노드를 가리킨다.

  **리스트의 임의 위치에 새로운 노드 삽입**

  두개의 next 포인터가 변경된다.

  - 만약 세 번째 위치에 노드가 추가되는 경우, 두 번째 노드에서 작업이 시작된다. 두 번째 노드를 찾고 새로운 노드를 삽입한다. 추가할 노드의 next 포인터는 두 번째 노드의 다음 노드를 가리킨다.
  - 두 번째 노드의 next 포인터가 이제 새로 추가된 노드를 가리킨다.

- 단일 연결 리스트의 노드 삭제 3가지 경우

  **첫 번째 노드 삭제**

  - head가 가리키는 첫 번째 노드를 가리키도록 새로운 임시 노드를 생성한다.
  - head가 다음 노드를 가리키도록 포인트를 변경하고 임시 노드를 제거한다.

  **마지막 노드 삭제**

  - 리스트를 탐색하면서 이전 노드의 주소를 함께 보관한다. 리스트의 마지막에 이를 때 까지 마지막 노드와 이전 노드를 가리키는 두개의 포인터를 갖는다.
  - 이전 노드의 next 포인터를 null로 변경한다.
  - 마지막 노드를 제거한다.

  **임의 노드 삭제**

  - 리스트를 탐색하면서 이전 노드도 함께 보관한다. 이전 노드의 next 포인터를 삭제될 노드의 next포인터로 변경한다.
  - 노드를 삭제한다.

- 단방향 연결 리스트 삭제하기

  임시 변수에 현재 노드를 저장하고 현재 노드를 삭제한다.

  임시 변수를 가지고 다음 노드로 간다.

  모든 노드에 대해 이 절차를 반복한다.

## 이중 연결 리스트. Doubly Linked List

리스트 내의 주어진 노드를 양방향으로 탐색할 수 있다. 이중 연결 리스트는 단방향 연결 리스트와 달리 이전 노드의 주소를 몰라도 삭제할 수 있다. 각 노드에 확장 포인터가 필요해서 그만큼 공간이 더 요구된다. 노드를 삽입하거나 삭제하려면 연산 과정이 더 필요하다.

- 이중 연결 리스트의 노드 삽입

  **처음에 삽입하기**

  - 새로운 노드의 next 포인터가 현재의 head를 가리키도록 하고 이전 노드를 가리키는 포인터를 null로 만든다.
  - head 노드의 왼쪽 포인터가 새로운 노드를 가리키도록 해 새로운 노드를 head로 만든다.

  **끝에 삽입하기**

  리스트를 끝까지 탐색한다.

  - 새로운 노드의 오른쪽 포인터를 null, 왼쪽 포인터가 리스트의 마지막 노드를 가리키도록 한다.
  - 마지막 노드의 오른쪽 포인터가 새로운 노드를 가리키게 한다.

  **임의 위치에 삽입하기**

  삽입될 위치까지 리스트를 탐색한다.

  - 새로운 노드의 오른쪽 포인터가 새로운 노드를 삽입하고자 하는 위치의 노드의 다음 노드를 가리키도록 한다. 새로운 노드의 왼쪽 포인터가 삽입하려는 위치의 노드를 가리키도록 한다.
  - 삽입할 위치의 노드의 오른쪽 포인터가 새로운 노드를 가리키게 하고 다음 노드의 왼쪽 포인터가 새로운 노드를 가리키도록 한다.

- 이중 연결 리스트 노드 삭제

  **첫번째 노드 삭제하기**

  - head가 가리키는 동일한 노드를 가리키는 임시 노드 생성
  - head 포인터를 다음 노드로 옮긴다. head 왼쪽 포인터를 null로 설정. 임시 노드를 제거한다.

  **마지막 노드 삭제하기**

  리스트 탐색, 마지막 이전 노드 정보를 보관.

  - 마지막 이전 노드의 next를 null로 변경
  - 마지막 노드 제거

  **중간 노드 삭제하기**

  리스트 탐색, 삭제 위치 이전 노드 정보를 보관

  - 이전 노드의 next 포인터를 삭제될 노드의 다음 노드를 가리키도록 한다.
  - 현재 노드 삭제

## 환형(환상) 연결 리스트

환형 연결 리스트는 앞의 두 연결 리스트와 달리 마지막이 없다. 그래서 탐색할 때 무한히 반복되지 않도록 주의해야한다. 각 노드는 successor를 갖고 있다. 여러 프로세스들이 같은 시간에 동일한 자원을 사용하고 이전의 다른 모든 프로세스들이 수행하기 전에 어떤 프로세스도 접근하면 안될 경우(RR 알고리즘)과 같은 상황에서 유용하다. 환형 리스트는 컴퓨터가 컴퓨팅 자원을 관리하는데 사용된다. 스택, 큐를 구현하는데 사용할 수 있다.

- 환형 연결 리스트의 노드 수 구하기

  노드 수를 계산하려면 current라는 더미를 가지고 head 노드에서 시작하여 current가 head에 다다를 때 까지 노드의 수를 계산한다.

- 환형 연결 리스트의 내용 출력하기

  head에서 시작한다면, 다시 head를 만날 때 까지 노드의 내용을 출력하고 다음 노드로 이동하기를 반복한다.

- 환형 연결 리스트에 노드 삽입하기

  **head 앞에 삽입하기**

  - next 포인터가 자기 자신을 가리키도록 초기화 한 노드를 생성
  - 새로운 노드의 next 포인터를 head를 가리키도록 하고 tail까지 리스트를 탐색
  - head 이전 노드가 새로운 노드를 가리키도록 갱신한다.

  **마지막에 삽입하기**

  - next 포인터가 자기 자신을 가리키도록 초기화 한 노드를 생성
  - 새로운 노드의 next 포인터를 head를 가리키도록 하고 tail까지 리스트를 탐색
  - 마지막 이전 노드의 next 포인터가 새로운 노드를 가리키도록 한다.

- 환형 연결 리스트 노드 삭제하기

  **첫 번째 노드 삭제하기**

  리스트를 탐색해서 tail노드를 찾는다.

  - head를 가리키는 임시 변수를 생성. tail 노드의 next 포인터를 head의 다음 노드를 가리키도록 한다.
  - head 포인터를 다음 노드로 변경. 첫 번째 노드 삭제

  **마지막 노드 삭제하기**

  리스트를 탐색해서 tail, tail이전 노드를 찾는다.

  - tail 이전 노드의 next 포인터가 head를 가리키도록 한다.
  - tail 노드를 제거한다.

## 메모리 최적화(Memory-Efficient) 이중 연결 리스트

이 구현 방법은 pointer difference에 기반하며 각 노드는 리스트의 순방향, 역방향 탐색에 하나의 포인터 필드만을 사용한다.
