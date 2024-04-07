# Java_TIL_Day_17 컬렉션 프레임워크



- 목차

  - 15장 컬렉션 프레임워크
    - 컬렉션이란?
    - 프레임워크란?
    - 컬렉션 프레임워크
    - 컬렉션 프레임워크의 주요 인터페이스
      - List(인터페이스)컬렉션
      - Set 컬렉션
      - Map 컬렉션
      - Collections 클래스 활용



## 15장 컬렉션 프레임워크




### 컬렉션이란?

- 사전적 의미로 요소(객체)를 수집해 저장하는것
- 자바에서는 객체를 수집해서 저장하는 역할



### 프레임워크란?

- 표준화, 정형화된 체계적인 프로그래밍 방식

- 미리 정해진 방식대로 프로그램을 작성

  - 누가 작성하든 프로그램이 표준화되기 때문에 프로그램 유지보수하기 쉬움

    

### 컬렉션 프레임워크

- 컬렉션을 다루기 위한 표준화된 프로그래밍 방식

  - 많은 양의 데이터와 객체들을 효율적으로 추가, 삭제, 검색 등을 할 수 있도록 제공되는 컬렉션 라이브러리
  - 인터페이스를 통해서 정형화된 방법으로 다양한 컬렉션 클래스 이용

- 사용이유

  - 배열의 문제점을 해결하고 객체들을 효율적으로 추가, 삭제, 검색 등을 할 수 있도록 하기 위해서 사용한다

    

- 배열의 문제점

  - 생성시 크기 고정되고 사용 시 크기 변경불가
    - 불특정 다수의 객체를 저장하기에는 문제가 있다

  - 객체를 삭제했을 때 해당 인덱스가 빈 상태로 있다
    - 새로운 객체를 저장하려면 어디 인덱스가 비었는지 확인하는 코드가 필요해서 불편하다.

  



### 컬렉션 프레임워크의 주요 인터페이스



### List(인터페이스)컬렉션

- #### 특징

  - 순서가 있는 데이터의 집합 (객체를 일렬로 늘어놓은 구조)

  - 인덱스로 관리

  - 객체 자체를 저장하는 것이 아니라 객체의 번지를 참조한다.

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
      //제네릭을 사용하지 않을 경우 // 자바 4 이전
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
      - 빈번하게 일어나면 LinkedList를 사용하는 것이 좋다.
      
    - Arrays.asList(T...a) 메소드
  
      - 고정된 객체들로 구성된 List를 생성할 때 사용
  
        ```java
        List<String> list1 = Arrays.asList("홍길동", "신용권", "감자바");
        for(String name : list1) {
            System.out.println(name);
        }
        ```

    

  - **Vector**

    - ArrayList와 동일한 내부구조
  
      ```java
      List<E> list = new Vector<E>();
      ```
  
    - ArrayList와 다른점
  
      - Vector는 동기화된 메소드로 구성되어 있기 때문에 멀티스레드가 동시에 이 메소드들을 실행할 수 없고, 하나의 스레드가 실행을 완료해야만 다른 스레드를 실행할 수 있다.
  
      - 멀티 스레드 환경에서 안전하게 객체를 추가, 삭제할 수 있다. (스레드 안전)
  
        
  
  - **LinkedList**
  
    - ArrayList와 사용 방법은 동일하지만 내부 구조가 다르다
  
    - ArrayList는 내부 배열에 객체를 저장해서 인덱스로 관리하지만, LinkedList는 인접 참조를 링크해서 체인처럼 관리
  
    - 특정 인덱스의 객체를 제거하거나 삽입할때 앞뒤 링크만 변경되고 나머지 링크는 변경되지 않는다. 
  
    - 만일 중간에서 제거 하면 제거한 부분 앞뒤의 체인이 서로 연결되어 변경된다.
  
      ```java
      List<E> list = new LinkedList<E>();
      ```
  
    - 처음 생성될 때에는 어떠한 링크도 만들어지지 않기 때문에 내부는 비어 있다고 보면 된다.
  
    - 끝부분에서부터 순차적으로 추가/삭제하는 경우는 ArrayList가 빠르지만 중간에 추가/삭제하는 경우에는 LinkedList가 더 빠르다.
  
      

