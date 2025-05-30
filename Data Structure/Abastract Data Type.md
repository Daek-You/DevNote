
# 1. 추상 데이터 타입 (ADT, Abstract Data Type)

![ADT](https://www.hanbit.co.kr/data/editor/20231027090152_zyaymrta.png)  

- `ADT`는 데이터와 그 데이터를 다루는 연산들을 하나의 단위로 묶어 정의한 것
- 이는 프로그래밍에서 `"무엇을 할 수 있는가?"`와 `"어떻게 구현되는가?"`를 분리하는 중요한 개념

### ADT의 2가지 핵심 요소

> <b>1. 외부 인터페이스 (External Interface)</b>
> - 데이터 객체와 연산의 명세을 말함
> - 사용자가 접근할 수 있는 기능과 서비스를 정의
> - `"무엇을 할 수 있는지"`를 나타냄
> - ex) 자동차를 운전할 때 운전자는 핸들, 브레이크, 가속 페달 등의 인터페이스만 사용하면 됨
>   - 운전자가 차의 내부 엔진 작동 방식이나 전기 회로에 대해 알 필요 없이 운전이 가능

> <b>2. 내부 구현 (Internal Implementation)</b>
> - 데이터 객체의 내부 표현 양식과 연산의 구현 내용을 말함
> - 실제 코드 작성 방법와 알고리즘에 해당
> - 사용자로부터 은폐되는 부분
>   - ex) 자동차를 설계하고 만드는 엔지니어 입장에서는 차의 내부 엔진 작동 방식이나 전기 회로에 대해 알아야 제작 및 수리가 가능

<br>

# 2. 연산 명세

- 사용자가 데이터 타입을 어떻게 사용할 수 있는지를 정의한 명세서
- 가전 제품을 샀을 때, 이걸 사용하기 위해 어떻게 해야 하는 지에 대해 작성해놓은 가이드 설명서라고 비유할 수 있음
    - 설명서에는 사용법(외부 인터페이스)만 작성되어 있으며, 내부 구조 및 작동 원리(내부 구현)에 대해서는 작성해놓지 않음

### 연산 명세 구성 요소

1. <b>`함수의 이름`</b>: 직관적이고 목적을 명확히 나타내는 이름
2. <b>`매개변수 타입`</b>: 함수가 받을 입력 값의 타입
3. <b>`반환 타입`</b>: 함수가 반환할 결과의 타입
4. <b>`연산의 목적과 기능`</b>: 함수가 무엇을 하는지에 대한 설명

### 연산 명세의 특징
- <b>`추상화(Abstraction)`</b>: 내부 구현 세부사항은 숨기고 핵심 기능만 노출
- <b>`정보 은닉(Information Hiding)`</b>: 구현 세부사항은 사용자로부터 은폐
- <b>`인터페이스 안정성`</b>: 내부 구현이 변경되어도, <u>인터페이스는 그대로 유지!</u>✅

> <b>Q. 왜 정보 은닉을 하는 것일까?</b>  
> 1. <b>유지보수성 향상</b>: 내부 구현을 변경해도, 사용자 코드는 수정할 필요가 없음
> 2. <b>코드재사용성 증가</b>: 잘 정의된 인터페이스를 통해, 다양한 상황에서 재사용 가능
> 3. <b>디버깅 용이성</b>: 문제가 발생했을 때, 인터페이스와 구현을 분리하여 디버깅 가능
> 4. <b>협업 효율성</b>: 팀원들이 인터페이스 규약만 지키면 독립적으로 작업 가능

> <b>ℹ️정보 은닉의 좋은 점 예시: 스마트폰의 앱 업데이트</b>
> - 스마트폰 앱이 업데이트되면 내부 코드는 크게 변경될 수 있지만, <u>사용자 인터페이스(버튼, 메뉴 등)는 일관성이 유지</u>됨 👉 사용자는 새로 바뀐 내용에 대해 알 필요 없이 이전에 사용하던 방식 그대로 사용하면 됨 (사용자 경험 유지)
> - 이것이 ADT에서 말하는 "사용자에 대한 영향 최소화"의 실제 사례  

<br>

# 3. 추상 데이터 타입의 예: 자연수
### 1) 객체 및 연산의 명세 (외부 인터페이스)

> **객체 정의**  

- 자연수 타입은 0부터 시작하여, 컴퓨터로 표현할 수 있는 최대 정수(`INT_MAX`)까지 범위에 속하는 정수들의 집합

> **연산 명세**  

```cpp
NatNum Zero();                          // 0을 반환하는 함수
Boolean Is_Zero(NatNum x);              // x가 0인지 확인하는 함수
NatNum Add(NatNum x, NatNum y);         // x와 y를 더한 결과를 반환하는 함수
Boolean Equal(NatNum x, NatNum y);      // x와 y가 같은지 확인하는 함수
NatNum Successor(NatNum x);             // x의 다음 수(후속자)를 반환하는 함수
NatNum Subtract(NatNum x, NatNum y);    // x에서 y를 뺀 결과를 반환하는 함수
```  

### 2) 내부 표현 양식 및 구현 내용 (내부 구현)

- 실제 연산이 어떻게 구현되는지를 보여주는 부분
- 사용자들에게는 일반적으로 공개되지 않음

```cpp
NatNum Zero() { 
    return 0; 
}

Boolean Is_Zero(NatNum x) { 
    return x == 0; 
}

NatNum Add(NatNum x, NatNum y) { 
    // 오버플로우 방지: 합이 INT_MAX를 초과하면 INT_MAX 반환
    return x + y <= INT_MAX ? x + y : INT_MAX; 
}

Boolean Equal(NatNum x, NatNum y) { 
    return x == y; 
}

NatNum Successor(NatNum x) { 
    // 이미 최대값이면 그대로 반환, 아니면 1 증가
    return x == INT_MAX ? x : x + 1; 
}

NatNum Subtract(NatNum x, NatNum y) { 
    // 음수가 되지 않도록 방지
    return x < y ? 0 : x - y; 
}
```  

<br>

# 4. ADT의 중요성과 이점

1. <b>코드 모듈화</b>: 독립적인 모듈로 개발하여 전체 시스템의 복잡성을 감소시킬 수 있음
2. <b>재사용성</b>: 잘 정의된 ADT는 다양한 프로그램에서 재사용이 가능
3. <b>유지보수 용이성</b>: 구현 변경이 사용자 코드에 영향을 미치지 않음
4. <b>추상화 수준 향상</b>: 복잡한 세부사항을 숨기고 중요한 개념에 집중할 수 있음
5. <b>소프트웨어 안정성</b>: 인터페이스가 안정적이므로, 시스템 전체의 안정성을 향상시킬 수 있음

### 🔗References
- 자료구조론, 윤성우 저, 한빛미디어
- Data Structures and Algorithm Analysis in C++, Mark Allen Weiss
- [C++ Reference - Abstract Data Types](https://en.cppreference.com/w/cpp/language/classes)
