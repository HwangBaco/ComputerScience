# Link Layer - Switch, VLAN

## Ethernet Switch

---

![Untitled](Link%20Layer%20-%20Switch,%20VLAN%20172fadaa41e247e9981eb10e088e9e22/Untitled.png)

HW적으로는 Switch라는 것이 하나 존재하는데, 이 것이 하는 일을 논리적으로 구분하면 layer 2 switch가 있고, layer 3 switch가 있는 것이다. 즉, switch라는 것이 layer 2와 layer 3를 위한 역할을 둘 다 수행하는 것이다.

그 중에서 layer 2 switch인 이더넷 스위치에 대해서만 알아보자.

이는 프레임을 저장/포워딩 하며, 들어오는 프레임에 대하여 MAC 주소를 확인하여 맞게 온건지, 맞게 나가는지 등을 관리한다. 이 때 사용되는 프로토콜은 CSMA/CD이다. 호스트는 스위치에 대해서 알 필요는 없고, 스위치는 작동을 위한 별도의 작업이 필요 없이 plug and play로 configured된다.

![Untitled](Link%20Layer%20-%20Switch,%20VLAN%20172fadaa41e247e9981eb10e088e9e22/Untitled%201.png)

스위치는 collision을 없애기 위한 장치이다. 예를 들어, 가운데에 switch가 없이 channel이 모두 연결되어 있는 구조라고 할 때, A에서 A`으로 데이터를 보내는 중에, B와 B`도 데이터 통신을 시도하면 collision이 발생할 것이다. 하지만 switch가 있음으로 인해 각 channel이 dedicated되어있으므로, 데이터 전송이 simultaneously하게 일어나도 큰 문제가 없는 것이다. 

![Untitled](Link%20Layer%20-%20Switch,%20VLAN%20172fadaa41e247e9981eb10e088e9e22/Untitled%202.png)

A가 A`으로 데이터를 전송할 때, 스위치는 데이터를 전송받을 때 어떻게 그 데이터가 4번 인터페이스로 전달되어야 한다는 것을 알 수 있을까? 이는 스위치가 switch table을 갖고 있기 때문에 알 수 있다. A는 전달하는 프레임에 A`의 MAC address를 담아 보내므로, 스위치는 이를 확인하여 해당 주소와 매핑되어있는 interface로 포워딩하는 것이다.

![Untitled](Link%20Layer%20-%20Switch,%20VLAN%20172fadaa41e247e9981eb10e088e9e22/Untitled%203.png)

만약 스위치가 처음 받는 source MAC address라면, 해당 경로에 대하여 MAC address와 interface에 대해서도 table에 기록함으로써 routing 정보를 테이블에 기록하여 추후에 사용하게 된다.

![Untitled](Link%20Layer%20-%20Switch,%20VLAN%20172fadaa41e247e9981eb10e088e9e22/Untitled%204.png)

다음은 프레임이 스위치에 도착했을 경우에, **스위치**가 수행하는 매커니즘을 pseudo code로 작성한 것이다. (위 슬라이드 요약)

```python
"""when switch received frame, """
if 발신 노드 MAC address entry in 스위치 테이블:
	record(frame[src_host].MAC_address)

if 목적 노드 MAC 주소 entry in 스위치 테이블:
	if 목적 노드 MAC address == 발신 노드 MAC address:
		drop(frame)
	else:
		forward(frame) # on interface following switch table
else if 목적 노드 MAC 주소 entry not in 스위치 테이블:
	flood(frame) # broadcast
```

`broadcast` : 받는 노드들은 destination MAC address를 확인하고, 자기꺼면 reply to switch , 자기께 아닌 노드들은 해당 프레임 discard

<aside>
💡 네트워크 설계시에 가급적이면 broadcast 또는 flood는 네트워크를 혼잡하게 만들기 때문에 최대한 사용하지 않도록 설계된다.

</aside>

![Untitled](Link%20Layer%20-%20Switch,%20VLAN%20172fadaa41e247e9981eb10e088e9e22/Untitled%205.png)

그렇다면 스위치가 여러 개가 한번에 연결되어 있다면 어떨까?

**아래 과정을 통해 switch table 채우는 거 시험에 나올 가능성 높음**

C노드에서 I 노드로 보내는 상황을 가정해보자.

