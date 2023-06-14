# Link Layer - Overview, Error detection, correction

## link layer Overview

---

링크 레이어에서는 다음과 같은 작업을 수행합니다.

- error detection, correction
- link layer addressing
- local area networks : ethernet, VLAN (virtual LAN ; 독립적인 LAN 환경 구축)

*** LAN (local Area network ; ethernet) / WAN(wide area network ; 스마트폰 LTE도 WAN이다.)

링크 레이어 특징

- link layer에서는 네트워크 레이어의 데이터그램을 캡슐화한 “프레임”을 전송 유닛으로 사용합니다.
- 채널 하나에 multiple access를 해야한다. → channel을 sharing 하는 방법을 공부한다.
- L2 switch(그냥 스위치 ; 링크 레이어), L3 switch(router ; 네트워크 레이어)

![Untitled](Link%20Layer%20-%20Overview,%20Error%20detection,%20correction%20140b615cc29c4e30952decd931276525/Untitled.png)

링크 레이어는 바로 아래의 physical layer를 이용해서 데이터를 전송하는 계층이라고 생각하면 된다.

![Untitled](Link%20Layer%20-%20Overview,%20Error%20detection,%20correction%20140b615cc29c4e30952decd931276525/Untitled%201.png)

인터넷의 링크 레이어는 대부분 이더넷을 사용한다고 봐도 무방할 정도로 이더넷을 많이 사용하고 있다.

![Untitled](Link%20Layer%20-%20Overview,%20Error%20detection,%20correction%20140b615cc29c4e30952decd931276525/Untitled%202.png)

![attenuation : 감쇄](Link%20Layer%20-%20Overview,%20Error%20detection,%20correction%20140b615cc29c4e30952decd931276525/Untitled%203.png)

attenuation : 감쇄

### Frame

링크 레이어에서는 데이터그램을 프레임으로 encapsulation을 하는데, 일반적으로 지금까진 헤더만 붙이면서 encapsulation을 했다면, 링크 레이어에서 프레이밍 할 때에는 header와 trailer를 같이 붙인다.

왜 trailer가 있을까? 링크 레이어는 실제로 physical layer를 이용하여 전기 신호로 데이터를 보내는 계층이다. 이 때 header부터 trailer까지 전체를 전기 신호로 보내는 과정에서 error detection 같은 것들을 수행해야 하는데, checksum을 header에 넣으면 전부 보내기 전에 전체를 스캔하고 보내야 하는 오버헤드가 발생할 수 있다. 하지만 trailer에 checksum을 넣음으로써 보낼때 쭉 그대로 스캔하고 별도의 추가적인 과정 없이 checksum을 확인하고 보낼 수 있게 되는 것이다. (그 외에 다른 이유가 있는지는 알아볼 것)

### MAC address

MAC address는 하드웨어 주소이다. 수신자가 자신이 받을 데이터인지 아닌지를 빠르게 확인하기 위해 주소를 봐야 하므로 MAC 주소는 header에 들어가야 한다. 네트워크 레이어에서는 IP 주소를 사용했지만, 링크 레이어는 피지컬 레이어를 이용하여 실제로 전기 신호로 데이터를 송신하는 계층이므로 하드웨어 주소인 MAC 주소가 header에 담기는 것이다.

### reliable delivery mechanism in unreliable link system

광섬유의 경우 일반적으로 전기 신호와 전혀 다른 “빛” 파장을 가지고 통신을 하기 때문에 low bit-error가 발생한다. 하지만 광섬유는 가격이 저렴하지 않아서 모든 경우에 적용되지 않아, 구리선을 많이 사용하게 되는데 이는 상대적으로 error가 발생할 수 있는 확률이 높고, wireless의 경우엔 error rates가 훨씬 높아진다. 따라서 이렇게 unreliable한 통신 환경에서 reliable한 통신을 만들기 위한 매커니즘이 필요하다.

### 왜 링크 레이어에서도 error detection을 해야 하는가? 상위 레이어에서도 계속 했는데.

여기서 할 수 있는 error detection을 굳이 상위 레이어에서 하기 위해서 발생하는 오버헤드가 존재한다.(우회하는 개념이라고 이해하면 된다.)

### flow control

### error detection

### error correction

### half-duplex & full-duplex

half duplex는 양방향 통행이 가능한 길 위에서 통행 시 일방 통행으로 운영하는 것을 의미하고, full duplex는 완전 양방향 통행을 의미한다.

![Untitled](Link%20Layer%20-%20Overview,%20Error%20detection,%20correction%20140b615cc29c4e30952decd931276525/Untitled%204.png)

