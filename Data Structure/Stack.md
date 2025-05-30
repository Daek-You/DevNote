# 1. 스택(Stack)

<img src="https://user-images.githubusercontent.com/50010735/234495371-1c7f0c5d-8717-4da3-babd-8ced13c37583.png" width="1200">  

- 스택은 <b>후입선출(LIFO, Last-In-First-Out)</b> 원칙을 따르는 선형 자료구조
- 이는 가장 마지막에 들어간 데이터가 가장 먼저 나오고, 가장 먼저 들어간 데이터가 가장 나중에 나온다는 의미
- 스택은 <b>접시를 쌓아두는 모습</b>과 유사하며, 새로운 접시는 항상 맨 위에 쌓이고 접시를 꺼낼 때도 맨 위에서부터 꺼내게 됨  

### 스택의 주요 특징

> ℹ️스택에 쌓여 있는 데이터 중 가장 마지막 데이터를 `top`이라고 부릅니다.  

1. 데이터의 삽입(`push`)과 삭제(`pop`)가 항상 한 쪽 끝(스택의 `top`)에서만 이루어진다.
2. 스택의 중간에 있는 데이터에 직접 접근할 수 없다.
3. 스택의 맨 위 요소만 확인 가능하며, 이를 `peek` 연산이라고 한다.  

### 스택의 기본 연산

<img src="https://media.geeksforgeeks.org/wp-content/cdn-uploads/20230726165552/Stack-Data-Structure.png" width="1200">  

1. `Push`: 스택의 맨 위에 데이터를 추가하는 연산
2. `Pop`: 스택의 맨 위에서 데이터를 제거하고 반환하는 연산
3. `Peek/Top`: 스택의 맨 위 데이터를 제거하지 않고 확인하는 연산
4. `isEmpty`: 스택이 비어있는지 확인하는 연산
5. `isFull`: 스택이 가득 찼는지 확인하는 연산 (배열 기반 구현 시 사용)  

### 스택의 활용 사례

> <b>📌함수 호출과 프로그램 실행</b>  
> - 함수 호출 시, 함수의 매개변수와 지역변수, 반환 주소 등이 스택에 저장된다.  
> - 재귀 함수 호출 시, 각 함수의 상태가 스택에 쌓인다.  

> <b>📌수식 계산 및 구문 분석</b>  
> - [후위 표기식(Postfix notation)](/Data%20Structure/PostfixNotation.md) 계산에 활용된다.  
> - 컴파일러의 구문 분석 과정에서 괄호 매칭, 연산자 우선순위 처리에 사용된다.  

> <b>📌실행 취소(Undo) 기능</b>  
> - 텍스트 에디터나 그래픽 소프트웨어의 실행 취소 기능은 스택으로 구현된다.  
> - 사용자의 모든 작업을 스택에 저장하고, 실행 취소 시 가장 최근 작업부터 되돌린다.  

> <b>📌웹 브라우저 방문 기록</b>  
> - 웹 브라우저의 "뒤로 가기" 기능은 스택을 이용해 구현된다.  

> <b>📌깊이 우선 탐색(DFS, Depth-First Search)</b>  
> - [그래프(Graph)](/Data%20Structure/Graph%20Overview.md)나 [트리(Tree)](/Data%20Structure/Tree%20Overview.md) 구조에서 깊이 우선 탐색을 구현할 때 스택이 사용된다.  

<br>

# 2. 스택의 구현 방법
### [배열](/Data%20Structure/Array.md)을 이용한 구현

![스택 배열 구현](/Resources/Images/스택%20배열%20구현.png)  

- 배열 기반 스택은 구현이 간단하고 접근 속도가 빠름
- 배열이기 때문에 스택 생성 시, 최대 크기를 미리 지정해야 함
- 크기가 고정되어 있어, 오버플로우 문제가 발생할 수 있음
- 스택이 가득 차면, 크기를 동적으로 조정해야 함 (배열 재할당)  

