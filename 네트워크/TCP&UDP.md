## TCP/IP 계층구조

TCP/IP란?

TCP와 IP 두가지 프로토콜 방식을 조합하여 인터넷 통신하는 것을 TCP/IP라고 부른다.

- IP
    - 패킷 데이터들을 최대한 빨리 특정 목적지 주소로 보내는 프로토콜
    - 패킷 전달 여부를 보충하지 않고 패킷을 보낸 순서와 받는 순서가 다름
- TCP
    - 패킷 통신은 데이터를 작은 단위로 나눠서 전송 → 순서가 뒤섞이거나 내용이 유실된다는 단점이 있음
    - 패킷을 정상적으로 받을 수 있도록 하는 프로토콜
    - 패킷 전달 여부를 보증, 패킷을 송신 순서대로 받게 해줌

⇒ 송신자가 수신자에게 IP를 사용하여 최대한 빠르게 패킷을 전송하면, TCP를 활용해 패킷을 정상적으로 수신

TCP/IP계층은 TCP/IP 프로토콜 통신 과정에 초점을 맞추어 OSI 7계층을 좀 더 단순화 시킨 계층을 의미

- **이러한 계층적인 구조의 특징**
    - 각 계층별 처리 역할이 다르기 때문에, 계층별 간섭을 최소화할 수 있다.
    - 특정 계층에서 문제가 생기면, 해당 계층을 살펴보면 되기 때문에, 유지 보수가 편리
    - 다른 계층끼리는 데이터의 전달 과정을 구체적으로 알 필요가 없기 때문에, 데이터의 캡슐화와 은닉이 가능

데이터 전송시 : 상위 계층에서 하위 계층으로 이동하면서 이동 마다 필요한 정보(헤더)가 추가됨 ⇒ 캡슐화

데이터 수신 시 : 하위 계층에서 상위 계층으로 이동하면서 헤더를 읽고 알맞은 행동을 취한 후, 헤더 제거 ⇒ 역캡슐화

### 응용 계층

어플리케이션이 네트워크에 접근 가능하도록 해주는 역할

사용자 ↔ 소프트웨어 간 소통 담당 계층

ex) telnet, ftp, dhcp, ftp, http, smtp, dns

### 전송 계층

통신 노드 간 신뢰성 있는 데이터 전송을 보장(패킷 오류 검사, 재전송 요구)

TCP(Transmission Control Protocol) : 연결 지향, 확인 응답으로 신뢰성 있는 전송 기능

UDP(User Datagram Protocol) : 패킷의 정확한 전달을 보장하지 않음, 송수신의 책임은 상위의 Application이 가짐

### 인터넷 계층

주소 관리, 패킷을 최종 목적지까지 라우팅하는 계층

- IP (Internet Protocol)
    - 호스트들과 네트워크에서 주소 관리
    - 패킷을 라우팅
- ARP(Address Resolution Protocol) : IP와 매칭되는 MAC Address를 쿼리
- ICMP(Internet Control Message Protocol) : 패킷 전송에 관한 에러 메시지를 처리

### 네트워크 인터페이스 계층

신뢰성 있는 데이터 전송을 담당하는 계층

MAC주소가 이 계층에서 사용됨.

데이터 전기신호로 변환한 뒤 물리적 주소인 MAC주소를 사용해 알맞은 기기로 데이터를 전달함

## UDP

> 비연결형 프로토콜! 속도가 빠르다
> 

### 특성

- 비연결성
    - 연결설정 없이 데이터를 전송
    - 연결설정을 위한 지연 시간이 없음
    - 데이터의 전송 순서가 바뀔 수 있다!
- 비상태정보
    - 연결 정보나 상태 정보를 저장하지 않음
    - TCP같은 3-way handshaking같은 과정이 없다 = 수신 여부 확인X
- 경량의 오버헤드
    - TCP 세그먼트 헤더는 20바이트, UDP 8바이트
- 비정규적인 송신률
    - 일부 패킷의 손실이 생기더라도 최소 전송률을 요구하는 실시간 전송에 사용됨

## TCP

> 연결 지향적 프로토콜! 안정적으로, 순서대로, 에러 없이!
> 

**연결 지향적 프로토콜이란 무엇일까요?**

### 개념

UDP 로는 이를 만족시킬 수 없으므로 다른 프로토콜이 필요하여 탄생한 것이 TCP

