# 18장 IO 기반 입출력 및 네트워킹



### java.io 패키지

- 프로그램에서는 데이터를 외부에서 읽고 다시 외부로 출력하는 작업이 빈번히 일어난다.
- 자바에서 데이터는 스트림을 통해 입출력되므로 스트림의 특징을 잘 이해해야 한다.
- 스트림은 단일 방향으로 연속적으로 흘러가는 것 (데이터는 출발지에서 나와 도착지로 들어간다는 개념)
- 자바의 기본적인 데이터 입출력(input / output) API 제공



### 자바의 스트림(Stream)

- 입출력 장치와 자바 응용 프로그램 연결 통로

- 입력 스트림

  - 입력 장치로부터 자바 프로그램으로 데이터 전달하는 소프트웨어 모듈

- 출력 스트림

  - 자바 프로그램에서 출력 장치로 데이터를 보내는 소프트웨어 모듈

- 입출력 스트림 기본 단위 : 바이트(byte)

- 자바 입출력 스트림 특징

  - 단방향
  - 선입선출구조 (FIFO)

  

- 바이트 기반 스트림

  - 이미지, 멀티미디어, 문자 등 모든 종류의 데이터를 받고 보내는 것 가능
  - 입출력되는 데이터를 단순 바이트의 스트림으로 처리

- 문자 기반 스트림

  - 문자만 입출력하는 스트림
  - 문자만 받고 보낼 수 있도록 특화



### File 클래스

- 파일 시스템의 파일을 표현하는 클래스

- #### File 클래스 메소드

  - mkdir()
    - 새로운 디렉토리 생성 후 결과 반환(true / false)
    - 최하위 

- 파일 읽기 / 쓰기에 사용되는 클래스
  - 바이트 기반
    - FileInputStream / FileOutputStream
  - 문자 기반
    - FileReader / FileWriter
  
  

### 입력 스트림과 출력 스트림

-  프로그램이 데이터를 입력받을 때에는 입력 스트림(InputStream)
   -  출발지는 키보드, 파일, 네트워크상의 프로그램이 될 수 있다.
   
-  프로그램이 데이터를 보낼 때에는 출력 스트림(OutputStream)
   -  도착지는 모니터, 파일, 네트워크상의 프로그램이 될 수 있다.
   
- 항상 프로그램을 기준으로 데이터가 들어오면 입력, 데이터가 나가면 출력 스트림이다.

- 자바의 기본적인 데이터 입출력 API는 java.io 패키지에서 제공

- 스트림 클래스 2종류

  - 바이트 기반 스트림

    -  그림, 멀티미디어, 문자 등 모든 종류의 데이터를 받고 보낼 수 있다.
    -  입력 스트림의 최상위 클래스 : InputStream
    -  출력 스트림의 최상위 클래스 : OutputStream

  - 문자 기반 스트림

    -  오로지 문자만 받고 보낼 수 있도록 특화되어 있다.
    -  입력 스트림의 최상위 클래스 : Reader
    -  출력 스트림의 최상위 클래스 : Writer

- #### InputStream

  - 바이트 기반 입력 스트림의 최상위 클래스

  - 바이트 기반 입력 스트림이 기본적으로 가져야 할 메소드

    - read() 메소드

      - 입력 스트림으로부터 1바이트를 읽고 4바이트 int 타입으로 리턴한다.

      - 더 이상 입력 스트림으로부터 바이트를 읽을 수 없다면 read() 메소드는 -1 을 리턴한다.

        ```java
        InputStream is = new FileInputStream("C:/test.jpg");
        int readByte;
        while((readByte=is.read()) != -1){
            ...
        }
        ```

    - read(byte[] b) 메소드

      -  입력 스트림으로부터 매개값으로 주어진 바이트 배열의 길이만큼 바이트를 읽고 배열에 저장한다.
      -  읽은 바이트 수를 리턴한다.
      -  read() 메소드는 100번을 루핑해서 읽어야 한다고 할 때 read(byte[] b) 메소드는 루핑 횟수가 현저히 줄어든다.

    - read(byte[] b, int off, int len) 메소드
    
      - 입력 스트림으로부터 len개의 바이트만큼 읽고, 매개값으로 주어진 바이트 배열 b[off]부터 len개까지 저장한다.
    
    - close() 메소드
    
      - InputStream을 더 이상 사용하지 않을 경우에는 close() 메소드를 호출해서 InputStream에서 사용했던 시스템 자원을 풀어준다.

