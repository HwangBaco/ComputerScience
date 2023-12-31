# Network Layer - DHCP, NAT

## IPv4 address exhaustion (IP 주소 고갈 문제)

---

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled.png)

IP주소가 version 4가 표준으로 채택될 때와 현재 상황은 많이 달라졌기에, 이대로 유지하면 IP 주소가 고갈될 것이다 하는 의견이 1992년부터 제시되어 왔습니다. 따라서 이를 해결하기 위한 대안으로 제시되어온 것이 DHCP, NAT, IPv6가 있습니다. IPv4는 2011년 이미 고갈되었고, 새 주소를 받을 수 없으며, 이미 있는 주소를 재사용하고 있습니다.

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled%201.png)

IP주소가 있어야 인터넷을 사용할 수 있으므로(없으면 못씀), 이를 어떻게 얻는지를 확인해보자. 서버가 주는대로 IP 주소를 받는 방식이 DHCP(자동으로 IP 주소 받기)입니다. 즉, 고정된 IP가 아니라 유동 IP를 받게끔 컴퓨터에 설정되어 있습니다. 하지만 만약 고정된 IP를 받은 유저라면 별도로 `다음 IP 주소 사용` 라디오 버튼을 활성화해서 설정이 가능합니다. 오늘날에는 새 IP 주소를 주지 않으므로 대부분 DHCP를 사용하여 IP 주소를 할당해줍니다. 

## DHCP (Dynamic Host Configuration Protocol)

---

호스트가 네트워크를 요청할 때, 서버로부터 호스트가 다이나믹하게 IP 주소를 얻게끔 하는 프로토콜을 의미합니다.

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled%202.png)

DHCP를 수행하는 과정은 다음과 같습니다.

1. 호스트가 DHCP discover 메세지를 브로드캐스트하여 서버에 자신을 알립니다.
    
    → 서브넷 안에서 브로드캐스트가 되고, 라우터 영역을 넘어가지 않습니다.
    
2. DHCP 서버는 DHCP가 가능함을 알리는 메세지를 응답합니다.
3. 호스트는 DHCP ip 주소 요청 메세지를 서버에 보냅니다.
4. 서버는 ip 주소를 할당해주고 ack 메세지를 보내줍니다.

위 시나리오는 아래 과정을 요약한 겁니다.

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled%203.png)

오른쪽의 메세지들이 해당 discover, offer, request, ack을 수행하는 실제 메세지입니다. 

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled%204.png)

클라이언트(0.0.0.0:68)는 네트워크에 DHCP 서버를 찾기 위해 브로드캐스트(모든 장치에게 전달되는) Discover 메시지를 보냅니다. 즉, 서브넷 안의 모든 호스트와 서버가 듣게 됩니다. 클라이언트는 서버의 주소를 모르기 때문에 브로드캐스트로 진행하게 됩니다.

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled%205.png)

서버 또한 클라이언트가 아직 ip 주소가 없는 상황이기 때문에 요청을 한 클라이언트에게 주소를 할당해주기 위해 offer 메세지 또한 브로드캐스트로 위 메세지와 같이 ip 주소를 할당하여 클라이언트에 전달해주게 됩니다.

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled%206.png)

클라이언트는 ip 주소에 대한 정보를 받았지만, 아직 사용에 대한 확정이 되지 않았으므로 해당 주소를 사용하겠다는 요청을 브로드캐스트로 전달합니다. lifetime은 3600초 이므로 1시간동안 해당 메세지가 유효함을 나타내며, 이는 길게는 8일까지도 부여할 수 있다고 합니다.(교수님 피셜)

여기서 클라이언트는 서버의 주소(223.1.2.5)를 알고 있을텐데도 유니캐스트가 아니라 브로드캐스트로 날렸습니다. 여기서 의문점이 들어야 합니다. 교수님 피셜로, 여기서 브로드캐스트를 하는 이유는 DCHP 서버가 여러개일 경우, IP 주소 자원에 대한 동시성 이슈를 최소화하기 위해 브로드캐스트를 하기도 하며, 타 호스트에도 “해당 주소를 사용하겠다”고 알리는 용도로도 사용될 수 있습니다.

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled%207.png)

