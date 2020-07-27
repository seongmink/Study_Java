# IO 기반 입출력 및 네트워킹

## IO 패키지 소개

자바에서는 데이터를 스트림(Stream)을 통해 입출력된다. 스트림은 단일 방향으로 연속적으로 흘러가는 것을 말하는데, 물이 높은 곳에서 낮은 곳으로 흐르듯이 데이터는 출발지에서 나와 도착지로 들어간다는 개념이다.

## 입력 스트림과 출력 스트림

프로그램이 데이터를 입력받을 때는 입력 스트림(InputStream)이라고 부르고, 프로그램이 데이터를 보낼 때에는 출력 스트림(OutputStream)이라고 부른다. 입력 스트림의 출발지는 키보드, 파일, 네트워크상의 프로그램이 될 수 있고, 출력 스트림의 도착지는 모니터, 파일, 네트워크상의 프로그램이 될 수 있다.

항상 프로그램을 기준으로 데이터가 들어오면 입력 스트림이고, 데이터가 나가면 출력 스트림이라는 것을 명심해야 한다. 프로그램이 네트워크상의 다른 프로그램과 데이터 교환을 하기 위해서는 양쪽 모두 입력 스트림과 출력 스트림이 따로 필요하다. 스트림의 특성이 단방향이므로 하나의 스트림으로 입력과 출력을 모두 할 수 없기 때문이다.

자바의 기본적인 데이터 입출력(IO: Input/Output) API는 java.io 패키지에서 제공하고 있다.

| java.io 패키지의 주요 클래스                                 | 설명                                                  |
| ------------------------------------------------------------ | ----------------------------------------------------- |
| File                                                         | 파일 시스템의 파일 정보를 얻기 위한 클래스            |
| Console                                                      | 콘솔로부터 문자를 입출력하기 위한 클래스              |
| InputStream / OutputStream                                   | 바이트 단위 입출력을 위한 최상위 입출력 스트림 클래스 |
| FileInputStream / FileOutputStream<br />DataInputStream / DataOutputStream<br />ObjectInputStream / ObjectOutputStream<br />PrintStream<br />BufferedInputStream / BufferedOutputStream | 바이트 단위 입출력을 위한 하위 스트림 클래스          |
| Reader / Writer                                              | 문자 단위 입출력을 위한 최상위 입출력 클래스          |
| FileReader / FileWriter<br />InputStreamReader / OutputStreamWriter<br />PrintWriter<br />BufferedReader / BufferedWriter | 문자 단위 입출력을 위한 하위 스트림 클래스            |

스트림 클래스는 크게 두 종류로 구분된다. 하나는 바이트(byte) 기반 스트림이고, 다른 하나는 문자(character) 기반 스트림이다. **바이트 기반 스트림은 그림, 멀티미디어, 문자 등 모든 종류의 데이터를 받고 보낼 수 있으나, 문자 기반 스트림은 오로지 문자만 받고 보낼 수 있도록 특화되어 있다.** 

| 구분          | 바이트 기반 스트림<br />(입력 스트림) | 바이트 기반 스트림<br />(출력 스트림) | 문자 기반 스트림<br />(입력 스트림) | 문자 기반 스트림<br />(출력 스트림) |
| ------------- | ------------------------------------- | ------------------------------------- | ----------------------------------- | ----------------------------------- |
| 최상위 클래스 | InputStream                           | OutputStream                          | Reader                              | Writer                              |
| 하위 클래스   | XXXInputStream                        | XXXOutputStream                       | XXXReader                           | XXXWriter                           |

InputStream은 바이트 기반 입력 스트림의 최상위 클래스이고, OutputStream은 바이트 기반 출력 스트림의 최상위 클래스이다. 이 클래스들을 각각 상속받는 하위 클래스는 접미사로 InputStream, OutputStream이 붙는다. Reader는 문자 기반 입력 스트림의 최상위 클래스이고, Writer는 문자 기반 출력 스트림의 최상위 클래스이다. 이 클래스들을 각각 상속받는 하위 클래스는 접미사로 Reader 또는 Writer가 붙는다.

예를 들어 그림, 멀티미디어, 텍스트 등의 파일을 바이트 단위로 읽어들일 때에는 FileInputStream을 사용하고, 바이트 단위로 저장할 때에는 FileOutputStream을 사용한다. 텍스트 파일의 경우, 문자 단위로 읽어들일 때에는 FileReader를 사용하고, 문자 단위로 저장할 때에는 FileWriter를 사용한다.

