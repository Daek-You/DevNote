# 1. Node.js

<img src="https://nodejs.org/en/next-data/og/announcement/Node.js%20%E2%80%94%20%EC%96%B4%EB%94%94%EC%84%9C%EB%93%A0%20JavaScript%EB%A5%BC%20%EC%8B%A4%ED%96%89%ED%95%98%EC%84%B8%EC%9A%94">  

- `Node.js`는 자바스크립트 코드를 <b>브라우저 밖에서 실행할 수 있게 해주는 런타임 환경</b>
- 빈번한 I/O 처리에 있어서의 우수한 성능, 서버 확장의 용이성, 무엇보다도 자바스크립트라는 프론트엔드 필수 언어로 백엔드까지 작성할 수 있다는 엄청난 장점 때문에 출시 이후로 빠르게 점유율을 높여가고 있음
- 2009년 5월 27일에 처음 출시되었으며, 오픈소스 자바스크립트 엔진인 `Chrome V8`에 비동기 이벤트 처리 라이브러리인 `libuv`를 결합하여 구현되었음

### 설치하기

> <b>🔧Windows 11</b>  

1. [Node.js Download](https://nodejs.org/ko)에 접속하여 `LTS` 버전을 받는다.  
2. 다운받은 설치 파일을 실행하여 `Node.js`를 설치한다.  
3. 설치가 완료되면 `CMD`나 `Windows PowerShell`을 열고, `Node.js` 버전 확인 명령어를 쳐서 잘 설치됐는지 확인한다.  

```bash
# // 또는 node -v
node --version

# example output: v22.14.0
```  

4. `Node.js`가 설치되었으면, 이제 [NPM(Node Package Manager)](https://namu.wiki/w/npm)을 이용하여 <b>타입스크립트 컴파일러</b>를 설치합니다. 터미널에서 다음 명령어를 실행하여 전역(Global)로 설치합니다.  

> <b>❌`Windows PowerShell`에서 `npm` 명령어 실행 오류가 나는 경우</b> 👉 [🔗해결 방법](https://sugook.tistory.com/177)  
> 근데 과정이 좀 번거로우니, `Git bash` 써서 진행합시다. (Git bash는 오류 안 남)

```bash
npm install -g typescript
```  

- 타입스크립트 설치가 정상적으로 완료됐다면, 다음 명령어를 통해 버전이 잘 나오는지 확인  

```bash
tsc -v
```  

<br>

# 2. 간단한 타입스크립트 예제 프로젝트 만들기

> <b>사용 IDE</b>: [Visual Studio Code](https://code.visualstudio.com/)  

1. 프로젝트로 지정할 디렉토리로 이동하여, 다음 명령어를 실행한다.  

```bash
tsc --init
```  

- 해당 명령어를 실행하고 나면, 디렉토리 안에 `tsconfig.json` 파일이 생성된 걸 볼 수 있다.
- `tsconfig.json`은 타입스크립트 프로젝트의 심장부라고 볼 수 있으며, <b>타입스크립트 컴파일러에게 "이 프로젝트를 어떻게 처리해야 할지"에 대한 상세한 지침을 제공</b>
- 타입스크립트는 우리가 작성한 `.ts` 파일을 브라우저나 `Node.js`가 이해할 수 있는 자바스크립트로 변환해야 함
- 이 변환 과정에서 어떤 버전의 자바스크립트로 변환할지, 어떤 파일을 포함하고 제외할지, 얼마나 엄격하게 타입 검사를 할지 등 수많은 결정이 필요
- `tsconfig.json`은 이런 모든 설정을 한 곳에 모아둔 <b>환경설정 파일</b>

2. `helloTypeScript.ts` 파일을 만들어 간단하게 작성해본다.  

```ts
function hello(name: string): string {
    return `Hello, ${name}!`;
}

console.log(hello("TypeScript"));
```  

3. 터미널에 `tsc`를 입력하여 프로젝트를 전체 빌드한다. 빌드하고 나면, `outDir`로 지정된 경로에 변환된 자바스크립트 파일들이 생성된다.
4. 변환된 자바스크립트를 `Node.js`를 활용하여 실행해본다.  

```bash
node dist/helloTypeScript.js

# Hello, TypeScript!
```  
