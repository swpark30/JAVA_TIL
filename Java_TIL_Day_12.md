# Java TIL Day 12



- 목차
  - 10장 예외 처리
    - 예외와 예외 클래스

  - 11장 기본 API 클래스
    - Object 클래스
    - System 클래스




## 10장 예외 처리



### 예외와 예외 클래스



- #### 오류의 종류

  - 에러(Error)
    - 컴파일 에러
    - 런타임 에러

  - 예외(Exception)




- #### 에러

  - 하드웨어의 잘못된 동작 또는 고장으로 인한 오류
  - **컴파일 에러** 
    - 문법에 맞지 않을 경우 발생
  - **런타임 에러**
    - 실행 시간 오류 - 프로그램 실행 중에 발생하는 오류
    - 잘못된 메모리 접근 오류, 논리 오류, 사용자의 잘못된 입력 등



- #### 예외

  - 사용자의 잘못된 조작 또는 개발자의 잘못된 코딩으로 인한 오류
  - 예외 처리를 통해 정상 실행 상태로 돌아갈 수 있음
  - 오류 중에서 대처가능한 오류



- #### 예외의 종류

  - **일반 예외(Exception)**
    - 컴파일 체크
    - 예외 처리 코드가 없으면 컴파일 오류 발생
  - **실행 예외(RuntimeException)**
    - 예외 처리 코드를 생략하더라도 컴파일이 되는 예외
    - 경험에 따라 예외 처리 코드 작성 필요



