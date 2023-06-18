# Network Layer - Overview, VC, Router

Network Layer : 트랜스포트 레이어가 사용하는 레이어,

트랜스포트 레이어에서 신뢰성있게 데이터 패킷을 보낼 때, 이러한 작용을 실제로 이루어주는 레이어가 네트워크 레이어이다. 즉, 실제로 데이터를 전송하는 레이어가 네트워크 레이어이다.

1. 네트워크 레이어에서 수행하는 서비스
2. 포워딩과 라우팅의 차이
3. 라우터에서 어떤 일을 하는지(네트워크 레이어의 실질적인 일은 라우터가 대부분 한다.)
4. SDN(Software-defined network)
5. ICMP(Internet Control Message Protocol) ~ 네트워크 매니지먼트 프로토콜 (네트워크를 잘 작동시키기 위한 메세지들)

### 버추얼 서킷과 데이터그램의 차이

### 라우터 안에 어떤 게 있는지?

### IP 주소란 무엇인가?

### 라우팅 알고리즘

- link state algorithm
- distance vector algorithm

## 네트워크 레이어

---

전송 세그먼트(PDU)를 전송하는 것을 담당하는 것이 network layer

PDU를 데이터그램으로 encapsulation하고, 이를 unpack하는 것이 network layer

네트워크 레이어는 모든 호스트와 라우터에 존재한다.

라우터는 passing through it 하는 모든 IP 데이터그램 안의  헤더 필드를 실행한다(?)

포워딩 : 패킷을 실제 라우터의 입력부로부터 적절한 출력부로 보내는 것

라우팅 : 소스로부터 데스티네이션까지 어떤 길로 갈지, 가장 코스트가 적게 드는 루트를 선정하는 것

*** 네트워크에서 “코스트”란 무엇이고, 이러한 길을 선정하는 알고리즘은 무엇이 있는가? (핵심)

라우팅 알고리즘에 의해 라우팅/포워딩 테이블을 만들어서 라우터에 넣어두고, 헤더에 있는 IP 주소를 통해 output link는 1번,2번,3번 등으로 연결하라는 결정을 하는 것이 포워딩이다.

![Untitled](Network%20Layer%20-%20Overview,%20VC,%20Router%2023e7e58a43ea43a9838d58fdbb82f1a4/Untitled.png)

### connection setup

네트워크0마다 다르지만,

네트워크 연결 수립을 잘 하기 위해 connection을 수립하는 경우가 있다. (ATM, frame relay, X.25)

송신, 수신 양 쪽에 virtual connection을 수립한다. 어떤 라우터를 통하게 될 지 등을 

transport 레이어 : 두 프로세스 간 연결 수립 (포트 번호로)

network 레이어 : 두 호스트 간의 virtual connection을 맺음 (IP 주소로)

### network service model

네트워크 서비스가 해줘야 할 게 뭐가 있을까?

delivery 보장 (신뢰성 있는 transfer)

40 밀리세컨드 이내 전송 보장 (timing)

순서에 맞게 데이터그램을 보내줘라. (*** 데이터그램 : 네트워크 레이어에서 사용하는 패킷이다.)

처리 용량을 보장해라

![Untitled](Network%20Layer%20-%20Overview,%20VC,%20Router%2023e7e58a43ea43a9838d58fdbb82f1a4/Untitled%201.png)

패킷 간 간격의 변동을 최소화해라. (VoIP는 음성 데이터를 보내는데, 20msec 간격으로 데이터가 잘 날아가야 원활하게 통신이 되는 걸텐데, space 간격의 변화가 너무 심하면 음성의 디스토션이 발생하기 때문에 이를 보장해달라 같은 맥락으로 이해하면 된다.)

—

![Untitled](Network%20Layer%20-%20Overview,%20VC,%20Router%2023e7e58a43ea43a9838d58fdbb82f1a4/Untitled%202.png)

