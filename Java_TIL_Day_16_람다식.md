# 14장 람다식



- 최근 들어 함수적 프로그래밍이 다시 부각되고 있는데, 병렬 처링와 이벤트 지향 프로그래밍에 적합하기 때문이다.



### 람다식이란?

- 익명 함수를 생성하기 위한 식
- 객체 지향 언어보다 함수 지향 언어에 가깝다
- 사용하는 이유
  - 자바 코드 매우 간결해진다.
  - 컬렉션의 요소를 필터링 또는 매핑을 통해 원하는 대용량 데이터를 쉽게 집계
    - (반복문에서 실행 속도가 느리다는 단점)



### 람다식 형식

- 매개 변수를 가진 코드 블록이지만, 런타임 시에는 익명 구현 객체를 생성한다.
  - 람다식 -> 매개 변수를 가진 코드 블록 -> 익명 구현 객체

```java
// 기본 익명 구현 객체
Runnable runnable = new Runnable() {
    public void run() {...}
}

// 람다식 익명 구현 객체
Runnable runnable = () -> {...};
인터페이스 변수 = 람다식;
```

- 위의 Runnable 인터페이스 변수에 대입되고 인터페이스 익명 구현 객체를 생성한다.

  

- #### 함수적 스타일의 람다식 작성법

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



### 함수적 인터페이스

- 람다식의 타겟 타입으로 모든 인터페이스를 사용하는 것은 아님

- 2개 이상의 추상 메소드가 선언된 인터페이스는 람다식을 이용해서 구현 객체를 생성할 수 없다.

- 하나의 추상 메소드가 선언된 인터페이스만이 람다식의 타겟 타입이 되고 이걸 함수적 인터페이스라고 한다.

- @functionalInterface

  - 함수적 인터페이스임을 표시하는 어노테이션

- 함수적 인터페이스가 가지고 있는 추상 메소드의 선언 형태에 따라서 작성 방법이 달라진다.

  - 매개 변수와 리턴값이 없는 람다식

    ```java
    @FunctionalInterface
    public interface MyFunctionalInterface {
        public void method();
    }
    
    // 위의 인터페이스랄 타겟 타입으로 갖는 람다식
    MyFunctionalInterface fi = () -> {...}
    
    // 인터페이스의 참조 변수는 다음과 같고 이 호출은 람다식의 중괄호를 실행한다. 
    fi.method();
    ```

  - 매개 변수가 있는 람다식

    ```java
    @FunctionalInterface
    public interface MyFunctionalInterface {
        public void method(int x);
    }
    
    // 위의 인터페이스랄 타겟 타입으로 갖는 람다식
    MyFunctionalInterface fi = (x) -> {...} 또는 x -> {...}
    
    // 인터페이스의 참조 변수는 다음과 같고 이 호출은 람다식의 중괄호를 실행한다. 
    fi.method(5);
    ```

  - 리턴값이 있는 람다식

    ```java
    @FunctionalInterface
    public interface MyFunctionalInterface {
        public int method(int x, int y);
    }
    
    // 위의 인터페이스랄 타겟 타입으로 갖는 람다식
    MyFunctionalInterface fi = (x,y) -> {...; return 값;}
    
    // 인터페이스의 참조 변수는 다음과 같고 이 호출은 람다식의 중괄호를 실행한다. 
    int result = fi.method(2, 5);
    ```



### 클래스 멤버와 로컬 변수 사용

- 람다식 실행 블록에서 클래스 멤버와 로컬 변수 사용이 가능하다.

  1. 클래스의 멤버(필드와 메소드)
     - 사용하는데 제약 없음
     
     - this 키워드 사용 주의
       - 일반적으로 익명 객체 내부에서 this는 익명 객체의 참조이지만, 람다식에서 this는 람다식을 실행한 객체의 참조이다.
       
         ```java
         public class UsingThis {
             public int outterField = 10;
             
             class Inner {
                 int innerField = 20;
                 
                 void method() {
                     MyFunctionalInterface fi = () -> {
                         // 바깥 객체의 참조를 얻기 위해서는 클래스명.this를 사용
                         System.out.println("outterField: " + outterField);
                         System.out.println("outterField: " + UsingThis.this.outterField + "\n");
                         // 람다식 내부에서 this는 Inner 객체를 참조
                         System.out.println("innerField: " + innerField);
                         System.out.println("innerField: " + this.innerField + "\n");
                     };
                     fi.method();
                 }
             }
         }
         ```
     
  2. 로컬 변수
     - 제약 사항 있음
     - 메소드의 매개 변수 또는 로컬 변수를 사용하면 이 두 변수는 final 특성을 가져야 한다.
     - 매개 변수, 로컬 변수를 람다식에서 읽는 것은 허용되지만, 람다식 내외부에서 변경할 수는 없다.



### 표준 API의 함수적 인터페이스

