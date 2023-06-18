# Network Layer - AS, BGP, SDN

## Intra-AS

---

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled.png)

실제 세상에선 위처럼 네트워크가 flat하지 않다. 위처럼 되는 경우에는 scalable하지 않다. 프로토콜도 모든 네트워크가 통일되어 있을 수 없다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%201.png)

Autonomous system은 자치구를 의미한다. 즉, 실제 네트워크는 워낙 광범위하기 때문에 AS 영역이 있고, 같은 AS안에서는 같은 프로토콜을 사용할 수 있다. 오른쪽 그림에서는 AS가 4개 정도 존재하는 것을 확인할 수 있다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%202.png)

AS 안에서의 알고리즘은 Link state algorithm, 그리고 Distance Vector algorithm이다. 사실 이것도 이론적인 개념이고, 실제는 조금 다를 수 있다. 이는 우측 상단에 나와있는데, OSPF(Open Shorteset Path FIrst) 알고리즘이 Link state 알고리즘과 대응되는 개념이고, RIP(Routing Information Protocol)은 Distance Vector 알고리즘과 대응되는 개념이다. 일반적으로 OSPF는 link state 알고리즘을 사용하며, 계층화된 설계로 구성되어 복잡한 네트워크에서 확장성과 안정성을 제공합니다. RIP는 distance vector 알고리즘을 사용하며, 구현이 간단하지만 제약사항이 있기 때문에 일반적으로 작은 네트워크에서 사용되는 경량 라우팅 프로토콜이다. 

이러한 알고리즘은 AS 내부(Intra AS)에서 적용되는 알고리즘이고, AS 외부에서 AS 간(Inter AS)에 적용되는 알고리즘은 BGP(Border Gateway Protocol)이 있다. 이는 Distance Vector 알고리즘과 유사한 부분이 있지만, 이 알고리즘은 실제론 distance를 가지고 하지 않고 path를 가지고 합니다. cost 개념보다는 path가 연결되어 있는지 아닌지를 판단하는 알고리즘입니다. 

각 라우터는 Intra AS Routing algorithm과 Inter AS Routing algorithm으로부터 형성된 Forwarding table을 가지고 있다. 이를 바탕으로 상황에 맞게 포워딩을 적용하게 된다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%203.png)

Inter AS task는, AS1을 예시로 들 때, AS2를 넘어선 other network에 destination이 있다면, AS2로부터 해당 정보는 AS1의 1b가 path가 있음을 듣고, 이를 1c 라우터는 AS3에 있는 3a라우터 등에게 먼저 전달하는 것이다. 즉, 목적지가 있다고 할 때 그 목적지로 갈 수 있는 “길이 있음”을 전달하는 것이다 

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%204.png)

AS 내부에서 사용하는 라우팅 알고리즘은 

- RIP(routing Information protocol),
- OSPF(open shortest path first),
- IGRP(Interior Gateway Routing Protocol)

→ Cisco : 샌프란시스코에서 회사가 처음 시작해서 이름을 이렇게 만듦

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%205.png)

위 프로토콜은 link state 알고리즘을 사용한다. 이 프로토콜은 application이나 transport 레이어의 프로토콜이 아니라 network layer 프로토콜이다. 그리고 route compute할 때에는 다익스트라 알고리즘을 사용한다. 또한 해킹 등으로부터 안전하게 유지하기 위해 모든 ospf 메세지는 authenticated 되어있어서 인증된 사람만 메세지를 받을 수 있다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%206.png)

계층적인 OSPF는 local area와 backbone 두 가지로 나뉜다.

local area는 우리가 쉽게 생각할 수 있는 internal 영역을 공유하는 라우터 집합이고, backbone은 해당 areas들과 공통적으로 연결된 구역이다. 여기서 backbone과 local area를 연결하는 라우터를 ABR(area border routers)라고 한다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%207.png)

다른 AS로 붙는 부분이 boundary router이다. AS는 backbone과 internal routers, 2개의 계층으로 나뉘어 있다. 일반적으로 backbone은 끊어지면 안되므로 ring 형태로 모두 연결된 모양으로 구성된다. backbone은 Open Shortest Path first(link state algorithm 사용)을 사용한다.

<aside>
💡 교수님 comment,

여기서 알아둬야 할 점은, AS가 위와 같이 계층형 구조로 이루어져 있고, 내부적으로 routing algorithm을 사용하는 것은 OSPF와 RIP를 사용한다는 것과, 이 것들은 link state algorithm과 distance vector algorithm을 사용한다는 점 정도만 알면 된다고 하셨다.

</aside>

