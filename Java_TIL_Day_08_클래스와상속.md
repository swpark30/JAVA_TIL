# Java_TIL_Day_08 (클래스와 상속)



- 목차
  - 6장 클래스
    - final 필드 (인스턴스 레벨에서의 상수)
    - 패키지 (package)
    - Java API (Application Programming Interface)
    - Getter와 Setter : 메소드
    - toString() 메소드
    - 어노테이션 (Annotation)

  - 7장 상속
    - 클래스 상속 (Inheritance)
    - 상속에서의 생성자






## 6장 클래스



### final 필드 (인스턴스 레벨에서의 상수)

- 최종적인 값을 갖고 있는 필드 (값을 변경할 수 없는 필드)

- 한 번 초기화되면 수정할 수 없는 필드

- 각 인스턴스 내에서 변하지 않음

- 클래스 레벨에서는 통용되지 않음

  

- final 필드의 초기값을 지정하는 방법

  - 필드 선언 시 : 단순히 고정된 값
  - 생성자에서 복잡한 초기화 코드가 필요하거나 객체 생성 시 외부 데이터로 
    초기화해야 하는 경우 생성자가 매개변수 받아서 초기화시킴



### 패키지 (package)

- 여러 클래스를 사용하는 방법

  1. 하나의 java 파일에 여러 개의 클래스 작성

     ```java
     // MultiClass.java
     
     class Add {}
     class Subtract {}
     class Multiply {}
     class Divide {}
     
     public class MultiClass {
         public static void main (String args[]){
             
         }
     }
     ```

     

  2. 같은 패키지의 다른 java 파일에서 만든 클래스 사용

     - 사용하고자 하는 클래스의 객체 생성해서 멤버 사용

  3. 다른 패키지에 있는 클래스 사용

     - import 패키지명.클래스명

     

#### 패키지(Package)란?

- 클래스를 기능별로 묶어서 그룹 이름을 붙여 놓은 것

- JDK 설치 시 미리 만들어진 많은 클래스들이 패키지로 묶여서 제공 (jar 파일로 압축되어 있음)

- 클래스 작성 시 패키지 선언을 사용하지 않으면 default package에 속하게 됨

  - 클래스를 컴파일 하는 과정에서 자동적으로 생성되는 폴더
  - 컴파일러는 클래스에 포함되어 있는 패키지 선언을 보고 파일 시스템의 폴더로 자동 생성

  

### Java API (Application Programming Interface)

- 자바를 사용하여 쉽게 구현할 수 있도록 한 클래스 라이브러리 집합



#### 패키지 사용 방법

- 다른 패키지의 클래스를 사용할 때는 클래스의 경로명을 선언하여 컴파일러에게 알림

  ```java
  import 패키지.클래스;
  
  예) import java.util.Scanner;
  	java.util 패키지에 있는 Scanner 클래스
  
  예) import java.text.DecimalFormat;
  	java.text 패키지에 있는 DecimalFormat 클래스
  
  하나의 패키지에 있는 여러 클래스를 사용할 경우
  	import 패키지.*;  //* : all의 의미
  	import java.util.*;
  		java.util 패키지에 있는 모든 클래스 
  	
      import 문 자동 추가 단축키 : Ctrl + Shift + O
  ```

  

#### 패키지 사용 이점

- 패키지를 계층 구조로 관리
  - 찾기 편리하고 효율적인 관리  가능
- 접근 제한
  - 접근 허용하지 않을 클래스는 다른 패키지에 저장 관리
- 동일한 이름의 클래스를 다른 패키지에서 사용 가능
  - 한 패키지에는 같은 이름의 클래스가 존재할 수 없지만
  - 패키지에 있으면 경로가 다르기 때문에 다른 클래스로 인식하여 구별 가능
- 높은 소프트웨어 재사용성
  - 유사한 기능을 수행하는 클래스나 인터페이스를 재 작성하지 않고 포함시켜서 사용
  - 경제적으로 코드를 작성/관리할 수 있고, 프로그램 개발 시간/노력을 단축시킬 수 있음 



### Getter와 Setter : 메소드

- Setter 

  - 멤버 필드의 값을 설정할 때 사용하는 메소드
  - setXxx() : set필드명()

  ```java
  예 : setName() - name 필드의 값 설정
  
  public void setName(String name) {
  	this.name = name;  //필드 값 설정
  }
  ```

  

