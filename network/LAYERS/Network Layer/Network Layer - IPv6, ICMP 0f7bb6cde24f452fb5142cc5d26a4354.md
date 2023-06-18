# Network Layer - IPv6, ICMP

![Untitled](Network%20Layer%20-%20IPv6,%20ICMP%200f7bb6cde24f452fb5142cc5d26a4354/Untitled.png)

32bit의 주소가 너무 짧아서 이미 고갈되었다.

option에 따라 헤더가 가변적이었어서 성능이 느렸다.

QoS : Quality of service

→ IPv6는 달라졌다.

- 중간 라우터에서 fragmentation이 없어져서 네트워크에서 핸들링이 쉬워졌다.
- 헤더 사이즈를 40byte로 고정하여 속도가 빨라졌다. + 확장이 용이해짐

![Untitled](Network%20Layer%20-%20IPv6,%20ICMP%200f7bb6cde24f452fb5142cc5d26a4354/Untitled%201.png)

IPv6는 IPv4의 헤더와 비교해 볼 필요가 있다.

- IPv4의 header length는 IPv6로 되면서 헤더 사이즈가 40byte로 고정되기 때문에 필요하지 않다.
- IPv6는 네트워크 중 라우팅 단계에서 fragmentation을 하지 않기 때문에 identifier, flags, fragment offset이 필요 없게 되었다.
- IPv6는 header checksum이 없다. (checksum을 안한다는 의미는 router/network layer에서 안한다는 의미. 이렇게 하기 때문에 속도가 빨라진다. 단, transport layer에서는 여전히 checksum을 하니까 그런 의미에서 error detection도 챙기고 네트워크 속도도 빠르게 가져가기 위한 선택. 굳이 여러번 checksum을 해서 오버헤드가 증가되는 것을 막기 위함)

![Untitled](Network%20Layer%20-%20IPv6,%20ICMP%200f7bb6cde24f452fb5142cc5d26a4354/Untitled%202.png)

- checksum은 network layer에서 제거됨
- option제거 : size가 고정되고, next header 필드를 둠
- IPv6 : fragmentation은 routing할 때에는(network layer) 수행하지 않고, 오직 source/destination nodes에서만 수행됨 (확장 헤더;next header 에 해당 정보가 들어감)

![Untitled](Network%20Layer%20-%20IPv6,%20ICMP%200f7bb6cde24f452fb5142cc5d26a4354/Untitled%203.png)

전 세계 네트워크를 IPv4에서 IPv6로 바꾸려면, 하루 날 잡고 전 세계가 같은 시간에 진행할 수 있다면 좋겠지만 이는 불가능에 수렴한다. 따라서 점차적으로 IPv4와 IPv6를 공존시키는 과도기를 진행하면서 차츰 업데이트해간다.

공존 방법 중 한 가지가 tunneling이다. tunneling은 IPv6 데이터그램이 IPv4의 payload로 IPv4 데이터그램에 담겨서 통신하는 방법을 의미한다. 

터널링의 대표적인 예시인 VPN(Virtual Private Network)은 tunneling으로 한다. 가상 사설 네트워크라는 의미로, 회사 안의 네트워크를 외부에서 접속하지 못하게 막으려면, 내부에서만 공유되는 사설 네트워크 환경을 구축하면 될 것이다. 이러한 환경을 구축할 때 터널링을 사용한다.

Host(Client) ↔ Public Network ↔ Server 중에서 IPv6으로 전환하기에 가장 어려운 곳은 바로 public network 이다.  

![Untitled](Network%20Layer%20-%20IPv6,%20ICMP%200f7bb6cde24f452fb5142cc5d26a4354/Untitled%204.png)

위처럼 IPv4와 IPv6를 둘 다 지원하는 통신은 dual stack을 활용한다고 한다. A→B로 갈 때에는 IPv6로 통신하다가, network 단계에서는 아직 IPv4인 곳이 많으므로 이를 IPv4의 payload에 담아서 전송하고, 다시 D→ E 단계에서 IPv4의 페이로드로부터 IPv6 데이터그램을 추출하고 이를 destination에 전달하는 방식이 tunneling이다.

## ICMP

---

![Untitled](Network%20Layer%20-%20IPv6,%20ICMP%200f7bb6cde24f452fb5142cc5d26a4354/Untitled%205.png)

ICMP는 인터넷 프로토콜 suite의 일부로 사용되는 프로토콜이다. “type + code + description”으로 이루어진 datagram 메세지를 통해 error reporting을 위해 사용된다. 또한 echo를 통해 서버가 살아있음을 확인하는 것인 ping을 사용하여 통신 상태를 지속적으로 체크한다. ping을 사용할 떄에는 RTT도 체크하여 서버로부터의 통신이 원활한지도 체크한다.

위의 type과 code 조합 중에서 중요한 건, 0 0 : echo reply, 8 0 : echo reply. **3 3 : destination port unreachable**, 11 0 : TTL expired

![Untitled](Network%20Layer%20-%20IPv6,%20ICMP%200f7bb6cde24f452fb5142cc5d26a4354/Untitled%206.png)

traceroute는 ICMP 또는 UDP 패킷을 사용하여 목적지까지의 경로를 추적하고, 네트워크 내에서 문제를 일으키는 포트를 색출하기 위해 사용됩니다.

Linux 는 UDP 패킷을 이용하여 traceroute를 수행하며, windows(dos)에서는 ICMP를 이용하여 tracert를 수행합니다. (위 설명은 Linux 기준으로 이루어졌습니다.)

여기서 주목할 점은, windows 환경에서는 UDP가 아니라 ICMP를 바로 사용하므로,  중간에 노드가 TTL Expired는 똑같이 오지만, destination에 도착했을 떄는, 윈도우 환경의 경우, UDP가 아니라 ICMP를 바로 보낸 거라서 destination port unreachable이 아니라 ICMP reply가 오게 됩니다.~

![Untitled](Network%20Layer%20-%20IPv6,%20ICMP%200f7bb6cde24f452fb5142cc5d26a4354/Untitled%207.png)

ICMP 또한 ICMPv6로 바뀌면서 변화한 게 몇 가지 있다.

- IPv6는 router에서 fragmentation을 하지 않으므로(호스트 단에서는 가능; 출발지와 목적지; TCP는 MTU를 고려해서 세그먼트 사이즈를 정하여 쪼갬. UDP는 IPv4는 호스트 단에서도 쪼개고, 필요시 라우팅 중에도 쪼개서 보넀었는데, IPv6에서는 호스트 단(노드)에서만 레이어를 거치며 쪼개서 보내게 되는 것이 일반적임.), 이를 활용하는 ICMPv6에서도 이에 대한 언급을 명확히 해줘야 한다. 즉, 패킷이 너무 크니까 전송할 수 없다고 말해주는 것이 “Packet Too Big”이라는 메세지를 말해주는 것을 의미한다. 패킷이 MTU(Maximum Transmission Unit)보다 큰 경우에 발생하는 것이고, 이런 경우에는 “다른 놈을 시켜서 전달한다………???????” - 최차봉