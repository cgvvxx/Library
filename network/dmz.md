# DMZ (DeMilitarized Zone)

<p align="center">
    <img src="https://docs.oracle.com/en/operating-systems/oracle-linux/6/security/images/deplarch2.png">
</p>

DMZ(Demilitarized zone)이란 내부 네트워크(여러 인트라넷 또는 내부 시스템 등의 보안 영역)와 외부 네트워크(비보안 영역) 구간 사이에 위치한 중간 지점으로, 외부에서 직접적으로 내부 네트워크로의 접근을 막아 보안이 뛰어나다. 즉, 외부에서 악의적인 목적으로 기업 내부의 네트워크나 PC 등에 직접 접속하지 못하도록 한다.

DMZ을 사용하므로써 얻을 수 있는 장점들은 다음과 같다.

1. 내부 및 외부 네트워크 격리 : DMZ는 중간 완충 지역으로서 안전한 내부 네트워크와 공격이 가득한 외부 네트워크 사이의 버퍼 역할을 직접 수행함으로 임의의 공격이 DMZ에서 종료되도록 하고 이는 기업의 중요한 데이터 서버에 대한 공격의 위험을 줄일 수 있다.
2. 트래픽 접근 제어 : DMZ에 서버를 배치하여 외부 네트워크에 서비스를 제공하여 인터넷 사용자가 해당 서비스를 이용할 수 있도록 할 수 있다.
3. 악성 트래픽 차단 : 악성 트래픽 탐지 장비를 배치하여 트래픽을 격리하여 비즈니스의 정상적인 운영성을 보장할 수 있다.

또한, 외부 연결이 필요한 시스템들(웹 서버, 이메일 서버 등)을 DMZ에 배치하므로써 외부 네트워크에서 DMZ는 자유롭게 들어올 수 있도록 설정하지만, 외부 네트워크에서 DMZ를 거쳐 내부 네트워크로 직접 들어오지 못하도록 필터링을 한다. 또한 내부에서 DMZ로의 접근은 가능하지만 DMZ부터 내부로의 접근은 허용 가능한 특정 접근만을 허락하도록 방화벽 설정을 해야 한다.

<br>

**Reference**

- [Recommended Deployment Configurations](https://docs.oracle.com/en/operating-systems/oracle-linux/6/security/ol_recdepcfg_sec.html)

- [What is DMZ in network?](https://forum.huawei.com/enterprise/en/what-is-dmz-in-network/thread/901633-867)
