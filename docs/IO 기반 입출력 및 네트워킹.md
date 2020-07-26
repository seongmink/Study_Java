# IO 기반 입출력 및 네트워킹

## IO 패키지 소개

자바에서는 데이털르 스트림(Stream)을 통해 입출력된다. 스트림은 단일 방향으로 연속적으로 흘러가는 것을 말하는데, 물이 높은 곳에서 낮은 곳으로 흐르듯이 데이터는 출발지에서 나와 도착지로 들어간다는 개념이다.

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

## 콘솔 입출력



## 파일 입출력



## 보조 스트림



## 네트워크 기초



## TCP 네트워킹



## UDP 네트워킹