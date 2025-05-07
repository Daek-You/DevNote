# 1. [WSL2](https://learn.microsoft.com/ko-kr/windows/wsl/about)란?

<img src="https://www.datocms-assets.com/104397/1708639789-step-by-step-procedure-to-install-wsl2-on-windows-and-run-ubuntu-on-windows-using-wsl2.jpeg?auto=format" width="1200">  

- `WSL(Windows Subsystem for Linux)2`는 <b>Windows OS 위에서 "실제 Linux 커널"을 구동</b>할 수 있게 해주는 Microsoft 공식 기능
- [Docker Desktop for Windows](/Backend/Containerization/Docker/Docker%20Desktop.md)가 내부적으로 Linux 컨테이너를 실행하기 위해 이 커널을 필요로 함
- 따라서, Windows 11에 `Docker Desktop`을 설치하기 전에, 먼저 <b>WSL2를 활성화해두어야 함</b>

## [WSL vs. WSL2](https://learn.microsoft.com/ko-kr/windows/wsl/compare-versions)
### 📌WSL

- `WSL(Windows Subsystem for Linux)`는 Windows 10부터 도입된 기능
- Windows 커널 위에서 <b>Linux 바이너리(ELF 형식)</b>를 직접 실행할 수 있게 해줌
- `WSL1`은 Windows 커널의 시스템 호출(`syscall`)을 <b>Linux 호환 호출로 번역하는 방식</b>으로 동작했으나, 모든 Linux 기능을 완벽히 지원하지는 못하였음

### ✍️WSL2

> <b>✅실제 Linux 커널 사용</b>  
> - WSL2는 Hyper-V 하이퍼바이저 기반의 경량 가산 머신을 내부에 띄워, "Microsoft가 배포하는 Linux 커널"을 직접 구동  
> - 따라서, 완전한 시스템 호출 호환성과 더 빠른 I/O 성능을 자랑  

> <b>✅향상된 파일 시스템 성능</b>  
> - 특히 Linux 파일 시스템(ext4 등)과 Windows 파일 시스템 간의 작업(ex. 많은 작은 파일을 읽고 쓰기) 속도가 WSL1 대비 최대 20배까지 빨라졌음  

> <b>✅자동 업데이트</b>  
> - Windows 업데이트와 별도로 `wsl --update` 명령으로 커널을 최신 버전으로 유지 가능  

### [주요 특징 요약 비교](https://learn.microsoft.com/ko-kr/windows/wsl/compare-versions)

| 구분 | WSL1 | WSL2 |
| --- | --- | --- |
| <b>커널</b> | Windows 커널 위 syscall 번역 계층 | 실제 Linux 커널(가상 머신) |
| <b>파일 I/O</b> | 느림 | 매우 빠름 |
| <b>호환성</b> | 일부 기능 제한 | 완전한 시스템 호출 호환 |
| <b>메모리</b> | Windows 프로세스처럼 경량 | 경량 VM이지만 동적으로 메모리 할당 |  

<br>

# 2. WSL2 설치

### Windows 11

- `Windows PowerShell`을 관리자 모드로 실행하여, 다음 한 줄의 명령어만 실행하면 된다.  

```powershell
wsl --install
```  
- 이 명령어 한 줄에는 다음과 같은 흐름이 자동으로 처리된다.  
    1. "Windows Subsystem for Linux" 기능 활성화
    2. "Virtual Machine Platform" 기능 활성화
    3. 최신 Linux 커널 패키지 설치
    4. WSL2를 기본 버전으로 설정
    5. Ubuntu 배포판(기본값) 설치
- 이후에 다음 명령어를 통해 설치가 잘 됐는지 확인하면 된다.  

```powershell
wsl -l -v
```  

- GUI 앱 실행, GPU 가속 등 WSL2 최신 기능이 Windows 11에서 기본으로 지원된다.

#### ❌[ERROR] Wsl/CallMsi/REGDB_E_CLASSNOTREG  

1. 다음 명령어를 통해 WSL 기능과 가상 머신 기능을 활성화 및 재부팅  

```powershell
# WSL(Windows Subsystem for Linux) 기능 활성화
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

# 가상 머신 플랫폼(Virtual Machine Platform) 기능 활성화
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```  

2. [Microsoft WSL Releases](https://github.com/microsoft/WSL/releases)에서 최신 버전을 받아 설치  
3. 다시 `wsl --install` 진행  

<br>

### Windows 10

> ✅`Windows 10 Version 2004(Build 19041)` 이상이라면, `Windows 11`과 똑같이 진행하면 된다.  

1. `Windows PowerShell`을 <b>관리자 권한</b>으로 실행한다.  
2. `PowerShell` 관리자 창에서 다음 두 명령어를 순서대로 실행해, Windows의 선택적 기능을 켠다.  

```powershell
# WSL(Windows Subsystem for Linux) 기능 활성화
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

# 가상 머신 플랫폼(Virtual Machine Platform) 기능 활성화
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```  

3. 기능 활성화 명령이 완료되면, <b>반드시 Windows를 재부팅</b>해야 한다. (그래야 변경 사항이 적용됨)  
4. 재부팅을 완료했다면, 다시 `PowerShell`을 <b>관리자 권한</b>으로 실행하여 아래 명령어를 실행한다.  

```powershell
# WSL 커널을 최신 버전으로 업데이트
wsl --update

# 새로 설치될 배포판의 기본 WSL 버전을 2로 설정
wsl --set-default-version 2
```  

5. Ubuntu 배포판을 설치한다.  

```powershell
# 배포판 설치치
wsl --install -d Ubuntu

# 설치된 배포판 및 WSL 버전 확인
wsl -l -v
```  

#### ❌"[`ERROR`] 가상 머신 플랫폼을 사용할 수 없습니다."
> - 시스템 BIOS/UEFI 설정에서 다음 옵션을 반드시 켜야 한다.  
> - Intel CPU: `Intel VT‑x(Intel Virtualization Technology)`  
> - AMD CPU: `AMD‑V(AMD Virtualization)`  
