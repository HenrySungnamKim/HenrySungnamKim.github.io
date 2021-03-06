---
layout: post
title:  "Coding Interview 1.Programming Basic Concept"
date:   2018-08-08
excerpt: "코딩인터뷰 1장 프로그래밍 기본 개념"
tag:
- 코딩 인터뷰
- 프로그래밍 기본
- 내용 정리
comments: true
---
* auto-gen TOC:
{:toc}

# 프로그래밍 기초 개념

> 도서 코딩 인터뷰 퀘스천을 읽고 정리한 내용입니다.

## 자료형

프로그래밍언어의 자료형은 미리 특징 지어진 값들을 가지는 데이터의 집합이다. 컴퓨터의 메모리는 모두 0, 1로 채워진다. 프로그래밍을 할 때 0과 1로 직접 쳐서 작성하기는 어렵기 때문에 언어와 컴파일러는 자료형을 제공한다.

- 시스템 정의 자료형(원시자료형, Primitive data types)

  시스템에 의해 정의된 자료형으로 int, float, char double, boolean등이 있다. 원시자료형에 할당되는 바이트 수는 프로그래밍 언어와 컴파일러, 운영체제에 달려 있다. 원시 자료형은 사칙연산과 같은 기본 연산을 제공하고 시스템은 연산의 수행을 제공한다.

- 사용자 정의 자료형

  예시로 C++ 구조체, Java의 클래스등이 있다. 시스템 정의 자료형을 조합하여 새로운 자료형을 정의하는 것을 말한다.

## 자료 구조. Data Stucture

자료구조는 컴퓨터 내에서 데이터를 저장하고 구조화하는 특정한 방법이다. 일반적인 유형으로 배열, 연결 리스트, 스택, 트리, 그래프 등이 있다.

구성요소를 어떻게 구조화하였는가에 따라 두가지 유형으로 분류된다.

- 선형 자료구조(Linear Data Structure) : 구성 요소들은 순차적으로 접근 되지만, 모든 요소들이 반드시 순차적으로 저장되지는 않는다. (연결 리스트, 스택, 큐)
- 비선형 자료구조 : 구성 요소들이 비순차적인 순서로 저장되고 접근된다. (트리, 그래프)

## 추상 자료형. Abstract Data Types

일반적으로 자료 구조와 연산을 결합해서 추상 자료형 이라고 한다. 일반적으로 사용되는 추상 자료형은 연결 리스트, 스택, 큐, 우선 순위 큐, 이진 트리, 딕셔너리, 분리 집합(Union, Find), 해쉬 테이블, 그래프 등이 있다. 추상 자료형은 두 부분으로 구성된다.

- 데이터 선언

- 연산자 선언

추상 자료형을 정의하면서 구현의 세부정보에 대해서는 신경쓰지 않는다. 이는 우리가 필요할 때만 구체화 된다. 추상 자료형들은 각각의 특징에 맞는 용도를 가지고 있고 몇몇은 전문적으로 고도화 되어있다.

## 메모리와 변수

컴퓨터에 메모리가 구성되는 방식에 대해 알아보자. 메모리는 바이트의 배열처럼 다룰 수 있다. 각각의 위치는 주소로 식별한다. 보통 0번은 유효한 메모리 주소가 아니다. 이 주소는 Integer라는 것을 알아야 한다. 아래 주소의 n값은 시스템 메인 메모리의 크기에 달려있다.

<img width="240" alt="screen shot 2018-08-08 at 1 54 09 pm" src="https://user-images.githubusercontent.com/37807838/43817093-c7f77952-9b12-11e8-9750-3d09173f7526.png">

어떤 위치(주소)를 읽거나 쓰기 위해 CPU는 메인 메모리 컨트롤러에 그 위치를 전달하여 접근한다. 변수를 생성할 때 컴파일러는 연속된 메모리 블록을 할당하고 이 크기는 변수의 크기에 달려있다. 컴파일러는 또한 변수 이름 x와 할당된 첫번째 바이트 주소를 연결짓는 내부적인 태그(Symbol table)를 유지한다.

