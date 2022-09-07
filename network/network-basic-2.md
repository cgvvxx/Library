# 네트워크 2

> "따라하면서 배우는 IT - 네트워크의 기초"를 공부하면서 정리한 내용입니다.

<br>
<br>

**Contents**

- [데이터링크 계층](#1-데이터링크-계층(2계층))

<br>
<br>

## 1. 데이터링크 계층(2계층)

### 1.1. MAC 주소

- MAC 주소(Media Access Control address)

  - 제조업체가 하드웨어 장비에 직접적으로 할당하는 물리적 주소(고유 식별자)

  - LAN 대역에서 통신할 때 사용하는 주소

  - 표준 MAC 주소는 총 48비트로 구성되어 있음

    <p align="center">
        <img src="https://sf.ezoiccdn.com/ezoimgfmt/networkencyclopedia.com/wp-content/uploads/2019/08/mac-address.jpg?ezimgfmt=ng:webp/ngcb2">
    </p>

    - 상위 24비트 = OUI(Organizational Unique Identifier) 제조업체 식별 ID
    - 하위 24비트 = 제조사에서 부여한 고유번호

### 1.2. 이더넷 프로토콜 (Ethernet Protocol)

- LAN 대역에서 통신할 때 가장 널리 사용되는 프로토콜 (LAN 대역 네트워크의 90% 이상이 이더넷 프로토콜 방식을 사용)
- 같은 네트워크(LAN 대역) 상에 존재하는 장비들 사이의 데이터 전달 및 오류 제어, 흐름 제어 수행 (다른 네트워크 대역과 통신하는 경우 3계층의 도움을 받아야 함)

- 이더넷은 네트워크에 연결된 각 기기들이 48비트 길이의 고유의 MAC 주소를 가지고 이 주소를 이용해 상호간에 데이터를 주고 받을 수 있도록 만들어짐

#### 1.2.1. 프레임(Frame)

<p align="center">
    <img src="https://www.electronics-notes.com/images/ethernet-layer-2-data-frame-format-01.svg">
</p>

- 이더넷 프레임(Ethernet Frame) : 헤더(Header) + 페이로드(Payload) + 트레일러(Trailer)

- 헤더(Header)
  - Destination address : 목적지 MAC 주소 (6 Byte)
  - Source address : 출발지 MAC 주소 (6Byte)
  - (Ethernet) Type : 상위 프로토콜(3계층)의 타입을 표현 (2 Byte)
    - IPv4면 0x0800, ARP면 0x0806
- 페이로드(Payload) : 상위계층에서 내려온 데이터 (46 ~ 1500 Byte)
- 트레일러(Trailer) = 푸터(Footer)
  - FCS(Frame Check Sequence) : 데이터 전송 중 오류가 발생했는지 체크 (4 Byte)

<br>

<br>

**Reference**

- [[따라學IT\] 네트워크 기초 - YouTube](https://www.youtube.com/watch?v=Av9UFzl_wis&list=PL0d8NnikouEWcF1jJueLdjRIC4HsUlULi)


- [MAC Address - Network Encyclopedia](https://networkencyclopedia.com/mac-address/)

- [Ethernet 802.3 Frame Format / Structure](https://www.electronics-notes.com/articles/connectivity/ethernet-ieee-802-3/data-frames-structure-format.php)