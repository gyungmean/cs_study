# AWS VPC
## public 서브넷을 인터넷과 연결하기
**서브넷에 속한 EC2가 인터넷과 연결되려면 필요한 조건**
- VPC에 `Internet Gateway`가 있어야함
- EC2가 `public IP`를 가져야함
- EC2가 위치한 서브넷이 Internet Gateway로 패킷을 보낼 수 있어야함.
- 서브넷의 라우팅 테이블에 0.0.0.0/0 -> Internet Gateway(target) 규칙이 있어야함.
![image](https://github.com/gyungmean/cs_study/assets/70059000/5119b1b5-7b8f-4ff5-a285-cd5920f87f69)


## 내부 서브넷끼리 통신하도록 연결하기
- public subnet
    - 인터넷과 직접 연결되는 서브넷
    - 인터넷 라우트 규칙이 있다.
    - NAT, Web server, 로드밸런서 등이 위치
- private subnet
    - 인터넷이랑 연결이 안된 서브넷
    - 인터넷 라우트 규칙이 없다.
    - Application server, DB 등이 위치
![image](https://github.com/gyungmean/cs_study/assets/70059000/aa7ecdcc-692b-4b63-9e7e-e3daec30b36a)
- private subnet의 라우트 테이블은 target이 loacal만 존재
- 그림 속 EC2-B의 경우 보안 그룹에서 같은 VPN의 SSH와 ping을 허용한다.
- public 서브넷 EC2로 SSH접속을 할 경우 -> private EC2로 접속이 되는 것이다.


## private subnet에서 인터넷 연결하기
- private 서브넷의 서버에서 인터넷에 패킷을 보내려면 public 서브넷으로 패킷을 보내고 public 서브넷을 통해 패킷을 받아야함
- 주소 변환 과정 
    - `NAT`는 EC2의 아웃바운드 패킷에서 도착지를 자신의 주소로 바꿈 그리고 private ip를 기록한다.
    - 외부 인터넷은 return 패킷을 이 NAT Gate Way로 보낸다.
    - NAT Gate Way는 private ip의 EC2로 해당 트래픽을 전달한다.
    ![image](https://github.com/gyungmean/cs_study/assets/70059000/ce8c4637-8095-4449-b9f4-49694432bcf7)
- AWS에서 이를 구현하려면?
    ![image](https://github.com/gyungmean/cs_study/assets/70059000/0dd2ece4-6e79-4d94-a2a1-9142791007b0)
    - AZ에 public subnet을 만든다. 
    - 인터넷 게이트웨이와 연결된 라우트 테이블을 적용시킨다.
    - public 서브넷에 NAT Gateway를 둔다.
    - private 서브넷 라우트 테이블에 0.0.0.0/0 -> nat gateway 규칙을 추가한다.
    - private 서브넷의 ec2는 인터넷에 접근하려고 할 때 이 nat gateway를 통하게 된다.
    
### NAT란??
로컬 호스트가 공용 인터넷에 액세스할 수 있게 하고 외부에서는 직접 액세스 할 수 없도록 보호한다.


---
# :pencil: 퀴즈
NAT는 왜 필요하고, AWS 네트워크에서 어떻게 구현할 수 있을까요?