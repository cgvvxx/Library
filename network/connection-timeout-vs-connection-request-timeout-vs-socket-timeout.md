# Connection Timeout, Connection Request Timeout and Socket Timeout

Java 어플리케이션에서 http 호출을 사용할 때 일반적으로 Apache HttpClient를 사용한다. 이 때 Apache HttpClient 내에는 다음과 같이 세 가지 timeout을 정의한다. 각각의 timeout에 대한 차이는 아래 그림과 같은데 해당 timeout에 대한 내용들을 간략하게 정리해보았다.

- Apache Http Client : HTTP 프로토콜을 손쉽게 사용할 수 있게 해주는 클라이언트측 HTTP 전송 라이브러리

<p align="center"><img src="https://i.stack.imgur.com/ZcdtT.png"></p>

<br>

## 1. Connection Timeout

> Determines the timeout in milliseconds until a connection is established

클라이언트가 서버와 connection을 맺을 때까지의 timeout을 나타낸다. 웹 브라우저(클라이언트)는 3-way handshake 방식을 이용하여 서버와 connection을 맺는다. 이 때, 해당 작업이 수행되어 connection이 이루어지기까지의 걸리는 시간을 나타내는 timeout이다.

<p align="center"><img src="https://junhyunny.github.io/images/kind-of-request-timeout-1.jpg"></p>

## 2. Connection Request Timeout

> Returns the timeout in milliseconds used when requesting a connection from the connection manager

connection pool(HTTP connection manager)로부터 connection을 반환받기까지의 timeout을 나타낸다. connection pool에 connection을 요청한 뒤, connection을 받기까지 걸리는 시간을 나타내는 timeout이다.

- connection pool이란?

  - connection pool에 DB connection 정보를 저장하고 관리하며, 클라이언트 요청이 와서 connection 정보가 필요할때 마다 connection 객체를 빌려주고 해당 객체의 역할이 끝나게 되면 다시 connection 객체를 반납하여 connection pool에 반납한다.
  - connection pool을 사용하면 클라이언트가 빠르게 DB 접속을 할 수 있고, connection 수를 제한함으로 써 DB에 과다한 부하를 막을 수 있고 연결이 끝난 connection을 재활용함으로써 새로 객체를 만드는 비용을 줄일 수 있다. 
  
  <p align="center"><img src="https://linked2ev.github.io/assets/img/devlog/201908/cp-s1.png"></p>

## 3. Socket Timeout

> Determines the timeout for waiting for data or, put differently, a maximum period inactivity between two consecutive data packets

서버와 connection이 이루어진 이후, 서버에서 클라이언트로 보내는 데이터 패킷 간의 시간 간격에 대한 timeout이다. HTTP 통신을 할 때 일반적으로 서버는 여러 개의 패킷으로 데이터를 클라이언트에 전송하는데, 이 때 각 패킷들 간의 시간 간격을 나타내는 timeout이다. 단, socket timeout은 전체 응답 시간이 아닌 각각의 개별 패킷들 사이의 timeout을 나타낸다. 예를 들어, socket  timeout이 1초이고, 5개의 패킷이 각각 0.8초 만에 도착하여 총 응답시간이 4초가 된다고 해도 socket timeout이 발생하지 않는다.

<p align="center"><img src="https://junhyunny.github.io/images/kind-of-request-timeout-2.jpg"></p>

<br>

**Reference**

- [Request Timeout 종류](https://junhyunny.github.io/information/kind-of-request-timeout/)

- [What is the difference between the setConnectionTimeout , setSoTimeout and "http.connection-manager.timeout" in apache HttpClient API?](https://stackoverflow.com/questions/18184899/what-is-the-difference-between-the-setconnectiontimeout-setsotimeout-and-http)
- [커넥션 풀(Connection pool)이란?](https://linked2ev.github.io/spring/2019/08/14/Spring-3-커넥션-풀이란/)