- ### InputStream

  InputStream은 **바이트 기반 입력 스트림**의 최상위 클래스로 추상 클래스이다. 모든 바이트 기반 입력 스트림은 이 클래스를 상속받아서 만들어진다. FileInputStream, BufferedInputStream, DataInputStream 클래스는 모두 InputStream 클래스를 상속하고 있다.

  InputStream 클래스에는 바이트 기반 입력 슽릠이 기본적으로 가져야 할 메소드가 정의되어 있다.

  | 리턴 타입 | 메소드                           | 설명                                                         |
  | --------- | -------------------------------- | ------------------------------------------------------------ |
  | int       | read()                           | 입력 스트림으로부터 1바이트를 읽고 읽은 바이트를 리턴한다.   |
  | int       | read(byte[] b)                   | 입력 스트림으로부터 읽은 바이트들을 매개값으로 주어진 바이트 배열 b에 저장하고 실제로 읽은 바이트 수를 리턴한다. |
  | int       | read(byte[] b, int off, int len) | 입력 스트림으로부터 len개의 바이트를 읽고 매개값으로 주어진 바이트 배열 b[off] 부터 len개까지 저장한다.<br />그리고 실제로 읽은 바이트수인 len개를 리턴한다. 만약 len개를 모두 읽지 못하면 실제로 읽은 바이트 수를 리턴한다. |
  | void      | close()                          | 사용한 시스템 자원을 반납하고 입력 스트림을 닫는다.          |

  - #### read() 메소드

    read() 메소드는 입력 스트림으로부터 1바이트를 읽고 4바이트 int 타입으로 리턴한다. 따라서 리턴된 4바이트 중 끝의 1바이트에만 데이터가 들어 있다.

    더 이상 입력 스트림으로부터 바이트를 읽을 수 없다면 read() 메소드는 -1을 리턴하는데, 이것을 이용하면 읽을 수 있는 마지막 바이트까지 루프를 돌며 한 바이트씩 읽을 수 있다.

    ```java
    InputStream is = new FileInputStream("C:/test.jpg");
    int readByte;
    while((readByte=is.read() != -1) { ... }
    ```

  - #### read(byte[] b) 메소드

    read(byte[] b) 메소드는 입력 스트림으로부터 매개값으로 주어진 바이트 배열의 길이만큼 바이트를 읽고 배열에 저장한다. 그리고 읽은 바이트 수를 리턴한다. 실제로 읽은 바이트 수가 배열의 길이보다 작을 경우 읽은 수만큼 리턴한다.

    read(byte[] b) 역시 입력 스트림으로부터 바이트를 더 이상 읽을 수 없다면 -1을 리턴하는데, 이 것을 이용하면 읽을 수 있는 마지막 바이트까지 루프를 돌며 읽을 수 있다.

    ```java
    InputStream is = new FileInputStream("C:/test.jpg");
    int readByteNo;
    byte[] readBytes = new byte[100];
    while((readByteNo=is.read(readBytes) != -1) { ... }
    ```

    read(byte[] b) 메소드는 한 번 읽을 때 매개값으로 주어진 바이트 배열 길이만큼 읽기 때문에 루핑 횟수가 현저히 줄어든다. 그러므로 **많은 양의 바이트를 읽을 때는 read(byte[] b) 메소드를 사용하는 것이 좋다.**

  - #### read(byte[] b, int off, int len) 메소드

    read(byte[] b, int off, int len) 메소드는 입력 스트림으로부터 len개의 바이트만큼 읽고, 매개값으로 주어진 바이트 배열 b[off]부터 len개까지 저장한다. 그리고 읽은 바이트 수인 len개를 리턴한다. 실제로 읽은 바이트 수가 len개보다 작을 경우 읽은 수만큼 리턴한다.

    read(byte[] b, int off, int len) 역시 입력 스트림으로부터 바이트를 더 이상 읽을 수 없다면 -1을 리턴한다. read(byte[] b)와의 차이점은 한 번에 읽어들이는 바이트 수를 len 매개값으로 조절할 수 있고, 배열에서 저장이 시작되는 인덱스를 지정할 수 있다는 점이다. 만약 off를 0으로, len을 배열의 길이로 준다면 read(byte[] b)와 동일하다.

    ```java
    InputStream is = new FileInputStream("C:/test.jpg");
    byte[] readBytes = new byte[100];
    int readByteNo = is.read(readBytes, 0, 100); // 0번째부터 100개 읽기
    ```

  - #### close() 메소드

    InputStream을 더 이상 사용하지 않을 경우에는 close() 메소드를 호출해서 InputStream에서 사용했던 시스템 자원을 풀어준다.

    ```java
    is.close();
    ```

