# Link Layer - MAC Protocols

## MAC protocols

---

![Untitled](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled.png)

link layer에서는 두 가지 방식이 적용된다. 1대1(ex. ppp)이냐, broadcast(shared or medium)이냐

ppp : 1대1 대응 방식으로 통신하는 것이다. 옛날에는 데이터를 주고받을 때 모뎀과 전화선을 사용하여 통신했고, 이때 사용된 프로토콜이 ppp였다. 지금은 주로 이더넷 스위치와 호스트에 사용된다. 여기는 그냥 선이 물리적으로 연결된 방식이 old-fashioned ehternet이고, 이는 스위치를 사용하지 않고 물리적으로 연결되어있는 구조였다.

여기서는 물리적으로 1대1로 연결되어 있는지, 아니면 여러 호스트가 스위치 등의 매개체(미디어)를 이용하여 연결되어 있는지를 기준으로 구분한다. 여러 호스트가 연결되어 공유되는 구조가 802.11과 같은 broadcast 방식이다.

![Untitled](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%201.png)

호스트들이 미디어를 동시에 사용할 때에는 여러 시그널 간 collision이 발생할 수 있다.

데이터를 주고받는 채널이 따로 있고, 컨트롤 채널이 따로 있고 하는 방식은 일반적으로 저렴하지 않으므로 잘 사용하지 않는다. 따라서 distributed algorithm을 적용하고, coordination을 위해 여러 채널을 사용하기보단 하나의 채널을 공유하는 방식으로 multiple access protocol을 구현한다.

![desiderata : 희망하는 상태](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%202.png)

desiderata : 희망하는 상태

broadcast 채널이 R bps를 가지고 있을 때,

이상적인 경우에는 다음 조건을 만족해야 한다.

1. 한 개의 노드만 데이터를 전송할 때에는 전송률이 R bps가 나와야 한다.
2. M 개의 노드가 데이터를 전송할 떄에는 전송률이 R/M bps가 나와야 한다.
3. transmission coordinating node가 있으면 안된다.
4. clock과 slot 동기화 작업이 없어야 한다.
5. 단순해야 한다.

![Untitled](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%203.png)

위는 채널을 나누는 방식을 정의한 것이다.

- 채널을 쪼개어 할당 : 각 채널 조각을 배타적으로 사용할 수 있도록(독립적 사용) 할당하는 것이다.
- 채널이 나눠지지 않고, 공유하게 하면서, collision이 발생할 수 있음을 용인하는 방식이다. 즉, 빈 자리를 편하게 점유하여 사용하는 방식이다. collision을 허용하니 collision으로부터 recover할 수도 있다. 이렇게 적용하게 되면 혼자서 사용하면 채널을 모두 사용할 수 있고, 여럿이서 사용하면 collision만 발생하지 않는다면 나눠서 사용할 수 있기 때문에 효율적일 수 있다.
- taking turns : 노드들이 턴제 방식을 취하여 채널을 번갈아가며 사용한다. 이 때, 한 노드가 보낼 게 많다면 조금 더 오랫동안 턴을 점유할 수 있다.

![Untitled](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%204.png)

채널을 할당하는 MAC 프로토콜 중에 시간으로 나누는 방식을 TDMA(Time division multiple access)라고 합니다. 이 것의 단점은, 다른 곳들이 놀고 있어도 내가 할당받은 곳만 사용해야 하므로 idling이 발생할 수 있다는 것이다.

이는 단순히 생각하면 CPU가 time sharing을 하는 개념이라고 생각해도 좋을 듯 하다. 이렇게 나누진 않겠지만 1분을 6개로 쪼갠다고 하면 하나의 유닛을 slot이라고 부르고 10초당 1개의 슬롯에 해당하는 놈에게 주어진 데이터만 전송하는 것이다. 이를 활용하는 예시가 전화이다. 옛날에는 여러 개의 전화 통화를 하나의 전화선을 통해 전송하는데, 이 때 하나의 채널이 전송하는 시간을 슬롯으로 쪼개어 각 슬롯에 해당하는 시간 동안 할당된 데이터가 전송되어 여러 통화가 이루어지는 것이다.

![Untitled](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%205.png)

주파수로 할당해주는 것을 FDMA(frequency division multiple access)라고 합니다. 채널이 갖는 스펙트럼을 주파수 대역폭으로 분할합니다. 전송에 사용되지 않는 대역폭은 idle되는 구조입니다.

예를 들어, 라디오는 주파수를 나눠서 각 방송을 동시에 제공합니다. 각 라디오 방송은 “라디오 채널”이라는 전송 수단이 갖는 여러 주파수 대역을 쪼개어 일정 주파수 대역 안에서 송수신합니다.

