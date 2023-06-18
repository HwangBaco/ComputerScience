# Network Layer - Routing Algorithm

[Routing  Algorithm(라우팅 알고리즘)](https://carpediem9911.tistory.com/16)

[[네트워크] 5.2 DV algorithm & AS](https://velog.io/@lychee/네트워크-5.2-DV-algorithm-AS)

## 라우팅 프로토콜

---

![Untitled](Network%20Layer%20-%20Routing%20Algorithm%20e20002435d454bc38b9115ad7226604f/Untitled.png)

라우팅 프로토콜의 목표는 좋은 길을 찾는 것이다. 좋은 길이란, 코스트가 적고, bandwidth가 큰 길(속도가 빠른 길), 그리고 congestion이 적은 길을 의미한다. 알고리즘을 돌릴 때에는 이러한 요소들을 cost 라는 것으로 퉁쳐서 계산한다.

![Untitled](Network%20Layer%20-%20Routing%20Algorithm%20e20002435d454bc38b9115ad7226604f/Untitled%201.png)

알고리즘을 사용하기 위해 네트워크를 그래프로 그려서 표현한다. cost는 한 종류로 보고, 숫자로 표현할 것이다. 원래대로라면 cost도 유향 그래프 위에서 방향에 따라 cost도 다양해지는 것이 일반적이지만, 여기서는 단순화하기 위해 무향 그래프로 설계하고 양방향으로 cost를 동일하게 표현한다.

![Untitled](Network%20Layer%20-%20Routing%20Algorithm%20e20002435d454bc38b9115ad7226604f/Untitled%202.png)

위 슬라이드에 있는 예시를 보면, w에서 z로 가는 cost를 계산하면 5인 것으로 그래프에 나타나 있다. key question은 u에서 z로 갈 때 어떻게 가야 최소한의 cost로 갈 수 있냐 하는 것이다. 

![Untitled](Network%20Layer%20-%20Routing%20Algorithm%20e20002435d454bc38b9115ad7226604f/Untitled%203.png)

- 모든 라우터가 모든 네트워크의 cost 정보(topology)를 공유하고, 지속적으로 업데이트하는 구조를 global한 구조라고 하고, 이 때에는 link state algorithms를 적용한다.
    
    <aside>
    💡 **SDN도 완전한 topology를 공유하는 중앙 집중 구조입니다.**
    
    SDN은 "Software-Defined Networking"의 약자로, 네트워크 인프라를 제어하기 위해 소프트웨어로 정의된 중앙화된 제어 계층과 분산된 데이터 전달 계층을 가지는 네트워킹 아키텍처입니다.
    
    기존의 전통적인 네트워크 아키텍처에서는 네트워크 장비(스위치, 라우터)는 패킷 전달과 네트워크 제어 기능을 동시에 수행했습니다. 이에 반해, SDN은 네트워크 제어 기능을 중앙화된 컨트롤러로 분리하여 구현합니다. SDN에서는 네트워크 스위치는 데이터 전달 기능만 수행하고, 네트워크 제어 결정은 소프트웨어 기반의 컨트롤러에서 수행됩니다.
    
    SDN에서 컨트롤러는 네트워크를 중앙에서 관리하고, 네트워크 스위치에 대한 제어 및 정책을 정의합니다. 이를 통해 네트워크 관리자는 중앙에서 전체 네트워크를 관리하고, 네트워크 트래픽을 유연하게 제어할 수 있습니다. 또한, SDN은 프로그래밍 가능한 인터페이스를 제공하여 네트워크를 자동화하고 가상화된 환경을 구성하는 데에도 활용될 수 있습니다.
    
    SDN의 장점 중 하나는 네트워크 제어의 유연성과 집중화입니다. 컨트롤러 기반의 접근 방식을 통해 네트워크 제어 결정을 중앙에서 관리하고 프로그래밍 가능한 인터페이스를 활용하여 네트워크를 쉽게 조작할 수 있습니다. 이는 네트워크 운영의 효율성과 자동화를 촉진하며, 유연한 네트워크 정책 구현을 가능하게 합니다.
    
    SDN은 클라우드 컴퓨팅, 가상화, 대규모 데이터 센터 등의 환경에서 특히 유용하며, 네트워크 관리 및 운영의 복잡성을 감소시키고 새로운 서비스 및 기술 도입을 용이하게 합니다.
    
    </aside>
    
- 물리적으로 이웃한 라우터에 대한 정보만 알고, 이웃한 라우터로부터만 정보를 얻고, 각 계산은 각각의 라우터가 수행하는 걸 decentralized 구조라고 한다. 이 때에는 distance vector algorithms를 적용한다. (cf. AS)

다른 기준으로 분류할 때에는,

- 루트 cost 정보가 천천히 변화하는 경우에는 static 하다고 한다.
- 루트 cost 정보가 자주 일어나는 경우엔 dynamic하다고 한다.

### Link State Algorithms(Dijkstra’s algorithm)

![Untitled](Network%20Layer%20-%20Routing%20Algorithm%20e20002435d454bc38b9115ad7226604f/Untitled%204.png)

다익스트라 알고리즘이라고도 한다. (최단 경로 알고리즘)

각 노드가 네트워크 전체에 대한 정보(network topology)를 모두 알고 있는 경우에만 수행할 수 있는 알고리즘입니다. 즉, 각 노드는 전체 네트워크 cost값을 알고 있으므로 모든 노드가 link state algorithm을 적용하여 동일한 결과를 들고 있어야 하는 개념입니다. (모든 라우터에 알려야 함(broadcast) == link-state advertisement)

`c(x,y)` x노드에서 y노드까지 가는 데에 소요되는 cost

`D(v)` source에서 dest. node(v) 까지 가는 데에 지금까지 계산된 현재 cost

`p(v)` dest. node(v)에 도달하기 전의 전 노드 (predecessor node)

`N'` 선택된 노드의 집합

*** v는 destination node, w는 인접 노드

![Untitled](Network%20Layer%20-%20Routing%20Algorithm%20e20002435d454bc38b9115ad7226604f/Untitled%205.png)

c(x,y) : x노드부터 y노드까지의 cost를 의미. 직접 연결되어 있으면 cost가 나오고, 만약 중간에 cost가 계산되지 않은 노드가 끼어 있다면 무한대로 인식됨

D(v) : “지금까지 계산한 바에 따라” v노드부터 목적지까지 가는 데에 드는 cost

p(v) : 출발지부터 v노드까지 올 때, 거쳐온 직전 노드

N : 알고 있는 것에 의거한, 최소 비용 루트를 구성하는 노드들의 집합

위 다익스트라 알고리즘을 실습해보자.

![Untitled](Network%20Layer%20-%20Routing%20Algorithm%20e20002435d454bc38b9115ad7226604f/Untitled%206.png)

즉, u에서 z로 가는 최적의 경로는 p(z)부터 단계별로 올라가면서 predecessor node를 확인하면 된다. 예를 들어 위 경우를 살펴보면, p(z) == y이므로, p(y)를 보면 v라고 나오니까 → p(v) == w → p(w) == u 가 나온다. 즉, u부터 z까지 가는 최적의 경로는 uwvyz이다.

ties(똑같은 값)이 존재할 수 있음은 유념해야 한다. 이 경우에는 둘 중에 아무거나 선택해서 테이블을 채우면 된다. (이 때에는 결론적으로 최적 path가 두 개 이상이 될 수 있는데, 일반적으로는 발생하지 않는다고 봐도 된다.)

![또 다른 예시](Network%20Layer%20-%20Routing%20Algorithm%20e20002435d454bc38b9115ad7226604f/Untitled%207.png)

또 다른 예시

![Untitled](Network%20Layer%20-%20Routing%20Algorithm%20e20002435d454bc38b9115ad7226604f/Untitled%208.png)

위 예시를 통해 포워딩 테이블을 구성해보면 위 오른쪽 표와 같다. 해당 표는 u 노드에 대한 포워딩 테이블을 나타내고 있다. u 노드에게는 v로 가기 위해서는 바로 v로 가야 하고, 다른 노드들로 가기 위해서는 최적의 경로 상 모두 x노드로 가는 것이 가장 효율적이었기에 x노드로 포워딩하도록 가리키고 있는 걸 확인할 수 있다.

![Untitled](Network%20Layer%20-%20Routing%20Algorithm%20e20002435d454bc38b9115ad7226604f/Untitled%209.png)

Dijkstra algorithm 이슈 : oscillations possible

e.g. 

## distance vertor algorithm(Dynamic programming)

bellman-ford equation**(dynamic programming)**

인접한 노드들로부터 정보를 받아 최단 경로를 판단하는 알고리즘 (내가 가려고 하는 dest node까지의 최단 path를 판단하기 위해 주변 노드에 물어보는 것)

↔ 다익스트라 알고리즘은 전체 노드의 엣지를 알고 있어야 함.

![Untitled](Network%20Layer%20-%20Routing%20Algorithm%20e20002435d454bc38b9115ad7226604f/Untitled%2010.png)

// given

dv(z)=5, dx(z)=3, dw(z)=3 과 같은 정보는 각 v,x,w노드가 가지고 있다고 가정.

u와 인접한 노드가 위 그래프처럼 v,x,w라고 할 때,

// when

u가 z까지 가는 최단 경로를 구하려면

// then

bellman-ford 알고리즘에 따라 du(z) = min{ c(u,v) + dv(z), c(u,x) + dx(z), c(u,w) + dw(z) } 로 판단 가능하다.

처음엔, 각 노드가 갖고 있는 다른 노드의 정보가 다를 수 있다. 어떤 노드는 주변 노드에 대한 데이터는 모두 가지고 있지만, 엣지가 2 이상인 노드의 데이터는 알지 못한다. 따라서 처음에는 벨멘 포드 알고리즘에 따라 얻은 정보가 완전 정확하지는 않을 수 있는데, 점차 진행하면서 정확해진다.

<aside>
💡 comment.

모든 노드의 정보를 라우터가 알고 있을 때에는 다익스트라가 더 좋긴 한데, 모르면 bellman-ford를 사용하는게 나음.

</aside>

특징

iterative : 반복적으로 정보를 교환, idistance vector가 수렴할 때까지 반복

asynchronous : 모든 노드가 서로 정확히 맞물려 동작할 필요가 없다…?

distributed : 직접 연결된 이웃으로부터 정보를 받고, 컴퓨팅. 계산된 결과를 다시 이웃에 배포

[📡 [Network] 거리 벡터 (Distance Vector) 라우팅 알고리즘이란? 문제와 해결책 (Poisones Reverse)](https://uzun.dev/182)

![Large N : Network](Network%20Layer%20-%20Routing%20Algorithm%20e20002435d454bc38b9115ad7226604f/Untitled%2011.png)

Large N : Network

distance vector는 계속(from time to time) 더욱 최적화되며, 각 노드가 주변 이웃들에게 해당 데이터를 broadcast하게 됩니다. 만약 새롭게 정보를 받으면, B-F를 돌려서 값을 업데이트한다. 이렇게 계속 하다 보면 일정 값으로 수렴하게 된다.

![Untitled](Network%20Layer%20-%20Routing%20Algorithm%20e20002435d454bc38b9115ad7226604f/Untitled%2012.png)

Distance vector 알고리즘은 변동이 발생했을 때에만 주변에 알리는 시스템이고, **비동기적으로** 서로의 노드가 업데이트된다. 변동이 생겼을 때 주변 노드에 broadcast해주면 전달을 받는 노드는 그 자체적으로 recompute하여 값을 다시 계산해보고, 만약 다시 계산해본 값이 이전 결과와 다를 경우에는 이 노드도 주변 노드에 notify(broadcast)해야 한다.

![위 슬라이드를 보면서 distance vector 매커니즘 익히기](Network%20Layer%20-%20Routing%20Algorithm%20e20002435d454bc38b9115ad7226604f/Untitled%2013.png)

위 슬라이드를 보면서 distance vector 매커니즘 익히기

![Untitled](Network%20Layer%20-%20Routing%20Algorithm%20e20002435d454bc38b9115ad7226604f/Untitled%2014.png)

t0 : y가 link-cost의 변화를 감지하고, 그 distance vector를 업데이트 한 뒤 주변 노드에 알립니다. 여기서는 **y가 보기에 c(y,x)가 4에서 1로 변화했기 때문에 해당 값을 z에게 전달합니다.**

$$
Dy(x) = min[ c(y,x)+Dx(x), c(y,z) + Dz(x)] = min[(1+0, 1+50)] = 1
$$

t1 : **z가** y로부터 **업데이트 소식을 확인받은 뒤에** 자신의 forwarding table을 업데이트하고, x로 가는 최소값을 **recompute하니 5에서 2로 줄어들었기에**, **이를 주변 노드에 다시 알려줍니다.**

$$
Dz(x) = min[ c(z,x)+Dx(x), c(z,y) + Dy(x)] = min[(50+0, 1+1)] = 2
$$

t2 : **y는 z의 업데이트 소식을 듣고, y가 갖고 있는 distance table을 업데이트합니다.** 하지만 **이 때에** y의 dv는 변하지 않았으므로 **y는 z에게 전달할 내용이 없습니다.**

위처럼 좋은 소식은 바로 주변에 전해지는데, DV algorithm에 따르면 나쁜 소식은 느리게 전파됩니다.

![Untitled](Network%20Layer%20-%20Routing%20Algorithm%20e20002435d454bc38b9115ad7226604f/Untitled%2015.png)

기존에 z가 x로 가는 cost로 알고 있던 것이 

$$
Dz(x) = 1 + Dy(x) = 5
$$

였던 상황임을 가정한다.

이후에 다음 c(y,x)가 4에서 60으로 변화한 경우, 이는 cost가 줄어든 상황이 아니기 때문에 y가 해당 뉴스를 주변 노드에 전달하지 않는다. 그럴 경우 다음과 같이 상황이 흘러간다.

$$
t0 : Dy(x) = 1 + Dz(x) = 1 + 5 = 6
$$

위처럼 c(y,x)가 변했는데도, z는 해당 정보를 알 수 없으므로 Dz(x)는 여전히 y를 거쳐 간다고 할 때 5가 걸린다고 알고 있게 되는 상황이 된다. 따라서 y가 Dz(x)가 몇이냐고 z에 물어보면, z는 5라고 y에게 대답할 것이다. (이럴 때 y는 z가 말해주는 값이 자신(y)을 통과하는 경로라는 것은 알지 못한다.) 따라서 아이러니하게도 Dy(x) = 6이 된다. 이러면 Dy(x) 는 값이 업데이트 되었으므로 이를 z에게 알리고, z는 이를 recompute한다.

$$
t1 : Dz(x) = 1 + Dy(x) = 1 + 6 = 7
$$

그럼 다시 z는 변화된 Dy(x)값을 가지고 Dz(x) 값을 recompute한다. 그리고 값이 다시 바뀌었으니 y노드에 이 사실을 알린다.

$$
t2 : Dy(x) = 1 + Dz(x) = 1 + 7 = 8
$$

이러한 사실은 6 + n의 값이 50을 넘기기 전까지는 알 수 없게 된다. 왜냐하면 좋은 방향으로 개선되지 않았기 때문에 주변 노드에 notify하지 않기 때문이다.

$$
tn : ... 50 < 6 + n
$$

이를 해결하기 위한 방법으로 poisoned reverse가 있다. poisoned reverse는 나와 바로 이웃한 노드로 가는 cost가 아니라, 다른 코스트에 대하여는 해당 경로에 대한 cost를 무한대로 보내는 방법이다. 이렇게 하면 항상 해당 cost를 계산할 때 개선될 수 밖에 없으므로 notify하게 되니까 위와 같이 bad news travels slow라고 해도 bad news가 발생하지 않으므로 큰 문제가 없는 것이다.

> 위 문제는 아래 포스팅 참고하셈(거리 벡터 알고리즘의 무한 계수 문제)
> 
> 
> [[Network] 네트워크 계층 : 라우팅(Link-State Routing)](https://hyeo-noo.tistory.com/292)
> 

![Untitled](Network%20Layer%20-%20Routing%20Algorithm%20e20002435d454bc38b9115ad7226604f/Untitled%2016.png)

Link state algorithm의 경우에는 모든 노드에 대하여 edge cost에 대한 내용을 전파해야 하지만, distance vector algorithm은 주변 노드에만 이를 알려주면 된다. 따라서 전달 속도도 `LS`의 경우 n개의 노드에 전부 전파해야 하므로 O(nE)가 걸리지만, `DV`는 이웃한 노드에만 전달하면 되므로 일반적인 경우에서는 짧다. 하지만 `count-to-infinity` 문제가 발생하여 많은 루프를 돌아야 하는 경우가 발생할 수 있다는 단점이 존재한다. 

또한 `LS`의 경우에는 어떤 노드가 잘못된 값을 가지고 있을 경우, 각 노드가 전체 네트워크 정보(topology)를 공유하고 있으므로 이를 알려주고, 각 노드가 각자의 포워딩 테이블을 recompute하는 구조였는데, `DV`의 경우에는 각 노드는 각각 cost table을 가지고 있으므로, 주변 노드가 업데이트가 원활히 되지 않은 상황에서 전파를 하면 문제가 발생할 수 있다.