## BGP (Border Gateway Protocol)”

---

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%208.png)

> *de facto standard* : 표준화 단체에서 만든 것은 아니지만, 사실상 표준이 된 것을 의미합니다. 산업 등의 특정 분야에서 널리 통용되어 사용되는 것으로, 공식적으로 표준화된 것은 아니지만 사실상 많은 사람들에 의해 인정되는 “표준”과 같은 개념입니다.
> 

BGP는 사실상 표준이 된 도메인 간의 라우팅 프로토콜입니다. BGP는 마치 접착제와 같이 경계에서 인터넷을 하나로 붙여주는 역할을 한다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%209.png)

- eBGP : 주변 AS로부터 서브넷 접근 정보를 얻는 BGP를 의미합니다.
- iBGP : AS 내부 라우터들에게 접근 정보들을 전파하는 BGP를 의미합니다.
    
    → 연결 여부(reachability)와 정책(policy)을 파악하여, 다른 네트워크에 대한 좋은 접근 경로를 결정하는 프로토콜입니다.
    

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2010.png)

BGP는 여러 BGP를 사용하는 라우터 간에 TCP 커넥션이 수립되어 있고, 각 라우터가 얻게 되는 path vector를 주변에 advertising하는 것이 원칙입니다. 하지만 만약에 “자신”이 포함된 path가 자신에게 도달했을 경우에는 그 path는 우회한 path라고 간주하여 advertising을 하지 않습니다. 위의 그림 중 AS2, AS3, 1.0.0.0/8 과 같은 정보가 path입니다. 이를 해석하면, 1C라우터 입장에서 AS2와 AS3를 거치면 X라우터(1.0.0.0/8)에 도달할 수 있구나 하는 것을 의미합니다. 

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2011.png)

BGP를 통해 전달하는 정보는 AS-PATH와 NEXT-HOP입니다. AS-PATH는 이전에 봤던 path와 동일한 의미이고, NEXT-HOP은 해당 path를 타고 가기 위해 활용해야 하는 가장 가까운 HOP(라우터)에 대한 정보를 의미합니다. 

BGP는 policy-based routing입니다. 

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2012.png)

정책이란, 단순하게 말하면 B가 B A w라는 path를, 자신과 연결된 “C라우터에 전달하지 않겠다”라고 정하여 advertising을 C에 대해서만 하지 않는 것이다. 네트워크는 여러 이해관계가 얽혀 있으므로, B가 C에게 해당 path를 알려준다 하여 얻는 게 없다면 정책적으로 이를 충분히 제한할 수 있고, 이는 BGP의 특징 중 하나입니다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2013.png)

이제 **각 AS는 policy에 따라 송신과 수신을 결정하며**, path와 next-hop에 대한 정보를 서로 advertising하는 모습이 위 그림에 나타나 있습니다. AS3에서 AS2로 eBGP에 의해 AS3, X라는 path정보를 같이 주고, next-hop은 아마 3a를 줬을 겁니다. 그러면 이후 AS2 내부에서 2c는 모든 내부 라우터에게 iBGP로 AS-Path는 AS3, X 로, next-hop은 2c를 통하라고 advertising을 하고, 이는 AS1로 eBGP할 때, AS2, AS3, X로 구성되는 걸 확인할 수 있습니다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2014.png)

위 그림에서 1c입장에서는 AS3,X와 AS2, AS3,X 두 가지 path를 모두 수신했을 때, policy에 따라 어느 path를 선택할 건지 결정하게 됩니다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2015.png)

BGP는 

`open` 메세지로 semi-permanent TCP connection을 수립

`update` 메세지로 path를 advertising하는 메세지를 보내고

`keep alive` 메세지로 일종의 ping을 줘서 TCP 연결을 유지한다. TCP는 오랫동안 신호가 없으면 죽은 커넥션으로 인식하고 끊어버리기 때문에 이처럼 연결 유지를 위한 작업이 필요하다.

`notification`은 이전 메세지에 에러가 있거나 connection을 종료할 때 사용되는 메세지이다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2016.png)

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2017.png)

위 사진들을 통해 알 수 있는 것은, iBGP나 eBGP를 통해 AS별로 path 정보를 받은 걸 바탕으로 각각 forwarding table을 작성하며, distant prefix(여기서는 라우터 X와 동일시함)까지 가기 위해서 각각의 라우터는 어떤 링크로 데이터를 라우팅해야 하는지를 작성된 forwarding table을 통해 결정합니다. 위 예시에서 1d 라우터는 데이터를 받고, 이 데이터가 X로 가야 하는 경우에는 interface가 1인 링크를 태워 보내야 한다고 하는 것이고, 1a는 interface 2를 타고 보내야 한다고 되어 있는 것입니다.

