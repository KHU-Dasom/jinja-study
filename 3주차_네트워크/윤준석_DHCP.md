# **DHCP**

## **🌐 DHCP란?**

- Dynamic Host Configuration Protocol의 약자로 Host IP 구성 관리를 단순화하는 IP 표준 프로토콜임.
- 쉽게 말해서 DHCP 서버가 클라이언트의 IP주소, 서브넷 마스크, 게이트웨이, DNS 서버 등의 정보를 자동으로 할당해주는 프로토콜이라고 생각하면 됨.
- 네트워크 관리자가 해야 할 작업을 간소화하며, DHCP 없이는 수동으로 IP 주소를 할당해야 함.

## **📖 DHCP 동작 방식**

- 클라이언트가 네트워크에 연결 시 IP주소를 요청함.
- IP주소 요청은 DHCP 서버로 전달되며 서버는 주소를 할당하고 모니터링하며 연결 해제 시 주소를 다시 가져옴.
- 해당 IP주소는 다른 클라이언트에 재할당할 수 있음.
- 클라이언트는 IP주소를 이용해 내부 및 공용 네트워크와 통신할 수 있음.

---

- DHCP의 동작 방식에는 Lease(임대), Renewal(갱신), Release(해제)가 있음.
  - DHCP를 통한 IP주소 할당은 임대 개념임.
  - DHCP 서버가 클라이언트에게 영구적으로 IP주소를 할당하는 것이 아니라 임대 기간을 정하여 그 기간 동안만 사용하게 함.
  - 임대기간 이후에 해당 IP주소를 사용하려면 갱신(Renewal)을 해야 함.
  - 더 이상 해당 IP주소가 필요하지 않다면 해제(Release)를 해야 함.

### **Lease - DHCP DORA Process**