<aside>
💡 그 외에 모바일은 CDMA(Code-division multiple access), LTE 와 같은 프로토콜을 사용합니다.

</aside>

### Random access protocols

![Untitled](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%206.png)

랜덤 엑세스는 collisions를 감안하더라도, 굳이 파티셔닝을 하지 않는 것이 더 효율적일 것이다라고 판단하여 적용한 것이다. 따라서 collision detection과 recovering에 대해 대비하는 프로토콜이다. 이러한 프로토콜의 대표적인 예시가 ALOHA이다

![Untitled](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%207.png)

모든 프레임은 동일한 사이즈에, 타임 슬롯도 동일한 크기로 설정된다. 그리고 slot이 시작될 때에만 transmit이 시작될 수 있다. 노드들은 동기화되고, 2개 이상의 노드가 슬롯에서 데이터를 전송할 때, 모든 노드는 동기화 과정에서 collision을 감지한다.

만약 노드가 새로 데이터를 전송할 때, collision이 발생하지 않는다면 다음 슬롯에서도 전송이 가능하다. 즉, collision만 발생하지 않는다면 random access 환경에서는 모든 슬롯을 하나의 노드가 전부 사용할 수 있음을 의미한다. 하지만 collision이 발생하면 재전송을 해야 한다.

![Untitled](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%208.png)

구조가 단순하고, 노드가 혼자만 active일 경우 채널의 슬롯을 모두 이용하여 전송을 할 수 있다는 장점이 있다. 하지만 collision이 발생하면 슬롯이 낭비되며, *clock 동기화가 필요하다.(?)*

![Untitled](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%209.png)

시작하고 끝나기 전까지 겹치는 부분이 발생한다.

![중요한건 아니고, 방식만 알고자 하고 넘어감.](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%2010.png)

중요한건 아니고, 방식만 알고자 하고 넘어감.

![Untitled](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%2011.png)

`...`

![위 내용보다 더 중요함, 더 발전된 거임](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%2012.png)

위 내용보다 더 중요함, 더 발전된 거임

와이파이에서 사용하는 기술인 CSMA는 carried sense multiple access로, “말을 하기 전에 들어라(LBT)”라는 원리를 바탕으로 운영되는 매커니즘이다. 즉, 채널을 sensing 할 수 있는 매커니즘이 있다는 것을 바탕으로 하는 것이다. 유선에서 주로 많이 사용하는 매커니즘이며, 무선 방식에서도 적용된다. 만약 채널이 idle이라면 전송할 때 전체 프레임을 사용하고, 만약 채널이 바쁘면 transmission 방식을 defer하는 방식을 말한다.

<aside>
💡 CSMA/CA (~/Collision Avoidance)는 무선 환경에서 주로 사용하는 프로토콜인데, 이런게 있구나 하고 넘어가라

</aside>

![Untitled](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%2013.png)

위 예시를 봐보자. 전파가 전달되는 시간을 고려하며 봐야 한다. t0 시점에서 yellow는 아무 노드도 transmission을 하지 않기에 데이터 전송을 한다. 그리고 t1 시점에서 red 또한 아무 노드도 transmission을 하지 않는다고 느끼기 때문에 데이터 전송을 하게 되는데, 이는 yellow 전송이 propagation delay가 있기에 red가 알아채지 못한 시간임을 볼 수 있다. 위처럼 겹치게 될 경우에는 겹치는 영역의 모든 패킷은 전부 깨져버리기 때문에 시간이 낭비된다. CSMA는 위와 같은 threat이 있다.

![Untitled](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%2014.png)

어차피 collision이 발생하면 시간만 낭비되면서 파괴되는 패킷을 전송할 바엔, collision이 감지되면 해당 전송을 버려버림으로써 채널 낭비를 줄일 수 있다는 것이 CSMA/CD이다.

![Untitled](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%2015.png)

ethernet과 같은 wired LAN에서는 signal sensing이 쉽기 때문에 이를 구현하기 쉽지만, wifi와 같은 wireless LAN에서는 구현이 어렵다.

![Untitled](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%2016.png)

이더넷은 CSMA/CD를 위한 알고리즘이 있다.