신뢰성이 없는 인터넷을 통해 종단간에 신뢰성 있는 바이트 스트림을 전송 하도록 특별히 설계

송신자와 수신자 모두가 소켓이라고 부르는 종단점을 생성함으로써 이루어진다. 

TCP Header에는 Code Bit(Flag bit)라는 부분이 존재

6Bit로 이루어져 있고 각각 한 bit들이 의미를 갖고 있음

Urg-Ack-Psh-Rst-Syn-Fin 순서

해당 위치의 비트가 1이면 해당 패킷이 어떠한 내용을 담고 있는 패킷인지를 나타냄

SYN 패킷일 경우엔 000010이 되고 ACK 패킷일 경우에는 010000이 되는 것이다.

### 특성

- 접속형
    - 3-way handshaking과정을 통해 연결을 설정
    - 4-way handshaking과정을 통해 연결을 해제
- 신뢰성
    - 신뢰성이 높은 전송을 하기 때문에 UDP보다 속도가 느림
    - 신뢰성있는 전송이 중요할 때 사용하는 프로토콜! ex)파일전송
- 흐름제어
    - 데이터 처리 속도를 조절하여 수신자의 버퍼 오버플로우 방지
    - 일정 크기의 버퍼 공간을 갖고, 송신하는 TCP가 수신 측이 갖고 있는 버퍼 크기만큼 데이터를 보내도록 제어, 처리 속도가 느린 수신 측 호스트의 버퍼 크기를 넘치게 하는 것을 방지
- 혼잡제어
    - 네트워크 내의 패킷 수가 과도하게 증가하지 않도록 방지

### 3-way handshake

**왜 3-way handshake가 필요할까요? 2-way handshake로는 안되는 이유가 뭘까요?**
SYN (synchronize sequence number) - 연결 확인을 보내는 무작위 숫자 값

ACK (acknowledgement) - Client 혹은 Server로부터 받은 SYN에 1을 더해 SYN을 잘 받았다는 말

**왜 SYN의 값은 무작위 숫자 값이어야할까요?**

| 상태 | 설명 |
| --- | --- |
| CLOSED | 연결 수립을 시작하기 전의 기본 상태(연결 없음) |
| LISTEN | 포트가 열린 상태로 연결 요청 대기 중 |
| SYN-SENT | SYN을 요청한 상태 |
| SYN-RECEIVED | SYN 요청을 받고 상대방의 응답을 기다리는 중 |
| ESTABLISHED | 연결 수립이 완료된 상태, 서로 데이터를 교환할 수 있다. |

### 4-way handshake

1. close를 실행한 클라이언트가 FIN을 보내고 FIN-WAIT-1 상태로 대기
2. 서버는 CLOSE-WAIT로 바꾸고 ACK 전달 + 동시에 해당 포트에 연결되어 있는 애플리케이션에게 close 요청
3. ACK를 받은 클라이언트 상태를 FIN-WAIT-2로 변경
4. close요청을 받은 서버 애플리케이션은 종료 프로세스를 진행하고 FIN을 클라이언트로 보내 LAST_ACK 상태로 바꾼다.
5. FIN을 받은 클라이언트는 ACK를 서버에서 다시 전송하고 TIME-WAIT으로 상태를 바꿈
6. 클라이언트는 일정 시간이 지나면 자동으로 CLOSE됨 TIME-WAIT가 없으면 패킷 손실이 발생할 수 있음

| 상태 | 설명 |
| --- | --- |
| FIN-WAIT-1 | 자신이 보낸 FIN에 대해 ACK를 기다리거나 상대의 FIN을 기다림 |
| FIN-WAIT-2 | 자신이 보낸 FIN에 대해 ACK를 받았고 상대방의 FIN을 기다림 |
| CLOSE-WAIT | 상대의 FIN을 받을 상태, 상대방 FIN에 대한 ACK를 보내고 애플리케이션에 종료를 알림 |
| LAST-ACK | CLOSE-WAIT상태를 처리 후 자신의 FIN요청을 보낸 후 FIN에 대한 ACK를 기다리는 상태 |
| TIME-WAIT | 모든 FIN에 대한 ACK를 받고 연결 종료가 완료된 상태, 새 연겨로가 겹치지 않도록 일정 시간 기다린 후 CLOSED로 전 |