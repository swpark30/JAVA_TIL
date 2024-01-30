# Java_TIL_Day_13 (기본 API 클래스)



- 목차

  - 11장 기본 API 클래스

    - System 클래스
    - String 클래스
    - 정규 표현식 (Regular Expression)
    - Arrays 클래스
    - Wrapper 클래스(포장 클래스)
    - Math, Random 클래스
    - Date, Calendar 클래스
    - Format 클래스

    



## 11장 기본 API 클래스



### System 클래스



- #### 현재시간 읽기 

  - currentTimeMillis() : 밀리 세컨드

  - nanoTime() : 나노 세컨드 단위의 값 리턴

  - 주로 프로그램 실행 소요 시간 구할 때 이용

    

- #### 시스템 프로퍼티 읽기(getProperty())

  - 시스템 프로퍼티
    - JVM이 시작할 때 자동으로 설정되는 시스템의 속성값
    - 대표적인 키와 값 (key, value)




- #### 환경 변수 읽기 (getenv())

  - 운영체제가 제공하는 환경 변수 값(문자열)을 읽음



### String 클래스



- #### String 생성자

  - 바이트 배열을 문자열로 변환



- #### String 메소드

  - 문자열의 추출, 비교, 찾기, 분리, 변환 등과 같은 다양한 메소드 포함

    

  - **문자 추출 (chatAt(인덱스))**
    - 주어진 인덱스의 문자 리턴

      ```java
      String subject = "자바 프로그래밍";
      char charValue = subject.charAt(3); // "프"가 리턴된다
      ```
      
      
      
      
    
  - **바이트 배열로 변환(getBytes())**
    - 시스템의 기본 문자셋으로 인코딩된 바이트 배열 얻기
      
      ```java
      byte[] bytes = "문자열".getBytes();
      ```
      
      
    
  - **문자열 찾기(indexOf("문자열"))**
  
    - 매개값으로 주어진 문자열이 시작되는 인덱스 반환
  
    - 사용방법
  
      ```java
      String subject = "자바 프로그래밍";
      int index = subject.indexOf("프로그래밍");
      ```
  
    
  
  - **문자열 대치(replace("문자열1", "문자열2"))**
  
    - 첫 번째 매개값인 문자열을 찾아서 두번째 매개값인 문자열로 대치
  
    - 사용방법
  
      ```java
      String oldstr = "자바 프로그래밍";
      String newstr = oldstr.replace("자바", "JAVA");
      ```

    
  
  - **문자열 잘라내기 : substring()**
  
    - 문자열  일부 추출

    - 주어진 시작과 끝 인덱스 사이의 문자열 추출

    - 사용방법

      ```java
      String ssn = "880812-1423454";
      String firstNum = ss.substring(0, 6); // 0번째 인덱스부터 6번째 인덱스까지
      String secondNum = ssn.substring(7); // 7번째 인덱스부터 끝까지
      ```

    

  - **알파벳 소/대문자 변경 : toLowerCase() / toUpperCase()**

    - 문자열을 모두 소문자/대문자로 바꾼 새로운 문자열을 생성한 후 반환

    - 사용방법

      ```java
      String original = "Java Programming";
      String lowerCase = original.toLowerCase(); // 결과 : java programming
      String upperCase = original.toUpperCase(); // 결과 : JAVA PROGRAMMING
      ```
      
      
      
      

  - **문자열의 앞뒤 공백 잘라내기 : trim()**

    - 문자열의 앞뒤 공백을 제거한 새로운 문자열을 생성해서 반환

      

  - **문자열 변환 : valueOf()**

    - 기본 타입의 숫자값을 문자열로 변환

    - String 클래스에 매개변수 타입별로 메소드 오버로딩되어 있음

      
  
  - **문자열 분리 방법**
  
    - String split() 메소드 이용

    - java.util.StringTokenizer 클래스 이용

    - 토큰 

      - 쪼개진 문자열 단위

    - String split()
  
      - 구분자로 문자열 분리 (123-456)
  
      - 정규표현식을 구분자로 해서 부분 문자열 분리
  
      - 배열에 저장하고 리턴
  
      - String tel = “010-1234-1234”;

        ```java
        String[] tels = tel.split(“-”); // "-" 기준으로 분리된다.
        ```

        
  
    - StringTokenizer()
  
      - 문자열에서 구분자로 분리한 토큰을 반환

      - 사용방법
  
        ```java
        String text = "홍길동/이수홍/박연수";
        StringTokenizer st = new StringTokenizer(text,"/");
        
        for(int i=0;i<countTokens;i++) {
        			String token = st.nextToken();
        			System.out.println(token);
        		}
        ```
  
      
  
  - StringBuffer(), StringBuilder()
  
    - 버퍼에 문자열 저장
    - buffer : 데이터를 임시로 저장하는 메모리
      - 버퍼 내부에서 추가, 수정, 삭제 작업 가능
    - 멀티 스레드 환경 : StringBuffer 사용
    - 단일 스레드 환경 : StringBuilder 사용
  
  