<aside>
💡 prefix는 IP주소를 구성하는 네트워크 ID(서브넷 ID 포함)와 호스트 ID 중 네트워크 부분을 나타냅니다. 즉, IP 주소 192.168.0.0/24라고 할 때, “/24”가 prefix인 겁니다.
BGP가 IP 주소 공간을 프리픽스 단위로 관리하고 경로 선택에 활용하기 때문에 이러한 용어가 등장하게 되었습니다.

</aside>

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2018.png)

핫 포테이토 라우팅은 단순하게 2d 라우터가 iBGP로 path에 대한 정보를 얻었을 때, 주변 엣지를 통한 cost 중 최소 코스트인 링크로 데이터를 링킹하는 것을 의미한다. 이 때에 AS 간 cost는 고려하지 않는 것이 hot potato routing이다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2019.png)

low cost를 무조건 고르는 게 아니라, policy 준수가 우선되는 것이 BGP의 특징이다. 그리고 Path를 선정할 때 가급적 짧은 path로 고르게 되는데, 부분적으로 hot potato routing을 적용한다. 즉, 당장 바로 다음 라우터를 선정할 때 가장 가까운 라우터로 선정하는 것을 의미한다. hot potato routing은 별개의 라우팅 알고리즘으로, 그리디 알고리즘으로 이해하면 되고 policy decision 등의 여러 조건을 따지지 않고 당장 앞의 상황에서 최선의 선택만을 고려하는 것을 의미한다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2020.png)

inter 라우팅과 intra 라우팅의 차이는 policy를 사용하냐 안하냐의 차이이다.

- intra AS에서는 policy를 사용하지 않습니다.
- inter AS에서는 policy에 입각하여 traffic을 control하게 됩니다.

그리고 계층화되어있는 구조는 forwarding table size를 줄여주고, 업데이트 트래픽을 감소시킵니다.

- intra AS는 퍼포먼스에 초점이 맞춰져 있습니다.
- inter AS는 퍼포먼스보다 policy가 더 우선됩니다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2021.png)

- Unicast : 한 명에게 routing하는 것
- Broadcast : 모두에게 routing하는 것
- multicast : 여러 명에게 routing하는 것
- anycast : 동일한 IP 주소를 가진 여러 대의 서버나 라우터를 대상으로, 그 중에서 가장 가까운 서버나 라우터에 응답을 보내는 방식을 의미합니다. 보통 DNS에서 많이 사용함. (아래 참고)
- Geocast : location-based multicast라고 이해하면 된다. 지역 기반의 멀티캐스트다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2022.png)

애니캐스트는 DNS에서 주로 사용됩니다. 구글의 DNS 서버는 모두 8.8.8.8이라는 IP 주소를 공유하고 있는데, 지역마다 다른 DNS 서버를 물리적으로 구현해 두었습니다. 따라서 클라이언트는 구글 DNS에 DNS 쿼리를 보내면, 지역적으로 가장 가까운 8.8.8.8의 DNS 서버에 쿼리가 날라가도록 라우팅되어 더 빠른 응답을 제공할 수 있습니다.

## SDN(software-defined network)

---

### 등장 배경

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2023.png)

전통적인 네트워크 라우팅 알고리즘과 트래픽 엔지니어링은 link weight를 정의해야만 수행이 가능했습니다. 그리고 오로지 link weight를 기준으로 라우팅을 구성하였습니다. 하지만 네트워크 라우팅은 그렇게 평면적으로 판단해서는 안된다는 문제 제기가 있던 겁니다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2024.png)

만약 데이터를 라우팅할 때 병렬적으로 전송을 하기 위한 라우팅을 하고 싶어도, 이는 기존 알고리즘으로는 불가능한 선택지였습니다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2025.png)

빨간 트래픽과 파란 트래픽을 구분하여 라우팅하고 싶은데, 기존의 destination based forwarding과 Link state(Open shortest path first), 그리고 distance vector(RIP) 알고리즘으로는 불가능했습니다.

### 정의

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2026.png)

네트워크를 소프트웨어로 정의해서 사용하겠다 하는 것이 SDN입니다. 라우팅, 포워딩, 스위칭하는 것을 하나의 router가 수행하는 것이 monolithic하다고 합니다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2027.png)

forwarding table을 구성하고, 라우팅을 명령하고 하는 일들이 소프트웨어의 영역이라고 보고, router가 갖고 있던 여러 작업들을 software로 할 수 있는 영역(control plane)과 hardware 영역(data plane)으로 구분하는 것이 SDN의 핵심입니다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2028.png)

