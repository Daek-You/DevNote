

## 1. [IGW] 외부 인터넷과 AWS 내부망 사이의 경계

외부 인터넷에서의 요청은 AWS 내부망으로 들어서는 첫 진입점인 `IGW(Internet Gateway)`를 지나게 됩니다. `IGW`는 다음과 같은 두 가지 주요 기능을 수행합니다.  

1. **[VPC(Virtual Private Cloud)](VPC(Virtual%20%20Private%20Cloud).md) 라우팅 테이블에게 외부 인터넷으로 향하는 경로 제공**
2. **Public IP 주소가 있는 [EC2 Instance](EC2.md)에 대해, [NAT(Network Address Translation)](https://namu.wiki/w/NAT)을 수행**  

`IGW`는 수평적으로 확장되고, 중복으로 구성되어 있어 단일 장애점이 없습니다. 이러한 `IGW`는 AWS 네트워크 경계에서 첫 번째 방어선 역할을 수행하며, [DDoS](https://namu.wiki/w/%EB%B6%84%EC%82%B0%20%EC%84%9C%EB%B9%84%EC%8A%A4%20%EA%B1%B0%EB%B6%80%20%EA%B3%B5%EA%B2%A9)와 같은 악성 트래픽을 필터링합니다.  

## 2. [VPC] AWS 내부망에서 특정 고객의 전용 네트워크로

`IGW`를 통과했다면, 이제 AWS 클라우드의 내부망으로 들어설 차례입니다. AWS 클라우드 서비스를 사용하는 다양한 고객들이 있고, AWS는 이에 맞춰서 <b>각 사용자(계정) 전용으로 구성된 네트워크</b>를 지급하게 되는데, 이를 `VPC(Virtual Private Cloud)`라고 합니다.  

`VPC`는 실제 별도의 물리적 장치를 추가하여 지급하는 것이 아닌, 소프트웨어 기술을 사용해 논리적으로 만들어 지급하는 가상 네트워크입니다. [SDN(Software Defined Networking)](https://www.juniper.net/kr/ko/research-topics/what-is-sdn.html)이라는 기술을 통해 `VPC`가 만들어졌습니다.  

이러한 `VPC`는 기본적으로 다른 `VPC`들과 완전히 격리되어 있어, 보안성을 높여줍니다. 위에서 말했듯, 각 `VPC`는 자체적으로 가상 네트워크 구조를 가지며, AWS가 구성한 물리적인 인프라 위에 추상화된 계층으로 존재하게 됩니다. 또한, `VPC` 내의 모든 네트워크 트래픽은 기본적으로 해당 VPC 내에 유지됩니다.  

나머지는 시간 날 때 틈틈히 적겠습니다.