그러면 서버도 마지막으로 ip 주소 사용 요청에 대한 승낙으로 ACK를 브로드캐스트로 보내면서 DHCP 시나리오는 마감합니다.

> 일반적으로 DHCP 서버는 우리 집 환경 안에서는 AP(공유기)에 있습니다. 이는 사설 IP주소를 할당해줍니다. 하지만 public IP는 이러한 사설 AP가 가지고 있지 않고, 대개 네트워크를 관리하는 통신사가 직접 할당해줘야 합니다.
> 

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled%208.png)

DHCP 서버는 IP 주소만 주는 게 아니고, first-hop router 주소도 제공합니다. 이는 일반적으로 (기본) 게이트웨이라고 부르는데, AP의 ip address를 의미합니다. 나는 서브넷 안에서 놀 목적으로 ip 주소를 받는 게 아닐 것이므로, 기본 게이트웨이(우리집의 AP == 공유기)가 내가 인터넷을 이용하여 데이터를 전송하기 위한 첫 번째 라우터기 때문에, 그 주소도 같이 제공하게 됩니다.

그리고 DNS의 이름과 IP 주소를 알려줍니다. 내가 url 도메인을 이용하여 접근하기 위해서는 DNS 서버 이름도 알아야 하기 때문입니다.

그리고 네트워크 마스크(== 서브넷 마스크)를 제공합니다.

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled%209.png)

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled%2010.png)

DHCP 서버와의 통신은 브로드캐스트만 하니까 대부분 UDP를 사용하게 됩니다.

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled%2011.png)

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled%2012.png)

192.168.X.X는 사설 IP 이니, 사설 IP를 할당하고 있음을 확인할 수 있다.

위 사진 중 ACK 패킷을 보면, Router 라고 알려준 게 first hop 인 기본 게이트웨이의 ip 주소이다.

255.255.248.0이라는 서브넷마스크를 CIDR로 변환하라는 문제도 나올 수 있음.

1111 1111. 1111 1111. 1111 1000. 0000 0000 이므로 CIDR는 /21임을 알 수 있다. 여기서 248만 알면 되는데 255가 1111 1111이고 248은 255 - 7이므로 0111 만 빼 주면 된다고 생각하면 된다.

아울러, first-hop 주소가 보통 시작주소가 되므로, 할당받는 IP 주소의 서브넷은 192.168.192.1/21 이다. 여기서 192는 128 + 64 이니 1100 0000 이다. 즉, IP 주소는 총 2048개를 수용할 수 있는 서브넷 환경이고, 커버되는 ip 주소 범위는 192.168.192.0 ~ 192.168.199.255 이다. 그리고 할당받은 IP 주소는 192.168.195.43이므로 범위 안에 있음을 확실하게 확인할 수 있다.

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled%2013.png)

위 사진은 서브넷으로 할당받은 영역을 총 8개의 그룹으로 ip 주소를 조직화하려는 사진입니다. 따라서, 8개 그룹으로 나누려면 3개의 비트를 할당하여 서브넷 마스크를 쪼개면 총 8개의 서브넷 조직화가 이루어질 수 있는 것입니다.

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled%2014.png)

인터넷이 Fly-By-Night-ISP는 인터페이스가 1번, ISPs-R-Us는 인터페이스가 2인 라우팅 테이블을 가지고 있는 상황에서,

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled%2015.png)