### Set 컬렉션

- #### 특징

  - 수학의 집합에 비유
  - 저장 순서가 유지되지 않음

    - get() 메소드 없음
  - 객체 중복 저장 불가

  

- **Iterator** 메소드

  - Set 컬렉션은 인덱스로 객체를 검색해서 가져오는 메소드가 없어서 전체 객체를 대상으로 한번씩 가져오는 반복자를 제공한다.

    ```java
    Set<String> set = ...;
    Iterator<String> iterator = set.iterator(); 
    ```




- #### 구현 클래스

  - **HashSet**

    - 객체를 순서없이 저장

    - 동일 객체 및 동등 객체는 중복 저장하지 않음

    - 동등 객체 판단 방법

      - 객체 저장하기 전에 hashCode()를 호출해서 해시코드를 얻어내어 저장되어 있는 객체들의 
        해시코드와 비교하여 동일한지 판단한다.
      - 동일한 해시코드가 있다면 다시 equals() 메소드로 두 객체를 비교해서 true가 나오면 동일한 객체로 판단하고 중복 저장하지 않는다.
      
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

  - 키와 값의 쌍으로 이루어진 Entry 객체를 저장하는 구조
  - 키와 값은 모두 객체
  - 키는 중복될 수 없지만 값은 중복 저장 가능
  - 기존에 저장된 키와 동일한 키로 값을 저장하면 기존의 값은 없어지고 새로운 값으로 대치된다.

  

- #### 구현 클래스

  - **HashMap**

    - Map 인터페이스를 구현한 대표적인 Map 컬렉션이다.
    - 키로 사용할 객체는 hashCode()와 equals() 메소드를 재정의해서 동등 객체가 될 조건을 정해야 한다.
    - 키 타입은 String 많이 사용
    - 키와 값의 타입은 기본 타입을 사용할 수 없고 클래스 및 인터페이스 타입만 가능하다.

    ``` java
    Map<K, V> map = new HashMap<K, V>(); // K : 키타입 V : 값타입
    
    map
    map.get(K); // 입력한 K값에 대한 V값을 가져옴
    ```

    

  - **HashTable**

    - 키 객체 만드는 법과 내부 구조는 HashMap과 동일
    - HashMap과의 차이점
      - HashTable은 동기화된 메소드로 구성되어 있기 때문에 멀티 스레드가 동시에 이 메소드들을 실행할 수 없다.
      - 여러개의 스레드가 동시에 Hashtable에 접근해서 객체를 추가,
        삭제하더라도 스레드에 안전
    - Hashtable은 멀티 스레드 환경에서 주로 사용
    - HashMap은 단일 스레드 환경에서 주로 사용
      - 동기화하면 객체 잠금이 발생해서 성능이 저하된다.

    

  - **Properties** 

    - Hashtable의 하위 클래스이기 때문에 Hashtable의 모든 특징을 그대로 가지고 있다.

    - 차이점

      - Hashtable은 키와 값을 다양한 타입으로 지정이 가능한데 Properties는 키와 값을 String 타입으로 제한한 컬렉션이다.

    - 어플의 옵션 정보, 데이터베이스 연결 정보, 국제화 정보가 저장된 프로퍼티 파일을 읽을 때 주로 사용한다.

    - 프로퍼티 파일

      - 키와 값이 = 기호로 연결되어 있는 ISO 8859-1 문제셋으로 저장된다.

      - 이 문제셋으로 직접 표현할 수 없는 한글은 유니코드로 변환되어 저장된다.

        ```properties
        driver=oracle.jdbc.OracleDirver
        url=jdbc:oracle:thin:@localhost:1521:orcl
        username=scott
        password=tiger
        ```

      - 파일 읽기

        - Properties 객체를 생성하고, load() 메소드를 호출하면 된다.

          ```java
          Properties properties = new Properties();
          // 데이터를 읽기 위해 FileReader 객체를 매개값으로 받는다.
          properties.load(new FileReader("C:/~/database.properties")) 
          ```

      - 프로퍼티 파일은 일반적으로 클래스 파일과 함께 저장된다.

      - 클래스 파일과 동일한 위치에 있는 프로퍼티 파일을 읽고 객체를 생성하는 코드

        ```java
        String path = 클래스.class.getResource("database.properties").getPath();
        path = URLDecoder.decode(path, "utf-8"); // 경로에 한글이 있을 경우 한글을 복원
        Properties properties = new Properties();
        properties.load(new FileReader(path));
        ```

      - Properties 객체에서 해당 키의 값을 읽으려면 getProperty() 메소드를 사용한다.

  - LinkedHashMap
  - TreeMap



