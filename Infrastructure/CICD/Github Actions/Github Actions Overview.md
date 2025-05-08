# Github Actions 구성 요소
### 1. 워크플로우 (Workflow)

<img src="https://docs.github.com/assets/cb-25535/mw-1440/images/help/actions/overview-actions-simple.webp">  

- <b>정의</b>: 자동화할 작업의 전체 프로세스를 정의한 파일. `.github/workflows` 디렉토리 안에 `YAML(.yml 또는 .yaml)` 형식으로 작성됨
- <b>역할</b>: 어떤 이벤트가 발생했을 때 실행될지, 어떤 잡들로 구성될지, 각 잡은 어떤 스텝들을 수행할지를 명시. AWS EC2 배포를 위한 전체 계획이 이 워크플로우 파일에 담김
- <b>예시</b>: 코드가 `main` 브랜치에 푸시될 때마다 실행되는 워크플로우를 정의

### 2. 이벤트 (Event)

- <b>정의</b>: 워크플로우 실행을 트리거(trigger)하는 특정 활동
- <b>역할</b>: 저장소에 코드를 푸시하거나, `Pull Request`가 열리거나, 특정 스케줄 시간이 되거나, 심지어 웹훅을 수신하는 등 다양한 이벤트가 워크플로우를 시작하게 할 수 있음. EC2 배포 워크플로우는 보통 `push` 이벤트나 `pull_request` 이벤트에 연결됨
- <b>예시</b>: `on: [push]`는 코드가 푸시될 때마다 워크플로우를 실행하라는 의미

### 3. 잡 (Job)