컴퓨터 내에서도 network를 실현하기 위해 위와 같은 구조를 가진다. CPU는 application, transport, network 레이어 그리고 link 레이어의 일부 작업을 지원합니다. 그리고 NIC 하드웨어는 컴퓨터 내에서 링크 계층과 물리 계층을 구현하며, 컴퓨터와 네트워크 간의 물리적인 연결을 담당하여, 이를 통해 컴퓨터는 네트워크를 이용하여 데이터를 전송하고 수신할 수 있는 겁니다.

CPU는 전송하고자 하는 데이터를 application, transport, network 레이어와 링크 레이어 작업을 처리한 뒤, 디바이스 드라이버를 통해 NIC로 보냅니다. 그러면 NIC는 링크 레이어 작업을 마저 처리하며, 데이터를 전기 신호로 적절한 포트에 전송합니다. 이 때, CPU와 NIC는 드라이버를 통해 연결됩니다.

## Error detection

---

![Untitled](Link%20Layer%20-%20Overview,%20Error%20detection,%20correction%20140b615cc29c4e30952decd931276525/Untitled%205.png)

error detection and correction을 위해 EDC bit을 둔다. 하지만 이는 100% reliable하다고 볼 순 없고, EDC를 더 크게 가져가면 더 좋게 error detection과 correction을 할 수 있지만 이는 트레이드오프를 고려해야 한다.

![Untitled](Link%20Layer%20-%20Overview,%20Error%20detection,%20correction%20140b615cc29c4e30952decd931276525/Untitled%206.png)

single bit parity는 비트 데이터의 오류를 감지할 수 있는 간단한 솔루션 중 하나이다. 데이터 비트의 1이 홀수면 parity bit가 1이 되어 전체 데이터의 1의 개수를 짝수로 맞추고, 데이터 비트의 1이 짝수면 parity bit이 0이 되어 전체 데이터의 1의 개수를 짝수로 맞춘다. 

single bit parity 뿐만 아니라 2차원 bit parity도 적용할 수 있다. 2차원 parity를 사용할 경우에는 어떤 비트가 잘못됐는지 위치를 알아낼 수도 있다.

parity checking은 위처럼 **단일 비트 오류만을 감지할 수 있기 때문에** 정말 가끔씩 error가 발생하는 것에 적절할 수 있는 솔루션이고, 자주 에러가 발생할 수 있는 환경에서는 부적절할 수 있다. link layer는 이러한 이유로 parity bit를 사용하기보단 더 나은 방법인 CRC를 적용한다. 이는 parity check보다 더 강력한 오류 감지 기능을 제공한다. 

![Untitled](Link%20Layer%20-%20Overview,%20Error%20detection,%20correction%20140b615cc29c4e30952decd931276525/Untitled%207.png)

checksum은 16bit integer의 시퀸스이고, 16비트씩 되어있는 데이터와 체크섬을 전부 더하는 거고, 캐리가 생기면 밑에 더해주고 난 뒤의 결과를 1의 보수를 한 뒤에 얻는 값이다. 이는 논리적인 영역이기 때문에 소프트웨어로 해결하는 것이 가장 적절한데, link layer는 일반적으로 하드웨어적인 처리를 하여 빠르게 전송하는 것이 목적인 계층인데,  checksum은 더하기 등의 연산은 소프트웨어적인 부적절하다.

![Untitled](Link%20Layer%20-%20Overview,%20Error%20detection,%20correction%20140b615cc29c4e30952decd931276525/Untitled%208.png)

CRC는 링크 레이어에서 사용하는 error detection 방법이다. parity는 한 비트만 에러를 검출할 수 있는데, 실제로는 여러 비트가 깨질 수 있는 거라서 부적절하고, crc는 여러 비트가 깨지더라도 알아낼 수 있고, r bits의 사이즈를 키울수록 더 정확히 알 수 있기 때문에 이 방식이 더 선호된다.

여기서는 shift 연산을 이용하여 곱하기 2와 나누기 2를 사용합니다. 곱하기 n의 경우에는 연산이 느릴 수 있지만, 곱하기/나누기 2의 경우에는 shift 연산으로 표현이 가능하기 때문에 훨씬 간단해집니다.

![Untitled](Link%20Layer%20-%20Overview,%20Error%20detection,%20correction%20140b615cc29c4e30952decd931276525/Untitled%209.png)

여기서 G는 4자리인 경우는 1001, 5자리인 경우는 11001과 같이 정해져 있는 값입니다. 여기서 r은 3자리를 구하기로 했으므로 3이 되고, D=101110인 것에 shift 연산을 3번 한 값을 G=1001로 나누기(XOR)하면 remainder를 구하면 011이 나온다. **위 example은 시험 문제로도 나올 수 있으니 꼭 풀어볼 것**

위 나누기 연산을 XOR로 표현하듯이, 만약 1001을 1010으로 나눈다고 하면 결과는 0이 아니라 11이 되어야 한다.