ATM에는 CBR(constant bit rate), VBR(variable bit rate), ABR(available bit rate ; “가용 비트레이트==네트워크 용량”을 파악하면서 버퍼 오버플로우가 발생하지 않도록 조절해서 보내는 것.), UBR(Undefined-bit rate)등이 있다.

- CBR : 일정한 처리량(bandwidth)를 보장한다.
- VBR : 가변적으로 처리하며 처리량을 보장한다.
- ABR : 최소 처리량을 보장한다.

## Virtual Circuit and datagram networks

---

네트워크 레이어에서 사용되는 경로 설정의 두 가지 개념으로, 

- datagram network(인터넷 프로토콜)는 커넥션이 없는 서비스이다.
- 하지만 virtual circuit network는 커넥션이 있는 서비스이다.

![only 인터넷 프로토콜의 측면에서 설명하는 것. 따라서 network layer에서는 여러 가지 종류가 있는 게 아니고 오로지 한 개 이다. 네트워크 레이어는 엔드포인트 시스템(호스트)과 network core(라우터 등) 모두에 존재한다.](Network%20Layer%20-%20Overview,%20VC,%20Router%2023e7e58a43ea43a9838d58fdbb82f1a4/Untitled%203.png)

only 인터넷 프로토콜의 측면에서 설명하는 것. 따라서 network layer에서는 여러 가지 종류가 있는 게 아니고 오로지 한 개 이다. 네트워크 레이어는 엔드포인트 시스템(호스트)과 network core(라우터 등) 모두에 존재한다.

### virtual circuit

출발지부터 도착지까지 통화가 끝날 때 까지는 해당 통화가 유지하는 것이 virtual circuit이다.

옛날에는 보이스 통신 시에 서킷(데이터 전송 유닛의 한 종류)을 전송하여 통신을 구현했는데, 요즘은 HD voice와 같은 서비스를 구현하기 위해 패킷(데이터그램)을 전송하며 음성 통신을 구현한다.

패킷의 경우 포워딩을 할 때 데스티네이션 호스트 주소가 아니라, VC identifier를 각 packet이 가지고

있어 이를 통해 한다. 출발지부터 종착지까지의 모든 라우터는 각 연결마다 stateless가 아니라 state를 저장하는 방식으로 운영된다.

virtual circuit의 경우, 세팅을 해두기 때문에 bandwidth나 buffers (link, router resources)를 reserve할 수 있다는 장점이 존재한다. 즉, 이들(dedicated resources)을 virtual controller에 할당하여 예측 가능한 서비스라는 점이 장점이다.

- virtual circuit을 구성하는 것
    - path (source ~ destination)
    - VC numbers (각 paht에 따라 각 링크에 숫자가 부여되어 있음)
    - forwarding table (라우터에 VC 전용 entries forwarding table이 존재)
        
        ![빨간 라인이 VC call set-up된 것.](Network%20Layer%20-%20Overview,%20VC,%20Router%2023e7e58a43ea43a9838d58fdbb82f1a4/Untitled%204.png)
        
        빨간 라인이 VC call set-up된 것.
        
- **VC에 속한 패킷들은 종착지 주소가 아니라 VC numbers를 가지고 있다.** 이를 통해 forwarding할 때 참고한다.
- VC numbers는 각 링크마다 바뀔 수 있다. (forwarding table에 의해 계속 바뀐다.)

> virtual circuit은 datagram network와 달리 call set-up이 필요하므로 아래의 1~4번과 같은 과정이 필요하다.
> 
> 
> ![인터넷에선 VC를 안 씀, ATM등의 일부 단말기에서 활용중](Network%20Layer%20-%20Overview,%20VC,%20Router%2023e7e58a43ea43a9838d58fdbb82f1a4/Untitled%205.png)
> 
> 인터넷에선 VC를 안 씀, ATM등의 일부 단말기에서 활용중
> 

### datagram networks

