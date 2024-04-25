# 17장 JavaFX



### JavaFX 개요

- 크로스 플랫폼에서 실행하는 리치 클라이언트 애플리케이션을 개발하기 위한 그래픽과 미디어 패키지를 말한다.
- 자바 7부터 JDK에 포함되어 있다.
- 패키지 연혁
  - AWT -> Swing -> JavaFX
- 어도비의 플래쉬(flash)와 마이크로소프트의 실버라이트(silverlight)의 대항마로 만들어졌다.
- javaFX 애플리케이션 구성 요소
  - 레이아웃
    - 자바 코드 파일 or FXML 파일
  - 외관 및 스타일
    - CSS 파일
  - 비즈니스 로직
    - 자바 코드 파일
  - 리소스
    - 그림 파일
    - 동영상 파일 등등



### JavaFX 애플리케이션 개발 시작

- 개발하려면 제일 먼저 메인 클래스를 작성해야한다.

- 메인 클래스

  - 메인 클래스는 추상 클래스인 javafx.application.Application을 상속받고, start() 메소드를 재정의해야 한다.
  - 윈도우를 무대로 표현한다.

- JavaFX 라이프사이클

  - Application.launch() -> 기본 생성자 -> init() -> start() -> 사용 -> Platform.exit() 호출 또는 마지막 Stage 닫힘 -> stop() -> 종료
  - JavaFX 애플리케이션에서 윈도우를 비롯한 UI 생성 및 수정 작업, 입력 이벤트 처리 등은 모두 JavaFX Application Thread가 관장한다.

- 메인 클래스 실행 매개값 얻기

  - init() 메소드의 역할은 메인 클래스의 실행 매개값을 얻어 애플리케이션의 초기값으로 이용할 수 있도록 하는 것이다.

    ```java
    // 아래 실행 옵션을 주었다고 가정
    java AppMain --ip=192.168.0.5 --port=50001
        
    // 위의 매개값은 init() 메소드에서 2가지 방법으로 얻을 수 있다
    Parameters params = getParameters();
    List<String> list = params.getRaw(); // 1번째 방법
    Map<String, String> map = params.getNamed(); // 2번째 방법
    ```

- 무대(Stage)와 장면(Scene)

  - JavaFX는 윈도우를 무대로 표현한다.

  - 무대는 한 번에 하나의 장면을 가질 수 잇고 JavaFX는 장면을 javafx.scene.Scene으로 표현한다.

  - 메인 윈도우는 start() 메소드의 primaryStage 매개값으로 전달되지만, 장면은 직접 생성해야 한다.

    ```java
    Scene scene = new Scene(Parent root);
    ```

  - 장면을 생성한 후에는 윈도우에 올려야 하는데, Stage의 setScene() 메소드를 사용한다.



### JavaFX 레이아웃

- 장면에 다양한 컨트롤이 포함되는데 이들을 배치하는 것이 레이아웃이다.
- 작성하는 2가지 방법
  - 자바 코드로 작성하는 프로그램적 레이아웃
  - FXML로 작성하는 선언적 레이아웃
