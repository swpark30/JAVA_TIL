# Java_TIL_Day_05



- 목차
  - 4장 조건문과 반복문
    - 기타 제어문
      - continue 문
    - 랜덤 숫자
  - 5장 참조 타입
    - 데이터 타입 분류
    - JVM이 사용하는 메모리 영역
    - 참조 변수의 ==, != 연산
    - null과 NullPointerException
    - String 타입
    - 배열



## 4장 조건문과 반복문



### continue 문

- 수행 중인 문장은 중단하고, 다음 반복 계속 수행
- 반복문은 종료하지 않고 계속 반복 수행



### 랜덤 숫자

- 랜덤 숫자 생성하기 위한 2가지 방법

  

  - Math.random() 메소드 사용하는 방법

    - 1~10 범위의 랜덤 숫자 생성

      ```java
      int num = (int)(Math.random() * 10) + 1;
      ```

      

  - Random 클래스 사용하는 방법

    ```java
    import java.util.Random
    
    Random r = new Random();
    - int x = r.nextInt(10); // 0~9 범위 10개 중에서 생성
    - int x = r.nextInt(10) + 1; // 1~10 범위 10개 중에서 생성
    ```



## 5장 참조 타입



### 데이터 타입 분류

- 변수의 메모리 사용
  - 기본 타입 변수 - 실제 값을 변수 안에 저장
  - 참조 타입 변수 - 주소를 통해 객체를 참조 
  - 참조 타입 변수에는 주소가 저장



### JVM이 사용하는 메모리 영역

- 운영체제(OS)에서 할당 받은 메모리 영역(Runtime Data Area)을 세 영역으로 구분 
  - 메소드 영역
    - JVM 시작할 때 생성
    - 로딩된 클래스 바이크 코드 내용을 분석 후 저장
    - 모든 스레드 공유
  
  - 힙 영역
    - JVM 시작할 때 생성
    - 객체 / 배열 저장
    - 사용되지 않는 객체는 Garbage Collector가 자동 제거
  
  - JVM 스택
    - 스레드 별 생성
    - 메소드 호출할 때마다 프레임을 스택에 추가
    - 메소드 종료하면 프레임 제거
    - 프레임 내부의 로컬 변수 스택에 변수 저장
    - 변수에 값이 저장될 때 변수 생성
  
  - 생성
  
    - 변수는 스택 영역에 생성되고
    - 객체와 배열은 힙 영역에 생성된다
      - 힙 영역에 생성된 객체와 배열을 스택 영역의 변수나 다른 객체의 필드에서 참조
  
    

### 참조 변수의 ==, != 연산

- 기본 타입 변수
  - 변수의 값이 같은지 다른지 조사
- 참조 타입
  - 동일한 객체를 참조하는지 다른 객체를 참조하는지 조사



### null과 NullPointerException



- #### null (널)

  - 참조하는 객체가 없다는 의미

  - 변수가 참조하는 객체가 없을 경우 초기값으로 사용 가능

    ```java
    String str = null;
    ```

  - 참조 타입의 변수에만 저장 가능

  - null로 초기화된 참조 변수는 스택 영역 생성

  

- #### NullPointerException

  - 의미
    - Exception : 예외
    - 사용자의 잘못된 조작이나 잘못된 코딩으로 인해 
      발생하는 프로그램 오류로 예외 처리 가능
  - null 값을 가지고 있는 객체의 필드(변수:데이터)나 
    메소드(기능, 작업)를 사용하려 했을 때 발생하는 오류
  - null 값을 가지고 있는 변수는 참조할 객체가 없기 때문에 사용할 수 없음



### String 타입

- 문자열을 저장하는 클래스 타입

  ``` java
  String name;
  name = “홍길동”;
  String phone = “010-1234-1234”; //초기화
  String birthday = “19980630”;
  ```

- String 객체

  - 문자열은 String 객체로 생성되고 변수는 String 객체 참조한다

    - “홍길동”, “010-1234-1234”, ….

    - 변수 : name, phone 

  