만약 Fly-By-Night-ISP에 있던 조직1의 서브넷을 ISPs-R-Us에 이전시키려고 할 때, 복잡하게 할 것 없이, 라우팅 테이블 매칭을 체크할 때 Longest-Prefix-First 원칙에 의해 CIDR값(/20 … )을 기준으로 가장 먼저 긴 prefix값을 기준으로 라우팅 테이블을 확인하므로, 200.23.18.0/23은 /23이기 때문에 해당 주소로 오는 요청은 1번 인터페이스에 매핑된 정보를 먼저 검색하는 게 아니라 200.23.18.0/23이므로 먼저 훑기 때문에 라우팅 테이블에만 정보를 입력해주면 효율적으로 서브넷을 다른 서브넷과 병합할 수 있습니다.

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled%2016.png)

IP 주소는 ICANN이라는 곳에서 관리합니다. ip 주소를 할당하고, DNS를 관리하고, 도메인 이름도 할당하고 dispute를 해결합니다.

## NAT

---

우리는 AP에 공인 IP를 하나만 받는데, 공유기 환경 안에서 각각의 네트워크 장치는 각각의 ip 주소를 할당받아 사용하고 있습니다. 이게 어떻게 이루어질 수 있는 걸까요?

NAT는 네트워크 주소 변환을 하는 것을 의미합니다. 포트포워딩도 NAT 중 하나의 작업입니다. 포트포워딩을 통해 우리는 외부의 인터넷과 나의 컴퓨터를 연결시킬 수 있습니다.

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled%2017.png)

외부에서 내 컴퓨터로 요청을 보내기 위해서는 우선 알려진 퍼블릭 IP인 AP의 IP 주소 + 특정 포트로 요청을 보냅니다. 그렇게 되면, 해당 요청은 AP가 들고 있는 포워딩 테이블에 기입된 매핑 정보에 따라 나의 컴퓨터의 로컬 IP 주소로 연결되게 됩니다. 또한 내가 갖는 응답도 AP의 주소로 돌려주면 해당 응답 또한 AP의 포워딩 테이블에 따라 AP의 ip 주소로 변환되어 응답으로 날아가게 됩니다. 즉, 이 과정에서 데이터그램은 항상 ip 주소를 들고 있는데, 외부 인터넷 환경에서는 AP 의 “공인 IP 주소 + 특정 포트”를 가지고 있을 것이고, 내부 서브넷 환경에서는 내 컴퓨터의 “로컬 IP 주소 + 특정 포트”를 가지고 있을 겁니다. 이러한 과정을 통해 우리는 공용 IP를 하나만 가지고도 여러 네트워크 장치를 운영할 수 있는 겁니다.

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled%2018.png)

사설 IP의 IP 주소 범위는 10.X.X.X도 있고, 172.16.0.0 ~ 172.31.255.255도 있고, 192.168.X.X도 있습니다. 하지만 그 호스팅 규모가 다르기 때문에 적용되는 범위가 별도로 존재합니다. 일반적으로 가정집의 경우에는 192.168.X.X를 많이 사용하고, 항공대학교의 경우에는 172.16.0.0~ 172.31.255.255의 사설 ip를 사용합니다.

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled%2019.png)

하나의 IP 주소를 여러 호스트가 사용하기 위해 등장한 것이 NAT이다. NAT는 사설 IP라는 개념을 도입하여, IPv4 주소 부족 문제를 해결한 솔루션이다.

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled%2020.png)

문제를 제기하는 포인트:

port는 layer 4(트랜스포트 레이어)이다. 라우터는 layer 3에서 활약해야 하며, NAT는 라우터를 활용하는 건데, port는 layer 4의 요소이므로 잘 맞지 않다.

![Untitled](Network%20Layer%20-%20DHCP,%20NAT%201a362c36c6184afca78659fa6656f559/Untitled%2021.png)

> 위처럼 port forwarding 을 설정해두면, 사용자가 임의로 지우지 않는 이상 계속 설정되어 있다. 이는 일종의 방화벽이 뚫려있다고 할 수 있는 포인트이므로 이를 많이 열어둘수록 보안에 취약하다는 단점이 존재한다.
>