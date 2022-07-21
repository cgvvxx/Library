# 네트워크 1

> "따라하면서 배우는 IT - 네트워크의 기초"를 공부하면서 정리한 내용입니다.

<br>
<br>

**Contents**

- [네트워크란?](#1-네트워크란)

- [네트워크 아키텍쳐](#2-네트워크-아키텍쳐)

<br>
<br>

## 1. 네트워크란?

### 1.1. 네트워크

- 컴퓨터 또는 통신 장비(이를 노드라고 함)들이 데이터를 공유할 수 있게 하는 디지털 전기 통신망, 또는 분산되어 있는 컴퓨터를 통신망으로 연결한 것

- 네트워크에서 여러 장치들은 노드 간 연결을 사용하여 서로에게 데이터를 교환
- 인터넷 :  문서, 그림, 영상과 같은 여러 가지 데이터를 공유할 수 있는 TCP/IP로 이루어진 네트워크들의 집합이자 전세계적으로 가장 큰 네트워크의 종류 중 하나

### 1.2. 네트워크의 분류

#### 1.2.1. 크기에 따른 분류

- PAN(Personal Area Network) : 가장 작은 규모의 네트워크

- LAN(Local Area Network) : 근거리 영역의 네트워크
- MAN(Metropolitan Area Network) : 대도시 영역의 네트워크
- WAN(Wide Area Network) : 광대역 네트워크, 가까운 지역끼리 묶인 LAN과 LAN을 다시 하나로 묶은 네트워크
- VLAN, CAN etc.

<p align="center">
<img src="https://www.smartsheet.com/sites/default/files/styles/900px/public/IC-Network-Types.jpg?itok=3khmkHuo" width="500" height="360">
</p>

#### 1.2.2. 연결형태에 따른 분류

- Star형

  - 중앙 장비에 모든 노드가 연결된 형태, 일반적으로 LAN 대역에서 많이 사용
  - 허브가 네트워크 중앙에 위치하여 다른 모든 노드를 연결
  - 허브가 고장나면 네트워크 통신이 불가하고, 네트워크의 처리 능력이 허브에 좌우됨
  - ex. 가정집에서 공유기를 통해 핸드폰, 컴퓨터, tv 등을 연결
- Mesh형
  - 여러 노드들이 서로 그물처럼 연결된 형태, WAN 대역에서 많이 사용
  - 네트워크가 복잡하고 비용이 많이 들지만, 신뢰성이 높아 중요한 네트워크에 사용
- Bus형
  - 네트워크의 노드들이 일자형 케이블(버스)에 연결되어 있는 형태
  - 케이블의 시작과 끝에는 터미네이터가 존재하여 신호가 케이블로 돌아오는 것을 막음
- Ring형 : 노드가 링에 순차적으로 연결될 형태
- Tree형 : 트리 형태의 노드에 허브를 두고 노드를 연결하는 형태
- 혼합형 : 여러 형태를 혼합한 형태 
  - ex. 인터넷

<p align="center">
	<img src="https://www.computerhope.com/jargon/n/nettopo.gif" width="500" height="350">
</p>

### 1.3. 네트워크의 통신방식

- 유니 캐스트 : 특정 대상과의 1:1 통신, 네트워크에서 가장 많이 사용하는 방식

- 멀티 캐스트 : 특정 다수와 1:N 통신
- 브로드 캐스트 : 자신의 호스트가 속해있는 네트워크 대역(로컬 LAN)의 모든 대상과 1:N 통신
- 애니 캐스트 : 주소가 같은 호스트들 중 가장 가깝거나, 가장 효율적으로 서비스할 수 있는 호스트와 1:1 통신

<p align="center">
	<img src="https://www.elektronik-kompendium.de/sites/net/bilder/25051211.png" width="450" height="250">
</p>

### 1.4. 프로토콜과 패킷

#### 1.4.1. 프로토콜

- 네트워크에서 노드와 노드가 통신할 때 두 노드간 데이터에 일정한 형태를 규정하는 통신 규약

- 신호 송신의 순서, 데이터의 표현법, 오류 검출법 등을 포함함
  - TCP/IP : 인터넷 접속을 위한 기본 프로토콜
  - FTP/TFTP(20, 21/69) : 파일 송수신
  - Telnet(23) : 컴퓨터 원격 접속 및 사용
  - SMTP/POP3(25/110) : 이메일 송수신
  - DNS(53) : 도메인 네임을 IP 주소로 매핑
  - HTTP(80) : WWW에서 HTML 문서 송수신
  - DHCP(67, 68) : 인터넷 주소 자동 할당
  - SNMP(161) : 네트워크 관리와 모니터링
- 가까운 곳과 통신할 때 - Ethernet 프로토콜 / MAC 주소
- 멀리 있는 곳과 통신할 때 - ICMP, IPv4, ARP / IP 주소
- 여러가지 프로그램으로 통신할 때 - TCP, UDP / 포트 번호

#### 1.4.2. 패킷

- 네트워크에서 전달되는 데이터의 형식화된 블록

- 데이터를 한번에 보내면 데이터 손실과 대역폭 차지 등의 문제가 발생할 수 있으므로 패킷 단위로 잘라서 데이터를 전송
- 헤더, 데이터(페이로드), 트레일러의 세 부분으로 구성 (트레일러는 없는 경우도 많음)
  - 계층별로 패킷의 이름이 다름, PDU(Protocol Data Unit)

    - 4계층의 PDU = 세그먼트
    - 3계층의 PDU = 패킷
    - 2계층의 PDU = 프레임

<p align="center">
	<img src="https://i0.wp.com/networkustad.com/wp-content/uploads/2019/06/Datal-link-layer-frame-fields.png?resize=768%2C194&ssl=1">
</p>

- 캡슐화와 역캡슐화

  - 서로 다른 계층 간의 데이터를 주고 받는 과정에서, 계층 별로 맡은 역할에 따라 추가적으로 정보가 생성되고, 이것을 헤더의 형태로 하나씩 붙여나가게됨
  - 캡슐화(Encapsulation) : 데이터 송신 시 하위 계층으로 패킷을 보내는 경우, 상위 계층에서 하위 계층로 캡슐화를 진행. 하위계층에서 필요로 하는 추가 정보를 헤더 또는 트레일러에 추가
  - 역캡슐화(Decapsulation) :  데이터 수신 시 상위 계층으로 패킷을 전달하고, 전달된 패킷의 프로토콜들을 하위 프로토콜부터 하나씩 제거하면서 데이터를 확인하는 과정

<p align="center">
	<img src="https://cdn.kastatic.org/ka-perseus-images/e5fdf560fdb40a1c0b3c3ce96f570e5f00fff161.svg">
</p>

<br>

## 2. 네트워크 아키텍쳐

### 2.1. 네트워크 아키텍쳐

- 복잡한 네트워크 시스템을 추상화, 모듈화, 계층화의 방법으로 프로토콜의 조합으로 단순화한 시킨 구조

  - 추상화 : 복잡한 네트워크 시스템을 이해하기 쉽도록 핵심적인 기능들을 뽑아 단순화 시키는 방법
  - 모듈화 : 복잡한 네트워크 시스템이 데이터를 전송하는 과정을 논리적인 단계로 기능의 따라 작은 단위로 분할하는 방법
  - 계층화 : 분할된 모듈을 계층적 구조로 배열하는 방법
- 각 계층마다 고유한 기능의 프로토콜이 존재하고, 프로토콜이 정의된 기능을 수행하면서 네트워크의 역할(데이트의 교환)을 하게 되므로 네트워크 아키텍쳐는 결국 프로토콜들의 집합
- 각 계층은 독립된 형태로 동작하므로, 다른 계층에 영향을 주지 않음
- 대표적으로 TCP/IP 4계층 모델, OSI 7계층 모델이 있음

<p align="center">
	<img src="https://blog.kakaocdn.net/dn/Ehzi5/btqErLLHrZY/n4k4Uht5svcVVJWAGPVJfK/img.png" width="850" height="480">
</p>

### 2.2. TCP/IP 4계층 모델

- TCP/IP = Transmission Control Protocl / Internet Protocol

- 1960년대 미 국방부에서 사용된 프로토콜로 지속적으로 표준화되어 신뢰성이 우수함
- Network Interface / Internet / Transport / Application 의 4계층으로 구성
  - 1계층 - 네트워크 인터페이스 계층(Network Interface Layer) 

    - 같은 네트워크 안에서 물리적으로 인접하여 연결되어 있는 네트워크 장비 간의 데이터 전송
    - Eternet : 유선 LAN, Wi-Fi : 무선 LAN 등

  - 2계층 - 인터넷 계층(Internet Layer)
    - 서로 다른 네트워크에 있는 컴퓨터 간의 데이터 전송
    - 대부분 IP 프로토콜을 사용

  - 3계층 - 전송 계층(Transport Layer)
    - 데이터의 흐름을 제어하여 데이터 전송의 신뢰성과 효율성을 관리하고, 목적지 컴퓨터에 전송된 데이터를 적절한 어플리케이션에 배분
    - 대부분 TCP 프로토콜을 사용

  - 4계층 - 응용 계층(Application Layer)
    - 데이터의 내용을 보고 어플리케이션의 목적에 따른 서비스를 사용자에게 제공
    - 사용하는 프로토콜이 거의 정해져있는 하위 계층들과 달리 어플리케이션이 제공하는 서비스마다 별도의 규칙이 존재하기 때문에 다양한 프로토콜 존재
    - HTTP : 웹 서비스, FTP : 파일 전송, SMTP, POP3 : 메일 전송 등


### 2.3. OSI 7계층 모델

- OSI = Open System Interconnection)

- ISO 표준은 1984년 개발된 OSI 7계층 모델이지만 실제로 구현되는 예가 많지 않음
- Physical / DataLink / Network / Transport / Session / Presentation / Application 의 7계층으로 구성
  - 1계층 - 물리 계층(Physical Layer)

    - 전기적, 기계적, 기능적잉ㄴ 특성을 이용해서 통신 케이블로 데이터 전송
    - 통신단위는 비트, 데이터 전달만(데이터가 무엇인지 에러가 있는지 신경 X)
    - 통신 케이블, 리피터, 허브

    2계층 - 데이터링크 계층(Data Link Layer)

    - 물리계층을 통해 송수신되는 정보의 오류와 흐름을 관리하여 안전한 정보 전달을 수행 - 에러 검출, 재전송, 흐름 제어
    - MAC 주소를 이용하여 통신, 전송단위 프레임
    - 브리지, 스위치

    3계층 - 네트워크 계층(Network Layer)

    - 데이터를 목적지까지 가장 안전하고 빠르게 전달하는 기능 - 라우팅, 흐름 제어, 세그멘테이션, 오류 제어, 인터네트워킹
    - IP 주소 할당
    - 라우터

    4계층 - 전송 계층(Transport Layer)

    - 연결 기반 - 패킷들의 전송이 유효한지 확인하고 전송 실패한 패킷들을 다시 전송
    - 패킷 생선
    - TCP/UDP 프로토콜

    5계층 - 세션 계층(Session Layer)

    6계층 - 표현 계층(Presentation Layer)

    7계층 - 응용 계층(Application Layer)


### 2.4. 비교

- 두 모델 모두 계층간 역할 정의를 통한 계층적 네트워크 모델

- OSI 7계층 모델은 역할 기반의 계층을, TCP/IP 4계층 모델은 프로토콜 기반의 계층적 모델
- OSI 7계층 모델은 통신 전반에 대한 표준을 제시하고, TCP/IP 4계층 모델은 데이터 전송 기술에 특화

<br>
<br>

**Reference**

- [[따라學IT\] 네트워크 기초 - YouTube](https://www.youtube.com/watch?v=Av9UFzl_wis&list=PL0d8NnikouEWcF1jJueLdjRIC4HsUlULi)

- [OSI 참조 모델, TCP/IP 모델](https://owlyr.tistory.com/13)
- [쉽게 이해하는 네트워크 5. 프로토콜과 네트워크 아키텍처 - OSI 모델과 TCP/IP 모델](https://better-together.tistory.com/65?category=887984)