- #### OutputStream

  -  바이트 기반 출력 스트림의 최상위 클래스

  -  write(int b) 메소드

     - 매개 변수로 주어진 int 값에서 끝에 있는 1바이트만 출력 스트림으로 보낸다.

       ```java
       OutputStream os = new FileOutputStream("C:/test.txt");
       byte[] data = "ABC".getBytes();
       for(int i=0; i<data.length; i++) {
           os.write(data[i]); // "A", "B", "C"를 하나씩 출력
       }
       ```

  -  write(byte[] b) 메소드

     -  매개값으로 주어진 바이트 배열의 모든 바이트를 출력 스트림으로 보낸다.

  -  write(byte[] b, int off, int len) 메소드

     -  b[off]부터 len개의 바이트를 출력 스트림으로 보낸다.

  -  flush()와 close() 메소드

     -  출력 스트림은 내부에 작은 버퍼(buffer)가 있어서 데이터가 출력되기 전에 버퍼에 쌓여있다가 순서대로 출력된다.
     -  flush() 메소드
        -  버퍼에 잔류하고 있는 데이터를 모두 출력시키고 버퍼를 비우는 역할을 한다.
        -  프로그램에서 더 이상 출력할 데이터가 없다면 flush() 메소드를 마지막으로 호출하여 버퍼에 잔류하는 모든 데이터가 출력되도록 해야 한다.
     -  close() 메소드
        -  OutputStream을 더 이상 사용하지 않을 경우에는 close() 메소드를 호출해서 시스템 자원을 풀어준다.

- #### Reader

  - 문자 기반 입력 스트림의 최상위 클래스

  - 주요 메소드

    - read() 메소드

      - 입력 스트림으로부터 한 개의 문자(2바이트)를 읽고 4바이트 int 타입으로 리턴한다.

        ```java
        Reader reader = new FileReader("C:/test.txt");
        int readData;
        while((readData=reader.read()) != -1) {
            char charData = (char)readData;
        }
        ```

    - read(char[] cbuf) 메소드

      - 입력 스트림으로부터 매개값으로 주어진 문자 배열의 길이만큼 문자를 읽고 배열에 저장한다.
      - 읽은 문자 수를 리턴한다.

    - read(char[] cbuf, int off, int len) 메소드

      - 입력 스트림으로부터 len개의 문자만큼 읽고 매개값으로 주어진 문자 배열 cbuf[off]부터 len개까지 저장한다.

    - close() 메소드

- #### Writer

  - 문자 기반 출력 스트림의 최상위 클래스

  - 주요 메소드

    - write(int c) 메소드

      - 매개 변수로 주어진 int 값에서 끝에 있는 2바이트만 출력 스트림으로 보낸다.

        ```java
        Writer writer = new FileWriter("C:/test.txt");
        char[] data = "홍길동".toCharArray();
        for(int i=0; i<data.length; i++){
            writer.write(data[i]); // "홍", "길", "동"을 하니씩 출력
        }
        ```

    - write(char[] cbuf) 메소드

      - 매개값으로 주어진 char[] 배열의 모든 문자를 출력 스트림으로 보낸다.

    - write(char[] c, int off, int len) 메소드

      - c[off]부터 len개의 문자를 출력스트림으로 보낸다.

    - write(String str)와 write(String str, int off, int len) 메소드

      - 문자열을 좀 더 쉽게 보내기 위해 위의 2개의 메소드를 제공한다.
      - write(String str)
        - 문자열 전체를 출력 스트림으로 보낸다.
      - write(String str, int off, int len)
        - 주어진 문자열 off순번부터 len개까지의 문자를 보낸다.

  - 문자 출력 스트림은 내부에 작은 버퍼가 있어서 데이터가 출력되기 전에 버퍼에 쌓여있다가 순서대로 출력된다.



### 콘솔 입출력

- 콘솔은 시스템을 사용하기 위해 키보드로 입력을 받고 화면으로 출력하는 소프트웨어를 말한다.

- 유닉스나 리눅스 운영체제는 터미널에 해당하고, 윈도우 운영체제는 명령 프롬프트에 해당

- 이클립스에도 Console 뷰가 있는데, 자바는 콘솔로부터 데이터를 입력받을 때 System.in을 사용하고, 콘솔에 데이터를 출력할 때 System.out을 사용한다.

- 에러를 출력할 때에는 System.err를 사용한다.

- #### System.in 필드

  - 