- SizeOf 연산자는 변수의 크기를 알아내는데 사용된다. 예를 들어 Sizeof(x)가 4를 반환한다면 integer는 메모리에서 4개의 연속된 byte가 필요하다는 것이다. x의 주소가 2000이라고 한다면 실제 x에 의해 사용되는 메모리 주소는 2000, 2001, 2002, 2003이 되는 것이다.
- Address of a Variables(&) 를 사용해서 변수의 주소를 얻을 수 있다. 일반적으로 주소값이 클 경우 16진수로 출력된다.

## 포인터

포인터는 다른 변수의 주소를 보관할 수 있는 변수이다. 포인터의 선언(Declaration)을 하려면 가리킬 변수의 자료형을 명시해야 한다. void형 포인터는 어떤 형이든 가리킬 수 있다. 하지만 몇가지 제약이 존재한다.

**포인터의 사용**
```
    int x = 10;
    int *ptr = &x;

코드에서 정수형 포인터 ptr은 x의 주소값을 할당받게 된다. (10이 아니다!)

포인터로 할 수 있는 일반적인 연산은 포인터가 가리키는 메모리 주소에 있는 값을 획득하는 간접지정(Indirection)이다. 기호로 *이다. 포인터를 선언할 때 사용하는 기호와 같으나 둘을 혼돈해서는 안된다.

    #include <stdio.h>
    int main(void){
      int x = 10;
      int * ptr = &x;
      printf("x의값은 %d\n", x); //x의값은 10
      printf("ptr의 주소 %p\n", ptr);//ptr의 주소 0x7ffee1cf58b8
      printf("주소에 있는 값 %d\n", *ptr);//주소에 있는 값 10
      *ptr = 25;
      printf("x가 가진 값  %d\n", x); //x가 가진 값  25
      return 0;
    }
```
모든 형을 가리킬 수 있는 void포인터의 제약 중 하나는 간접 참조를 할 수 없다는 것이다. 이는 각각의 자료형이 차지하는 메모리의 크기가 서로 다르기 때문이다. 컴파일러는 가리키는 주소에서 전체 값을 읽어들이기 위해 얼마나 많은 바이트를 읽어야 할 지 알아야하지만 void포인터는 그렇게 할 수 없다.

**포인터 연산**
```
포인터는 산술연산을 적용할 수 있다. 하지만 미묘하게 차이가 있다.

    #include <stdio.h>
    int main(void){
      char *cptr =(char*)2;
      printf("cptr before : %p",cptr);//cptr before : 0x2
      cptr++;
      printf("cptr after : %p",cptr);//cptr after : 0x3

      int *iptr= (int*)2;
      printf("iptr before : %p",iptr);//iptr before : 0x2
      iptr++;
      printf("iptr after : %p", iptr);//iptr after : 0x6
    }
```
int형과 char형의 결과가 왜 다를까? 해답은 변수의 SizeOf값에 있다. int의 길이는 4바이트, char형의 크기는 1바이트 이기 때문이다.

void포인터의 또 다른 제약은 컴파일러가 얼마나 이동해야 다음 값이 있는지 알 수 없기 때문에 포인터에 산술연산을 할 수 없다. void포인터는 사용하기 전에 나중에 반환하는 포인터 유형의 주소를 보관하기 위해서만 사용할 수 있다.

**배열과 포인터**

배열과 포인터는 밀접한 관계를 가지고 있고 대부분 둘은 같은 것으로 다룬다. 배열로 선언된 변수는 배열의 크기 만한 메모리 블록의 시작 주소를 가리키는 포인터와 같이 여겨진다. 예를 들어 *array=4와 array[0]=4와 같은 의미이다. 일반적으로 표현하면 *(array+n)=array[n]과 같이 표현한다. 배열을 가리키는 포인터와 배열의 유일한 차이점은 컴파일러가 배열에 필요한 저장 공간을 기록하기 위해 몇가지 확장된 정보를 유지한다는 점이다.

