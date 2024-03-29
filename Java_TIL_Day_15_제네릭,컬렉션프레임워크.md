# Java_TIL_Day_15 (제네릭,컬렉션 프레임워크)



- 목차
  - 13장 제네릭
    - 제네릭이란?
    - 멀티 타입 파라미터
    - 제네릭 메소드
    - 제네릭 타입의 상속과 구현
  - 15장 컬렉션 프레임워크
    - 컬렉션이란?
    - 프레임워크란?
    - 컬렉션 프레임워크
    - 컬렉션 프레임워크의 주요 인터페이스
      - List(인터페이스)컬렉션
      - Set 컬렉션
      - Map 컬렉션
      - Collections 클래스 활용



## 13장 제네릭



### 제네릭이란??

- 클래스(인터페이스)나 메소드를 정의할 때 타입을 파라미터로 이용하는 기법

- 클래스나 인터페이스를 설계할 때 구체적인 타입을 명시하지 않고 타입 파라미터로 대체했다가 실제 클래스 사용될 때 구체적인 타입을 지정

- 사용이유

  - 제네릭 타입을 이용함으로써 잘못된 타입이 사용될 수 있는 문제를 컴파일 과정에서 제거 할 수 있게 됐기 때문에

- 사용 이점

  - 컴파일 시 강한 타입 체크를 할 수 있다.
    - 실행 시 타입 에러가 나는 것보다는 컴파일 시에 미리 타입을 강하게 체크해서 에러를 사전에 방지 한다.
  - 타입 변환을 제거한다.
    - 비제네릭 코드는 불필요한 타입 변환을 하기 때문에 프로그램 성능에 악영향을 끼친다.



### 제네릭 타입(`class<T>`, `interface<T>`)

- 타입을 파라미터로 가지는 클래스와 이터페이스를 말한다.
- 타입 파라미터의 이름은 T이다.
- 타입 변환이 빈번해지면 전체 프로그램 성능에 좋지 못한 결과를 가져올 수 있다.

- 사용 방법

  ```java
  public class 클래명<T> {  …. }
  public interface 인터페이스명<T> {  }
  
  ex) // 실제 설계 적용 
  public class Box<T> {
      private T t;
      public T get() {return t;}
      public void set(T t) {this.t = t;}
  } 
  
  Gen<String> gen = new Gen<String>(); // 객체 호출 방법
  Gen<Integer> gen2 = new Gen<Integer>();
  ```

- 기본 데이터 타입은 올 수 없다

  - <int> X , <Integer> O

  ```java
  제네릭을 사용하지 않을 경우
  List list = new ArrayList();
  list.add(“hello”);
  String str = (String) list.get(0); // 강제 타입 변환 Object -> String
  
  제네릭을 사용할 경우
  List<String> list = new ArrayList<String>();
  list.add(“hello”);
  String str = list.get(0);  // 강제 타입 변환 불필요
  ```



### 멀티 타입 파라미터(class<K,V,...>, interface<K,V,...>)

- 제네릭 타입은 두 개 이상의 멀티 타입 파라미터 사용 가능

- 각 타입 파라미터는 콤마로 구분

  ```java
  class 클래스명<K, V, …> { … } 
  interface 인터페이스명<K, V, …> { …} 
  
  Product<String, String> product = new Product<>(); // <String, String>이라고 적어야하지만 자바 7부터는														간단하게 사용한다
  ```



### 제네릭 메소드

- 매개변수 타입과 리턴 타입으로 타입 파라미터를 갖는 메소드

- 선언 방법

  ```java
  public <타입파라미터, ..> 리턴타입 메소드명(매개변수, ..) {...}
  
  public <T> Box<T> boxing(T t){...}
  ```

  

- **제네릭 메소드 호출방법**

  1. 명시적으로 구체적 타입 지정

     ```java
     리턴타입 변수 = <구체적 타입>메소드명(매개값);
     ex)
         Box<Integer> box = <Integer>boxing(100);
     ```

     

  2. 매개값을 보고 구체적 타입을 추정

     ```java
     리턴타입 변수 = 메소드명(매개값);
     ex)
         Box<Integer> box = boxing(100);
     ```



### 제네릭 타입의 상속과 구현

- 제네릭 타입을 부모 클래스로 사용할 경우
  - 타입 파라미터는 자식 클래스에도 기술해야 한다.

    ```java
    public class ChildProduct<T,M> extends Product<T,M>{...}
    ```

  - 자식 클래스는 추가적인 타입 파라미터 가질 수 있음

    

- 제네릭 인터페이스를 구현한 클래스도 제네릭 타입

  ```java
  public class StorageImpl<T> implements Storage<T>{...}
  ```

  



## 15장 컬렉션 프레임워크



### 컬렉션이란?

- 사전적 의미로 요소(객체)를 수집해 저장하는것



### 프레임워크란?

- 표준화, 정형화된 체계적인 프로그래밍 방식

- 미리 정해진 방식대로 프로그램을 작성

  - 누가 작성하든 프로그램이 표준화되기 때문에 프로그램 유지보수하기 쉬움

    

### 컬렉션 프레임워크