virtual circuit과 달리, call set-up이 없다.

라우터에서도 state를 유지하지 않는다.

네트워크 레벨에서는 connection을 가지고 있지 않다.

따라서 datagram network에서는 패킷이 destination host address를 들고 있고, 이를 이용해서 forwarding을 한다.

라우터에는 패킷을 forwarding하기 위한 table이 있는데, 모든 주소에 대하여 1대1매핑을 하는 건 비효율적이므로 address-range를 정해서 포워딩을 진행한다.

![Untitled](Network%20Layer%20-%20Overview,%20VC,%20Router%2023e7e58a43ea43a9838d58fdbb82f1a4/Untitled%206.png)

![Untitled](Network%20Layer%20-%20Overview,%20VC,%20Router%2023e7e58a43ea43a9838d58fdbb82f1a4/Untitled%207.png)

이 페이지는 forwarding table의 예시이다. 
- examples (1) : 연결된 인터페이스는 0번
- examples (2) : 연결된 인터페이스는 1번 (1번과 2번 모두 적용 가능하지만 longest prefix matching 규칙에 따라 연결되는 인터페이스는 1번이 된다.)

![Untitled](Network%20Layer%20-%20Overview,%20VC,%20Router%2023e7e58a43ea43a9838d58fdbb82f1a4/Untitled%208.png)

네트워크에서 많은 일을 해주는 건 VC(ATM에 사용), 적은 일을 해주는 건 Datagram(인터넷에 사용).

- datagram : 네트워크에서 많은 일을 해주지 않으므로 단말의 성능이 좋아야 한다.
- VC : 단말이 상대적으로 빈약하여 네트워크에서 많은 일을 해줘야 한다. (단말의 성능이 좋지 못헀던 과거의 전화기 등에서는 VC를 사용했다) 하지만 오늘날 휴대폰의 성능이 좋아지면서 Datagram을 적용하고 있다.

위 자료 중 Internet의 many link types와 같은 특성으로 인해 해당 부분이 발전하게 되었다.

## Router Architecture

---

![Untitled](Network%20Layer%20-%20Overview,%20VC,%20Router%2023e7e58a43ea43a9838d58fdbb82f1a4/Untitled%209.png)

라우터는 routing과 forwarding 두 가지 기능을 수행한다.

위 그림 중 routing processor는 컴퓨터라고 생각하면 된다. routing processor는 라우팅 알고리즘을 바탕으로 알고리즘을 하고, 데이터 흐름을 제어하고 관리하는 영역에 존재하며, 소프트웨어의 역할을 수행한다.

> 이 부분은 SDN에서는 굳이 router에 같이 두지 않고, 독립적으로 위치시킨 뒤에 모든 라우터로부터 데이터를 받아 처리를 하는 방식으로 수행한다.
> 

그리고 실제 데이터그램 패킷을 스위칭하는 영역인 forwarding data plane은 하드웨어의 영역이다.

![Untitled](Network%20Layer%20-%20Overview,%20VC,%20Router%2023e7e58a43ea43a9838d58fdbb82f1a4/Untitled%2010.png)

위 그림은 앞서 다뤘던 그림의 상세 버전이다. 여기서 초록색은 physical layer, 파란색은 data-link layer이다. 우선은 빨간색 박스(input buffer)에 대해서 집중해보자.

input port에서는 switch fabric에 진입하기 전에 들어오는 속도에 따라 forwarding table을 이용하여 input port processing을 완료하는 것이 목표이다. 만약 forwarding rate보다 빠르게 datagram이 도착할 경우에는 queueing을 진행한다.

![Untitled](Network%20Layer%20-%20Overview,%20VC,%20Router%2023e7e58a43ea43a9838d58fdbb82f1a4/Untitled%2011.png)

switching fabrics은 input buffer로부터 outer buffer로 패킷을 전송하는 파트이다. 스위칭 rate는 당연히 input으로부터 output으로의 패킷 전달률이 좋아야 하며, 전달하는 방식은 3가지가 존재한다.

