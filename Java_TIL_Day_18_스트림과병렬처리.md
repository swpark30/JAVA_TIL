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

- 요소 전체를 반복하는 것을 말한다.
- peek() 메소드
  - 중간 처리 메소드
  - 중간 처리 단계에서 전체 요소를 루핑하면서 추가적인 작업을 하기 위해 사용
- forEach()
  - 최종 처리 메소드
  - 파이프라인 마지막에 루핑하면서 요소를 하나씩 처리한다.



### 매칭(allMatch(), anyMatch(),  noneMatch())

- 스트림 클래스는 최종 처리 단계에서 요소들이 특정 조건에 만족하는지 조사할 수 있도록 메소드를 제공한다.

- allMatch()

  - 모든 요소들이 매개값으로 주어진 Predicate의 조건을 만족하는지 조사한다.

- anyMatch()

  - 최소한 한 개의 요소가 매개값으로 주어진 Predicate의 조건을 만족하는지 조사한다.

- noneMatch()

  - 모든 요소들이 매개값으로 주어진 Predicate의 조건을 만족하지 않는지 조사한다.

    

 ### 기본 집계(sum(), count(), average(), max(), min())

- 집계는 최종 처리 기능으로 요소들을 처리해서 카운티ㅇ, 합계, 평균값, 최대값, 최소값 등과 같이 하나의 값으로 산출하는 것을 말한다.
- Optional 클래스
  - 단순히 집계 값만 저장하는 것이 아니라, 집계 값이 존재하지 않을 경우 디폴트 값을 설정할 수도 있고, 집계 값을 처리하는 Consumer도 등록할 수 있다.



### 커스텀 집계(reduce())

- 프로그램화해서 다양한 집계 결과물을 만들 수 있도록 하기위해 제공한다.



### 수집(collect())

- 스트림은 요소들을 필터링 또는 매핑한 후 요소들을 수집하는 최종 처리 메소드인 collect를 제공한다.

- 필요한 요소만 컬렉션으로 담을 수 있고, 요소들을 그룹핑한 후 집계할 수 있다.

- 필터링한 요소 수집

  - 스트림의 collect 메소드는 필터링 또는 매핑된 요소들을 새로운 컬렉션에 수집하고, 이 컬렉션을 리턴한다.

- 사용자 정의 컨테이너에 수집

  - List, Set, Map 같은 컬렉션이 아니라 사용자 정의 컨테이너 객체에 수집하는 방법

  - 스트림에서 읽은 남학생을 MaleStudent에 수집하는 코드

    ``` java
    Stream<Student> totalStream = totalList.stream();
    Stream<Student> maleStream = totalStream.filter(s -> s.getSex() == Student.Sex.MALE);
    
    Supplier<MaleStudent> supplier = ()->new MaleStudent();
    BiConsumer<MaleStudent, Student> accumulator = (ms, s)-> ms.accumulate(s);
    BiConsumer<MaleStudent, MaleStudent> combiner = (ms1, ms2)-> ms1.combine(ms2);
    
    MaleStudent maleStudent = maleStream.collect(supplier, accumulator, combiner);
    
    // 상기 코드에서 변수를 생략
    MaleStudent maleStudent = totalList.Stream()
        .filter(s -> s.getSex() == Student.Sex.MALE)
        .collect(
    		() -> new MaleStudent(),
        	(r, t) -> r.accumulate(t),
        	(r1,r2) -> r1.combine(r2)
    );
    
    // 람다식을 메소드 참조로 변경할 시
    MaleStudent maleStudent = totalList.Stream()
        .filter(s -> s.getSex() == Student.Sex.MALE)
        .collect(MaleStudent :: new, MaleStudent :: accumulate, MaleStudent :: combine);
    ```

- 요소를 그룹핑해서 수집

  - collect()를 호출할 때 Collectors의 groupingBy() 또는 groupingByConcurrent()가 리턴하는 Collector를 매개값으로 대입하면 된다.



### 병렬 처리

