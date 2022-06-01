# Java_TIL_Day_15



- 목차
  - 12장 멀티 스레드
    - 개념
    - 메인 스레드





## 12장 멀티 스레드



### 개념

- #### 프로세스(process)

  - 실행 중인 하나의 프로그램(애플리케이션)
  - 하나의 프로그램이 다중 프로세스를 만들기도 함
    - ex) 크롬 브라우저를 두 개 실행하면 두 개의 프로세스 생성

  

- #### 멀티 태스킹(multi tasking)

  - 두 가지 이상의 작업을 동시에 처리하는 것
  - 운영체제는 CPU 및 메모리 자원을 프로세스마다 적절히 할당해 주고
    병렬로 실행시킴
    - ex) 워드 문서 작업하면서 윈도우 미디어 플레이어로 음악을 들음
  - 멀티 프로세스
    - 독립적으로 프로그램들을 실행하고 여러 가지 작업 처리
  - 멀티 스레드
    - 한 개의 프로그램을 실행하고 내부적으로 여러 가지 작업 처리



### 메인 스레드

- 모든 자바 프로그램은 메인 스레드가 main() 메소드를 실행하며 시작
- main() 메소드의 첫 코드부터 아래로 순차적으로 실행
- 실행 조건
  - 마지막 코드 실행
  - return 문을 만나면 종료
- main 스레드는 작업 스레드들을 만들어 병렬로 코드를 실행
  - 멀티 스레드 생성해 멀티 태스킹 수행
- 스레드 종료
  - 싱글 스레드
    - 메인 스레드가 종료되면 프로세스도 종료
  - 멀티 스레드
    - 실행 중인 스레드가 하나라도 있으면 프로세스 미 종료
    - 몇 개의 작업을 병렬로 실행할지 결정하는 것이 선행되어야 함



### 작업 스레드 생성과 실행

- 생성 방법 2가지

  1. Thread 클래스로부터 직접 생성
     - Runnable 인터페이스 사용(구현)
     
     - Runnable 인터페이스를 매개값으로 갖는 생성자 호출
     
       ```java
       class Task implements Runnable{
           public void run(){
               스레드가 실행할 코드
           }
       }
       Runnable task = new Task();
       Thread thread = new Thread(Runnable target : task);
       // 코드 절약하기 위해 Thread 생성자를 호출할 때 Runnable 익명 객체를 매개값으로 사용 가능
       
       Thread thread = new Thread(new Runnable(){
           스레드가 실행할 코드
       });
       ```
     
     - Runnable 인터페이스
     
       - run() 메소드 하나만 정의되어 있음
     
       - Runnable은 작업 내용을 가지고 있는 객체이지 실제 스레드 아님
     
       - Runnable 구현 생성한 후 이것을 매개값으로 해서
          Thread 생성자를 호출하면 비로소 작업 스레드 생성
     
         
     
  2. Thread 하위 클래스로부터 생성
     - Thread 클래스 상속
     
     - Thread 클래스 상속 후 run 메소드 재정의해서 스레드가 실행할 코드 작성
     
       ```java
       public class WorkerThread extends Thread(){
           // run 메소드 오버라이딩
       }
       ```
     
       

- 실행 방법

  - 작업 스레드는 생성되는 즉시 실행되는 것이 아니라 start()메소드를 호출해야만 실행
  
    ```java
    thread.start()
    ```
  
  - start() 메소드가 호출되면 작업 스레드는 매개값으로 받은 Runnable run() 메소드를 실행하면서 자신의 작업을 처리



- #### 스레드 이름

  - 메인 스레드 이름 : main

  - 작업 스레드 이름 (자동 설정) : Thread-n
    
    ```java
    thread.getName(); // 스레드 이름 반환
    ```
    
  - 작업 스레드 이름 변경
    
    ```java
    thread.setName("스레드 이름");
    ```
    
  - 코드 실행하는 현재 스레드 객체 참조 얻기
    
    ```java
    Thread thread = Thread.currentThread();
    ```
    
    

- #### 스레드 우선 순위

  - 동시성과 병렬성
  
    - 동시성
      - 멀티 작업을 위해 하나의 코어에서 멀티 스레드가 번갈아가며 실행하는 성질
    - 병렬성
      - 멀디 작업을 위해 멀티 코어에서 개별 스레드를 동시에 실행하는 성질
  
    
  
  - 코어
  
    - CPU 칩에서 물리적으로 연산하는 유닛 개수
    - 입력된 명령어를 계산하고 해석하는 단위
  
    
  
  - 스레드의 개수가 코어의 수보다 많을 경우
  
    - 스레드를 어떤 순서로 동시적으로 실행할 것인가 결정
      - 스레드 스케쥴링
    - 스케쥴링에 의해 스레드들은 번갈아 가면 run() 메소드 조금씩 실행
  
  - 자바의 스레드 스케쥴링
    - 우선 순위 방식(코드로 제어 가능)
      - 우선 순위가 높은 스레드가 실행 상태를 더 많이 가지도록 스케쥴링
      
        ```java
        thread.setPriority(Thread.MAX_PRIORITY);
        thread.setPriority(Thread.MIN_PRIORITY);
        ```
      
    - 순환 할당 방식(코드로 제어 불가)
      - 시간 할당량(Time Slice)을 정해서 하나의 스레드를 정해진 시간만큼 실행



