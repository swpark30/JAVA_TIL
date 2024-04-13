# 16장 스트림과 병렬 처리



### Stream (스트림)

- 자바 8부터 추가된 컬렉션의 저장 요소를 하나씩 참조해서 람다식으로 처리할 수 있도록 해주는 반복자이다.

- 자바 7 이전까지는 List<String> 컬렉션에서 요소를 순차적으로 처리하기 위해 Iterator 반복자를 사용했지만 Stream을 사용해서 변경하면 아래와 같다

  ```java
  // Iterator 반복자 사용시
  List<String> list = Arrays.asList("홍길동", "신용권", "김자바");
  Iterator<String> iterator = list.iterator();
  while(iterator.hasNext()) {
      String name = iterator.next();
      System.out.println(name);
  }
  
  // Stream 사용시
  List<String> list = Arrays.asList("홍길동", "신용권", "김자바");
  Stream<String> stream = list.stream();
  stream.forEach(name -> System.out.println(name));
  ```

- Iterator와의 차이점

  - 람다식으로 요소 처리 코드를 제공한다.

    ```java
    Stream<Student> stream = list.stream();
    stream.forEach(s -> {
        String name = s.getName();
        int score = s.getScore();
        System.out.println(name + "-" + score); // List 컬렉션에서 Student를 가져와 람다식의 매개값으로 제공한다.
    });
    ```

  - 내부 반복자를 사용하므로 병렬 처리가 쉽다.

    - 외부 반복자란 개발자가 코드로 직접 컬렉션의 요소를 반복해서 가져오는 코드 패턴을 말한다.
    - 내부 반복자는 컬렉션 내부에서 요소들을 반복시키고, 개발자는 요소당 처리해야할 코드만 제공하는 코드 패턴을 말한다.
    - 내부 반복자는 요소들의 반복 순서를 변경하거나, 멀티 코어 CPU를 최대한 활용하기 위해 요소들을 분배시켜 병렬 작업을 할 수 있게 도와준다.

  - 중간 처리와 최종 처리 작업을 수행한다.

    - 중간 처리 - 매핑, 필터링, 정렬을 수행
    - 최종 처리 - 반복, 카운팅, 평균, 총합 등의 집계 처리를 수행



### 스트림의 종류

- BaseStream 인터페이스를 부모로 해서 자식 인터페이스들이 상속 관계를 이루고 있다.

- BaseStream

  - Stream
  - IntStream
  - LongStream
  - DoubleStream

- BaseStream은 직접적으로 사용되지 않고 하위 스트림들이 직접적으로 이용되는 스트림이다.

- 이 스트림 인터페이스의 구현 객체는 다양한 소스로부터 얻을 수 있다.

  ```java
  Stream<Student> stream = studentList.stream(); // 컬렉션으로부터 스트림 얻기
  
  Stream<String> strStream = Arrays.stream(strArray); // 배열로부터 스트림 얻기
  
  IntStream stream = IntStream.rangeClosed(1, 100); // 숫자 범위로부터 스트림 얻기
  
  Path path = Paths.get("src/sec02/stream_kind/linedata.txt"); // 파일로부터 스트림 얻기
  Stream<String> stream = Files.lines(path, Charset.defaultCharset()); // 운영체제의 기본 문자셋
  
  Stream<Path> stream = Files.list(path); // 디렉토리로부터 스트림 얻기
  ```



### 스트림 파이프라인

- 리덕션(Reduction)

  - 대량의 데이터를 가공해서 축소하는 것
  - 데이터의 합계, 평균값, 카운팅, 최대값, 최소값 등이 대표적인 리덕션의 결과물이다.

- 컬렉션 요소를 리덕션의 결과물로 바로 집계할 수 없을 경우에는 집계하기 좋도록 필터링, 매핑, 정렬, 그룹핑 등의 중간 처리가 필요하다.

- 스트림은 중간 처리와 최종 처리를 파이프라인으로 해결한다.

- 파이프라인

  - 여러 개의 스트림이 연결되어 있는 구조

  - 최종 처리를 제외하고는 모두 중간 처리 스트림이다.

  - 자바 코드로 표현

    ```java
    Stream<Member> maleFemaleStream = list.stream();
    Stream<Member> maleStream = maleFemaleStream.filter(m -> m.getSex()==Member.MALE);
    IntStream ageStream = maleStream.mapToInt(Member::getAge);
    OptionalDouble optionalDouble = ageStream.average();
    double ageAvg = optionalDouble.getAsDouble();
    
    // 로컬 변수를 생략하고 연결한 파이프라인 코드
    double ageAvg = list.stream()
        .filter(m -> m.getSex()==Member.MALE)
        .mapToInt(Member::getAge)
        .average()
        .getAsDouble();
    ```

- 중간 스트림이 생성될 때 요소들이 바로 중간 처리 되는 것이 아니라 최종 처리가 시작되기 전까지 중간 처리는 지연 된다.

- 스트림 파이프라인에서 메소드의 리턴 타입이 스트림이면 중간 처리 메소드이고, 기본 타입이거나 OptionalXXX라면 최종 처리 메소드이다.



### 필터링(distinct(), filter())

- 중간 처리 기능으로 요소를 걸러내는 역할을 한다.
- distinct()와 filter() 메소드는 모든 스트림이 가지고 있는 공통 메소드이다.
- distinct() 메소드는 중복을 제거하는데 Stream의 경우 Object.equals(Object)가 true이면 동일한 객체로 판단하고 중복을 제거한다.
- filter() 메소드는 매개값으로 주어진 Predicate가 true를 리턴하는 요소만 필터링한다.



### 매핑(flatMapXXX(), mapXXX(), asXXXStream(), boxed())

- 중간 처리 기능으로 스트림의 요소를 다른 요소로 대체하는 작업
- flatMapXXX() 메소드
  - 요소를 대체하는 복수 개의 요소들로 구성된 새로운 스트림을 리턴한다.
- mapXXX() 메소드
  - 요소를 대체하는 요소로 구성된 새로운 스트림을 리턴한다.
- asXXXStream(), boxed() 메소드
  - asDoubleStream() 메소드는 IntStream의 int 요소 또는 LongStream의 long 요소를 double 요소로 타입 변환해서 DoubleStream을 생성한다.
  - boxed() 메소드는 int, long, double 요소를 Integer, Long, Double 요소로 박싱해서 Stream을 생성한다.



### 정렬(sorted())

- 요소가 최종 처리되기 전에 중간 단계에서 요소를 정렬해서 최종 처리 순서를 변경할 수 있다.
- Comparable을 구현한 요소에서만 sorted() 메소드를 호출해야 한다. 



### 루핑(peek(), forEach())