- 컬렉션을 다루기 위한 표준화된 프로그래밍 방식

  - 많은 양의 데이터와 객체들을 효율적으로 추가, 삭제, 검색 등을 할 수 있도록 제공되는 컬렉션 라이브러리
  - 인터페이스를 통해서 정형화된 방법으로 다양한 컬렉션 클래스 이용

- 사용이유

  - 배열의 문제점을 해결하고 객체들을 효율적으로 추가, 삭제, 검색 등을 할 수 있도록 하기 위해서

    

- 배열의 문제점

  - 생성시 크기 고정되고 사용 시 크기 변경불가
    - 불특정 다수의 객체를 저장하기에는 문제가 있다

  - 객체를 삭제했을 때 해당 인덱스가 빈 상태로 있다
    - 새로운 객체를 저장하려면 어디 인덱스가 비었는지 확인하는 코드가 필요해서 불편하다.

  



### 컬렉션 프레임워크의 주요 인터페이스



### List(인터페이스)컬렉션

- #### 특징

  - 순서가 있는 데이터의 집합

  - 인덱스로 관리

  - 중복해서 객체 저장 가능

    

- #### 구현 클래스

  - **ArrayList**
  
    - 크기가 가변적으로 변하는 선형 리스트
  
    - 배열과 유사
      - 순차 리스트, 인덱스 사용
  
    - 배열과의 차이
  
      - 객체 추가 가능
      - 저장 용량 초과 시 자동으로 저장 용량 증가
      - 기본 10개
      - 처음부터 크게 설정 가능
  
      ```java
      //제네릭을 사용하지 않을 경우
      List list = new ArrayList();
      list.add("hello");
      String str = (String)list.get(0);
      
      //제네릭을 사용할 경우
      List<E> list = new ArrayList<E>();
      ```
  
    - 객체 추가
  
      - 인덱스 0부터 차례로 저장된다
  
    - 객체 제거
  
      - 제거된 객체의 바로 뒤 인덱스부터 앞으로 하나씩 당겨진다.
  
        - 빈번한 객체 삽입과 삭제시 사용하는걸 자제해야한다.
  
          
  
  - **Iterator**
  
    - 요소가 순서대로 저장된 컬렉션에서 요소를 순차적으로 검색할 때 사용
  
      ```java
      Vector<Integer> v = new Vector<Integer>();
      Iterator<Integer> it = v.iterator(); 
      // 벡터 v의 요소를 순차적으로 검색할 Iterator 객체 반환
      ```
  
      
  
  - **LinkedList**
  
    - ArrayList와 사용 방법은 동일하지만 내부 구조가 다르다
    - 인접 참조를 링크해서 체인처럼 관리
  
  - **Vector**
  
    - ArrayList와 동일한 내부구조
  
    - 스레드 동기화가 되어 있기 때문에 여러 스레드가 동시에 Vector에 접근해서 객체를 추가, 삭제 하더라도 스레드는 안전하다.
  
      ```java
      List<E> list = new Vector<E>();
      ```
  
      

### Set 컬렉션

- #### 특징

  - 수학의 집합에 비유

  - 저장 순서가 유지되지 않음

    - get() 메소드 없음

  - 객체 중복 저장 불가

    

- #### 구현 클래스

  - **HashSet**
  
    - 객체를 순서없이 저장
  
    - 동일 객체 및 동등 객체는 중복 저장하지 않음
  
    - 동등 객체 판단 방법
      - 객체 저장하기 전에 hashCode()를 호출해서 해시코드를 얻어내어 저장되어 있는 객체들의 
        해시코드와 비교하여 동일한지 판단한다.
  
      ```java
      Set<E> set = new HashSet<E>();
      ```
  
    
  
  - 사용자 정의 클래스 중복 안 되게 저장
  
    - 사용자 정의 클래스에서 객체가 중복 저장되지 않게 하려면
      hashCode()와 equals() (둘 다) 재정의 해서 동등 객체가 될 조건을 정해야 함
  
    
  
  - LinkedHashSet
  
  - TreeSet



### Map 컬렉션

- #### 특징

  - 키와 값의 쌍으로 이루어진 데이터의 집합
  - 키와 값은 모두 객체
  - 키는 중복될 수 없지만 값은 중복 저장 가능

  

- #### 구현 클래스

  - **HashMap**
  
    - 키 타입은 String 많이 사용
  
    ``` java
    Map<K, V> map = new HashMap<K, V>(); // K : 키타입 V : 값타입
    
    map
    map.get(K); // 입력한 K값에 대한 V값을 가져옴
    ```
  
    
  
  - **HashTable**
  
    - 키 객체 만드는 법은 HashMap과 동일
    - Hashtable은 스레드 동기화가 된 상태
      - 여러개의 스레드가 동시에 Hashtable에 접근해서 객체를 추가,
        삭제하더라도 스레드에 안전
    - Hashtable은 멀티 스레드 환경에서 주로 사용
    - HashMap은 단일 스레드 환경에서 주로 사용
      - 동기화하면 객체 잠금이 발생해서 성능이 저하된다.
  
    
  
  - LinkedHashMap
  
  - TreeMap
  
  - Properties
  
    

### Collections 클래스 활용

- 컬렉션을 다루는 유용한 메소드 지원(static)
  - sort() : 정렬
  - reverse() : 역순으로 정렬
  - max() / min()

