# 네트워크 2

> "따라하면서 배우는 IT - 네트워크의 기초"를 공부하면서 정리한 내용입니다.

<br>
<br>

**Contents**

- [데이터링크 계층 (2계층)](#1-데이터링크-계층)

- [네트워크 계층 (1계층)](#1-네트워크-계층)

<br>
<br>

## 1. 데이터링크 계층 

### 1.1. 이더넷 프로토콜 (Ethernet Protocol)

- LAN 대역에서 통신할 때 가장 널리 사용되는 프로토콜 (LAN 대역 네트워크의 90% 이상이 이더넷 프로토콜 방식을 사용)
- 같은 네트워크(LAN 대역) 상에 존재하는 장비들 사이의 데이터 전달 및 오류 제어, 흐름 제어 수행 (다른 네트워크 대역과 통신하는 경우 3계층의 도움을 받아야 함)
- 이더넷은 네트워크에 연결된 각 기기들이 48비트 길이의 고유의 MAC 주소를 가지고 이 주소를 이용해 상호간에 데이터를 주고 받을 수 있도록 만들어짐

#### 1.1.1. MAC 주소

- MAC 주소(Media Access Control address)

  - 제조업체가 하드웨어 장비에 직접적으로 할당하는 물리적 주소(고유 식별자)

  - LAN 대역에서 통신할 때 사용하는 주소

  - 표준 MAC 주소는 총 48비트로 구성되어 있음

    <p align="center">
        <img src="https://sf.ezoiccdn.com/ezoimgfmt/networkencyclopedia.com/wp-content/uploads/2019/08/mac-address.jpg?ezimgfmt=ng:webp/ngcb2">
    </p>
    
    - 상위 24비트 = OUI(Organizational Unique Identifier) 제조업체 식별 ID
    
    - 하위 24비트 = 제조사에서 부여한 고유번호

#### 1.1.2. 프레임(Frame)

<p align="center">
    <img src="https://www.electronics-notes.com/images/ethernet-layer-2-data-frame-format-01.svg">
</p>

- 이더넷 프레임(Ethernet Frame) : 헤더(Header) + 페이로드(Payload) + 트레일러(Trailer)

- 헤더(Header)
  - Destination address : 목적지 MAC 주소 (6 Byte)
  - Source address : 출발지 MAC 주소 (6 Byte)
  - (Ethernet) Type : 상위 프로토콜(3계층)의 타입을 표현 (2 Byte)
    - IPv4면 0x0800, ARP면 0x0806
- 페이로드(Payload) : 상위계층에서 내려온 데이터 (46 ~ 1500 Byte)
- 트레일러(Trailer) = 푸터(Footer)

  - FCS(Frame Check Sequence) : 데이터 전송 중 오류가 발생했는지 체크 (4 Byte)

<br>

## 2. 네트워크 계층

### 2.1. 인터넷 프로토콜 (IP, Internet Protocol)

- 3계층은 다른 네트워크 대역(WAN) or 서로 다른 LAN 대역, 즉, 멀리 떨어진 곳에 존재하는 네트워크 상에서 데이터를 교환하기 위한 프로토콜
- 라우팅 프로토콜을 기반으로 발신에서 착신까지의 패킷의 경로를 가장 효율적인 경로로 전송할 수 있도록 제어
- 네트워크 계층에서의 통신은 비신뢰성(IP 프로토콜이 전송하는 데이터가 정확하게 갔는지 확인하지 않는 것)과 비연결성(데이터 전송 이전에 미리 설정 과정을 거치지 않는 것)을 가짐
  - 데이터가 정확하게 전달될 것을 보장하지 않고, 중복된 패킷을 전달하거나 패킷의 순서를 잘못 전달할 가능성을 가짐
  - 데이터의 정확하고 순차적인 전달은 그보다 상위 프로토콜인 TCP에서 보장


#### 2.1.1. IP 주소(IP address)

- 컴퓨터 네트워크에서 장치들이 서로를 인식하고 통신을 하기 위해 할당된 주소

- 다른 네트워크를 식별 가능하며, 변경 불가능한 MAC 주소와 달리 IP 주소는 유동적으로 변경 가능

- IANA(Internet Assigned Numbers Authority)라는 국제기구에서 모든 IP 주소를 관리하고 대륙별로 할당하며, 2011년 부로 IPv4 주소가 모두 소진되어 할당이 중지됨

- IPv4(IPversion4)와 IPv6(IPversion6)의 두 종류 존재

- IPv4의 표현

  <p align="center">
      <img src="https://2.bp.blogspot.com/-SlPGguDipQI/VppF8tIeULI/AAAAAAAABz0/CaQMQImDk4Y/s1600/Pengertian%2Bdan%2BStruktur%2BPengalamatan%2BJaringan%2BIPv4%2B%2528IP%2Bversi%2B4%2529.jpg">
  </p>

  - 32비트를 8비트 단위(옥텟 Octet)로 끊어 온점(.)으로 구분하여 10진수로 표현
  - 네트워크 ID + 호스트 ID, 두 블록으로 나누어서 구분
    - 네트워크 ID : 전체 네트워크에서 각각의 네트워크를 구분하기 위한 ID
    - 호스트 ID : 하나의 네트워크에서 각각의 호스트(개인)을 구분하기 위한 ID

- 클래스(Class)

  <p align="center">
      <img src="https://media.geeksforgeeks.org/wp-content/uploads/20211030221518/hostandnetid-660x413.png">
  </p>

  - 하나의 네트워크에서 몇 개의 호스트 주소를 가질 수 있느냐에 따라 클래스를 나눌 수 있음 = Classful Network
  - IP 주소를 5개의 Class (A ~ E)로 나누어서 분배하는 방식
  - 상위 비트의 주소 시작값을 보고 각 클래스를 구분

  | 클래스 | 용도           | 최상위 비트 | 네트워크 ID 비트 수 | 호스트 ID 비트 수 | 시작 IP 주소 | 마지막 IP 주소  | 네트워크 수    | 호스트 수       |
  | ------ | -------------- | ----------- | ------------------- | ----------------- | ------------ | --------------- | -------------- | --------------- |
  | A      | 대규모         | 0xxx        | 8                   | 24                | 0.0.0.0      | 127.255.255.255 | 128(2**7)      | 16777216(2**24) |
  | B      | 중규모         | 10xx        | 16                  | 16                | 128.0.0.0    | 191.255.255.255 | 16384(2**14)   | 65536(2**16)    |
  | C      | 소규모         | 110x        | 24                  | 8                 | 192.0.0.0    | 223.255.255.255 | 2097152(2**21) | 256(2**8)       |
  | D      | 멀티캐스트용   | 1110        | N/A                 | N/A               | 224.0.0.0    | 239.255.255.255 | N/A            | N/A             |
  | E      | 연구용, 예약용 | 1111        | N/A                 | N/A               | 240.0.0.0    | 255.255.255.255 | N/A            | N/A             |

- 서브넷마스크(Subnet mask)

  <p align="center">
      <img src="https://www.connecteddots.online/images/blogimgs/IP%20Address%20192-168-0-1%20Subnet%20Mask%20255-255-255-0.png">
  </p>

  - Classful 네트워크의 경우 효율적이고 체계적인 IP 관리를 위해 도입되었지만 불필요한 IP 주소의 낭비로 IP 주소 부족 문제가 심화됨
  - IP 주소의 낭비를 줄이고, 네트워크의 성능을 향상시키기 위하여 대규모 네트워크를 논리적인 작은 네트워크 단위로 분할하는 서브네팅(Subnetting)을 도입
  - IP 주소에서 어디까지가 네트워크 ID이고, 어디부터 호스트 ID를 구분하는지 표시하는 역할
  - IP 주소와 같이 32비트로 구성되며 네트워크 ID를 표시하는 비트는 1, 호스트 ID를 표시하는 부분은 0
    - 따라서 서브넷마스트의 0 또는 1은 반드시 연속적으로 배치되어야 함
  - 또는 서브넷 마스크의 가장 앞 비트부터 1의 개수를 Prefix 표기로 사용하기도 함
    - 192.168.100.1/24 : 세번째 옥텟까지 네트워크 ID를 표시

- 게이트웨이(Gateway)

  - 서로 다른 네트워크간의 통신을 가능하게 하는 장비나 호스트를 지칭 (일반적으로 라우터)
  - 일반적으로 서버나 호스트에서 특정 패킷을 받았을 때, 자기 자신의 패킷이 아니면 그냥 버리지만 게이트웨이는 라우팅테이블을 확인하여 받은 패킷을 다른 적합한 네트워크로 전달해주는 역할

- 공인IP vs 사설IP

  - 공인 IP
    - 외부에 공개되어 있는 IP
    - 전세계에서 유일하며 ISP(인터넷 서비스 공급자, Internet Service Provider)를 통해 제공 받음
  - 사설 IP
    - 외부에서 접근할 수 없는 IP (외부 네트워크 대역에서는 사설 IP 대역을 볼 수 없음)
    - 공인 IP가 할당된 라우터나 공유기를 통해 로컬 네트워크에 연결된 기기에 사설 IP를 할당하여 사용
    - 포트포워딩을 통해 공인 IP에서 사설 IP로 들어올수 있음
  - 예를 들어, 네이버에 검색해서 나오는 ip주소는 공인 IP, cmd에서 치는 ip주소는 사설 IP
  - NAT(Network Address Translation, 네트워크 주소 변환 기술)을 이용하여 하나의 공인 IP 주소에 여러 개의 호스트가 사설 IP를 가지고 통신할 수 있게 해줌

- Bogon IP
  - 특수한 목적으로 사용하도록 정의된 IP 
  - 네트워크 주소 : 각 네트워크에서 해당 네트워크를 구분할 수 있는 주소, 각 네트워크 대역의 가장 작은 주소로 할당
  - 브로드캐스트 주소 : 동일한 네트워크에 속한 모든 호스트에게 데이터를 전달할 수 있는 주소, 각 네트워크 대역의 가장 큰 주소로 할당
    - IP 주소 : 172.16.20.20, 서브넷마스크 : 255.255.252.0 이라고 하면
    - 172.16.20.20 = **10101100.00010000.000101**00.00010100
    - 255.255.252.0 = **11111111.11111111.111111**00.00000000
    - 네트워크 주소 = **10101100.00010000.000101**00.00000000 = 172.16.20.0
    - 브로드캐스트 주소 = **10101100.00010000.000101**11.11111111 = 172.16.23.255

#### 2.1.2. IP 패킷 구조 (IPv4)

<p align="center">
    <img src="https://storage.googleapis.com/tb-img/production/21/05/F1_Shraddha_Raju_11.05.2021_D16.png">
</p>

- 헤더 필드와 데이터 필드로 나누며, 헤더 필드의 경우 20 Byte ~ 60 Byte
  - 일반적으로는 가장 뒤 Option 없이 사용됨 = 20 Byte

- Version : IP 프로토콜의 버전, IPv4인 경우 0x4 (4 bit)
- HLEN (Header Length) : 옵셜 필드를 포함한 헤더의 길이 = 전체 길이 / 4, 일반적으로 옵션이 없는 경우 0x5 (4 bit)
- Service Type(TOS, Type Of Service) : IP 패킷의 우선순위 결정 (현재는 사용되지 않음), 일반적으로 0x00 (1 Byte)
- Total length : 헤더와 페이로드(상위 계층으로부터 캡슐화 되어 내려온 데이터)까지 합친 IP 패킷의 전체 길이 (2 Byte)
- Identification : 패킷을 조립하거나 분해될 때 사용하는 식별 번호 (2 Byte)
- Flags : 패킷의 단편화 여부를 제공 (3 bit)
  - 첫번째 비트 : 예약된 필드로 사용하지 않음 (1 bit)
  - 두번째 비트(D, Don't fragment) : 1이면 패킷분할 불가능 (1 bit)
  - 세번째 비트(M, More fragments) : 0이면 해당 패킷이 분할된 패킷 중 가장 마지막 패킷 (1 bit)
- Fragmentation offset : 패킷 재조립시 분할된 패킷 간의 순서 (13 bit)
- Time to live (TTL) : 패킷이 경유할 수 있는 네트워크의 수 (1 Byte)
- Protocol : 상위 프로토콜의 타입 제공 (1 Byte)
  - ICMP = 1, TCP = 6, UDP = 17
- Header checksum : 오류 발생 검사 (2 Byte)
- Source IP address : 출발지 IP 주소 (4 Byte)
- Destination IP address : 목적지 IP 주소 (4 Byte)
- Option : 가변적인 옵션, 하나 추가 시마다 4 Byte 추가, 최대 10개까지 추가 가능

### 2.2. 주소 결정 프로토콜 (ARP, Address Resolution Protocol)

- 동일 네트워크 대역에서 IP 주소를 물리적 네트워크 주소인 MAC 주소로 매핑시켜주는 프로토콜
- 같은 LAN 대역에서의 통신을 위해 IP 주소를 사용하지만, 실제 장비에서 데이터 전송을 위해서는 MAC 주소가 필요하므로 ARP를 통해 IP 주소만 알더라도 LAN 대역에서 통신이 가능케 함

#### 2.2.1. ARP 패킷 구조

<p align="center">
    <img src="https://www.tutorialandexample.com/wp-content/uploads/2020/10/image-278.png">
</p>

- Hardware type : 이더넷인 경우 0x0001 (2 Byte) 
- Protocol type  : IPv4인 경우 0x0800 (2 Byte)
- Hardware address length : MAC주소의 길이 = 0x06 (1 Byte)
- Protocol address length : IP주소의 길이 = 0x04 (1 Byte)
- Operation code : 요청(Request)인 경우 0x0001, 응답(Reply)인 경우 0x0002 (2 Byte)
- Sender hardware address : 출발지 MAC 주소 (6 Byte)
- Sender protocol address : 출발지 IPv4 주소 (4 Byte)
- Target hardware addreess : 목적지 MAC 주소 (6 Byte)
- Target protocol address : 목적지 IPv4 주소 (4 Byte)

#### 2.2.2. ARP 통신 과정

<p align="center">
    <img src="https://www.tutorialandexample.com/wp-content/uploads/2020/10/image-274.png">
</p>

- 위와 같이 A 시스템에서 IP 주소가 141.23.56.23인 것을 아는 B 시스템의 MAC 주소를 알고 싶어한다고 할 때, ARP 통신 과정은 다음과 같다.
  1. A 시스템은 Sender IP 주소와 MAC 주소를 본인의 주소로, Target IP 주소는 141.23.56.23, Target MAC 주소는 0으로 채워넣어 ARP Request 패킷을 생성
  2. 생성한 ARP Request 패킷을 모든 시스템에 브로드캐스트(FF:FF:FF:FF:FF:FF) 방식으로 전송
  3. B 시스템은 수신받은 ARP Request 패킷을 열어 Target IP 주소가 본인임을 확인
  4. B 시스템은 Sender IP 주소와 MAC 주소(A4:6E:F4:59:83:AB)를 본인의 주소로, Target IP와 MAC 주소를 ARP Request의 Sender IP와 MAC 주소로 채워넣어 ARP Reply 패킷을 생성
  5. 생성한 ARP Reply 패킷을 A 시스템에 유니캐스트 방식으로 전송
  6. A 시스템은 해당 ARP Reply 패킷을 통해 B 시스템의 MAC 주소를 알 수 있고, ARP 캐시 테이블에 B 시스템의 IP 주소와 MAC 주소를 작성
  7. 이 후, B 시스템으로 데이터를 보내려고 할 때, ARP 캐시 테이블을 통해 MAC 주소를 확인하여 데이터 전송 가능
- ARP 캐시 테이블(ARP Cache Table)
  - 논리적인 IP 주소와 물리적인 MAC 주소의 대응 관계를 일정 시간동안 저장하는 테이블
  - 데이터를 전송할 때 ARP 캐시 테이블을 검사하여 매핑관계가 저장되어 있을 경우 브로드캐스트를 하지 않고 원하는 주소에 데이터 전송 가능

### 2.3. 인터넷 제어 메세지 프로토콜(ICMP, Internet Control Message Protocol)

- IP 프로토콜의 비신뢰성(전송 상태에 대한 관리를 하지 않음)을 보완하기 위한 프로토콜
- 상대방과 통신이 되는지 여부를 확인하기 위해 사용되며, 네트워크 내에서 IP 패킷 처리 시 발생하는 에러 상황에 대한 보고(Error reporting)의 역할
- 직접 데이터링크 계층으로 전달되지 않고, IP 패킷 내 데이터그램에 캡슐화되어 전달
- 윈도우의 ping과 같은 명령어에 ICMP 프로토콜을 활용

<br>

<br>

**Reference**

- [[따라學IT\] 네트워크 기초 - YouTube](https://www.youtube.com/watch?v=Av9UFzl_wis&list=PL0d8NnikouEWcF1jJueLdjRIC4HsUlULi)

- [MAC Address](https://networkencyclopedia.com/mac-address/)

- [Ethernet 802.3 Frame Format / Structure](https://www.electronics-notes.com/articles/connectivity/ethernet-ieee-802-3/data-frames-structure-format.php)

- [ARP - Address Mapping](https://www.tutorialandexample.com/address-mapping)

- [How to master IP Subnetting](https://www.connecteddots.online/resources/blog/how-to-master-ip-subnetting-part-one)