- 멀티 코어 CPU 환경에서 하나의 작업을 분할해서 각각의 코어가 병렬적으로 처리하는 것
- 병렬 처리의 목적
  - 작업 처리 시간을 줄이기 위해
- 동시성(Concurrency)과 병렬성(Parallelism)
  - 멀티 스레드는 동시성 또는 병렬성으로 실행되기 때문에 이 용어들에 대해 정확히 이해하는 것이 좋다.
  - 동시성
    - 멀티 작업을 위해 멀티 스레드가 번갈아가며 실행하는 성질
  - 병렬성
    - 멀티 작업을 위해 멀티 코어를 이용해서 동시에 실행하는 성질
    - 데이터 병렬성
      - 전체 데이터를 쪼개어 서브 데이터들로 만들고 이 서브 데이터들을 병렬 처리해서 작업을 빨리 끝내는 것
      - 자바 8에서 지원하는 병렬 스트림은 데이터 병렬성을 구현한 것이다.
      - 예를 들어 쿼드 코어 CPU일 경우 4개의 서브 요소들로 나누고, 4개의 스레드가 각각의 서브 요소들을 병렬 처리한다.
    - 작업 병렬성
      - 서로 다른 작업을 병렬 처리하는 것
      - 대표적인 예는 웹 서버이다.
      - 웹 서버는 각각의 브라우저에서 요청한 내용을 개별 스레드에서 병렬로 처리한다.
  - 싱글 코어 CPU를 이용한 멀티 작업은 병렬적으로 실행되는 것처럼 보이지만, 사실은 번갈아가며 실행하는 동시성 작업이다. 
- 포크조인(ForkJoin) 프레임워크
  - 병렬 스트림은 요소들을 병렬 처리하기 위해 포크조인 프레임워크를 사용한다.
  - 병렬 스트림을 이용하면 런타임 시에 포크조인 프레임워크가 동작한다.
  - 포크 단계
    - 전체 데이터를 서브 데이터로 분리한다.
    - 서브 데이터를 멀티 코어에서 병렬로 처리한다.
  - 조인 단계
    - 서브 결과를 결합해서 최종 결과를 만들어 낸다.
  - 병렬 처리 스트림은 실제로 포크 단계에서 차례대로 요소를 4등분하지 않고 내부적으로 서브 요소를 나누는 알고리즘이 있다.
  - 이 프레임워크는 포크와 조인 기능 이외에 스레드풀인 ForkJoinPool을 제공하고 이걸 사용해서 작업 스레드를 관리한다.
- 병렬 스트림 생성
  - 병렬 스트림을 이용할 경우에는 백그라운드에서 포크조인 프레임워크가 사용되기 때문에 쉽게 병렬처리를 할 수 있다.

- 병렬 처리 성능
  - 스트림 병렬 처리가 스트림 순차 처리보다 항상 실행 성능이 좋다고 판단해서는 안 된다.
  - 병렬 처리에 영향을 미치는 3가지 요인
    - 요소의 수와 요소당 처리 시간
      - 컬렉션에 요소의 수가 적고 요소당 처리 시간이 짧으면 순차 처리가 오히려 병렬 처리보다 빠를 수 있다.
      - 병렬 처리는 스레드풀 생성, 스레드 생성이라는 추가적인 비용이 발생하기 때문이다.
    - 스트림 소스의 종류
      - 배열은 인덱스로 요소를 관리하기 때문에 포크 단계에서 요소를 쉽게 분리할 수 있어 병렬 처리 시간이 절약된다.
      - 반면에 HashSet,TreeSet은 요소 분리가 쉽지 않고, LinkedList 역시 링크를 따라가야 하므로 요소 분리가 쉽지 않다.
    - 코어의 수
      - 싱글 코어 CPU일 경우에는 순차 처리가 빠르다. 병렬 스트림을 사용할 경우 스레드의 수만 증가하고 동시성 작업으로 처리되기 때문에 좋지 못한 결과를 준다.
      - 코어 수가 많으면 많을수록 병렬 작업 처리 속도는 빨라진다.



