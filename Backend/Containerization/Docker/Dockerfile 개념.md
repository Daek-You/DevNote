# 1. 도커 파일(Dockerfile): "설계도"

![Dockerfile](/Resources/Images/Dockerfile.png)  

- `도커 파일(Dockerfile)`은 [도커 이미지(Docker Image)](/Backend/Containerization/Docker/Docker%20Image.md)를 만들기 위한 간단한 <b>텍스트 파일</b>
- 요리 레시피처럼, <b>도커 이미지를 어떻게 만들어야 하는지 단계별로 적어놓은 파일</b>이라고 생각하면 됨
- 프로그래밍을 공부했다면, 소스 코드로 프로그램을 만드는 과정을 알 거라 생각함
- 도커 파일도 비슷한 개념으로, 이 <b>파일에 적힌 명령어들을 도커가 하나씩 실행</b>하면서, 최종적으로 `이미지(Image)`라는 결과물을 만들어 냄
- 예를 들어, <b>"나는 Ubuntu를 기반으로 하고, Python을 설치하고, 내 프로그램 코드를 넣어서 실행하는 이미지를 만들고 싶어"</b>라는 요구사항을 도커 파일로 표현할 수 있음

<br>

# 2. 기본 문법과 주요 명령어

- 도커 파일은 각 줄마다 <b>하나의 명령어</b>와 그 <b>옵션</b>들로 구성되어 있음
- 대소문자를 구별하지 않지만, <b>가독성을 위해 명령어는 대문자로 쓰는 것</b>이 관례

### 1) `FROM`: 기반이 되는 도커 이미지 지정

```dockerfile
FROM ubuntu:20.04
```  

- 여기서 `ubuntu:20.04`는 Ubuntu 20.04 버전을 기반으로 한다는 의미
- 다양한 기반 이미지(Node.js, Python, Java 등)들을 [도커 허브(Docker Hub)](/Backend/Containerization/Docker/Docker%20Hub.md)에서 찾을 수 있음
- 하지만 모든 기반 이미지가 도커 허브에 있는 것은 아니며, <b>없을 경우에는 도커 파일을 통해 직접 이미지로 만들어서 사용</b>해야 함
    - 예를 들어어, `Nginx`, `MySQL`과 같은 프로그램들은 공식 사이트에서 이미지를 제공해주지만, 우리가 만드는 프로젝트(SpringBoot, Express.js 등)는 고유하므로 우리가 직접 도커 파일로 작성해야 함  

### 2) `WORKDIR`: 작업 디렉토리 설정

```dockerfile
WORKDIR /app
```  

- `WORKDIR`은 이후에 나올 모든 <b>명령어들이 실행될 디렉토리를 지정</b> 👉 리눅스(Linux)의 `cd` 명령어와 비슷
- 위 코드를 예시로 들면, `WORKDIR /app` 이후의 모든 명령어들은 도커 컨테이너 내부의 `/app` 디렉토리에서 실행됨

### 3) `COPY`: 파일 복사하기

```dockerfile
# 현재 디렉토리의 모든 파일을 WORKDIR에서 지정한 컨테이너 내부 디렉토리로 복사
COPY . .
```  

- 내 컴퓨터의 파일을 도커 이미지 안으로 복사하는 명령어
- 첫 번째 `.`은 <b>"현재 디렉토리의 모든 파일"</b>, 두 번째 `.`은 <b>"현재 작업 디렉토리(WORKDIR로 지정한 곳)"</b>을 의미
- 만약 특정 파일만 복사하고 싶다면, 다음과 같이 진행  
    ```dockerfile
    COPY app.py .
    COPY requirements.txt .
    ```  

### 4) `RUN`: 명령어 실행하기

```dockerfile
RUN apt-get update
RUN apt-get install -y python3 python3-pip
```  

- 이미지를 만드는 과정에서 실행할 명령어를 지정
- 패키지 설치, 파일 생성 등의 작업에 사용됨
- 여러 명령어를 하나로 묶으면 이미지 크기를 줄일 수 있음  
    ```dockerfile
    RUN apt-get update && apt-get install -y python3 python3-pip
    ```  

### 5) `ENV`: 환경 변수 설정하기

```dockerfile
ENV PORT=8080
ENV APP_HOME=/app
```  

- 컨테이너 내에서 사용할 환경 변수를 설정

### 6) `EXPOSE`: 사용할 포트 번호 알려주기

```dockerfile
EXPOSE 8080
```  

- 컨테이너가 어떤 포트를 사용할 지에 대해 도커에게 알려줌
- <b>실제로 포트를 열지는 않고, 문서화 역할만 수행</b>  

### 7) `CMD`: 컨테이너 실행 명령어

```dockerfile
CMD ["python3", "app.py"]
```  

- 컨테이너가 시작될 때 실행할 명령어를 지정
- 도커 파일에서 마지막에 한 번만 사용하는 것이 일반적
- 배열 형태로 작성하는 것이 권장됨: 첫 번째 요소는 실행할 명령어, 나머지는 해당 명령어의 인자들
- ‼️<b>중요</b>: `docker run` 명령어에서 추가 인자를 제공하면 CMD 명령어를 덮어씀
    - 여러 개의 CMD가 있다면 마지막 CMD만 적용됨