### 검색 기능을 강화시킨 컬렉션

- 검색 기능을 강화시킨 TreeSet과 TreeMap이 있다.

- 이 컬렉션들은 이진 트리를 이용해서 계층적 구조(Tree 구조)를 가지면서 객체를 저장한다.

- 이진 트리 구조
  - 여러 개의 노드가 트리 형태로 연결된 구조
  - 루트 노드라고 불리는 하나의 노드에서부터 시작해서 각 노드에 최대 2개의 노드를 연결할 수 있는 구조
  - 부모 노드의 값보다 작은 노드는 왼쪽에, 큰 노드는 오른쪽에 위치시킨다.
  - 범위 검색을 쉽게 할 수 있는 이유
    - 값들이 정렬되어 있어 그룹핑이 쉽기 때문이다.

- **TreeSet**
  - 이진 트리를 기반으로한 Set 컬렉션

  - 객체를 저장하면 자동으로 정렬

  - 부모값과 비교해서 낮은 것은 왼쪽 자식 노드에, 높은 것은 오른쪽 자식 노드에 저장

    ```java
    TreeSet<E> treeSet = new TreeSet<E>();
    ```

  - descendingIterator() :  내림차순으로 정렬된 Iterator 객체를 리턴한다.

  - descendingSet() : 내림차순으로 정렬된 NavigableSet 객체를 리턴한다.

- **TreeMap**

  - 이진 트리를 기반으로 한 Map 컬렉션

  - TreeSet과의 차이점

    - 키와 값이 저장된 Map.Entry를 저장한다는 점이다.

  - 객체를 저장하면 자동으로 정렬된다.

  - 부모 키값과 비교해서 키 값이 낮은 것은 왼쪽 자식 노드에, 키 값이 높은 것은 오른쪽 자식 노드에 Map.Entry 객체를 저장한다.

    ```java
    TreeMap<K, V> treeMap = new TreeMap<K, V>();
    ```

  - 정렬 관련 메소드

    - NavigableSet<K>
    - NavigableMap<K,V>

- Comparable과 Comparator

  - TreeSet의 객체와 TreeMap의 키는 저장과 동시에 자동 오름차순으로 정렬된다.

  - 숫자 타입은 값으로, 문자열은 유니코드로 정렬한다.

  - Integer, Double, String은 모두 Comparable 인터페이스를 구현하고 있다.

  - TreeSet의 객체와 TreeMap의 키가 Comparable을 구현하고 있지 않을 경우에는 저장하는 순간 ClassCastException이 발생한다.

  - Comparable 비구현 객체를 정렬하는 방법은 생성장의 매개값으로 정렬자(Comparator)를 제공하면 된다.

    ```java
    TreeSet<E> treeSet = new TreeSet<E>(new AscendingComparator()); // 오름차순 정렬자
    
    TreeMap<K, V> treeSet = new TreeMap<K, V>(new DescendingComparator()); // 내림차순 정렬자
    ```




### LIFO와 FIFO 컬렉션






### Collections 클래스 활용

- 컬렉션을 다루는 유용한 메소드 지원(static)
  - sort() : 정렬
  - reverse() : 역순으로 정렬
  - max() / min()