### 정규 표현식 (Regular Expression)

- 문자열이 정해져 있는 형식으로 구성되어 있는지 검증할 때 사용



- #### Pattern 클래스

  - 정규 표현식으로 문자열을 검증하는 역할
  - matches(“정규식”, “입력된 문자열”);
  - 결과는 boolean 타입

​	

### Arrays 클래스

- 배열 조작 기능을 가지고 있는 클래스
- 복사, 정렬, 검색 등
- 제공하는 정적 메소드 : Arrays.copyOf()처럼 정적 메소드이기 때문에그냥 클래스 이름을 써서 사용하면 된다.



- #### Arrays 클래스 메소드

  - 배열 복사

    - Arrays.copyOf(원본배열, 복사할 길이)

      - 0 ~ (복사할 길이 -1)까지 항목 복사

      ```java
      char[] arr1 = {'J','A','V','A'};
      char[] arr2 = Arrays.copyOf(arr1, arr1.length);
      ```
  
      
  
    - copyOfRange (원본배열, 시작 인덱스, 끝 인덱스)
  
      - 시작 인덱스 ~ (끝 인덱스-1)까지 항목 복사
  
      ```java
      char[] arr1 = {'J','A','V','A'};
      char[] arr2 = Arrays.copyOfRange(arr1, 1, 3); // 1 ~ (3-1)인덱스까지
      ```

      

    - System.arraycopy()

      

  - 배열 항목 정렬 : Arrays.sort(배열)
  
    - 항목 오름차순으로 정렬
  
    - 기본 타입이거나 String배열 자동 정렬
  
      
  
  - 배열 항목 검색 : Arrays.binarySearch()
  
    - 특정 값 위치한 인덱스 얻는 것
    - Arrays.sort(배열)로 먼저 정렬하고
      Arrays.binarySearch(배열, 찾는 값) 메소드로 항목을 찾음



### Wrapper 클래스(포장 클래스)



- #### Wrapper 객체

  - 기본타입(byte, char, short, int, long, float, double, boolean) 값을 내부에 두고 포장하는 객체

    

- #### 박싱(Boxing)과 언박싱(Unboxing)

  - 박싱(Boxing)

    - 기본 타입의 값을 Wrapper 객체로 만드는 과정

    - 기본 타입 (int) : 100

    - Wrapper 클래스 : Integer

    - Integer obj1 = new Integer(100);

    - 사용 방법

      1. 생성자 이용

      2. valueOf() 메소드 이용

         ```java
         Integer obj = Integer.valueOf(1000);
         ```
         
         

  - 언박싱(Unboxing)

    - 포장 객체에서 기본 타입의 값을 얻어 내는 과정

      ```java
      int value = obj.intValue();
      ```

      

  - 자동 박싱과 언방식

    - 자동 박싱
      - 포장 클래스 타입에 기본값이 대입될 경우 발생
      - Integer obj = 100; // 자동 박싱
    - 자동 언박싱
      - 기본 타입에 포장 객체가 대입될 경우 발생

  

- #### 숫자 문자열을 기본 타입 값으로 변환

  - parse + 기본타입명() : 정적 메소드
  
    ```java
    int num =Integer.parseInt(“1000”);
    ```
  
    

### Math, Random 클래스



- #### Math 클래스

  - 수학 계산에 사용할 수 있는 정적 메소드 제공

    

- #### Random 클래스

  - boolean, int, long, float, double 난수 리턴

  - 난수를 생성하는 알고리즘에 사용되는 종자값(seed) 설정 가능

    - 종자값이 같으면 동일한 난수 발생

      

  - Random() // seed 값 없을 경우

    - 현재 시간을 초기값으로 하는 난수 발생
    - 실행할 때마다 다른 난수 발생

  - Random(seed)

    - seed 값을 초기값으로 하는 난수 발생
    - 실행할 때마다 동일한 난수 발생



### Date, Calendar 클래스



- #### Date 클래스

  - 날짜를 표현하는 클래스

  - import java.uti.Date;

    ```java
    Date now = new Date();
    ```
    
    

- #### Calendar 클래스

  - 달력을 표현한 추상 클래스
  
  - OS에 설정된 시간대(TimeZone) 기준의 Calendar 객체 얻기
  
    ```java
    Calendar now = Calendar.getInstance();
    ```
  
    

### Format 클래스

- 숫자와 날짜를 원하는 형식의 문자열로 변환
- 종류
  - 숫자 형식 : DecimalFormat
    - 적용할 패턴 선택해 생성자 매개값으로 지정 후 객체 생성
  - 날짜 형식 : SimpleDateFormat
  - 매개변수화된 문자열 형식 : MessageFormat
    - 일정한 형식으로 문자열 포멧
    - 문자열 데이터가 들어갈 자리를 표시해 두고 프로그램 실행 중에 동적으로
      데이터를 삽입해 문자열 완성



### Java.time 패키지

- 날짜와 시간을 나타내는 여러가지 API가 새롭게 추가됨

