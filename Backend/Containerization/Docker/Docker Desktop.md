# 1. Docker Desktop

<img src="https://www.docker.com/app/uploads/2023/07/docker-desktop-421_f2-1110x653.png" width="1200">  

- Windows 환경에서 [도커 컨테이너](/Backend/Containerization/Docker/Docker%20Container.md)를 쉽고 빠르게 관리할 수 있게 해주는 올인원 패키지
- Docker Engine, CLI, GUI 대시보드, 쿠버네티스(Kubernetes) 지원 등을 통합하여 제공
- Windows에서는 내부적으로 [WSL2](/OS/WSL2.md)를 백엔드로 활용해 Linux 컨테이너를 실행하며, `PowerShell`이나 `CMD`, 또는 `WSL Shell`에서 `docker` 명령어를 바로 사용할 수 있음
- 설치 과정은 공식 웹사이트에서 설치 파일을 내려 받아 관리자 권한으로 실행하는 매우 간단한 방식이며, 설치 후 자동으로 WSL2 엔진 설정과 배포한 통합을 처리해줌

### 구성 요소
- <b>`Docker Engine`</b>: 컨테이너 런타임 엔진으로, 이미지 빌드와 컨테이너 실행을 담당
- <b>`Docker CLI`</b>: docker 명령어를 통해 이미지, 컨테이너, 네트워크, 볼륨 등을 명령줄에서 관리
- <b>`Dashboard (GUI)`</b>: 컨테이너, 이미지, 볼륨, 네트워크 상태를 시각적으로 모니터링하고 제어할 수 있는 그래픽 인터페이스를 제공
- <b>`Kubernetes (선택적)`</b>: 내장 쿠버네티스 클러스터를 활성화해 마이크로서비스를 로컬에서 테스트할 수 있음
- <b>`WSL 2 백엔드`</b>: Windows에서는 WSL 2 커널 위에서 Linux 컨테이너가 구동되므로, 가볍고 빠른 성능을 발휘  

<br>

# 2. 설치
### 📌시스템 요구사항

- <b>운영 체제</b>: `Windows 10 (Build 19041 이상)` 또는 `Windows 11 Pro/Enterprise/Education` 에디션  
- <b>가상화 지원</b>: BIOS/UEFI에서 `IntelVT-x` 또는 `AMD-V` 활성화 필요  
- <b>하드웨어</b>: `4GB` 이상 메모리, `64-bit CPU` 권장  
- [<b>WSL2</b>](/OS/WSL2.md): 사전 설치 및 WSL2 기본 버전 설정 필요  

### 🖥️설치 방법

1. [Docker Desktop 공식 페이지](https://www.docker.com/products/docker-desktop/)에서 Windows용 설치 프로그램(`Docker Desktop Installer.exe`)을 다운로드한다.  
2. 다운로드한 설치 파일을 <b>관리자 권한</b>으로 실행하여 설치를 진행한다.  
3. 설치가 완료되면 로그아웃(시스템 재부팅)을 하라는 메시지가 나타나는데, 그대로 진행  
4. 재부팅 후에는 버전 확인 명령어로 테스트  

```powershell
docker --version
```  

5. 테스트 컨테이너를 실행해서 정상적으로 잘 작동하는지 확인  

```powershell
docker run hello-world
```  