- ### OutputStream

   OutputStream은 **바이트 기반 출력 스트림**의 최상위 클래스로 추상 클래스이다. 모든 바이트 기반 출력 스트림 클래스는 이 클래스를 상속받아서 만들어진다. FileOutputStream, PrintStream, BufferedOutputStream, DataOutputStream 클래스는 모두 OutputStream 클래스를 상속하고 있다.

  OutputStream 클래스에는 모든 바이트 기반 출력 스트림이 기본적으로 가져야 할 메소드가 정의되어 있다.

  | 리턴 타입 | 메소드                            | 설명                                                         |
  | --------- | --------------------------------- | ------------------------------------------------------------ |
  | void      | write(int b)                      | 출력 스트림으로 1바이트를 보낸다(b의 끝 1바이트)             |
  | void      | write(byte[] b)                   | 출력 스트림으로 주어진 바이트 배열 b의 모든 바이트를 보낸다. |
  | void      | write(byte[] b, int off, int len) | 출력 스트림으로 주어진 바이트 배열 b[off]부터 len개까지의 바이트를 보낸다. |
  | void      | flush()                           | 버퍼에 잔류하는 모든 바이트를 출력한다.                      |
  | void      | close()                           | 사용한 시스템 자원을 반납하고 출력 스트림을 닫는다.          |

  - #### write(int b) 메소드

    write(int b) 메소드는 매개 변수로 주어진 **int 값에서 끝에 있는 1바이트만** 출력 스트림으로 보낸다. 매개 변수가 int 타입이므로 4바이트 모두를 보내는 것으로 오해할 수 있다.

    ```java
    OutputStream os = new FileOutputStream("C:/test.txt");
    byte[] data = "ABC".getBytes();
    for(int i=0; i<data.length; i++) {
        os.write(data[i]); // "A", "B", "C"를 하나씩 출력
    }
    ```

  - #### write(byte[] b) 메소드

    write(byte[] b)는 매개값으로 주어진 바이트 배열의 **모든 바이트**를 출력 스트림으로 보낸다.

    ```java
    OutputStream os = new FileOutputStream("C:/test.txt");
    byte[] data = "ABC".getBytes();
    os.write(data); // "ABC" 모두 출력
    ```

  - #### write(byte[] b, int off, int len) 메소드

    write(byte[] b, int off, int len)은 b[off]부터 len개의 바이트를 출력 스트림으로 보낸다.

    ```java
    OutputStream os = new FileOutputStream("C:/test.txt");
    byte[] data = "ABC".getBytes();
    os.write(data, 1, 2); // "BC"만 출력
    ```

  - #### flush()와 close() 메소드

    출력 스트림은 내부에 작은 버퍼(buffer)가 있어서 데이터가 출력되기 전에 버퍼에 쌓여있다가 순서대로 출력된다. flush() 메소드는 버퍼에 잔류하고 있는 데이터를 모두 출력시키고 버퍼를 비우는 역할을 한다. 프로그램에서 더 이상 출력할 데이터가 없다면 flush() 메소드를 마지막으로 호출하여 버퍼에 잔류하는 모든 데이터가 출력되도록 해야 한다. OutputStream을 더 이상 사용하지 않을 경우에는 close() 메소드를 호출해서 OutputStream에서 사용했던 시스템 자원을 풀어준다.

    ```java
    OutputStream os = new FileOutputStream("C:/test.txt");
    byte[] data = "ABC".getBytes();
    os.write(data);
    os.flush();
    os.close();
    ```