- Getter

  - 멤버 필드의 값을 가져와서 사용할 때 사용하는 메소드

  - getXxx() : get필드명()

    ```java
    예 : getName() - name 필드의 값 반환
    
    public String getName(){
    				return name;
    }
    ```

  - 클래스 멤버 데이터 보호 방법

    - 클래스의 멤버 필드를 private으로 선언해서 클래스 외부에서는 직접 
      접근하지 못하게 하고 멤버 메소드를 통해서만 접근하도록 함



### toString() 메소드

- 객체가 텍스트 값으로 표시되거나 문자열이 예상되는 방식으로 참조될 때 
  자동으로 호출되는 메소드
- 객체가 가지고 있는 정보나 값들을 문자열 형태로 반환
- 객체 출력 시 toString() 자동 호출
- System.out.println(객체);



### 어노테이션 (Annotation)

- 프로그램에게 추가적인 정보를 제공해주는 메타데이터(metadata)

- 어노테이션 용도

  - 컴파일러에게 코드 작성 문법 에러 체크하도록 정보 제공
  - 소프트웨어 개발 툴이 빌드나 배치 시 코드를 자동 생성하게 정보 제공
  - 실행 시 특정 기능을 실행하도록 정보 제공

- 컴파일러에게 코드 작성 문법 에러 체크하도록 정보 제공 예

  - @Override 어노테이션

    - 메소드가 오버라이드(재정의)된 것임을 컴파일러에게 알려주어 
      컴파일러가 오버라이드를 검사하게 함
    - 정확히 오버라이드 되지 않았으면 컴파일러가 에러 발생

    

#### 오버라이딩

- 부모 클래스를 자식 클래스에서 상속 받을 때
- 부모 클래스의 메소드를 자식 클래스에 적합하게 재정의하는 것 (수정해서 사용하는 것)
- 바에서 기본적으로 제공하는 어노테이션
  - @Override
    - 메소드가 오버라이드 됐는지 검증
    - 클래스 또는 구현해야 할 인터페이스에서 해당 메소드를 찾을 수 없다면 컴파일 오류
  - @Deprecated
    - 메소드를 사용하지 말도록 유도
    - 만일 사용한다면 컴파일 경고
  - @SuppressWarnings
    - 컴파일 경고 무시
- 나중에 스프링 프레임워크에서 많은 어노테이션 사용
  - @Controller
  - @Service
  - @Repository






## 7장 상속



### 클래스 상속 (Inheritance)

- 현실 세계

  - 부모가 자식에게 물려주는 행위
  - 부모가 자식을 선택해서 물려줌

- 객체 지향 프로그램

  - 한 클래스에서 정의된 멤버 필드와 메소드를 다른 클래스가 물려 받는 것

  - 자식이 부모를 선택해서 물려받음

  - 상속 대상 : 부모 클래스의 필드와 메소드

    ```java
    class Parent{// 부모클래스
        
    }
    
    class Child extends Parent { // 상속된 자식 클래스
        
    }
    ```

    

#### 상속 개념의 활용

- 상속의 효과
  - 부모 클래스를 재사용해서 자식 클래스를 빨리 개발 개방
  - 클래스 사이의 멤버 중복 선언 불필요
  - 클래스 간 계층적 분류 및 관리 (효율성)
  - 유지 보수 편리성 제공
  - 즉, 경제적, 효율적, 편리하다
- 상속 대상 제한
  - 부모 클래스의 private 접근 갖는 필드와 메소드 제외
  - 부모 클래스가 다른 패키지에 있을 경우 default 접근 갖는 필드와 메소드 제외



#### 자바 상속의 특징

- 다중 상속을 지원하지 않는다
- extends 다음에는 오직 한 개의 클래스만 사용한다
- 상속 횟수 제한 없음
- 계층구조의 최상위에 있는 클래스는 java.lang.Object 클래스
- 따라서 모든 클래스는 Object 클래스로부터 상속 받는 것
  (Object 상속을 표현하지 않더라도 자동으로 상속 받게 됨)



### 상속에서의 생성자



#### 생성자 

- 객체(인스턴스) 초기화 목적
- 부모 클래스의 생성자 : 부모 클래스의 멤버 초기화
- 자식 클래스의 생성자 : 자식 클래스의 멤버 초기화
- 자식 클래스의 객체(인스턴스)가 생성될 때
  - 부모 클래스의 멤버를 상속받기 때문
  - 부모 클래스의 생성자와 자식 클래스의 생성자 모두 실행된다
    (자식 클래스의 객체가 생성될 때 자식 클래스의 생성자가 자동 호출되면서 
    부모 클래스의 생성자도 자동 호출됨) 



#### 생성자 실행 순서

1. 부모 클래스의 생성자 먼저 실행된다
2. 자식 클래스의 생성자가 실행된다 

