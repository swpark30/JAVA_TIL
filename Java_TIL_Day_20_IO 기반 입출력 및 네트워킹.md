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

  - 자바는 프로그램이 콘솔로부터 데이터를 입력받을 수 있도록 System 클래스의 in 정적 필드를 제공한다.

  - System.in은 InputStream 타입의 필드이므로 InputStream 변수로 참조 가능

    ```java
    InputStream is = System.in;
    int asciiCode = is.read(); // 아스키코드로 불러온다.
    char inputChar = (char)is.read(); // 아스키코드를 문자로 얻고 싶을 때
    ```

  - read() 메소드는 1바이트만 읽기 때문에 1바이트의 아스키코드로 표현되는 숫자, 영어, 특수문자는 프로그램에서 잘 읽을 수 있지만, 한글과 같은 2바이트를 필요로 하는 유니코드는 read() 메소드로 읽을 수 없다.

  - 한글을 얻기 위해서는 우선 read(byte[] b)나 read(char[] cbuf, int off, int len) 메소드로 전체 입력된 내용을 바이트 배열로 받고, 이 배열을 이용해서 String 객체를 생성하면 된다.

    ```java
    byte[] byteData = new byte[15];
    int readByteNo = System.in.read(byteData);
    String strData = new String(byteData, 0, readByteNo-2); // -2를 하는 이유는 엔터키에 해당하는 마지막 두 바이트를 제외하기 위해서이다.
    ```

- #### System.out 필드

  - 콘솔로 데이터를 출력하기 위해서는 System 클래스의 out 정적 필드를 사용한다.

  - out은 PrintStream 타입의 필드이다. (PrintStream은 OutputStream의 하위 클래스이다.)

    ```java
    OutputStream os = System.out;
    ```

  - write() 메소드는 아스키 코드를 문자로 콘솔에 출력한다.

    ```java
    byte b = 97;
    os.write(b); // 'a'가 출력된다.
    os.flush();
    ```

  - 출력도 입력과 똑같이 2바이트인 한글은 출력할 수 없다, 출력하기 위해서 한글을 바이트 배열로 얻은 다음 write(byte[] b)나 write(byte[] b, int off, int len) 메소드로 콘솔에 출력한다.

    ```java
    String name = "홍길동";
    byte[] nameBytes = name.getBytes();
    os.write(nameBytes);
    os.flush();
    ```

- #### Console 클래스

  - 자바 6부터는 콜솔에서 입력받은 문자열을 쉽게 읽을 수 있도록 java.io.Console 클래스를 제공한다.

    ```java
    Console console = System.console();
    ```

  - 이클립스에서 실행하면 System.console() 메소드는 null을 리턴하기 때문에 반드시 명령 프롬프트에서 실행해야 한다.

- #### Scanner 클래스

  - Console 클래스는 콘솔로부터 문자열은 읽을 수 있지만 기본 타입 값을 바로 읽을 수는 없다.

  - java.io 패키지의 클래스는 아니지만, java.util 패키지의 Scanner 클래스를 이용하면 콘솔로부터 기본 타입의 값을 바로 읽을 수 있다.

    ```java
    Scanner scanner = new Scanner(System.in);
    ```

  - Scanner가 콘솔에서만 사용되는 것이 아니고, 생성자 매개값에는 File, InputStream, Path 등과 같이 다양한 입력 소스를 지정할 수도 있다.




### 파일 입출력

- #### File 클래스

  -  O 패키지에서 제공하는 File 클래스는 파일 크기, 파일 속성, 파일 이름 등의 정보를 얻어내는 기능과 파일 생성 및 삭제 기능을 제공하고 있다.

  -  디렉토리를 생성하고 디렉토리에 존재하는 파일 리스트를 얻어내는 기능도 있다.

  -  파일의 데이터를 읽고 쓰는 기능은 지원하지 않는다.

  -  파일의 입출력은 스트림을 사용해야 한다.

     ```java
     // C:\Temp 디렉토리의 file.txt 파일을 File 객체로 생성하는 코드
     File file = new File("C:\\Temp\\file.txt");
     File file = new File("C:/Temp/file.txt");
     ```

- #### FileInputStream

   - 파일로부터 바이트 단위로 읽어들일 때 사용하는 바이트 기반 입력 스트림이다.
   - 