- ### Reader

  Reader는 **문자 기반 입력 스트림**의 최상위 클래스로 추상 클래스이다. 모든 문자 기반 입력 스트림은 이 클래스를 상속받아서 만들어진다. FileReader, BufferedReader, InputStreamReader 클래스는 모두 Reader 클래스를 상속하고 있다.

  Reader 클래스에는 문자 기반 입력 스트림이 기본적으로 가져야 할 메소드가 정의되어 있다.

  | 리턴 타입 | 메소드                              | 설명                                                         |
  | --------- | ----------------------------------- | ------------------------------------------------------------ |
  | int       | read()                              | 입력 스트림으로부터 한 개의 문자를 읽고 리턴한다.            |
  | int       | read(char[] cbuf)                   | 입력 스트림으로부터 읽은 문자들을 매개값으로 주어진 문자 배열 cbuf에 저장하고 실제로 읽은 문자 수를 리턴한다. |
  | int       | read(char[] cbuf, int off, int len) | 입력 스트림으로부터 len개의 문자를 읽고 매개값으로 주어진 문자 배열 cbuf[off]부터 len개까지 저장한다.<br />그리고 실제로 읽은 문자 수인 len개를 리턴한다. |
  | void      | close()                             | 사용한 시스템 자원을 반납하고 입력 스트림을 닫는다.          |

  - #### read() 메소드

    read() 메소드는 입력 스트림으로부터 한 개의 문자(2바이트)를 읽고 4바이트 int 타입으로 리턴한다. 따라서 **리턴된 4바이트 중 끝에 있는 2바이트에 문자 데이터가 들어있다.**

    read() 메소드가 리턴한 int 값을 char 타입으로 변환하면 읽은 문자를 얻을 수 있다.

    ```java
    char charData = (char) read();
    ```

    더 이상 입력 스트림으로부터 문자를 읽을 수 없다면 read() 메소드는 -1을 리턴하는데 이것을 이용하면 읽을 수 있는 마지막 문자까지 루프를 돌며 한 문자씩 읽을 수 있다.

    ```java
    Reader reader = new FileReader("C:/test.txt");
    int readData;
    while((readData=reader.read()) != -1) {
        char charData = (char) readData;
    }
    ```

  - #### read(char[] cbuf) 메소드

    read(char[] cbuf) 메소드는 입력 스트림으로부터 매개값으로 주어진 문자 배열의 길이만큼 문자를 읽고 배열에 저장한다. 그리고 읽은 문자 수를 리턴한다. 실제로 읽은 문자 수가 배열의 길이보다 작을 경우 읽은 수만큼만 리턴한다.

    ```java
    Reader reader = new FileReader("C:/test.txt");
    int readData;
    char[] cbuf = new char[2];
    while((readData=reader.read(cbuf)) != -1) { ... }
    ```

    read(char[] cbuf) 메소드는 한 번 읽을 때 주어진 배열 길이만큼 읽기 때문에 루핑 횟수가 현저히 줄어든다. 그러므로 **많은 양의 문자를 읽을 때는 read(char[] cbuf) 메소드를 사용하는 것이 좋다.**

  - #### read(char[] cbuf, int off, int len) 메소드

    read(char[] cbuf, int off, int len) 메소드는 입력 스트림으로부터 len개의 문자만큼 읽고 매개값으로 주어진 문자 배열 cbuf[off]부터 len개까지 저장한다. 그리고 읽은 문자 수인 len개를 리턴한다. 실제로 읽은 문자 수가 len개보다 작을 경우 읽은 수만큼 리턴한다.

    read(char[] cbuf, int off, int len) 역시 입력 스트림으로부터 문자를 더 이상 읽을 수 없다면 -1을 리턴한다. read(char[] cbuf)와의 차이점은 한 번에 읽어들이는 문자 수를 len 매개값으로 조절할 수 있고, 배열에서 저장이 시작되는 인덱스를 지정할 수 있다는 점이다. 만약 off를 0으로, len을 배열의 길이로 준다면 read(char[] cbuf)와 동일하다.

    ```java
    Reader reader = new FileReader("C:/test.txt");
    char[] cbuf = new char[100];
    int readCharNo = reader.read(cbuf, 0, 100);
    ```

  - #### close() 메소드

    마지막으로 Reader를 더 이상 사용하지 않을 경우에는 close() 메소드를 호출해서 Reader에서 사용했던 시스템 자원을 풀어준다.

    ```java
    reader.close();
    ```