- String 객체 생성 방법 2가지

  

  - 변수에 문자열을 저장하여 생성하는 방법

    ```java
    String name1 = “홍길동”;
    String name2 = “홍길동”;
    // name1과 name2는 동일한 String 객체 참조
    ```

    

  - new 연산자를 사용하는 방법

    ```java
    String name1 = new String(“홍길동”);
    String name2 = new String(“홍길동”);
    // name1과 name2는 서로 다른 String 객체 참조
    // 문자열(값)은 동일
    ```

    

### 배열

- 같은 타입의 데이터를 연속된 공간에 저장하는 자료구조

- 크기와 데이터 타입이 같으며 동일한 이름을 갖는 원소들의 연속적 저장 영역

- 배열의 원소는 메모리 내에서 순서대로 저장

- 각 배열의 원소는 인덱스([0]부터 시작)로 구별

  ```java
  int[] num = new int[3];
  ```

  

- #### 배열 이름

  - 배열의 저장 위치를 가리키는 참조(레퍼런스) 변수
  - 즉, 배열이 저장된 곳의 시작 주소 번지를 갖고 있음
  - 따라서 배열은 기본 타입이 아닌 참조 타입(레퍼런스 타입)



- #### 배열을 사용하는 이유

  - 같은 타입의 변수 사용 시 변수 선언 개수를 줄일 수 있음

  - 반복문(for문)을 활용하여 프로그램을 간단히 작성할 수 있기 때문이다

    ```java
    int num1, num2, ……, ,num100;
    int[] num = new int[100];
    
    for(int i = 0; i < 100; i++){
        sum += num[i]
    }
    ```

  

- #### 배열 선언 형식 (2가지)

  - 데이터 타입[ ] 변수;

  ```java
  int[ ] num;
  double[ ] average;
  String[ ] name;
  ```

  - 데이터 타입 변수[ ];

  ```java
  int num[ ];
  double average[ ];
  String name[ ];
  ```

  - 주의

    - 배열은 선언 후 메모리 공간을 할당 받아야만 사용할 수 있음

    - 참조 변수만 선언하면 사용할 수 없음

      ```java
      int[ ] num; // 아직 사용할 수 없음
      ```

      

- #### 배열의 메모리 공간 할당

  - new 키워드를 사용하여 메모리 공간 할당

    ```java
    int[ ] a = new int[5]; // 선언과 동시에 할당
    // 또는
     int[ ] b;   // 선언만 (아직 사용할 수 없음)
    b = new int[5]; // 메모리 할당
    ```

  

- #### 배열의 초기값

  - 메모리를 할당 받을 때 자료형의 기본값으로 초기화

    ```java
    숫자 : 0, char[] = '\u0000'(정수), 0.0 (실수) // \u0000 : null의 유니코드
    boolean : false
    참조 타입 : null
    int[ ] a = new int[5]; // 초기값 : 0
    double[ ] a = new double[5]; // 초기값 : 0.0
    boolean[ ] b = new boolean[5]; // 초기값 : false
    String[ ] s = new String[5]; // 초기값 : null 
    ```

  

- #### 배열의 초기화 리스트

  - 배열을 선언할 때 값을 바로 할당

  - 선언 + 기억 장소 할당 + 원소에 값 저장을 한 번에 수행

    ```java
    int[ ] num = {1, 2, 3, 4, 5};
    String[ ] nations = {“Korea”, “Japan”, “China”};
    // 주의!!!
    // {1, 2, 3, 4, 5}는 초기화만 가능
    
    int[ ] num;
    num = {1, 2, 3, 4, 5} // 불가능하다
    ```

  

- #### 배열 사용

  - 배열 원소에 접근하여 원소에 값을 저장 또는 원소의 값을 가져오는 것

  - 배열 원소에 값 저장

  - 첨자(인덱스)를 사용하여 원소 선택하고 값 저장

    ```java
    num[2] = 10; // 3번째 원소에 10 저장
    ```

  

- #### 배열 원소에 들어 있는 값 출력

  - 변수 x에 배열의 첫 번째 원소값 저장

    ```java
    int x = num[0];
    System.out.println(num[1]);
    ```

  - 주의!!

    - 배열의 전체 원소에 값을 저장하거나 출력하기 위해서는 반복문 사용
    - 배열은 크기가 정해져 있기 때문에 반복 횟수를 정할 수 있으므로 for 문 사용
    - 반복문 시작은 반드시 0부터 (첨자가 [0]부터 시작하므로)