# 1. 도커 이미지(Docker Image)

![도커 이미지와 케이크 상자](/Resources/Images/Docker%20Image%20예시.png)  

- 도커 이미지는 <b>애플리케이션과 그 애플리케이션을 실행하는 데 필요한 모든 것을 포함하는 패키지</b>
- 프로그램 실행에 필요한 코드, 런타임, 라이브러리, 환경 변수 및 구성 파일 등을 하나로 묶어둔 것이라고 생각하면 됨
- 도커 이미지는 <b>"케이크 레시피와 모든 재료를 담은 상자"</b>에 비유할 수 있음
- 이 상자에는 다음과 같은 것들이 포함되어 있고, 이 상자만 있으면 <b>어디서든 정확히 같은 케이크를 구울 수 있음</b>  

> <b>🍰케이크 상자 구성품</b>  
> - 레시피(실행 방법)
> - 밀가루, 설탕, 달걀(코드와 파일들)
> - 오븐 온도 설정(환경 설정)
> - 케이크틀(구동 환경)  

- 마찬가지로 도커 이미지만 있으면, <b>어떤 컴퓨터에서든 동일한 환경에서 애플리케이션을 실행할 수 있음</b>  

<br>

# 2. 도커 이미지의 구조
### 레이어(Layer) 개념 이해하기

![도커 이미지 레이어(맘에 들진 않지만,,)](/Resources/Images/Docker%20Image%20Layer%20예시.png)  

- 도커 이미지 레이어는 <b>투명 OHP 필름</b>을 겹쳐 놓은 것과 비슷함

> - 첫 번째 필름에는 기본 운영체제가 그려져 있음  
> - 두 번째 필름에는 프로그래밍 언어 설치가 추가됨  
> - 세 번째 필름에는 라이브러리 설치가 추가됨  
> - 네 번째 필름에는 애플리케이션 코드가 추가됨  

- 모든 필름을 겹쳐놓으면 완성된 도커 이미지가 됨
- 이러한 레이어 구조는 <b>효율적인 저장과 전송을 가능하게 해 줌</b>  

> <b>📌레이어의 특징</b>  
> 1. <b>읽기 전용(Read-only)</b>: 이미지의 모든 레이어는 변경할 수 없다.  
> 2. <b>증분적(Incremental)</b>: 각 레이어는 이전 레이어에서 변경된 부분만 저장합니다.  
> 3. <b>공유 가능(Shareable)</b>: 여러 이미지가 동일한 기본 레이어를 공유할 수 있다.  

<br>

# 3. 도커 이미지 식별자  

- 도커 이미지는 <b>이름</b>, <b>태그</b>, <b>다이제스트</b>로 식별됨  

```java
이름: nginx
태그: latest
전체 참조: nginx:latest
다이제스트: nginx@sha256:123abc...
```  

### 태그(Tag)

- 태그는 이미지와 특정 버전을 식별하는 레이블  

> `ubuntu:20.04` - Ubuntu 버전 20.04  
> `node:14-alpine` - Node.js 14 버전의 Alpine Linux 기반  
> `gninx:latest` - Nginx의 최신 버전  

### 다이제스트(Digest)

- 다이제스트는 이미지 내용의 `SHA256` 해시값으로, 이미지를 고유하게 식별함  

```java
nginx@sha256:4c0fdbb800e8b0d0f64c11a81497684b8b314c3a0d91658e1319ecb640a0e0ed
```  

- 다이제스트는 이미지 내용이 정확히 일치함을 보장하므로, 프로덕션(Production) 환경에서는 태그보다 다이제스트를 사용하는 것이 더 안전  

<br>

# 4. 도커 이미지 관리 TIP

- 이미지 받고, 리스트 및 상세 확인, 제거, 빌드 이러한 내용은 공식 문서에 잘 나와있으니 참고
- 여기서는 알면 좋을 만한 내용만 구성하였음  

### 실행 중인 컨테이너에서 이미지 만들기

- 컨테이너의 현재 상태를 이미지로 저장

```bash
# 컨테이너 ID 또는 이름으로 이미지 생성
docker commit my_container myapp:2.0
```  

### 다른 이미지에서 복사하기

- 기존 이미지를 새 이름으로 복사  

```bash
docker tag nginx:latest mynginx:1.0
```  

<br>

# 5. 이미지 최적화하기
### 1) 가벼운 베이스 이미지 사용하기

```dockerfile
# 기본 Node.js 이미지 (약 900MB)
FROM node:14

# Alpine 기반 이미지 (약 110MB)
FROM node:14-alpine
```  

- 큰 용량의 도커 이미지는 `pull`에 많은 시간을 소요
- 가능하면 `Apline`이나 `slim` 버전의 이미지를 사용할 것  

### 2) 다단계 빌드(Multi-stage build) 활용하기

```dockerfile
# 빌드 단계
FROM node:14 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# 실행 단계
FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```  

- 빌드 환경과 실행 환경을 분리하여 최종 이미지 크기를 줄이기