- ### Writer

  Writer는 **문자 기반 출력 스트림**의 최상위 클래스로 추상 클래스이다. 모든 문자 기반 출력스트림 클래스는 이 클래스를 상속받아서 만들어진다. FileWriter, BufferedWriter, PrintWriter, OutputStreamWriter 클래스는 모두 Writer 클래스를 상속하고 있다.

  | 리턴 타입 | 메소드                               | 설명                                                         |
  | --------- | ------------------------------------ | ------------------------------------------------------------ |
  | void      | write(int c)                         | 출력 스트림으로 주어진 한 문자를 보낸다(c의 끝 2바이트)      |
  | void      | write(char[] cbuf)                   | 출력 스트림으로 주어진 문자 배열 cbuf의 모든 문자를 보낸다.  |
  | void      | write(char[] cbuf, int off, int len) | 출력 스트림으로 주어진 문자 배열 cbuf[off]부터 len개까지의 문자를 보낸다. |
  | void      | write(String str)                    | 출력 스트림으로 주어진 문자열을 전부 보낸다.                 |
  | void      | write(String str, int off, int len)  | 출력 스트림으로 주어진 문자열 off 순번부터 len개까지의 문자를 보낸다. |
  | void      | flush()                              | 버퍼에 잔류하는 모든 문자열을 출력한다.                      |
  | void      | close()                              | 사용한 시스템 자원을 반납하고 출력 스트림을 닫는다.          |
  - #### write(int c) 메소드

    write(int c) 메소드는 매개 변수로 주어진 int 값에서 끝에 있는 2바이트(한 개의 문자)만 출력 스트림으로 보낸다. 매개 변수가 int 타입이므로 4바이트 모두를 보내는 것으로 오해할 수 있다.

    ```java
    Writer writer = new FileWriter("C:/test.txt");
    char[] data = "홍길동".toCharArray();
    for(int i=0; i<data.length; i++) {
        writer.write(data[i]); // "홍", "길", "동"을 하나씩 출력
    }
    ```

  - #### write(char[] cbuf) 메소드

    write(char[] cbuf) 메소드는 매개값으로 주어진 char[] 배열의 모든 문자를 출력 스트림으로 보낸다.

    ```java
    Writer writer = new FileWriter("C:/test.txt");
    char[] data = "홍길동".charArray();
    writer.write(data); // "홍길동" 모두 출력
    ```

  - #### write(char[] c, int off, int len) 메소드

    write(char[] c, int off, int len) 은 c[off]부터 len개의 문자를 출력스트림으로 보낸다.

    ```java
    Writer writer = new FileWriter("C:/test.txt");
    char[] data = "홍길동".charArray();
    writer.write(data, 1, 2); // "길동"만 출력
    ```

  - #### write(String str)와 write(String str, int off, int len) 메소드

    Writer는 문자열을 좀 더 쉽게 보내기 위해서 write(String str)와 write(String str, int off, int len) 메소드를 제공한다. write(String str)은 문자열 전체를 출력 스트림으로 보내고, write(String str, int off, int len) 은 주어진 문자열 off 순번부터 len개까지의 문자를 보낸다.

    - write(String str) : 문자열 전체

    - write(String str, int off, int len) : off 부터 len개(만약 write(str, 3, 2)면 str의 3번 인덱스부터 2개)

    문자 출력 스트림은 내부에 작은 버퍼(buffer)가 있어서 데이터가 출력되기 전에 버퍼에 쌓여있다가 순서대로 출력된다. flush() 메소드는 버퍼에 잔류하고 있는 데이터를 모두 출력시키고 버퍼를 비우는 역할을 한다. 프로그램에서 더 이상 출력할 데이터가 없다면 flush() 메소드를 마지막으로 호출하여 버퍼에 잔류하는 모든 데이터가 출력되도록 해야 한다. Writer를 더 이상 사용하지 않을 경우에는 close() 메소드를 호출해서 Writer에서 사용했던 시스템 자원을 풀어준다.

    ```java
    Writer writer = new FileWriter("C:/test.txt");
    String data = "안녕 자바 프로그램";
    writer.write(data);
    writer.flush();
    writer.close();
    ```

## 콘솔 입출력



## 파일 입출력



## 보조 스트림



## 네트워크 기초



## TCP 네트워킹



## UDP 네트워킹