**동적 메모리 할당**

앞서서 포인터가 다른 변수의 주소를 보관한다는 것을 알았다. 또다른 기능으로 컴파일 시점에 정의된 변수를 위해서가 아닌, 프로그램 실행 시점에 동적으로 할당되는(heap) 메모리의 주소를 붙들어 둘 수 있다는 것이다.

**함수 포인터**

실행 코드 또한 메모리에 저장된다. 따라서 함수의 주소 또한 얻을 수 있다. 일반적으로 함수의 주소를 저장하는데 함수 포인터를 이용한다. 함수 포인터는 순서가 없기 때문에 할당과 간접 지정에 제약이 있고, 함수 포인터에 산술 연산을 할 수 없다.

## 파라미터 전달 기법

**실제 변수와 형식 변수(Actual Parameters and Formal Parameters)**
```
    //실제 변수를 이용한 호출
    int main(){
    	long i =1;
    	double j =2;
    	func(i,j);
    }
    //형식 변수를 이용한 호출
    void func(long param1, double param2){
    }
```
**언어별 파라미터 전달 기법 지원**

Pass by value : C, Pascal, Ada, Scheme, Algol68

Pass by result : Ada

Pass by value-result : Fortran, Ada

Pass by reference : C, Fortran, Cobol

Pass by name : Algol60

## 바인딩. Binding

바인딩은 이름(변수, 배열, 라벨, 절차)을 그것이 포함하는(메모리주소, 데이터 형, 실제 값)에 연결하는 작업이다. 바인딩은 프로그램의 생명주기를 기준으로 다양한 시점에 발생할 수 있다.

- 정적 바인딩

  프로그램 실행 이전에 이루어지며, 실행 중에 변경되지 않는다. (Early Binding)

  ex : 상수에 값을 바인딩, 함수의 정의에 함수 호출을 바인딩

- 동적 바인딩

  프로그램이 실행 되는 동안 발생, 변경. (Late Binding)

  ex : 메모리 주소와 포인트 변수를 바인딩, 가상 멤버함수 정의에 멤버 함수 호출을 바인딩

## 스코프. Scope

변수의 바인딩 변경이 없거나 최소한 변수의 재선언이 허락되지 않는 최대의 범위.

- 정적 스코프

  프로그램의 물리적 구조로 정의된다. 스코프의 결정은 컴파일러가 수행.

- 동적 스코프

  동적 스코프 규칙은 일반적으로 해석형 언어에서 사용한다. 이런 언어들은 동적 스코프 규칙이 적용될 때 자료형에 대한 결정이 항상 가능하지 않아서 컴파일 시 자료형을 체크할 수 없다.

## 기억 영역 분류 .Storage Classes

객체에 할당된 기억 영역(Storage)과 이러한 할당이 얼마나 지속되어야 하는지를 결정한다. 또한 프로그램에서 변수들을 사용할 수 있는 영역인 스코프를 결정한다.

- 자동 기억 영역(Auto Storage Class)

  기본 기억 영역. 이 영역에 할당되는 변수를 자동변수라 하며, 사용하는 함수 내에 선언된다. auto키워드를 이용하면 자동변수를 선언할 수 있다. private(local)한 특성. 기억영역에 대한 지정자 없이 함수 내에서 선언된 변수는 기본적으로 자동변수이다.

- 외부 기억 영역

  이 영역에 할당되는 변수는 함수 외부에서 선언된다. 프로그램 전체 생존주기 동안 변수들은 살아있다. 전역 변수처럼 기본값은 0이다. 이들은 프로그램 내 모든 함수에서 접근가능하다. 이름이 동일한 변수가 있다면 지역변수가 전역변수보다 우선순위를 갖는다. extern키워드로 선언할 수 있다.

*변수의 선언 시 값을 할당하는 것을 초기화(Initialization)이라고 하고, 나중에 할당하는 것을 할당(Assignment)이라고 한다.*