- #### 실행 예외

  - NullPointerException

    - 객체 참조가 없는 상태

    - null 값을 갖는 참조변수로 객체 접근 연산자인 도트(.)사용했을 때 발생

      ```java 
      String data = null;
      System.out.println(data.equals("자바"));
      ```

    

  - ArrayIndexOutOfBoundsException

    - 배열에서 인덱스 범위 초과하여 사용할 경우 발생
  
      ```java
      int[] arr = {1,2,3};
      System.out.println(arr[5]);
  
    
  
  - NumberFormatException

    - 숫자로만 이루어진 문자열을 숫자로 타입 변환할 때 발생하는 예외

    - 숫자에 문자가 섞여 있을 경우 숫자로 타입 변환시 발생
  
      ```java
      String data1 = "100"; // 문자열 일영영
      		
      		// 문자열 "100"을 숫자로 변환
      		int value1 = Integer.parseInt(data1);
      		
      		System.out.println(value1);
      		
      		// 숫자에 문자가 섞여 있는 경우 숫자로 타입 변환시 예외 발생
      		String data2 = "a100";
      		int value2 = Integer.parseInt(data2);// 여기에서 예외 발생
      ```
  
    
  
  - ClassCastException
  
    - 타입 변환이 되지 않을 경우 발생
    - 타입 변환 발생
      - 상위 클래스와 하위 클래스 간 발생
      - 구현 클래스와 인터페이스 간에 발생



- main 메소드의 args값 설정
  - 메뉴 / Run / Run Configurations.. / Arguments 탭 / Program Arguments 에서 값 설정
    값 설정시 스페이스로 인덱스를 나눔



- #### 예외 처리 코드

  - 예외 발생 시 프로그램의 갑작스러운 종료를 막고, 정상 실행을 유지할 수 있도록 처리하는 코드

  - try ~ catch ~ finally 블록을 이용해서 예외 처리 코드 작성

  - 예외 처리 형식

    ```java
    try{
        예외가 발생할 가능성이 있는 실행 문장
    }catch(처리할 예외 타입 선언){
        예외 처리 문장(예외가 발생되지 않으면 이 부분은 수행되지 않음)
    }finally{생략 가능
        예외 발생 여부와 상관없이 무조건 실행되는 문장
    }
    
    ex)
        Scanner sc = new Scanner(System.in);
    		int num1, num2;
    
    		System.out.print("정수1 입력 : ");
    		num1 = sc.nextInt();
    
    		System.out.print("정수2 입력 : ");
    		num2 = sc.nextInt();
    
    		try {
    			System.out.println("나누기 결과 : " + (num1 / num2)); 
                // 필요한 구문이지만 예외가 발생하는 상황(6/0 같은)도 있기때문에 예외처리를 한다.
    		} catch (Exception e) {
    			System.out.println("0으로 나눌 수 없습니다");
    		}
    		
    		sc.close();
    ```
  
  - 자동으로 하는 방법
    
    - 메뉴 / Source / Surround with / Try-catch block 을 사용하여 자동으로 생성
  
  
  
  - ##### 다중 catch
  
    - 예외 별로 예외 처리 코드 다르게 구현
  
    - 예외를 처리할때 catch 블록은 **위에서부터 차례대로** 검색한다
  
    - 위에 예외가 먼저 실행되면 아래 예외들은 넘어가게 된다.
  
      ```java
      public static void main(String[] args) {
      		int[] arr = { 1, 2, 3 };
      		
      		try {
      			System.out.println(arr[5]);
      			
      			System.out.println(Integer.parseInt("a100"));
      		} catch (ArrayIndexOutOfBoundsException e) {
      			System.out.println("배열의 인덱스를 벗어납니다.");
      		} catch(NumberFormatException e) {
      			System.out.println("숫자로 변환할 수 없습니다");
      		} finally {
      			System.out.println("다시 실행하세요");
      		}
      	}
      }
      ```
  
      

- 예외 처리 떠넘기기(throws)

  - 메소드에서 처리하지 않은 예외를 호출한 곳으로 떠넘기는 역할

  - 메소드 선언부 끝에 추가해서 작성

    ```java
    리턴타입 메소드명(매개변수...) throws 예외클래스1, 예외클래스2, ...{
    }
    ```

  - throws가 붙어 있는 메소드는 반드시 try 블록 내에서 호출되어야 하고 



예 : DAO의 메소드를 호출하는 컨트롤러에게 예외 처리를 떠 넘길 수 있다

​        Main(화면 : 입력) -> 컨트롤러 -> DAO -> 데이터베이스



- #### 사용자 정의 예외

  - 예외 클래스 선언

    - 자바 표준 API에서 제공하지 않는 예외 처리
    - 개발자가 직접 정의해서 처리
    - 애플리케이션 서비스와 관련된 예외
      - ex) 잔고 부족 예외, 계좌 이체 실패 예외, 회원 가입 실패 예외, .....등

  - 선언 방법

    - 예외 클래스를 상속받는 클래스로 생성

      1. 컴파일러가 체크하는 일반 예외로 선언
         - Exception 상속
      2. 컴파일러가 체크하지 않는 실행 예외 선언
         - RuntimeException 상속

      ```java
      public class XXXXException extends [Exception | RuntimeException]{
          
      }
      ```

  - 예외 발생시키기(throw)

    - 사용자가 정의한 예외를 발생시킬 수 있다.

    - 사용방법
  
      ```java
      throw new XXXException("예외 메시지");
      ```

  
  
  
  - getMessage()
  
    - 예외 발생시킬 때 생성자 매개값으로 전달해서 예외 객체 내부에 저장된 메시지 리턴한다.
  
      ```java
      throw new XXXException("예외 메시지");
      ----------------------------------------------
      } catch(Exception e){
          String message = e.getMessage()
      }    
      ```
  
      
  
  - printStackTrace()
  
    - 예외 발생 코드를 추적한 내용을 모두 콘솔에 출력
  
    - 프로그램 테스트하면서 오류 찾을 때 유용하게 활용한다
  
      ```java
      e.printStackTrace(); // 예외 발생 경로를 추적
      ```
  
      





## 11장 기본 API 클래스





### 자바 API 도큐먼트



- #### 자바 API(Application Programming Interface)

  - 자바에서 기본적으로 제공하는 라이브러리

  - 프로그램 개발에 자주 사용되는 클래스 및 인터페이스 모음

    

- #### API 도큐먼트

  - 쉽게 API를 찾아 이용할 수 있도록 문서화한 것

  - HTML 페이지로 작성되어 있어 웹 브라우저로 바로 볼 수 있음

    

- #### java.lang 패키지

  - 자바 프로그램의 기본적인 클래스를 담은 패키지



- #### Object 클래스

  - 자바의 최상위 부모 클래스

  - 다른 클래스를 상속하지 않으면 java.lang.Object 클래스 상속 암시

  - **Object 클래스의 메소드**

    - Object 클래스는 필드는 없고 메소드들로 구성

    - 모든 클래스가 Object 클래스를 상속하기 때문에 Object 클래스의 메소드는 모든 클래스에서 사용 가능

      - equals()

        - 객체 비교

        - 기본적으로 == 연산자와 동일한 결과 리턴

          ```java
          obj1.equals(obj2);
          ```

      - hashCode()

        - 객체의 해시코드 반환
        - 객체 해시코드
          - 객체를 식별할 하나의 정수값
          - Object의 hashCode() 메소드는 객체의 메모리 번지를 
            이용해 해시코드를 만들어서 리턴

      - toString()

        - 객체 문자정보 반환
        - 객체를 문자열로 표현한 값

      - clone()

        - 원본 객체의 필드 값과 동일한 값을 가지는 새로운 객체를 생성
        - 객체 복제 이유
          - 객체를 안전하게 보호하기 위해서
          - 신뢰하지 않는 영역으로 원본 객체를 넘겨야할 경우 원본 객체의
            데이터가 훼손될 수 있기 때문에 복제된 객체를 만들어서 넘겨 객체 보호
        - 복제 방법
          - 얕은 복제(thin clone)
            - 필드 값만 복제
            - 참조 타입 필드는 번지 공유
          - 깊은 복제(deep clone)
            - 참조하고 있는 객체도 복제

      - finalize()

        - 객체 소멸자
        - 참조하지 않는 배열이나 객체는 쓰레기 수집기(Garbage Collector : GC)가 
          자동으로 힙영역에서 소멸시킴

      - System.gc()

        - Garbage Collector 호출

- #### Throwable 클래스

  - Java 언어의 모든 에러와 예외의 수퍼 클래스

  

- #### Objects 클래스

  - Object의 유틸리티 클래스




- #### System 클래스

  - 용도
    - 운영체제의 기능 일부 이용 가능
      - 프로그램 종료, 키보드로부터 입력, 모니터 출력, 메모리 정리, 현재 시간 읽기 등
  - exit() 메소드
    - 프로그램 종료
    - 강제적으로 JVM 종료
    - int 매개값 지정 - 종료 상태 값
    - exit(0) / exit(1)
    - 비정상 종료 : 0 이외의 값
    - 어떤 값을 주더라도 종료는 된다
    - System.exit(0)