1. `send` C는 S1에 I 노드로 보내고자 하는 프레임을 전송.
2. `record` S1은 swtich 테이블에 mac address : interface = C : 4로 기록
3. `flood` S1은 I 노드에 대한 정보가 switch 테이블에 없으므로 모든 인터페이스 대하여 broadcast 진행
4. `discard` A, B 노드는 프레임 헤더의 destination MAC address를 확인하고, 자신의 것이 아니니 discard  frame
5. `record / flood` S4의 스위치 테이블에도 C 노드에 대한 인터페이스(mac address : interface = C : 1) 기록 후, I 노드의 MAC 주소가 없으므로 broadcast
6.  `record / flood` S2의 스위치 테이블에도 C노드에 대한 인터페이스(mac address : interface = C : 1) 기록 후, I 노드의 MAC 주소가 없으므로 broadcast
7. `discard` D, E, F 노드는 프레임 헤더의 destination MAC address를 확인하고, 자신의 것이 아니니 모두 discard  frame
8. `record / flood` S3의 스위치 테이블에도 C노드에 대한 인터페이스(mac address : interface = C : 1) 기록 후, I 노드의 MAC 주소가 없으므로 broadcast
9. `discard` G, H노드는 프레임 헤더의 destination MAC address를 확인하고, 자신의 것이 아니니 모두 discard  frame
10. `receive / reply` I는 프레임 헤더의 destination MAC address를 확인하고, 자신의 것이므로 수신한 뒤 응답을 S3에 전송
11. `record / forward` S3는 mac address : interface = I : 4 기록 후, C : 1이라는 정보가 스위치 테이블에 있으므로 forward
12. `record / forward` S4는 mac address : interface = I : 3 기록 후, C : 1이라는 정보가 스위치 테이블에 있으므로 forward
13. `record / forward` S1는 mac address : interface = I : 1 기록 후, C : 4이라는 정보가 스위치 테이블에 있으므로 forward
14. `receive` C노드는 I 노드로부터 온 응답을 수신

### switch vs. router

![Untitled](Link%20Layer%20-%20Switch,%20VLAN%20172fadaa41e247e9981eb10e088e9e22/Untitled%206.png)

라우터 : 네트워크 레이어만 본다.

스위치 : 링크 레이어만 본다.

→ 둘 다 store-and-forward이다.

→ 둘 다 forwarding table을 가지고 있다.

![Untitled](Link%20Layer%20-%20Switch,%20VLAN%20172fadaa41e247e9981eb10e088e9e22/Untitled%207.png)

스위치로 연결되어 있으면 single broadcast domain 위에 있는 건데, 물리적으론 하나의 동일한 LAN 안에 있는 노드들이라도, 이를 구분하고 싶다는 니즈를 바탕으로, 내부적으로는 하나의 스위치를 중심으로 구성되어있는 LAN을 논리적으로 나눠서 구분되는 가상 LAN 환경을 구축하는걸 VLAN이라고 한다.

![Untitled](Link%20Layer%20-%20Switch,%20VLAN%20172fadaa41e247e9981eb10e088e9e22/Untitled%208.png)

구분하는 이유는 여러 이유가 있을 수 있고, 그 중 하나로 한 개의 LAN 안에 여러 그룹이 동시에 존재하는 경우 서로 불필요한 네트워크 트래픽을 고려해야 하고, 보안에 더 신경을 써야 한다. 하지만 논리적으로 구분함으로써 트래픽을 분리하고, 불필요한 flood 등의 오버헤드를 줄일 수 있습니다.

VLAN을 구현하는 방식은 크게 두 가지로 나눌 수 있습니다.

1. 포트 기반 VLAN (port-based VLAN)
    
    ![Untitled](Link%20Layer%20-%20Switch,%20VLAN%20172fadaa41e247e9981eb10e088e9e22/Untitled%209.png)
    
    포트 기반은 스위치 포트(인터페이스)를 VLAN을 나누는 기준으로 하여 하나하나 VLAN을 할당하는 방식을 말합니다. 이는 간단하여 작은 규모의 네트워크에서 주로 사용됩니다.
    
2. 태그 기반 VLAN (tag-based VLAN)
    
    ![Untitled](Link%20Layer%20-%20Switch,%20VLAN%20172fadaa41e247e9981eb10e088e9e22/Untitled%2010.png)
    

![Untitled](Link%20Layer%20-%20Switch,%20VLAN%20172fadaa41e247e9981eb10e088e9e22/Untitled%2011.png)

VLAN 또한 프로토콜이기 때문에 802.1Q라는 프로토콜 타입 이름이 존재합니다. 스위치가 데이터그램을 프레임으로 가공할 때 헤더 필드에 VLAN 태그를 추가하여 VLAN을 식별하고, 이를 보고 다른 장비들도 VLAN 정보를 공유합니다.