1. NIC는 네트워크 레이어로부터 데이터그램을 받고 프레임을 생성한다.
2. NIC가 채널이 비어있음을 감지하면 프레임 전송을 시작한다. 만약 NIC가 채널이 포화상태라고 느끼면, 우선 채널이 빌 때까지 기다리고 프레임을 전송한다.
3. 만약 NIC가 전체 프레임을 전송하면서 다른 프레임 전송을 감지하지 못했다면, NIC는 전송을 성공적으로 마친 것이다.
4. 하지만 NIC가 프레임 전송 중에 다른 프레임 전송을 감지했다면, 10101010과 같은 일정한 패턴으로 구성된 jam signal을 보내면서 전송을 그만한다.
5. 전송을 멈춘 뒤에 NIC는 binary exponential backoff를 수행한다. 이는 m번째 collision이 있은 후 0 ~ (2^m - 1) 중의 한 자연수인 K를 랜덤으로 골라서 K*512 bit 만큼 기다렸다가 다시 2번으로 돌아가서 전송을 재개하는 것을 의미한다. (colliision이 많을수록 더 긴 backoff 를 갖게 될 확률이 높다.)

![이런게 있구나 정도만 보고 넘어가셈](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%2017.png)

이런게 있구나 정도만 보고 넘어가셈

![대개 upstream은 데이터가 많지 않고, downstream은 데이터가 많음](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%2018.png)

대개 upstream은 데이터가 많지 않고, downstream은 데이터가 많음

slot을 할당하는 양은, 채널을 사용하고자 하는 유저의 수 만큼 슬롯을 할당한다.

- gpt
    
    Cable Access Network은 케이블 모뎀 종단 시스템 (CMTS), 스플리터 (Splitter), 케이블 모뎀 (Cable Modem) 등의 장비를 사용하여 데이터를 업스트림(Upstream) 및 다운스트림(Downstream)으로 전송하는 기술입니다. 다음은 간단한 예시와 함께 각 단계를 설명합니다.
    
    1. 단계: CMTS (Cable Modem Termination System)
        - CMTS는 케이블 헤드엔드에 위치한 중앙 장비입니다.
        - 인터넷 서비스 제공업체(ISP)의 네트워크와 케이블 TV 네트워크를 연결합니다.
        - CMTS는 다운스트림 데이터를 생성하고 업스트림 데이터를 수신합니다.
    2. 단계: 스플리터 (Splitter)
        - 스플리터는 CMTS에서 나오는 다운스트림 신호를 여러 개의 분기된 채널로 나눕니다.
        - 이렇게 나누어진 다운스트림 신호는 각각의 채널을 통해 다양한 가입자들에게 전달됩니다.
        - 스플리터는 케이블 네트워크에서 다운스트림 데이터를 가입자들에게 분배하는 역할을 합니다.
    3. 단계: 케이블 모뎀 (Cable Modem)
        - 가입자의 집이나 사무실에 설치된 케이블 모뎀은 다운스트림 데이터를 수신하고 업스트림 데이터를 전송합니다.
        - 다운스트림 데이터는 CMTS에서 생성된 데이터이며, 케이블 모뎀을 통해 가입자에게 전달됩니다.
        - 업스트림 데이터는 가입자가 인터넷을 통해 데이터를 전송할 때 케이블 모뎀을 통해 CMTS로 전송됩니다.
    4. 다운스트림 데이터 전송:
        - CMTS는 인터넷에서 가져온 데이터를 다운스트림 신호로 변환합니다.
        - 이 다운스트림 신호는 CMTS에서 스플리터를 통해 분기되고, 각 채널을 통해 가입자들에게 전달됩니다.
        - 케이블 모뎀은 해당 가입자에게 할당된 채널에서 다운스트림 데이터를 수신합니다.
        - 가입자는 케이블 모뎀을 통해 다운스트림 데이터를 받아서 인터넷을 이용하거나 멀티미디어 콘텐츠를 시청할 수 있습니다.
    5. 업스트림 데이터 전송:
        - 가입자가 인터넷을 통해 데이터를 전송하면, 케이블 모뎀은 해당 데이터를 업스트림 신호로 변환합니다.
        - 업스트림 신호는 가입자의 채널을 통해 CMTS로 전송됩니다.
        - CMTS는 다수의 가입자로부터 수신된 업스트림 데이터를 처리하고, 해당 데이터를 인터넷으로 전달하거나 다른 네트워크 장비로 라우팅합니다.
    
    이와 같이 Cable Access Network에서는 CMTS, 스플리터, 케이블 모뎀이 협력하여 다운스트림 데이터와 업스트림 데이터를 전송하고, 가입자는 케이블 모뎀을 통해 데이터를 수신하거나 전송합니다. 이를 통해 인터넷 접속과 멀티미디어 콘텐츠의 이용이 가능해집니다.
    

![contention : 분쟁](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%2019.png)

contention : 분쟁

나 쓸래요 하는 유저에게 