- 자바에서 제공되는 표준 API에서 한 개의 추상 메소드를 가지는 인터페이스들은 모두 람다식을 이용해서 익명 구현 객체로 표현이 가능하다.

  ```java
  public class RunnableExample {
      public static void main(String[] args) {
          Runnable runnable = () -> {
              for(int i=0; i<10; i++) {
                  System.out.println(i);
              }
          };
          Thread thread = new Thread(runnable);
          thread.start();
      }
  }
  
  // 위에처럼 인터페이스 변수에 람다식을 사용해도 되고 아래처럼 생성자를 호출할 때 람다식을 매개값으로 대입해도 된다
  Thread thread = new Thread(()-> {
      for(int i=0; i<10; i++) {
          System.out.println(i);
      }
  });
  ```

- java.util.function 패키지의 함수적 인터페이스

  - Consumer

    - 매개값은 있고, 리턴값은 없다.
    - accept() 메소드를 가지고 있다.

  - Supplier

    - 매개 변수가 없고 리턴값이 있다.
    - getXXX() 메소드를 가지고 있다.
    - 실행 후 호출한 곳으로 데이터를 리턴하는 역할을 한다.

  - Function

    - 매개 변수와 리턴값이 있다.

    - applyXXX() 메소드를 가지고 있다.

    - 매개값을 리턴값으로 매핑하는 역할을 한다.

      ```java
      // Student 객체를 학생 이름으로 매핑하는 것
      Function<Student, String> function = t -> {return t.getName();}
      ```

  - Operator

    - Function과 동일하게 매개 변수 리턴값이 있고 메소드를 가진다.
    - 메소드들은 매핑하는 역할보다 매개값을 이용해서 연산을 수행한 후 동일한 타입으로 리턴값을 제공하는 역할을 한다.

  - Predicate

    - 매개 변수가 있고 boolean 리턴값이 있다.
    - testXXX() 메소드를 가진다.
    - 매개값을 조사해서 true 또는 false를 리턴하는 역할을 한다. 

- andThen(), compose() 디폴트 메소드

  - 디폴트 및 정적 메소드는 추상 메소드가 아니기 때문에 함수적 인터페이스에 선언되어도 여전히 함수적 인터페이스의 성질을 잃지 않는다.

  - 두 개의 함수적 인터페이스를 순차적으로 연결하고, 첫 번째 처리 결과를 두 번째 매개값으로 제공해서 최종 결과값을 얻을 때 사용한다.

    ```java
    // andThen() 메소드
    // 인터페이스A부터 처리하고 결과를 인터페이스B의 매개값으로 제공 후 매개값으로 처리 후 최종 결과를 리턴
    인터페이스AB = 인터페이스A.andThen(인터페이스B);
    최종결과 = 인터페이스AB.method();
    
    // compose() 메소드
    // 인터페이스B부터 처리하고 결과를 인터페이스A의 매개값으로 제공 후 매개값으로 처리 후 최종 결과를 리턴
    인터페이스AB = 인터페이스A.compose(인터페이스B);
    최종결과 = 인터페이스AB.method();
    ```

- and(), or(), negate() 디폴트 메소드

  - Predicate 종류의 함수적 인터페이스는 and(), or(), negate() 디폴트 메소드를 가지고 있다.

- isEqual() 정적 메소드

  - Predicate<T> 함수적 인터페이스는 위의 정적 메소드를 제공한다.

- minBy(), maxBy() 정적 메소드

  - BinaryOperator<T> 함수적 인터페이스는 위의 정적 메소드를 제공한다.



### 메소드 참조

- 메소드를 참조해서 매개 변수의 정보 및 리턴 타입을 알아내어, 람다식에서 불필요한 매개 변수를 제거하는 것이 목적

- 메소드 참조는 정적,인스턴스 메소드, 생성자를 참조할 수 있다

  ```java
  // 정적 메소드를 호출하는 람다식
  (left, right) -> Math.max(left, right);
  
  // 메소드 참조
  Math :: max; // 람다식과 마찬가지로 인터페이스의 익명 구현 객체로 생성된다.
  ```

- 정적 메소드와 인스턴스 메소드 참조

  ```java
  // 정적 메소드
  클래스 :: 메소드;
  
  // 인스턴스 메소드
  참조변수 :: 메소드;
  ```

- 매개 변수의 메소드 참조

  ```java
  // 람다식에서 제공되는 a 매개 변수의 메소드를 호출해서 b 매개 변수를 매개값으로 사용하는 경우
  (a,b) -> {a.instanceMethod(b);}
  
  // 메소드 참조로 표현
  클래스 :: instanceMethod;
  ```

- 생성자 참조

  - 생성자가 오버로딩되어 여러 개가 있을 경우, 컴파일러는 함수적 인터페이스의 추상 메소드와 동일한 매개 변수 타입과 개수를 가지고 있는 생성자를 찾아 실행한다.

  ```java
  // 단순히 객체 생성 후 리턴만 하는 람다식
  (a,b) -> {return new 클래스(a,b);}
  
  // 생성자 참조
  클래스 :: new
