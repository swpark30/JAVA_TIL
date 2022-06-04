# TIL Day 01

> 2022년 05월 19일 목요일



![Java 로고](http://www.ddaily.co.kr/data/photos/cdn/20170935/art_1504231904.jpg)



- 목차
  - 1장 자바 시작하기
    - 프로그래밍 언어
    - 자바 언어의 특징
    - 자바 가상 기계(JVM : Java Virtual Machine)
  - 2장 변수와 타입
    - 변수
      - 변수의 구성
      - 변수 예
      - 변수의 특징
      - 자바 예약어(자바 키워드)
      - 변수의 선언





## 1장 자바 시작하기



### 프로그래밍 언어

- 주어진 어떤 문제를 해결하기 위해 인간과 컴퓨터 사이에서 의사소통을 가능하게 하는 인공적인 언어
- 사람과 컴퓨터 사이의 의사교환 수단 
- 명령 --> 결과 받아서 출력



### 자바 언어의 특징

- 플랫폼 독립적(CPU나 운영체제에 상관없이 실행)
- 이식성이 높은 언어
- 진정한 의미의 객체 지향 언어(클래스, 상속, 다형성, 캡슐화 등)
- 멀티쓰레드 지원(하나의 프로그램 내에서 여러 작업 동시 수행 개념)





![image-20220519134317197](C:\Users\sang wan\AppData\Roaming\Typora\typora-user-images\image-20220519134317197.png)



#### 자바 가상 기계(JVM : Java Virtual Machine)

- java.exe 명령어에 의해 구동
- 바이트 코드를 기계어로 변환시키고 실행
- 각 운영체제에 맞는 자바 가상 기계(JVM) 실행
  - 운영체제에 종속적





## 2장 변수와 타입



### 변수



- #### 변수의 구성

  - 상수
  - 리터널
  - 변수의 사용 범위




- #### 변수 예

  - `int b = 100;`
    - b라는 변수에 정수값 100 저장




- #### 변수의 특징

  - 이름이 있다 - 변수명(메모리 주소에 붙여진 이름)

  - 기억장소 내에서의 주소가 있다

  - 변수에 저장되는 값이 있다

  - 저장되는 값의 유형(데이터 타입)이 있고 크기가 다르다

  - 프로그램 실행 중에 저장된 값이 변경될 수 있다.

    - ```java
      b = 300; // 덮어 쓴다 (이전에 있던 값 사라짐)
      b = 500;
      ```

      - 주의! - 변수는 동일한 이름으로 한 번만 선언

    - ```java
      int b = 100;
      int b = 300; // 변수 중복 선언 오류 발생
      ```

      - 2번 선언하면 오류 (중복 선언 오류)




- #### 자바 예약어

  - 자바 키워드(총 53개가 있다)

  - 자바 시스템에서 지정하여 사용하는 단어(이미 맡아놓은 단어)

  - 클래스명, 메소드명, 각종 변수명으로 사용할 수 없다

  - 모두 소문자로 되어 있다.

  - 자바에서 키워드를 적으면 보라색으로 변한다.

    | abstract | default |     if     |   package    |   this    |
    | :------: | :-----: | :--------: | :----------: | :-------: |
    |  assert  |   do    |    goto    |   private    |   throw   |
    | boolean  | double  | implements |  protected   |  throws   |
    |  break   |  else   |   import   |    public    | transient |
    |   byte   |  enum   | instanceof |    return    |   true    |
    |   case   | extends |    int     |    short     |    try    |
    |  catch   |  false  | interface  |    static    |   void    |
    |   char   |  final  |    long    |   strictfp   | volatile  |
    |  class   | finally |   native   |    super     |   while   |
    |  const   |  float  |    new     |    switch    |           |
    | continue |   for   |    null    | synchronized |           |

    

- #### 변수의 선언

  - 변수를 사용하기 위해서는 반드시 변수를 선언해야 한다.

  - 선언하는 이유

    - 실행 중에 필요한 공간을 할당 받기 위해서

  - 선언 방법

    ```java
    데이터 타입 변수명;
    int a;
    int a = 100; // 선언과 동시에 초기화
    ```