- #### 스레드 동기화

  - 공유 객체를 사용할 때 주의할점
    - 멀티 스레드가 하나의 객체를 공유해서 생기는 오류
    
  - 동기화 메소드 및 동기화 블록
    - 단 하나의 스레드만 실행할 수 있는 메소드 또는 블록
    
    - 다른 스레드는 메소드 또는 블록이 실행이 끝날 때까지 대기
    
    - 동기화 메소드 : synchronized 키워드 붙임
    
      ```java
      public synchronized void method(){
          임계 영역; // 단 하나의 스레드만 실행
      }
      ```
    
  - 임계영역
    - 멀티 프로그램에서 단 하나의 스레드만 실행할 수 있는 코드 영역
    - 자바에서는 임계 영역을 지정하기 위해 동기화 메소드와 동기화 블록 제공
    - 스레드가 객체 내부의 동기화 메소드 또는 블록에 들어가면
      즉시 객체에 잠금을 걸어 다른 스레드가 임계 영역 코드를 실행하지 못하게 함

  

- #### 스레드 상태

  - 스레드의 일반적인 상태
  - 스레드에서 일시 정지 상태 도입한 경우
    - 실행 상태에서 실행 디기 상태로 가지 않고 일시 정지 상태로 가기도 함
  - NEW 상태 (객체 생성)
  - RUNNABLE 상태
    - 실행 대기 상태

  

- #### 스레드 상태 제어

  - 실행 중인 스래드의 상태를 변경하는 것



## 14장 람다식



### 람다식이란?

- 익명 함수를 생성하기 위한 식
- 객체 지향 언어보다 함수 지향 언어
- 사용하는 이유
  - 코드 간결
  - 필터링 또는 매핑을 통해 대용량 데이터를 쉽게 집계
    - (반복문에서 실행 속도가 느리다는 단점)




### 람다식 형식

```java
Runnable runnable = () -> {...};
인터페이스 변수 = 람다식;
```

- 인터페이스 변수에 대입

  

- 함수적 스타일의 람다식 작성법

  - (타입 매개변수, ...) -> {실행문; ...}
  - (int a) -> {System.out.println(a);}
  - 기호 ->
    - 매개변수를 이용해서 중괄호 {} 실행한다는 의미

- 람다식의 다양한 표현 방법

  1. (a) -> {System.out.println(a);}
     - 매개변수 타입은 런타임 시에 대입되는 값에 따라 자동 인식될 수 있기 때문에
       매개변수 타입 생략 가능

  2. a -> {System.out.println(a);}
     - 매개변수가 1개인 경우 괄호 생략 가능
  3. a - > System.out.println(a);
     - 실행문이 1개인 경우 중괄호{} 생략 가능

  4. () -> {System.out.println(a);}
     - 매개변수가 없는 경우에는 반드시 빈 괄호 있어야 함

  5. (x,y) -> {return x+y;}
     - 반환값이 있는 경우 return문 사용

  6. (x,y) -> x+y;
     - 중괄호{} 안에 return 문만 있는 경우, return 생략 가능



### 타겟 타입

- 람다식이 대입될 인터페이스
- 람다식은 대입될 인터페이스의 종류에 따라 작성 방법이 달라짐
- 함수적 인터페이스
  - @functionalInterface
    - 함수적 인터페이스임을 표시하는 어노테이션
  - 람다식의 타겟 타입으로 모든 인터페이스를 사용하는 것은 아님



### 외부 로컬 변수 접근

- 람다식 실행 블록에서 클래스 멤버와 로컬 변수 사용

  1. 클래스의 멤버(필드와 메소드)
     - 사용하는데 제약 없음
     - this 키워드 사용 주의
       - 
  2. 로컬 변수
     - 제약 사항 있음
     - final 특성을 가져야 함

  

## 18장 IO 기반 입출력 및 네트워킹



### java.io 패키지

- 자바의 기본적인 데이터 입출력(input / output) API 제공



### 자바의 스트림(Stream)

- 입출력 장치와 자바 응용 프로그램 연결 통로

- 입력 스트림

  - 입력 장치로부터 자바 프로그램으로 데이터 전달하는 소프트웨어 모듈

- 출력 스트림

  - 자바 프로그램에서 출력 장치로 데이터를 보내는 소프트웨어 모듈

- 입출력 스트림 기본 단위 : 바이트(byte)

- 자바 입출력 스트림 특징

  - 단방향
  - 선입선출구조 (FIFO)

  

- 바이트 기반 스트림

  - 이미지, 멀티미디어, 문자 등 모든 종류의 데이터를 받고 보내는 것 가능
  - 입출력되는 데이터를 단순 바이트의 스트림으로 처리

- 문자 기반 스트림

  - 문자만 입출력하는 스트림
  - 문자만 받고 보낼 수 있도록 특화



### File 클래스

- 파일 시스템의 파일을 표현하는 클래스

  - ㅇㅇ

- #### File 클래스 메소드

  - mkdir()
    - 새로운 디렉토리 생성 후 결과 반환(true / false)
    - 최하위 



- 파일 읽기 / 쓰기에 사용되는 클래스
  - 바이트 기반
    - FileInputStream / FileOutputStream
  - 문자 기반
    - FileReader / FileWriter
- 
