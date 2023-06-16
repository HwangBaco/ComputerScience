# Link Layer - MPLS, data center

## MPLS

---

![Untitled](Link%20Layer%20-%20MPLS,%20data%20center%208ae99330a9fa4b068d11e8e593086a6b/Untitled.png)

굳이 네트워크 레이어를 거치지 않고도 링크 레이어의 프레임에서 보이는 label을 기준으로 라우팅 path를 판단(일종의 라우팅 테이블이 있고, input label에 따라 output link를 결정)하므로 효율적으로 빠른 전송이 가능하다. (중요한 건 label)

위의 S는 stack의 S(안중요)

![Untitled](Link%20Layer%20-%20MPLS,%20data%20center%208ae99330a9fa4b068d11e8e593086a6b/Untitled%201.png)

위 사진을 참고하여 MPLS 동작원리 이해하기

![Untitled](Link%20Layer%20-%20MPLS,%20data%20center%208ae99330a9fa4b068d11e8e593086a6b/Untitled%202.png)

MPLS의 장점은, flexible하다는 것이다.

MPLS는 미리 백업 path를 만들어두는 개념이다.

![Untitled](Link%20Layer%20-%20MPLS,%20data%20center%208ae99330a9fa4b068d11e8e593086a6b/Untitled%203.png)

지금까지 우리가 배웠던 라우팅의 개념은, 위와 같이 목적지에 따라 모든 트래픽이 동일한 경로를 따라 전송되게 되어 있다.

![Untitled](Link%20Layer%20-%20MPLS,%20data%20center%208ae99330a9fa4b068d11e8e593086a6b/Untitled%204.png)

하지만 MPLS는 각 source에 따라 다른 경로를 지정해둘 수 있다. 이는 일종의 로드밸런싱 역할도 수행할 수 있다.

![Untitled](Link%20Layer%20-%20MPLS,%20data%20center%208ae99330a9fa4b068d11e8e593086a6b/Untitled%205.png)

지금까지 익혔던 IP 주소를 통한 전송 방식은 위와 같다. layer를 거치면서 ip 주소를 확인해야 루트를 결정할 수 있기에 프레임을 데이터그램으로 뜯고, 다시 씌우고 하는 과정들이 반복되고, 이는 오버헤드가 존재할 수 밖에 없다. (이는 처음에는 문제가 안되었었지만, 점차 트래픽이 많아짐에 따라 문제가 생겨난 것이다.)

![Untitled](Link%20Layer%20-%20MPLS,%20data%20center%208ae99330a9fa4b068d11e8e593086a6b/Untitled%206.png)

IP망과의 경계에서는 LER과 연결되어 일반적인 IP 주소를 기반으로 하는 라우팅도 지원된다.(flexible)

그리고 MPLS를 운영할 때에는 LER은 LSR로 연결되며 프레임 헤더에 있는 레이블 만으로 빠르게 라우터 간 전송을 이뤄내며, 그 과정에서 IP 주소를 확인할 필요가 없으므로 굳이 layer 3까지 가서 라우팅을 수행할 필요가 없으므로 매우 빠르게 수행된다.

![Untitled](Link%20Layer%20-%20MPLS,%20data%20center%208ae99330a9fa4b068d11e8e593086a6b/Untitled%207.png)

![Untitled](Link%20Layer%20-%20MPLS,%20data%20center%208ae99330a9fa4b068d11e8e593086a6b/Untitled%208.png)

아울러 Ingress LER은 modified link state flooding algorithm에 따라 매번 똑같은 라우팅만 수행하는게 아니라, 각 패스의 리소스 정보를 바탕으로 dynamic하게 라우팅을 수행하게 된다. 판단하는 파라미터는 link bandwidth와 reserved link bandwidth의 양으로 판단한다.

## Data center

---

![Untitled](Link%20Layer%20-%20MPLS,%20data%20center%208ae99330a9fa4b068d11e8e593086a6b/Untitled%209.png)

클라우드 데이터 센터의 경우, server racks에 있는 특정 컴퓨터를 지정해서 computing resource를 유저에게 할당하는 개념이 아니다. 서비스 제공자는 로드밸런싱을 통해서 유저가 신청한 컴퓨팅 파워만 제공하는 것이다. 따라서 IP 주소 또한 각 컴퓨터 리소스들이 하나하나 가지고 있는 개념이 아니다. 각각의 서버는 사설 IP 주소가 같은 놈도 충분히 발생할 수 있는 것이다.

> 위의 TOR은 top of rack이라는 의미다.
> 

![Untitled](Link%20Layer%20-%20MPLS,%20data%20center%208ae99330a9fa4b068d11e8e593086a6b/Untitled%2010.png)

위에서 언급한 것 처럼, 각각의 서버는 각 스위치에 “모두” 연결되어 어떤 스위치가 망가지더라도 최대한 모든 리소스를 사용할 수 있도록 구조화되어 있다.