![Untitled](Network%20Layer%20-%20Overview,%20VC,%20Router%2023e7e58a43ea43a9838d58fdbb82f1a4/Untitled%2012.png)

switching by memory는 전통적인 방식이며, 컴퓨터의 CPU를 활용하여 스위칭을 한다. 패킷은 시스템의 메모리에 복사되고, 메모리의 대역폭에 의존적으로 속도가 제한된다. 즉, 전송 속도가 데이터그램마다 크로싱되는 2개의 버스에 의존적이다.

 

![Untitled](Network%20Layer%20-%20Overview,%20VC,%20Router%2023e7e58a43ea43a9838d58fdbb82f1a4/Untitled%2013.png)

switching via a bus의 경우 input buffer로부터 들어오는 데이터그램이 공유되는 버스를 통해 outer buffer로 전송되는 구조이다.

따라서 해당 공유 버스의 대역폭에 따라 스위칭 속도가 의존적이다. 이는 해당 버스의 대역폭을 늘려두면 충분히 적절한 속도를 확보할 수 있다.

![Untitled](Network%20Layer%20-%20Overview,%20VC,%20Router%2023e7e58a43ea43a9838d58fdbb82f1a4/Untitled%2014.png)

공유 버스 대역폭에 속도가 의존되는 이슈를 극복한 게 switching via interconnection network이다.

우측 아래와 같은 banyan network, 또는 크로스바 와 같은 interconnection 방식의 경우에는 데이터그램을 고정된 길이의 cell 단위로 쪼개서 fabric을 통과시킵니다.

![Untitled](Network%20Layer%20-%20Overview,%20VC,%20Router%2023e7e58a43ea43a9838d58fdbb82f1a4/Untitled%2015.png)

Output buffer에는 queueing 전략이 다양할 수 있다. FIFO일 수도 있고, priority에 따라 queueing할 수도 있다. 만약 transmission rate보다 빠르게 fabric을 통과한 datagram의 경우에는 버퍼에 저장된다. 따라서 만약 버퍼가 부족하거나 혼잡이 심할 경 데이터그램은 유실될 수 있다.

![Untitled](Network%20Layer%20-%20Overview,%20VC,%20Router%2023e7e58a43ea43a9838d58fdbb82f1a4/Untitled%2016.png)

만약 arrival rate via switch가 output line speed를 넘어설 경우에는 버퍼링을 해야 하고, 버퍼가 넘쳐 흐르면 queueing delay 또는 유실이 발생하게 된다.

![Untitled](Network%20Layer%20-%20Overview,%20VC,%20Router%2023e7e58a43ea43a9838d58fdbb82f1a4/Untitled%2017.png)

![Untitled](Network%20Layer%20-%20Overview,%20VC,%20Router%2023e7e58a43ea43a9838d58fdbb82f1a4/Untitled%2018.png)

switching 이 input보다 느릴 경우에는 당연히 input에서 queueing이 발생하게 되는데, input buffer또한 오버플로우가 발생할 수 있음에 유의해야 한다.

HOL 블로킹 현상은 queued 되어있는 데이터그램 중에서 위 그림 중 왼쪽 그림을 보았을 때, 맨 아랫 줄에 빨간색이 초록색을 가리고 있는데, output port contention에 의해 맨 위의 빨간색이 직선으로 가면 되므로 먼저 가고, 아래의 빨간색은 못가고 있는 현상이 발생하게 된다. 하지만 그 뒤에 있는 초록색은 앞의 빨간색만 아니면 바로 transfer할 수 있는 조건인데, 앞의 빨간색 때문에 우선순위에서 계속 밀리고 있는 상황이다. 이렇게 맨 줄 앞의 데이터그램때문에 이전 데이터그램이 앞으로 가지 못하는 상황을 HOL 블로킹이라고 한다.