### 8) `ENTRYPOINT`: 애플리케이션 실행 명령어

```dockerfile
ENTRYPOINT ["python3", "app.py"]
```  

- `CMD`와 유사하지만 중요한 차이점이 존재
- `ENTRYPOINT`는 컨테이너가 시작될 때 <b>반드시 실행되는 명령어</b>
- `docker run` 명령어에서 추가 인자를 제공해도 덮어쓰지 않고, 대신 그 인자들이 `ENTRYPOINT`에 추가됨
- 배열 형태(`["명령어", "인자1", "인자2"]`)로 작성하는 것을 권장
- `CMD`와 `ENTRYPOINT`를 함께 사용할 때,
    - `ENTRYPOINT`는 실행할 실제 명령어를 정의
    - `CMD`는 기본 인자를 제공하며, 이는 `docker run` 시 재정의 가능   

```dockerfile
ENTRYPOINT ["python3"]
CMD ["app.py"]
```  

- 위 예제에서,
    - 컨테이너는 항상 `python3` 명령어로 시작
    - 기본적으로 `app.py`를 실행
    - 만약 `docker run myimage script.py`로 실행하면, `app.py` 대신 `script.py`가 실행됩니다
- <b>컨테이너가 항상 특정 애플리케이션으로 실행되게 하려면 `ENTRYPOINT`를, 기본 설정을 제공하되 필요시 변경 가능하게 하려면 `CMD`를 사용</b>  

<br>

# 3. 간단한 실습: Python 웹 어플리케이션

- 당신이 간단한 Flask 웹 어플리케이션을 개발하고 있고, 이를 도커 컨테이너로 실행하고 싶다고 가정
- 그러기 위해선 우선 도커 이미지를 만들어야 하고, 도커 이미지를 만들기 위해선 먼저 <b>도커 파일을 작성해야 함</b>

![Flask 도커 파일 작성 예제](/Resources/Images/dockerfile%20예제.png)  

```dockerfile
# Python 3.9 이미지를 기반으로 합니다
FROM python:3.9-slim

# 작업 디렉토리를 /app으로 설정합니다
WORKDIR /app

# 현재 디렉토리의 requirements.txt 파일을 컨테이너의 /app에 복사합니다
COPY requirements.txt .

# 필요한 패키지를 설치합니다
RUN pip install --no-cache-dir -r requirements.txt

# 나머지 소스 코드를 컨테이너로 복사합니다
COPY . .

# 환경 변수를 설정합니다
ENV PORT=5000

# 5000번 포트를 사용한다고 알려줍니다
EXPOSE 5000

# 컨테이너가 시작될 때 실행할 명령어입니다
CMD ["python", "app.py"]
```  

- 작성한 도커 파일을 빌드해서 도커 이미지로 만들기

```bash
# 이미지 빌드 (현재 디렉토리의 Dockerfile을 사용)
# -t는 이미지의 이름(태그)을 지정하는 옵션
docker build -t myapp .
```  

- 빌드한 도커 이미지를 컨테이너로 실행하기

```bash
# -p 5000:5000은 호스트의 5000번 포트와 컨테이너의 5000번 포트를 연결하는 옵션
docker run -p 5000:5000 myapp
```

<br>

# 4. TIP
### 1) `.dockerignore` 파일 사용하기

```dockerignore
# .dockerignore 예시
__pycache__/
*.pyc
.git/
.env
node_modules/
```  

- `.dockerignore` 파일을 사용하며 도커 이미지에 포함시키지 않을 파일을 지정할 수 있음
- 이는 [Git](https://namu.wiki/w/Git)의 `.gitignore`와 유사  

### 2) 레이어 최소화하기

```dockerfile
# 좋지 않은 예
RUN apt-get update
RUN apt-get install -y python3
RUN apt-get install -y python3-pip

# 좋은 예
RUN apt-get update && apt-get install -y python3 python3-pip
```  

- 도커 이미지는 여러 레이어(Layer)들로 구성됨
- 도커 파일에서 각 명령어(`FROM`, `RUN`, `COPY` 등)는 새 레이어를 생성함
- 레이어 수를 줄이면 이미지 크기와 빌드 시간을 줄일 수 있음  

### 3) 캐시 활용하기

```dockerfile
# 좋은 예: 종속성 파일을 먼저 복사하고 설치
COPY requirements.txt .
RUN pip install -r requirements.txt

# 소스 코드는 나중에 복사 (자주 변경됨)
COPY . .
```  

- 도커는 빌드 과정에서 캐시를 사용함
- <b>변경이 적은 레이어를 먼저 배치</b>하면, 빌드 시간을 단축할 수 있음
- 위 예제 코드와 같이 배치하면, 코드가 변경되더라도 패키지 설치 과정은 캐시에서 가져와 빌드 시간을 단축할 수 있음  

### 4) 공식 이미지 사용하기

- 가능하면 공식 도커 이미지를 사용할 것
- 이들은 최적화가 잘 되어 있고, 보안 업데이트를 정기적으로 지원해주기 때문  
