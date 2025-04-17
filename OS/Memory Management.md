# 1. 메모리 관리란?

<img src="https://miro.medium.com/v2/resize:fit:1100/format:webp/1*VF7y9EidYc7kk_igl7g2dg.jpeg" width="1200"/>  

- 메모리는 CPU, 보조기억장치와 함께 운영체제가 관리하는 3대 핵심 자원 중 하나
- <b>메모리 관리</b>란, 이러한 메모리 자원을 효율적으로 활용하기 위해 운영체제가 수행하는 다양한 활동을 의미
- 운영체제의 메모리 관리는 다음과 같은 목적을 가지고 있음

> <b>✏️메모리 관리의 목적</b>  
> <b>1. 여러 프로세스 간의 메모리 공유</b>: 제한된 메모리 자원을 여러 프로세스가 효율적으로 공유할 수 있도록 함  
> <b>2. 메모리 보호</b>: 한 프로세스가 다른 프로세스의 메모리 영역에 접근하지 못하도록 보호  
> <b>3. 메모리 활용도 극대화</b>: 메모리 낭비를 최소화하고 가용 메모리를 최대한 활용  
> <b>4. 가상 메모리 지원</b>: 물리적 메모리보다 큰 프로그램을 실행할 수 있도록 제공  

<br>

# 2. 메모리 관리를 어떻게 하는가?
### 기본 메모리 시스템

![Base, Limit Register](/Resources/Images/Base,Limit%20Register.webp)  

- 운영체제는 시스템 소프트웨어지만, 메모리 주소를 계산하는 부분에 관해서는 하드웨어의 도움이 필요
- 이를 위해, 가장 기본적인 메모리 관리 시스템에서는 2가지 레지스터를 사용

> <b>1. `Base Register`</b>: 프로세스의 시작 주소를 저장  
> <b>2. `Limit Register`</b>: 프로세스의 크기를 저장  

- 이 두 레지스터를 사용하면, 프로세스가 자신의 메모리 영역을 벗어나 다른 프로세스 영역에 접근하는 것을 방지할 수 있음

### 메모리 관리 과정

1. <b>할당(Allocation)</b>: 프로세스에게 필요한 메모리 공간을 할당  
2. <b>회수(Deallocation)</b>: 프로세스가 종료되면, 할당된 메모리를 회수
3. <b>보호(Protection)</b>: 프로세스가 자신의 메모리 영역만 접근하도록 제한
4. <b>공유(Sharing)</b>: 여러 프로세스가 특정 메모리 영역(ex. 공유 라이브러리)을 공유하도록 함
5. <b>가상화(Virtualization)</b>: 물리적 메모리보다 큰 주소 공간을 제공  

<br>

# 3. 메모리 관리를 위한 하드웨어
## 📌MMU(Memory Management Unit)

<img src="https://media.geeksforgeeks.org/wp-content/uploads/20240208232601/Screenshot-2024-02-08-231740.png" width="1200"/>  

- `PMMU(Page Memory Management Unit)`이라고도 하며, CPU와 메모리 사이에 위치한 하드웨어
- CPU에 의해 생성된 <b>논리적 가상 주소(Virtual Address)</b>를 컴퓨터의 메모리에서 <b>실제 물리적인 주소로 변환</b>해줌
- 일반적으로 프로세서(CPU)에 통합되어 있지만, 경우에 따라서는 별도의 통합 회로(IC)로 구성되기도 함  

### 기본적인 MMU 구조

- 가장 단순한 형태의 MMU는 다음 두 개의 레지스터로 구성됨
    1. `Relocation Register`: 프로그램의 시작 주소를 저장
    2. `Limit Register`: 프로그램의 크기를 저장  

> <b>❓Base Register와 Relocation Register는 서로 다른 건가요?</b>  
> - 용어만 다를 뿐, 동일한 하드웨어 컴포넌트를 지칭  
> - 문헌이나 교재에 따라, Base Register, Relocation Register, Base Address Register, Memory Base Register 등으로 불림  

### MMU의 작동 방식

![MMU 작동 방식](/Resources/Images/MMU%20작동%20방식.webp)  

1. CPU가 명령어를 실행하면서 메모리 참조를 위한 논리적 주소를 생성한다.
2. MMU는 이 주소가 유효한 범위 내에 있는지 확인한다.
3. 범위를 벗어나면, 인터럽트(메모리 보호 위반)를 발생시킨다.
4. 범위 내라면, `Relocation Register` 값을 더해, 물리적 주소를 계산한다.

$$ \mathrm{Physical \ Address} = \mathrm{Logical \ Address} + \mathrm{Relocation \ Register}$$  

5. 계산된 물리적 주소로 메모리에 접근한다.  

- 이러한 메커니즘을 통해, 프로세스는 다른 프로세스의 메모리 영역에 접근하지 못하도록 보호됨  

### 고급 하드웨어 지원

- 현대 컴퓨터 시스템에는 더 복잡한 메모리 관리를 위한 하드웨어도 지원
- 아래 내용은 아직 공부하는 단계에선 어려우니, 가볍게 이런 게 있구나 하고 넘어가도 됨  

> <b>📌TLB(Translation Lookaside Buffer)</b>  
> - 최근에 변환된 주소를 캐싱하여 주소 변환 속도를 향상시키는 캐시 메모리  
> - [페이징 시스템](/OS/Paging-System.md)에서 주소 변환 오버헤드를 줄이는 데 중요한 역할을 함  

> <b>📌페이지 테이블 레지스터(Page Table Register)</b>  
> - 페이지 테이블의 시작 주소를 가리키는 레지스터  
> - 페이징 시스템에서 가상 주소를 물리적 주소로 변환하는 데 사용됨  

> <b>📌세그먼트 테이블 레지스터(Segment Table Register)</b>  
> - 세그먼트 테이블의 시작 주소를 가리키는 레지스터  
> - [세그먼테이션 시스템](/OS/Segmentation-System.md)에서 가상 주소를 물리적 주소로 변환하는 데 사용됨  
