## ✏️ Promise

- 정의 : 비동기 작업의 성공과 실패를 반환하는 객체
- 목적 : callback hell의 한계 극복, 가독성, 유지보수성
- 특징
    - callback + asynchronous
    - 하나의 실행에 따라 각각 오류처리를 했어야 하지만 오류 처리 한 번에 가능
        
        → callback hell 문제의 해결 (가독성 향상)
        
    - es6에서 도입
- 사용 방법
    
    ```jsx
    //생성
    const promise = new Promise((resolve, reject) => {
    	let id = 1;
    	
    	const result = getUser(id); //1
    	
    	if (result.succcess) {  //2
    		resolve(result.user);
    	} else {                //3
    		reject("실패하였습니다.");
    	}
    })
    
    //사용
    promise
    	.then((result) => console.log(result)) //4
    	.catch((error) => console.log(error)) //5
    ```
    
    1. pending
        - api 요청 후 응답이 올 때까지 대기라고 생각하면 됨
        - 이때 다른 코드는 먼저 실행될 수 있음
        
        → 비동기/논블로킹의 성질을 가짐
        
    2. fulfilled
        - 반환된 api(함수)의 응답 상태가 성공일 때 promise의 상태를 성공 상태로 변경
        - resolve는 콜백함수
        - 결과로 resolve의 인자를 전달(반환) → 4에서 출력
    3. rejected
        - 응답 상태가 실패일 때 promise의 상태를 실패로 변경
        - reject 또한 콜백함수
        - 결과로 reject의 인자를 전달(반환) → 5에서 출력

<br>

### ▶️ Promise 체인

- 정의 : then()을 사용하여 비동기 작업을 순차적으로 진행하는 패턴
- 목적
    - callback hell의 해결 = 가독성 향상
    - catch() 오류 처리 한 번에 가능

<br>

### ▶️ Promise의 상태

- 대기(pending)
- 성공(fulfilled)
    - resolve 함수 실행
    - 성공을 반환하여 then() 실행
- 실패(rejected)
    - reject 함수 실행
    - 실패 반환하여 catch() 실행

<br>

### ▶️ Promise.all() 사용법

- 정의 : promise를 배열로 묶어 여러 then()을 동시 실행
- (차이점 짚고 가기) promise chain : 순차 실행 vs all() : 병렬 실행
- 사용 방법
    
    ```jsx
    function fetchData(url) {
    	return new Promise((resolve, reject) => {
    		//암튼 구현
    	})
    }
    
    const promise1 = fetchData('/link1');
    const promise2 = fetchData('/link2');
    const promise3 = fetchData('/link3');
    
    Promise.all([promise1, promise2, promise3])
    .then((results) => {
    	console.log(results); //세 promise의 모든 결과 반환
    })
    .catch((error) => {
    	console.log(error); //가장 먼저 발생하는 오류를 반환
    })
    ```
<br>    

> **싱글 스레드 언어인 js에서 어떻게 동시 실행이 가능한 것인가?**
> 
- fetch는 web api 영역에 의해 병렬 처리가 맞지만 promise는 아님
- “동시성” + “논블로킹” → 사실상 “병렬” 처리가 아님 (큐에 등록하는 예약 과정 : 비동기 - 기다리지 않음)
- 이벤트 루프의 실행 흐름을 알 필요가 있음
    1. 자바는 인터프리터 언어이기 때문에 동기적으로 동작하는 코드는 콜스택에서 실행됨
    2. promise나 fetch().then()처럼 js엔진에서 처리되는 것, setTimeout과 fetch 처럼 web api 영역에서 처리되는 것들은 바로 반환되어 대기가 필요한 경우 각각 마이크로 태스크 큐와 태스크 큐에 등록 
    3. 읽을 코드가 남았는지와는 무관하게 콜스택이 비었을 때 즉시 마이크로 태스크 큐에 있는 작업들이 메인 스레드에서 실행됨
    4. 마이크로 태스크 큐가 비면 태스크 큐가 실행됨
    5. 1번으로 돌아가서 1 ~4의 과정을 반복하는 것이 이벤트 루프!

