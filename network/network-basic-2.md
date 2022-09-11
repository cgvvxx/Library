# 네트워크 2

> "따라하면서 배우는 IT - 네트워크의 기초"를 공부하면서 정리한 내용입니다.

<br>
<br>

**Contents**

- [데이터링크 계층](#1-데이터링크-계층-(2계층))

- [네트워크 계층](#1-네트워크-계층-(3계층))

<br>
<br>

## 1. 데이터링크 계층 (2계층)

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

## 2. 네트워크 계층 (3계층)

### 2.1. 주소 결정 프로토콜 (ARP, Address Resolution Protocol)

- 동일 네트워크 대역에서 논리적 네트워크 주소인 IP 주소를 물리적 네트워크 주소인 MAC 주소로 매핑시켜주는 네트워크 계층의 프로토콜
- 같은 LAN 대역에서의 통신을 위해 IP 주소를 사용하지만, 실제 장비에서 데이터 전송을 위해서는 MAC 주소가 필요하므로 ARP를 통해 IP 주소만 알더라도 LAN 대역에서 통신이 가능케 함

#### 2.1.1. ARP 패킷 구조

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

#### 2.1.2. ARP 통신 과정

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

<br>

<br>

**Reference**

- [[따라學IT\] 네트워크 기초 - YouTube](https://www.youtube.com/watch?v=Av9UFzl_wis&list=PL0d8NnikouEWcF1jJueLdjRIC4HsUlULi)

- [MAC Address](https://networkencyclopedia.com/mac-address/)

- [Ethernet 802.3 Frame Format / Structure](https://www.electronics-notes.com/articles/connectivity/ethernet-ieee-802-3/data-frames-structure-format.php)

- [ARP - Address Mapping](https://www.tutorialandexample.com/address-mapping)