따라서 Control Agents만 라우터에 둬서 명령이 내려졌을 때 이를 잘 수행할 수 있도록 환경을 구축하고, logically centrallized 하여 정보를 모아두고 컴퓨팅하기 위한 remote controller를 소프트웨어로 구성하여 라우팅을 구성할 수 있으면 될 것이라고 생각한 게 SDN입니다. 이렇게 하면 훨씬 호율적이고 빠를 것이라 판단한 겁니다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2029.png)

그렇다면 왜 logically 중앙화되어야 하는 걸까?

- 네트워크 매니징이 쉽다.
- configuration에 문제가 발생할 때 처리가 쉽다. 예를 들어, 링크를 바꾼다던지 할 때 라우터에 가서 작업해야 했는데, 중앙화되면 중앙에서 전부 작업 가능

⇒ 유연성을 확보할 수 있다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2030.png)

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2031.png)

여기서는 forward 대신 flow라는 용어를 사용한다. openflow는 표준인데, data plane 영역 interface의 표준을 의미한다. control plane의 remote controller는 일종의 network os와 같은 역할을 한다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2032.png)

그렇다면 왜 forwarding table이 아니라 flow table인가?

forwarding은 말 그대로 routing만 하는 개념이라고 이해하면 된다. 하지만 여기서는 각각의 라우터가 포워딩 뿐만 아니라 스위칭, 방화벽 등의 기능을 수행하는 장치로도 얼마든지 적용할 수 있다. 그렇기 때문에 flow table에 어떤 명령을 적용해두냐에 따라 라우터의 기능이 달라지는 것이다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2033.png)

그리고 위 사진과 같이, data plane과 control plane 을 원활하게 연결하기 위해 control plane에 SDN controller가 있어서 인터페이스 역할을 하게 된다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2034.png)

데이터 플레인 영역의 하드웨어들은 SDN 적용시 조금 더 단순한 구조를 가지게 되므로 빨라지고, 값도 저렴해집니다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2035.png)

SNMP (simple network management protocol) 네트워크 관리 프로토콜로, 하드웨어를 관리하기 위해 southbound api에 배치되어 있다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2036.png)

SDN 구조를 구현하기 위해 사용되는 것이 openflow protocol입니다 .이는 SDN 구조의 OS라고 할 수 있는 controller와 hardware인 switch 사이에서 동작하는 일종의 인터페이스입니다. 논리적으로는 라우터 위에서 소프트웨어처럼 동작하는게 SDN이지만, 사실 이것도 그냥 TCP를 이용하여 메세지(커맨드)를 전달하는 것일 뿐입니다.  openflow msg는

- controller-to-switch
- asynchronous (switch-to-controller)
- symmetric(misc)

로 구성됩니다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2037.png)

controller는 switch에 기능, 설정, 상태변경 등의 작업을 수행합니다. 또한 아주 드물겠지만, packet을 스위치에 직접 전달하기도 합니다. 

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2038.png)

스위치는 컨트롤러에 패킷을 전송하거나, 스위치에서 flow table entry가 삭제되거나, controller에 port 변경을 알려줍니다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2039.png)

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2040.png)

위와 같은 과정이 어떻게 이루어지는지 그에 대한 예시입니다. 우선 라우터가 어떤 패킷을 받으면 이를 우선 SDN controller로 올리면 이는 openflow를 거쳐, link state info를 먼저 읽고, 다익스트라 등의 라우팅 알고리즘을 거친 뒤, link state를 업데이트한 뒤, 이렇게 처리한 결과를 라우터로 보냅니다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2041.png)

openflow의 flow table entries는 비단 기본적인 dst-based forwarding 뿐만 아니라, 어떤 포트로 들어오는 요청 또는 특정 IP 주소로부터 오는 요청은 block하는 firewall기능 또는 link layer에서 하는 역할인 특정 MAC 주소로부터 오는 요청을 특정 포트로 포워딩하는 것 등 여러 작업이 가능합니다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2042.png)

위와 같이 openflow는 라우터로 하여금 받은 데이터를 가지고 match + action을 작업합니다. 

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2043.png)

이처럼 강점이 많은 기술이지만, 레거시 환경이 이미 충분히 견고하게 운영되고 있으며 점차적으로 적용되기 위해 SDN이 적용되는 속도가 더딥니다.

![Untitled](Network%20Layer%20-%20AS,%20BGP,%20SDN%204f78d50cd1d941e8a7718a5ca3b30dd5/Untitled%2044.png)