<br>

---

<br>

## ✏️ Async/Await - 비동기/논블로킹 처리와 연관지어 이해할것

- 정의
    - async : 함수를 비동기적으로 처리하며 프로미스를 항상 반환하기 위해 함수 앞에 선언하는 키워드
    - await : async 함수의 try문 안에 작성하여 promise를 반환받을 때까지 일시 중단시키는 키워드
- 목적 : 비동기 작업의 동기적 구조를 지원함으로써 코드의 가독성, 유지보수성 향상
- 특징
    - await를 만나면 async 함수 내부가 일시 중단되는 것이지 외부 처리들이 중단되는 것은 아님
    → 논블로킹
    - 동기적 코드여도 async 함수 안에 있으면서 await 이후에 있으면서 promise 반환이 되지 않은 경우에서는 전부 마이크로태스크 큐에 들어감
    - await → promise를 반환 “받는”
    - async → promise를 await로 반환받은 후 비동기 함수가 호출된 곳으로 반환”하는”
- 사용 방법
    
    ```jsx
    async function fetchData() {
    	const result = await legacyFn(); //-> 프로미스 반환 받아 result에 저장
    	//...~~
    }
    
    fetchData(); //-> fetchData가 프로미스를 반환하는 함수이며 호출 시 프로미스 반환 받음
    ```
    
<br>

---

<br>

## ✏️ 자바스크립트 인터프리터

**자바스크립트는 인터프리터 언어이다.**

- 정의 : 한 번에 한 줄씩 번역하여 실행하는 언어
- 특징
    - 컴파일러에 비해 느릴 수 있음
    - 실행하면서 오류가 바로 검출되지만 디버깅 시 추적이 어려울 수 있음
    - 한 줄씩 번역하기 때문에 메모리 사용이 비효율적 (리소스 사용 측면)
        - 하지만 자바스크립트 엔진으로 인해 JIT 컴파일러를 동작시키면서 컴파일러에 비해 느리긴 하지만 비효율일 것까진 없음
    - 즉시 실행시켜 바로 결과를 확인할 수 있어 개발 생산성 향상
    - 플랫폼 독립성 - OS에 의존적이지 않아 다양한 시스템에서 사용 가능
    - Python, Javascript, Ruby 등의 언어가 해당
- 참고
    - 자바는 인터프리터, 컴파일러 둘다 적용되는 하이브리드 언어

<br><br>

## ✏️ 자바스크립트 엔진

- 정의 : 자바스크립트 코드를 실행하는 프로그램 또는 인터프리터
- 구조
    - 힙
        - 객체, 함수 등이 저장됨
        - 렉시컬 환경도 저장
        - 가변 공간
        - 비교적 큰 값이 저장될 수 있음
    - 스택
        - 원시값, 실행 컨텍스트 저장
        - 힙의 객체, 함수의 주소값/참조값이 저장됨
        - 선입 선출 구조
        - 고정된 공간으로 용량 제한 있음
        - 메모리 할당/해제의 속도가 빠름