- gpt
    
    DOCSIS는 "Data Over Cable Service Interface Specification"의 약자로, 케이블 통신망에서 데이터를 전송하기 위한 표준화된 인터페이스 규격입니다. DOCSIS는 케이블 모뎀과 CMTS 간의 상호 운용성을 보장하며, 가입자들에게 고속 인터넷 액세스와 다양한 서비스를 제공하는 데 사용됩니다.
    
    DOCSIS는 다음과 같은 주요 기능과 특징을 갖고 있습니다:
    
    1. 다양한 서비스 지원: DOCSIS는 다양한 서비스를 지원하기 위해 다운스트림과 업스트림 데이터 전송을 위한 주파수 분할 기능을 포함합니다. 이를 통해 인터넷 액세스, IP 전화, 비디오 스트리밍 등 다양한 서비스를 제공할 수 있습니다.
    2. 대역폭 관리: DOCSIS는 케이블 네트워크에서 대역폭을 효율적으로 관리하기 위한 기능을 제공합니다. 대역폭 할당과 우선순위 설정을 통해 다양한 서비스 요구 사항을 충족시키고, 공정한 대역폭 분배를 지원합니다.
    3. 보안 기능: DOCSIS는 데이터의 안전한 전송을 위해 암호화 및 인증 기능을 제공합니다. 사용자 인증, 데이터 암호화, 방화벽 기능 등을 통해 네트워크 보안을 강화할 수 있습니다.
    4. 진단 및 관리 기능: DOCSIS는 네트워크 상태의 모니터링, 문제 진단, 장애 처리 등을 위한 관리 기능을 제공합니다. 이를 통해 서비스 제공자는 네트워크 성능을 모니터링하고 문제를 신속하게 해결할 수 있습니다.
    
    DOCSIS는 케이블 통신망에서 고속 인터넷 액세스와 다양한 서비스를 제공하기 위한 중요한 표준 규격입니다. 이를 기반으로 제조사들은 DOCSIS 표준을 준수하는 케이블 모뎀과 CMTS 장비를 개발하여 상호 운용성을 보장하고, 가입자들은 안정적이고 고속의 인터넷 서비스를 이용할 수 있습니다.
    

DOCSIS : 케이블로 데이터를 보내는 것

`...`

![Untitled](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%2020.png)

지금까지 다뤘던 링크 레이어에서의 Multiple Access protocols들은 다음과 같은 특징이 있었습니다.

- channel partitioning
    - 전송량이 많을 때에는 효율적이지만, 만약 전송하고자 하는 노드가 하나 뿐이더라도 1/N의 대역폭만 사용해야 하는 방식이므로 전송량이 적을 때에는 효율적이지 않다.
- random access
    - 전송량이 적을 때에는 채널을 충분히 사용할 수 있기 때문에 효율적이지만, 만약 전송하고자 하는 노드가 많을 경우에는 collision이 발생함으로 유발되는 오버헤드가 크다.

위의 두 가지 방식들의 장점을 조합한 방식이 바로 taking turns 프로토콜이다.

![Untitled](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%2021.png)

taking turns 방식에서는 coordinator 역할을 하는 master 노드가 존재한다. master node는 데이터 전송을 요청하는 slave 노드에게 차례에 따라 토큰을 부여함으로써 데이터 전송을 할당합니다.

위와 같은 방식을 polling이라고 하며, 이는 데이터 전송과 별개의 작업인 polling 작업을 수행해야 하므로 발생하는 오버헤드가 있고, 그로 인해 유발되는 latency가 문제가 될 수 있으며, master node라는 single point of failure임을 유념해야 한다.

![Untitled](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%2022.png)

위는 한 채널을 공유하는 노드끼리 원형으로 turn(token)을 돌려가며 데이터를 전송하는 방식입니다. 이는 어찌보면 TDMA와 비슷하다고 생각할 수 있는데, TDMA와는 달리, nothing to send인 노드는 바로 토큰을 다음 노드에게 넘기기 때문에 그것과는 조금 다르다고 할 수 있습니다.

이 방식 또한 토큰을 주고받으면서 발생하는 오버헤드, 그리고 레이턴시가 문제일 수 있고, 토큰을 가지고 있던 노드가 죽어버리면 데이터 전송에 문제가 발생할 수 있기 때문에 single point of failure는 여전히 문제가 될 수 있다.

![Untitled](Link%20Layer%20-%20MAC%20Protocols%207da5e96f82f340cf96942dcaff5f7fad/Untitled%2023.png)

위 사진은 지금까지 본 내용을 요약한 것이다.

위 중에서 FDDI는 token ring을 dual로 사용하는 프로토콜이다.