![dhcp_lease](https://www.cisco.com/c/dam/en/us/support/docs/switches/catalyst-9300-series-switches/217366-configure-dhcp-in-ios-xe-evpn-vxlan-01.png)

1. DHCP Discover
   - 클라이언트가 네트워크에 연결 시 `DISCOVER` 메시지를 브로드캐스트로 전송함.
   - 클라이언트는 DHCP 서버의 주소를 모르기 때문에 브로드캐스트로 전송하여 DHCP 서버를 찾음.
2. DHCP Offer
   - DHCP 서버는 `OFFER` 메시지를 유니캐스트(또는 브로드캐스트)로 전송함.
   - DHCP 서버는 `DISCOVER` 메시지를 수신하면 `OFFER` 메시지를 클라이언트에게 전송함.
   - 구성 매개변수(예: IP 주소, 서브넷 마스크, 게이트웨이, DNS 서버 등)를 포함하여 전송함.
3. DHCP Request
   - 클라이언트가 `REQUEST` 메시지를 브로드캐스트로 전송함.
   - DHCP 서버의 존재를 알았기 때문에 `REQUEST` 메세지를 통해 하나의 DHCP 서버를 선택함.
   - 선택한 DHCP 서버에게 클라이언트가 사용할 네트워크 구성 매개변수를 요청함.
4. DHCP Ack
   - DHCP 서버는 `ACK` 메시지를 유니캐스트(또는 브로드캐스트)로 전송함.
   - 클라이언트가 DHCP 서버로 보낸 요청 네트워크 구성 매개변수를 확인한 뒤 해당 네트워크 정보를 전달해 줌.

### **Renewal**

![dhcp_renewal](https://crnetpackets.files.wordpress.com/2017/04/dhcp-basics-renew4.png)

- 임대 기간이 끝났을 때 IP주소 할당을 해제하고 다시 임대하게 되면 불필요한 브로드캐스트 트래픽이 발생함.
- 이를 위해 임대 기간이 끝났을 때 임대 기간을 연장하는 Renewal 과정이 존재함.
- 갱신 과정에서는 클라이언트가 DHCP 서버의 존재를 알고 있기 때문에 Discover & Offer 과정이 불필요함.

### **Release**

- 임대 기간이 끝났을 때 더 이상 해당 IP주소가 필요하지 않다면 해제(Release)를 해야 함.
- DHCP Release 과정을 통해 해제할 수 있음.
  - 클라이언트가 `RELEASE` 메시지를 유니캐스트로 전송하고 IP주소를 반환함.

## **💨 DHCP Relay Agent**

### **L2, L3, L4 Switch**

| Switch | Description |
| --- | --- |
| L2 Switch | MAC 주소를 기반으로 데이터를 전달하는 스위치, Layer2 Switch |
| L3 Switch | 공유기라고 생각하면 됨, 트래픽 모니터링, VLAN 등의 기능을 하는 스위치, Layer3 Switch |
| L4 Switch | 로드밸런싱 기반으로 트래픽 분산을 위한 스위치, Layer4 Switch |

---

![dhcp_relay_server](https://www.cisco.com/c/dam/en/us/support/docs/switches/catalyst-9300-series-switches/217366-configure-dhcp-in-ios-xe-evpn-vxlan-02.png)

- 위 DORA Process는 DHCP 서버가 같은 서브넷에 존재해야 가능하며 일반적으로 DHCP 서버는 다른 서브넷에 존재함.
- 클라이언트는 L2 네트워크에 존재하는데, L2 네트워크마다 DHCP 서버가 존재하는 것은 비효율적임.
- 보통 DHCP 서버는 L3 네트워크에 존재하며, 이를 해결하기 위해 L2 네트워크에는 DHCP Relay Agent가 존재함.
- 가정용에서 공유기를 사용하여 IP주소를 할당받는 경우 공유기 내에 DHCP 서버가 존재하므로 다른 네트워크에 요청할 필요가 없지만, 기업의 경우 DHCP가 라우터 밖에 존재하는 경우가 있음.

## **👍 DHCP 장점**

- IP주소 관리 용이
  - DHCP가 없는 네트워크에서는 IP주소를 수동으로 지정해줘야 함.
  - 충돌이 발생할 수 있으며, IP주소를 재사용하기 위해 IP주소를 해제해야 함.
  - 클라이언트가 다른 네트워크로 이동하게 되면 해당 클라이언트를 수동으로 수정해줘야 함.
- IP주소 구성 신뢰성
  - 동일한 IP주소를 이용하는 클라이언트 사이의 충돌을 방지함.
  - 충돌이 발생하는 경우 두 클라이언트 모두 인터넷에 연결하지 못하게 함.
- 높은 이동성
  - 클라이언트가 다른 네트워크로 이동하더라도 IP주소를 변경할 필요가 없음.

## **👎 DHCP 단점**

- IP주소 할당이 DHCP 서버에 의존적임.
  - DHCP 서버가 다운되면 클라이언트는 인터넷에 연결할 수 없음.
- DHCP로 인한 다양한 보안 문제가 발생함.
  - 승인받지 않은 DHCP 서버가 잘못된 정보를 클라이언트에게 제공할 수 있음.
  - 승인받지 않은 클라이언트가 DHCP 서버를 가로채 리소스에 대한 접근 권한을 얻을 수 있음.
  - 악성 클라이언트가 DHCP 리소스를 소모시킬 수 있음.

## **DHCP 보안 문제**

### **DHCP Spoofing**

- DHCP는 UDP 기반으로 동작하므로 해당 DHCP 서버가 인증 된 서버인지 확인할 수 없음.
- DHCP 프로토콜이 인증이 불가능한 취약점을 이용하여 DHCP 서버로 위장하여 조작된 정보를 전송하는 공격법.

![dhcp_spoofing](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F9947BD3359EC4B7B20)

- DHCP Snooping을 통해 DHCP Spoofing 공격을 방어할 수 있음.
  - DHCP 서버로부터 전달받은 DHCP 패킷을 검사하여 위장된 DHCP 서버로부터 전달받은 패킷인지 확인하는 기능임.

### **DHCP Starvation Attack**

![dhcp_starvation_attack](https://ars.els-cdn.com/content/image/1-s2.0-S0045790612001140-fx1.jpg)

- DHCP는 MAC주소 기반으로 클라이언트들에게 IP주소를 할당함.
- 이 점을 이용하여 MAC주소를 랜덤하게 변경하며 DHCP 서버에 요청을 보내 IP주소를 할당받는 공격법.
- DHCP 서버가 IP주소를 할당할 수 있는 최대 개수를 넘어서는 요청을 보내면서 서버의 자원을 소모시키고 가용 IP주소를 모두 할당하게 되면 정상 클라이언트는 IP주소를 할당받지 못함.
- DoS 공격이라고 할 수 있음.