- 레지스터 기억 영역(Register Storage Class)

  변수가 컴퓨터의 레지스터에 저장된다. register 키워드를 통해 선언한다. 메모리 접근속도가 매우 빠르기 때문에 이 영역에 선언함으로써 접근속도를 향상시킬 수 있다. 전역변수를 레지스터 변수로 선언하면 프로그램의 생존주기 동안 레지스터를 사용하므로 피해야한다.

- 정적 기억 영역(Static Storage Class)

  프로그램이 종료될 때 까지 지속된다. static키워드를 사용해서 선언한다.

  - 내부 정적 변수

    함수 내에서 정적으로 선언되고 할당된 변수

  - 외부 정적 변수

    프로그램의 가장 상위 레벨에 선언되어 프로그램의 모든 함수들이 사용할 수 있는 정적 변수.

  - 정적 함수

    함수를 선언된 모듈의 파일 내에서만 접근이 가능하고 다른 파일의 어떤 함수도 접근하지 못하도록 할 때 함수를 정적으로 선언한다.

## 기억 영역의 구성. Storage Organization

- Static Segment
  - Code Segment

    프로그램 코드가 저장. 프로그램이 실행되는 동안 변경되지 않는다. 읽기 전용으로 보호.

  - Data Segment

    간단히 전역 데이터를 저장한다. 코드를 제외한 프로그램의 정적 데이터가 보관된다. 수정이 가능하다.

    - 전역변수
    - 숫자와 문자열 값이 저장된 상수 리터럴
    - 호출 사이에 값이 유지되는 변수(예: 내부 정적변수)
- Stack Segment

  함수 호출에서 되돌아 올 때 기존의 Context와 변수를 사용하기 위해 함수의 현재 상태를 저장할 수 있는 매커니즘이 필요하다. 스택할당이 사용된다. 함수를 호출할 때, 스택 내에 함수에 대한 새로운 활성 레코드(프레임)을 밀어 넣는다. 피 호출자 함수가 호출자 함수에 제어를 반환할 때, 피호출자 함수의 활성 레코드는 스택에서 제거(pop)된다.

  - 지역변수
  - 형식 매개 변수 (Formal Parameters)
  - 활성화를 위해 필요한 추가 정보
  - 임시 변수
  - 반환 주소
- Heap Segment

  임시로 사용하는 메모리 공간을 동적으로 늘리고자 할 경우, 힙 할당 전략(Heap Allogaction Strategy)을 사용한다. 힙은 동적으로 할당되는 메모리 영역이며 프로그램 실행중에 늘어나거나 줄어들 수 있지만 스택과 달리 LIFO(Last In First Out)방식이 아니다. 관리하기가 복잡하다.

  일반적으로 프로그램에서 스택과 힙 두가지 할당을 사용하도록 구현하고 있다. 두가지 할당을 동시에 사용하는 방법은 프로그램 시작 시에 사용 가능한 메모리를 스택과 힙, 두개의 영역으로 분리하는 것이다.

## 프로그래밍 테크닉

- 비구조적 프로그래밍(Unstructured Programming)

  단순히 하나의 main으로 구성된 프로그램. 프로그램 크기가 커지면 문제가 된다.

- 절차적 프로그래밍(Procedural Programming)

  한군데 동일한 절차를 가진 문장을 조합하여 사용한다. 프로시저를 실행하고 절차가 완료된 후 제어의 흐름은 곧바로 호출된 지점 다음에서 진행된다.

- 모듈러 프로그래밍(Modular Programming)

  분할된 모듈에 공통의 기능들을 함께 모아 놓고 사용한다. 전체 프로그램을 프로시저 호출로 상호 작용하는 몇개의 작은 부품으로 분해한다.

- 객체지향 프로그래밍(Object-Oriented Programming)

  로직과 함수 대신 객체와 데이터로 구성된 프로그래밍 언어 모델. 다루고자 하는 모든 객체들의 관계를 식별한다. Data Modeling. 식별된 객체는 해당 객체의 클래스와 클래스가 가지는 데이터의 정의, 이들을 다룰 수 있는 모든 함수로 일반화 한다.
