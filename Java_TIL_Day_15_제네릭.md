# Java_TIL_Day_15 제네릭



- 목차
  - 13장 제네릭
    
    - 제네릭이란?
    - 제네릭 타입
    - 멀티 타입 파라미터
    - 제네릭 메소드
    - 제한된 타입 파라미터
    - 제네릭 타입의 상속과 구현
    
    

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



### 제네릭 메소드(<T,R> R method(T t))

- 매개변수 타입과 리턴 타입으로 타입 파라미터를 갖는 메소드

- 선언 방법

  ```java
  public <타입파라미터, ..> 리턴타입 메소드명(매개변수, ..) {...}
  
  public <T> Box<T> boxing(T t){...}
  ```

  

- **제네릭 메소드 호출방법**

  1. 코드에서 명시적으로 구체적 타입 지정

     ```java
     리턴타입 변수 = <구체적 타입>메소드명(매개값);
     ex)
         Box<Integer> box = <Integer>boxing(100);
     ```

     

  2. 컴파일러가 매개값을 보고 구체적 타입을 추정

     ```java
     리턴타입 변수 = 메소드명(매개값);
     ex)
         Box<Integer> box = boxing(100);
     ```



### 제한된 타입 파라미터(<T extends 최상위타입>)

- 제한된 타입 파라미터를 선언하려면 타입 파라미터 뒤에 extends 키워드를 붙이고 상위 타입을 명시하면 된다.

  ``` java
  public <T extends 상위타입> 리턴타입 메소드(매개변수, ...) {...}
  ```

- 타입 파라미터에 지정되는 구체적인 타입은 상위 타입이거나 상위 타입의 하위 또는 구현 클래스만 가능하다.

- 숫자 타입만 구체적인 타입으로 갖는 제네릭 메소드 compare()이다.

  ```java
  public <T extends Number> int compare(T t1, T t2) {
      double v1 = t1.doubleValue(); // Number의 doubleValue() 메소드 사용
      double v2 = t2.doubleValue();
      return Double.compare(v1,v2);
  }
  ```



### 와일드카드 타입(<?>, <? extends ...>, <? super ...>)

- 코드에서 ?를 일반적으로 와일드카드라고 부른다.
- 제네릭 타입을 매개값이나 리턴 타입으로 사용할 때, 와일드카드를 3가지 형태로 사용할 수 있다.
  - 제네릭타입<?>
    - 타입 파라미터를 대치하는 구체적인 타입으로 모든 클래스나 인터페이스 타입이 올 수 있다.
  - 제네릭타입<? extends 상위타입>
    - 구체적인 타입으로 상위 타입이나 하위 타입만 올 수 있다.
  - 제네릭타입<? super 하위타입>
    - 구체적인 타입으로 하위 타입이나 상위 타입이 올 수 있다.

- ex)

  ```java
  public class Course<T> {
      private String name;
      private T[] students;
      
      public Course(String name, int capacity) {
          this.name = name;
          student = (T[])(new Object[capacity]);
      }
      
      public String getName() {return name;}
      public T[] getStudents() {return students;}
      public void add(T t) {
          for(int i=0; i<students.length; i++) {
              if(students[i] == null) {
                  students[i] = t;
                  break;
              }
          }
      }
  }
  ```

- 수강생이 될 수 있는 타입이 다음 4가지 클래스로 가정한다.

  - Person
    - Worker
    - Student
      - HighStudent

- Course<?>

  - 수강생은 위 4개 모든 타입이 될 수 있다.

- Course<? extends Student>

  - 수강생은 Student와 HighStudent만 될 수 있다.

- Course<? super Worker>

  - 수강생은 Worker와 Person만 될 수 있다.



### 제네릭 타입의 상속과 구현

- 제네릭 타입도 다른 타입과 마찬가지로 부모 클래스가 될 수 있다
  
  
  
- 제네릭 타입을 부모 클래스로 사용할 경우
  
  - 타입 파라미터는 자식 클래스에도 기술해야 한다.
  
    ```java
    public class ChildProduct<T,M> extends Product<T,M>{...}
    ```
  
  - 자식 클래스는 추가적인 타입 파라미터 가질 수 있음
  
    ```java
    public class ChildProduct<T,M,C> extends Product<T,M>{...}
    ```
    
    
  
- 제네릭 인터페이스를 구현한 클래스도 제네릭 타입

  ```java
  public class StorageImpl<T> implements Storage<T>{...}
  ```

  - 타입 파라미터로 배열을 생성하려면 new T[n] 형태로 생성할 수 없고 (T[])(new Object[n])으로 생성해야 한다.

