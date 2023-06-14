# Link Layer - ARP, Ethernet

링크 레이어는 노드와 노드 간 데이터를 전송하는 계층이니까 LAN 계열이다.

라우터와 라우터 보단, 라우터와 라우터 사이의 엣지를 구성하는 노드와 노드 간에서 더욱 많이 다뤄지는 것이 link layer이다.

## MAC addresses and ARP

---

MAC address의 MAC은 Media Access Channel 또는 Multiple Access Channel이다. 

![Untitled](Link%20Layer%20-%20ARP,%20Ethernet%205b5a0fd3525c404ca585bb8f699b5458/Untitled.png)

IPv4는 32-bit IP address이고, 이는 네트워크 레이어에서 호스트 간 데이터 전송을 위한 인터페이스 역할을 한다. 즉, IP 주소는 네트워크 레이어에서 데이터 전송의 포워딩을 위해 사용된다.

이처럼 링크 레이어에서도 각 노드 간 데이터 전송을 위한 인터페이스로서 MAC 주소를 사용한다. (또는 LAN 또는 physical 또는 Ethernet 주소를 사용한다고도 한다.) 이는 물리적으로 연결된 두 노드 간에 프레임(링크 레이어 PDU) 전송을 위해 논리적인 개념으로서 사용되는 것이다. MAC 주소는 48-bit 으로 이루어져 있고, NIC의 ROM에 구워져 있다. (간혹 소프트웨어에서 설정 가능한 경우도 있다.)

MAC 주소는 6 Bytes(48 bits)로 구성되어 있고, 앞의 3 Bytes는 생산자 ID, 뒤 3 Bytes는 제품 ID로 구성된다.

![Untitled](Link%20Layer%20-%20ARP,%20Ethernet%205b5a0fd3525c404ca585bb8f699b5458/Untitled%201.png)

ARP는 Address Resolution Protocol이다. 

LAN은 유/무선을 모두 포괄하는 개념이다. LAN 내에서는 link layer의 노드 간에는 MAC 주소를 통해 통신을 하므로 IP 주소를 MAC 주소로 바꿔서 사용해야 한다. IP주소를 MAC주소로 바꿔주는 것이 ARP이다.

![Untitled](Link%20Layer%20-%20ARP,%20Ethernet%205b5a0fd3525c404ca585bb8f699b5458/Untitled%202.png)

MAC 주소는 LAN 내에서만 사용되므로, 같은 LAN 내에서 겹치는 MAC 주소가 있기는 어렵다.

IP 주소 == 집 주소

DHCP를 통해 IP 주소를 받는다. DHCP는 항공대 내 또는 통신사에 있다. 통신사든 항공대든, DHCP에서 IP 주소를 주면 그것은 네트워크 ID와 호스트 ID로 구성된다. 이는 다른 곳으로 내가 이동하면 얼마든지 변경될 수 있다. 이사가는 거랑 똑같다.

MAC 주소는 주민등록번호와 같은 고유한 신분 증명 번호이다. 이는 변하지 않는다.

![Untitled](Link%20Layer%20-%20ARP,%20Ethernet%205b5a0fd3525c404ca585bb8f699b5458/Untitled%203.png)

cmd 열어서 `arp -a` 치면 다음과 같이 ip address 와 매핑된 mac 주소를 확인할 수 있다.

> 참고로 cmd에서 `route print` 치면 ip address마다 연결된 라우터 인터페이스도 알 수 있다.
> 

![Untitled](Link%20Layer%20-%20ARP,%20Ethernet%205b5a0fd3525c404ca585bb8f699b5458/Untitled%204.png)

ARP table에는 하나의 LAN 안의 각 노드는 IP 주소와 MAC 주소가 매핑된 테이블을 가지고 있다. 이는 <IP address ; MAC address ; TTL>로 구성된다. 여기서 TTL은 유효한 lifetime을 나타내며, 일반적으로 20분이다.

![Untitled](Link%20Layer%20-%20ARP,%20Ethernet%205b5a0fd3525c404ca585bb8f699b5458/Untitled%205.png)

A가 B에게 데이터그램을 보내고 싶은데, B의 IP 주소만 알고 있고 MAC 주소를 모른다고 할 때, A는 B의 IP 주소를 담아 ARP 쿼리 패킷을 구성하여 이를 FF-FF-FF-FF-FF-FF MAC 주소로 보낸다(==broadcast). 이렇게 하면 동일 LAN(==라우터를 건너진 못하는 영역 내)에 있는 모든 노드가 해당 ARP 쿼리를 수신하게 되고 자신에게 온 것인지 목적지 필드에 담긴 IP 주소를 보고 취사하게 된다(맞지 않으면 discard). 이 때 B 노드도 해당 ARP 패킷을 받기 때문에 자신의 MAC 주소를 A에게 회신해주게 된다. B가 A에게 응답해줄 때에는 A가 보내온 프레임에 담긴 MAC 주소를 이용하여 Unicast로 응답한다.

A는 이를 받고, 이 정보가 TTL을 경과하여 timeout되기 전까진 ARP table에 등록하여 cache처럼 사용한다. ARP table은 어떤 네트워크 관리자가 배정해주는게 아니라, 노드가 LAN에 참여할 때 ARP쿼리를 통해 ARP 테이블을 자동으로 생성(Plug and play)한다.

![Untitled](Link%20Layer%20-%20ARP,%20Ethernet%205b5a0fd3525c404ca585bb8f699b5458/Untitled%206.png)