- 동작 과정
    1. 토큰화(파싱)
        - 토큰 : 코드에서 의미있는 최소 단위로 나눈 것
        - 종류
            - 구분자 : ;, (, {, , 등
            - 주석 : //(한 줄 주석), /**/(두 줄 이상 주석)
            - 연산자 : +, =, >, 등
            - 리터럴 : 숫자, 불리언, 문자열 등
            - 식별자 : 변수, 함수명 등
            - 키워드 : if, let, funciton, for, const, undefined 등
        - `let x = 5;` (gpt 결과)
            
            구조
            
            ```jsx
            0x001: [KEYWORD]    (let)  
            0x002: [IDENTIFIER] (x)  
            0x003: [OPERATOR]   (=)  
            0x004: [LITERAL]    (5)  
            0x005: [PUNCTUATION] (;)
            ```
            
            실제 결과
            
            ```jsx
            0010 1101 0000 0001
            0010 0010 0000 0010
            0011 0001 0000 0011
            0101 0000 0000 0100
            0110 0011 0000 0101
            ```
            
    2. AST 생성 (추상 구문 트리 생성)
        
        ```jsx
        [Program] → (포인터) → [VariableDeclaration(let)]  
          ├── [VariableDeclarator] → (포인터) → [Identifier(x)]  
          ├── [Literal(5)]
        ```
        
        - 토큰 간 관계 표현
        - 컴파일 시 오류 검출에 용이
    3. 실행 컨텍스트 생성
        - 실행 컨텍스트 : 자바스크립트 실행 시 생성되는 함수 정보 관리 환경
        - 변수, 함수, this바인딩 → 스택에 저장
        - 렉시컬 환경은 힙에 저장
    4. 렉시컬 환경, 스코프 결정
        - 변수, 함수의 선언 시점을 기준으로 고정된 스코프를 결정
    5. 호이스팅
        - 정의 : 변수와 함수를 파일 최상단으로 끌어올려 구조를 최적화하는 과정
        - 특징
            - 함수 선언문 → 제대로 올라감
            - `var` → undefined로 올라감
                - 재할당 / 재선언 가능
                - referrence error 없음
                
                원래 코드
                
                ```jsx
                a = 5;
                console.log(a);
                var a = 3; 
                var a = 0; 
                ```
                
                실제 실행 순서
                
                ```jsx
                var a;  // a가 undefined로 호이스
                a = 5;  // a에 5 할당 됨
                console.log(a); // 5 출력
                a = 3;  // a에 3 할당
                a = 0;  // var 선언 무시
                ```
                
            - `const`
                - TDZ zone → Reference error 발생
                - 위에 var에서 const, let으로 바꾸면 실행 안 됨
                - 재할당 불가능 / 재선언 불가능
                    
                    ```jsx
                    const a = 3;
                    a = 5; //불가능
                    const a = 1; //불가능
                    ```
                    
            - `let`
                - TDZ zone → Reference error 발생
                - 위에 var에서 const, let으로 바꾸면 실행 안 됨
                - 재할당 가능 / 재선언 불가능
                    
                    ```jsx
                    let a = 3;
                    a = 5; // 가능
                    let a = 1; //불가능
                    ```
                    
            - TDZ zone : `let`, `const` 선언 전까지 사용할 수 없는 스코프
    6. (실질적 실행 시작) 바이트 코드 생성
        
        `let x = 5;` 
        
        바이트 코드 결과
        
        ```jsx
        0x0001  LdaSmi 5
        0x0002  StaGlobal x
        0x0003  Return
        ```
        
    7. 변수 할당 및 실행
        - 정의 : 스코프 체인을 통해 변수를 할당하는 실질적 실행 과정
        - 목적 : 변수/함수 호출의 최적화
        - 스코프 체인
            - 현재 스코프에서 변수를 할당하고 없다면 상위 스코프에서 찾고 없다면 전역 스코프까지 찾고 없다면 Reference Error 반환
    - 토큰화 → AST 생성 → 실행 컨텍스트 생성 → 렉시컬 환경/스코프 설정 → 호이스팅 → (실행 시작) 바이트 코드 생성 → 변수 할당 및 실행(스코프 체인)

<br>

### ▶️ V8 엔진 내부 동작

토큰화 → AST 생성 → 바이트 코드 변환 → JIT 컴파일 → 최적화된 기계코드 생성 → 실행/GC

- Chrome, node.js 지원

<br>

### ▶️ JIT 컴파일러 역할

- 바이트 코드를 분석하여 자주 사용하는 코드 감지
- TurboFan을 이용하여 최적화된 기계코드로 변환
- 일반 인터프리터보다 빠르게 동작하도록 최적화
- 실행 중 과도한 최적화 제거 - Deoptimization

<br><br>

## ✏️ 렉시컬 스코프

- 정의 : 변수나 함수의 선언 시점에서 정해지는 변수의 유효 범위
- 목적 : 예측 가능하고 안정적인 실행 환경 제공
- 특징
    - 변수 호출 시점이 아닌 선언 시점에 유효범위가 정해짐 → 변수의 선언 시점을 기억하여 스코프 환경 설정 (체인 스코프)
    - 상위 스코프에 있는 변수를 기억하게 됨
    
- 렉시컬 스코프 : 해당 함수/변수의 렉시컬 환경에서 유효한 범위
- 렉시컬 환경 : 스코프 체인을 관리하고 실행 컨텍스트의 일부
- 변수는 스택(실행 컨텍스트)에 저장되지만 변수의 주소는 렉시컬 환경(힙)에 저장
- 실행 컨텍스트에서 렉시컬 스코프를 결정하는 참조주소가 저장된 렉시컬 환경을 찾아 변수를 할당하게 됨

<br><br>

## ✏️ 스코프 체인

- 정의 : 현재 스코프에서 변수를 찾고 없다면 상위, 전역 스코프까지 탐색해 변수를 찾는 과정
- 자바스크립트의 렉시컬 스코프 방식 때문에 스코프 체인 동작
    
    ```jsx
    const global = 1;
    function parentScope() {
    	const parent = 2;
    	return function childScope() {
    		const child = 3;
    		
    		console.log(child); //현재 스코프
    		console.log(parent); //상위 스코프
    		console.log(global); //전역 스코프(루트 스코프)
    	}
    }
    
    const parentCall = parentScope();
    parentCall();
    ```
    
<br>

---

<br>

## ✏️ 보충 내용

### ▶️ promise.all()과 promise.allSettled()의 차이점

- all()
    - 하나 실패하면 함수가 중단되고 전체 실패로 catch()문에 들어감
- allSettled()
    - 실패, 성공으로 결과가 나뉘지 않고 각각의 결과 자체만을 전부 반환

<br>

### ▶️ Aysnc메소드가 항상 Promise를 반환하는 이유

- async 키워드를 사용하는 순간 자동으로 성공 시 Promise.resolve(), 실패 시 Promise.reject()를 반환하도록 설계돼있음

<br>

---

<br>

## 👀 느낀점

오늘 내용이 지금까지 배운 것 중에 가장 빨리 이해되고 가장 재밌었다.<br>
그리고 오늘은 gpt가 내 말을 너무 잘 들었다. 그래서 빠른 이해를 할 수 있었다.<br>
그리고 딱 조사 다 하니까 3시간 50개 제한이 딱 끝남<br>
오늘 미니퀘스트 할 시간이 생겨서 너무 행복하다.

그리고 예상질문 우선 안 보고 혼자 파봐야지 해서 공부했는데 예상 질문을 보니 4개 정도 빼고 다 해결됐다.<br>
그래서 오!!!!! 또 한 번 지피티 찬양<br>
그리고 찐 파고들기의 진가를 본 느낌?<br>
내일 딥다이브? 꽤괜일지도?

코딩할 땐 집에서 괜찮은데 개념 정리할 때 집에서는 너무 안 돼서 맨날 카페에 가는데<br>
오늘 카페 3시간컷 이건 진짜 귀하다.

이벤트 루프까지 알아볼 생각까지는 없었는데 수업 때 나온 질문이 머리에 남아서 알아보다 보니 같이 공부하게 되었다. <br>
이렇게 명확한 게 있을까?<br>
암튼 너무 좋다.<br>
그냥 프론트 할까…<br>
암튼 스프링 배울 때까지 좀 더 고민!