> <b>🍳배열 기반 스택 구현에 필요한 요소</b>  
> 1. <b>배열(Array)</b>: 실제 데이터를 저장할 배열  
> 2. <b>용량(Capacity)</b>: 스택이 저장할 수 있는 최대 요소 수  
> 3. <b>top 포인터</b>: 스택의 최상위 요소를 가리키는 인덱스  

### [연결 리스트](/Data%20Structure/Linked%20List.md)를 이용한 스택 구현

![연결리스트 구현](https://blog.kakaocdn.net/dn/dZoWKB/btq5g5oMG07/5trCuDegjPwHtB7kdTUmkk/img.png)  

- 연결 리스트 기반 스택은 크기 제한 없이 동적으로 확장할 수 있다는 장점을 보유
- 동적으로 메모리를 할당하기 때문에 크기 제한이 없음
- 오버플로우 걱정이 없고, 데이터 양에 따라 자연스럽게 크기가 조절됨
- 다음 노드에 대한 포인터가 필요하므로, 추가 메모리를 요구
- 배열 기반 구현보다 메모리 사용이 비효율적일 수 있음(추가 메모리를 요구하므로)  

> <b>🍳연결 리스트 기반 스택 구현에 필요한 요소</b>  
> 1. <b>노드(Node)</b>: 데이터와 다음 노드를 가리키는 포인터를 포함  
> 2. <b>top 포인터</b>: 스택의 맨 위 노드를 가리킴  
> 3. <b>크기(size)</b>: 현재 스택에 저장된 요소 수(선택적)  

<br>

# 3. 스택의 시간 복잡도

| 연산 | 시간 복잡도 | 설명 |
| --- | --- | --- |
| `Push` | $\mathrm{O(1)}$ | 스택의 맨 위에 요소를 추가하는 연산으로, 항상 일정한 시간이 소요됨 |
| `Pop` | $\mathrm{O(1)}$ | 스택의 맨 위에 요소를 제거하는 연산으로, 항상 일정한 시간이 소요됨 |
| `Peek` | $\mathrm{O(1)}$ | 스택의 맨 위에 요소를 확인하는 연산으로, 항상 일정한 시간이 소요됨 |
| `Search` | $\mathrm{O(N)}$ | 특정 요소를 검색하려면 최악의 경우, 모든 요소를 하나씩 pop해야 하므로 $\mathrm{O(N)}$ |  

### 데이터 크기를 알고 있는 경우 👉 배열 기반
- 입력 데이터의 크기를 미리 알고 있고, 그 크기가 크지 않다면 배열 기반 스택이 효율적
- 메모리 낭비를 최소화하고 캐시 효율성이 좋음

### 데이터 크기를 모르는 경우 👉 연결 리스트 기반
- 입력 데이터의 크기를 예측할 수 없거나 매우 큰 경우, 연결 리스트 기반 스택이 적합
- 동적 할당을 통해 필요한 만큼만 메모리를 사용

### 성능 고려
- 배열 기반 스택은 메모리 지역성이 좋아 캐시 히트율이 높고 일반적으로 더 빠름
- 연결 리스트 기반 스택은 메모리 할당/해제 오버헤드가 있어 상대적으로 느릴 수 있음

### 메모리
- 배열 기반 스택은 미리 할당된 공간이 있어 데이터가 적을 때 메모리 낭비가 발생할 수 있음
- 연결 리스트 기반 스택은 필요한 만큼만 메모리를 사용하지만, 포인터 저장을 위한 추가 메모리가 필요
- 연결 리스트는 각 노드가 개별적으로 동적 할당됨
    - 이는 각 노드가 메모리의 다른 위치에 할당될 수 있어 <b>노드들이 메모리 전체에 흩어질 수 있음</b>
    - 이런 비연속적 메모리 할당은 <b>메모리 단편화(특히 외부 단편화)</b>를 발생시킬 수 있음
- 반면, 배열은 연속된 메모리 블록을 사용하므로 이러한 단편화 문제가 적음
