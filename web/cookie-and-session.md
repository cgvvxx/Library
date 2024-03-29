# Cookie & Session



## 1. HTTP 특징

- 비연결지향(connetionless) : 서버는 요청에 대한 응답을 보낸 후 연결을 끊음

- 무상태(stateless)

  - 연결을 끊는 순간 클라이언트와 서버 간의 통신이 끝나며 상태 정보가 유지되지 않음
  - 클라이언트와 서버가 주고 받는 메세지들은 서로 독립적

- 클라이언트와 서버의 지속적인 관계를 유지하기 위해 쿠키와 세션 존재

  

## 2. 쿠키(cookies)

- 서버가 사용자의 웹 브라우저에 전송하는 데이터 조각
- 사용자가 웹사이트에 방문하는 경우 웹사이트의 서버를 통해 사용자의 컴퓨터에 설치하는 작은 기록 정보 파일
  - 브라우저(클라이언트)는 쿠키를 로컬에 key-value 형태의 데이터로 저장
  - 쿠키를 저장한 후, 동일한 서버에 재요청 시 저장된 쿠키를 함께 전송
- 클라이언트에 300개까지 쿠키 저장 가능, 하나의 도메인당 20개의 값만 가질 수 있음, 하나의 쿠키값은 4KB까지 저장
- HTTP 쿠키는 상태가 있는 세션을 만들어 줌
- 쿠키는 두 요청이 동일한 브라우저에서 들어왔는지 아닌지를 판단할 때 주로 사용
  - 세션 관리 - EX. 로그인, 아이디 자동 완성, 공지 하루 안보기, 장바구니
  - 개인화 - EX. 사용자 선호 테마 설정
  - 트래킹 - EX. 사용자 행동 기록 및 분석
- 쿠키의 수명
  - Session cookies
    - 현재 세션이 종료되면 삭제됨
    - 브라우저가 현재 세션이 종료되는 시기를 정의
  - Persistent cookies(= Permanent cookies)
    - Expires 속성에 지정된 날짜 혹은 Max-Age 속성에 지정된 기간이 지나면 삭제



## 3. 세션(Session)

- 사이트와 특정 브라우저 사이의 상태를 유지시키는 것
- 쿠키를 기반으로 하여, 사용자 정보 파일을 브라우저에 저장하는 쿠키와 달리 세션은 서버 측에서 관리
- 클라이언트가 서버에 접속하면 서버가 특정 session id를 발급하고 클라이언트는 발급 받은 session id를 쿠키에 저장
- 클라이언트가 다시 서버에 접속하면 요청과 함께 session id가 저장된 쿠키를 서버에 전달
- ID는 세션을 구별하기 위해 필요하며 쿠키에는 ID만 저장



## 4. 쿠키와 세션의 차이

- 사용자의 정보가 저장되는 위치 ; 쿠키는 서버의 자원을 전혀 사용하지 않으며, 세션은 서버의 자원을 사용
- 세션은 서버의 처리가 필요하기 때문에 요청 속도는 쿠키가 세션보다 더 빠름
- 쿠키는 클라이언트 로컬에 저장되기 때문에 변질되거나 스니핑 당할 우려가 있지만 세션은 쿠키를 이용해 session id만 저장하기 때문에 세션이 쿠키보다 비교적 보안성이 우수
- 세션은 만료시간을 정할 수 있지만 브라우저가 종료되면 만료시간에 상관없이 삭제되고, 쿠키는 파일러 저장되기 때문에 브라우저를 종료해도 만료기간이 남아있다면 계속해서 정보가 남아 있을 수 있음

