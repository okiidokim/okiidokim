# ✏️ 프론트엔드

### ▶️ html (hypertext markup language)

- 정의 : 하이퍼링크를 포함한 문자나 이미지를 웹브라우저에 구조화하기 위한 언어
- 목적 : 시각적으로 웹 브라우저의 구성 요소를 표현하기 위함
- 태그 (p태그 예시)
    - `<p>` : 여는 태그
    - `</p>` : 닫는 태그

### ▶️ css (cascading style sheet)

- 정의 : 웹 문서의 스타일을 저장해 둔 스타일 시트
- 사용 방법
    1. style태그
        
        ```jsx
        <style>
        	h1{
        		color : blue;
        	}
        </style>
        ```
        
    2. css파일 + link 태그
        
        ```jsx
        <link rel="stylesheet" type="text/css" href="./style.css" />
        ```
        
- 요소의 적용 방법
    
    ```jsx
    <h1 class="title" id="a"> 제목 태그의 스타일 적용 방법 </h1>
    ```
    
    1. 요소 선택자
        
        ```jsx
        h1 {
        	color : blue;
        }
        ```
        
    2. 클래스 선택자
        
        ```jsx
        .title {
        	color : blue;
        }
        ```
        
    3. 아이디 선택자
        
        ```jsx
        #a {
        	color : blue;
        }
        ```
        

### ▶️ boxmodel

- 정의 : 각 요소를 시각적으로 배치하기 위한 기능
- 구성
    - margin : 외부 여백 → 요소 간 간격
    - border : 테두리 → style, color, width
    - padding : 내부 여백
    - content : 실질적 내용
    

### ▶️ 우리의 서버 구조

- 클라이언트-서버 구조
    - 사용자 → 클라이언트에 파일 요청 → 클라이언트 응답 → 서버에 데이터 요청 → 서버 응답
    - 사용자에게 보여지는 부분과 데이터 처리 부분의 분리
- mvc 패턴
    - model-controller-view
    - 유지보수성, 속도, 성능 향상을 위해 구현 목표를 분리하여 구현
- 웹의 발전
    - 노트북, 컴퓨터에 비해 성능이 낮고 화면이 작은 휴대폰의 등장과 급격히 많아지는 사용량
<br><br>
# ✏️ 웹의 이해

### ▶️ flex

- 정의 : 요소를 유연하게 배치하고 조정하기 위한 레이아웃 모델
- 목적 : 서로 다른 웹의 크기에 반응형으로 구현
- 사용 방법
    - 부모노드에 `display : flex` 정의
    - 방향
        - `flex-direction` : row, column, row-reverse, column-reverse
    - 정렬
        - `align-items` : flex-start, flex-end, center, stretch → 세로 정렬
        - `justify-content` → 가로 정렬
    - 기타
        - `flex-grow` : 1 → 내부 확장 시작 크기
        - `flex-wrap` : no-wrap, wrap, wrap-reverse → 컨테이너 초과 처리

### ▶️ DOM (document object model)

- 정의 : 웹 브라우저에 의해 html 문서를 객체로 파싱한 구조
- 동작 과정 : bytes → characters → token → node → DOM 트리
- 목적 : 실시간, 동적으로 웹 브라우저를 동작시키기
- API + javascript → 새로 고침 없이 동적으로 적용

💡 면접 때 이 질문 나온 거 대답 거의 못했는데 하하 제대로 알아야지^^ <br> 제대로 된 개념 습득의 중요성!

### ▶️ CSSOM (css object model)

- 정의 : 웹 브라우저가 css를 해석해 스타일을 조작하기 위한 객체 모델
- 동작 과정 : bytes → characters → token → cssom 트리 → DOM 트리 병합 → 렌더링
- 목적 : 스타일 정보에 집중하고 웹 브라우저에 동적으로 적용해 반응형 웹 구현

### ▶️ event

- 정의 : 웹 브라우저에서 사용자의 동작
- 구분
    
    ```jsx
    document.getElementById("idname").addEventListener('click',() => {
    	alert("핸들러 함수 부분");
    });
    ```
    
    1. eventListener
        - 사용자의 마우스 클릭, 이동, 키보드 입력을 감지하고 알리는 역할
    2. eventhandler
        - `() => { alert("핸들러 함수 부분"); });`
        - event를 감지했을 때 수행되는 함수

### ▶️ fetch

- 정의 : 클라이언트에서 서버에 비동기적으로 데이터를 요청하고 응답 받는 과정
- 사용 방법
    - `fetch(url)` 형식 → 뒤에 처리 함수 붙여서 구현
    - 응답 처리 : `.then()`
    - 응답 변환 : `.json()`
    - 오류 처리 : `.catch()`

### ▶️ lottie

https://lottiefiles.com/

- 정의 : json 형식의 애니메이션을 제공하는 서비스
- 특징
    - 픽셀 아닌 벡터
    - 파일 크기가 작음
    - 명확한 애니메이션 요소로 수정이 쉬움

💡 react-icons만 알아서 다른 애니메이션 서비스는 몰랐는데 보자마자 오? 하고 바로 북마크에 저장함 ㅎㅎ

### ▶️ SEO(search engine optimization)

- 정의 : 검색 엔진에 의해 쉽게 노출되기 위해 최적화하는 기술
- 방법
    1. 키워드 분석 및 구축
    2. 컨텐츠 최적화
    3. 웹 사이트 구조 → 간결, 명확한 구조로 ux 향상
    4. 유명한 사이트의 백링크
    5. sns 활용
- 장기적 전략, 노력이 중요
<br>

--- 

<br>

# ✏️ DB 추가

### 어제 내가 이해한 대로 정리한 2nf와 3nf

- 2NF : 서로 다른 후보키에 대해 서로 다른 집합으로 종속적이면 이를 분리함
- 3NF : 하나의 후보키로 연결되는 관계가 두 개로 나뉠 수 있는 집합인 걸 분리하는 것

### 보이드에게 질문 후 보완!

- 2nf는 복합키에서의 부분 종속 제거
    
    > 2nf는 복합키에서 부분종속이 일어나면 안된다는 건데, 부분종속이 두 개의 컬럼(과목코드와 학번)이 복합 키인 상황에 전공같은 속성이 '학번' 컬럼 하나만 보고도 결정된다면 부분 종속에 해당하는 거
    > 
- 3nf는 비-키 속성간의 이행 종속 제거
    - 강의에서 비-키라는 단어를 정확히 들은 건지 몰랐어서 넘어갔는데 non-key라고 한다.
    
    > non-key b가 PK에 종속되어 있으면서 non-key a에 종속되는걸 이행 종속이라고 하는데, 이 연결고리 끊어야한다.
    > 

💡 복합키, 키가 아닌 속성을 몰라서 이해하지 못했던 거라는 걸 알게 됐다! <br>
💡 모르겠으면 나중에 DB 과제 하면서 이해하는 게 가장 빠름! <br>
💡 또 모르겠으면 걍 다 쪼개기 → 4nf, 5nf까지는 실무에서 안 쓰임

- 예시 재확인
    
    
    | id | name | age | product | price |
    | --- | --- | --- | --- | --- |
    | 1 | 도현 | 24 | 노트북 | 200만 원 |
    | 2 | 다현 | 21 | 휴대폰 | 100만 원 |
    | 3 | 민준 | 19 | 노트북 | 200만 원 |
    | 4 | 다은 | 24 | 휴대폰 | 100만 원 |
    
    ⬇️ id와 product는 복합키 ⇒ 기본키 + 후보키 쪼개기
    
    | id | name | age | product |
    | --- | --- | --- | --- |
    | 1 | 도현 | 24 | 1 |
    | 2 | 다현 | 21 | 2 |
    | 3 | 민준 | 19 | 1 |
    | 4 | 도현 | 24 | 2 |
    
    | id | name | price |
    | --- | --- | --- |
    | 1 | 노트북 | 200만 원 |
    | 2 | 휴대폰 | 100만 원 |
    
    ⬇️ name과 product는 두 개의 non-key (아마도?) → 쪼개기
    
    | id | name | age |
    | --- | --- | --- |
    | 1 | 도현 | 24 |
    | 2 | 다현 | 21 |
    | 3 | 민준 | 19 |
    
    | id | user | product |
    | --- | --- | --- |
    | 1 | 1 | 1 |
    | 2 | 2 | 2 |
    | 3 | 3 | 1 |
    | 4 | 1 | 2 |

--- 

# 한 줄 정리

| 단어 | 뜻  | 비고 |
| --- | --- | --- |
| html | 하이퍼링크를 포함한 텍스트나 이미지를 웹브라우저에 동적으로 구조화할 수 있는 언어 | - hypertext markup language <br> - 태그 <br> - 시각적으로 브라우저의 구조 표현 |
| css | 계층적으로 웹 문서의 스타일을 저장해 둔 스타일 시트 | - cascading style sheet <br> —— <br> 1. <style> 태그 <br> 2. css 파일 + <link> 태그 <br> —— <br> 1. 요소 선택자 <br> 2. 클래스 선택자 <br> 3. 아이디 선택자 |
| boxmodel | 요소를 시각적으로 배치하기 위한 기능 | - margin : 외부 여백, 요소 간 간격 조정 <br> - border : 테두리 → style, width, color <br> - padding : 내부 여백 <br> - content : 내용 |
| mvc 패턴 | 속도, 유지보수성, 성능 향상을 최적화하기 위해 model, controller, view를 분리하여 구현하는 방법 | model-controller-view <br> 사용자 → 클라이언트 요청 → 클라이언트 응답 → 서버 요청 → 서버 응답 |
| flex | 요소를 유연하게 배치하고 조정할 수 있게 하는 레이아웃 모델 | 부모 노드 : `display : flex` <br> - `flex-direction` : row, column <br> - `align-items` : flex-start, flex-end, center, stretch <br> - `justify- content` → 가로 정렬 <br> - `flex-grow : 1` → 내부 확장 크기 <br> - `flex-wrap` : wrap, no-wrap, wrap-reverse |
| DOM | html 코드가 웹 브라우저에 의해 객체로 파싱된 구조 모델 | document object model <br> - bytes → characters → token → 노드 <br> - DOM : 노드의 구조적 모음 <br> - javascript + API로 웹의 실시간 동적 동작을 가능하게 함 |
| cssom | javascript로 조작하기 위해 웹 브라우저에서 css를 객체로 파싱한 모델 | css object model <br> - bytes → characters → token → cssom 트리 (DOM 트리에 병합) → 렌더링 |
| event | 사용자의 동작 | - 마우스 클릭, 이동, 키보드 입력 <br> - eventLister : 동작 발생을 감지하고 알림 <br> - eventhandler : 동작 발생 시 수행되는 함수 |
| fetch | 클라이언트에서 서버에 비동기적으로 데이터를 요청하고 응답 받는 과정 | - 응답 처리 : `.then()` <br> - 오류 처리 : `.catch()` <br> - 응답을 json으로 변환 : `.json()` |
| lottie | json 기반의 애니메이션을 제공하는 서비스 | - 작은 용량 <br> - 픽셀 아닌 벡터 <br> https://lottiefiles.com/ |
| SEO | 서비스가 검색 엔진에 의해 쉽게 노출되도록 최적화하는 기술 | search engine optimization <br> 1. 키워드 분석 및 구축 <br> 2. sns 활용 <br> 3. 유명 사이트의 백링크 <br> 4. 컨텐츠 최적화, ux 향상 |


---

# 👀 느낀점

- 어제에 비해 비교적 내용이 할 만 한 느낌?
어제는 진짜 오래 걸렸는데 프론트엔드 지식이 백엔드 지식에 비해 비교적 많은 것도 있었고 재밌게 공부했다.
- 보이드 왈 : **안전규칙은 피로 쓰이고, 개발실력은 똥으로 쓰입니다.**
- 후 진짜 내일부터 1주차 과제할 수 있다!!!! 사실 오늘 밤부터 시작해야 함… ㅎㅎ
- 그리고 내가 몰랐던 css 스타일 속성들이 좀 있어서 놀랐다. 인터넷 찾아도 이렇게 구조적으로 깔끔하게 정리해 준 블로그가 없는데 보이드는 그저 나만의 블로그~
- 웹 응용 듣기 귀찮았는데 어차피 풀스택이라 들어야 하지만 웹 기초 강의 들으니까 아 응용도 들어서 프론트 만렙걸 돼야겠다는 생각
- 물론 데이터베이스도 맨땅에 헤딩으로 이해하는 게 맞다고 하지만 사실 프론트도 똑같은 것 같다. 처음에 리액트 할  때 진짜 그지같이 구현해서 내 노트북에서만 레이아웃이 정상이었다. 근데 점점 flex, padding, margin을 프로젝트 경험으로 이해하면서 어느 정도 반응형으로 구현할 수 있었다.
- 2주 가까이 자바, 백엔드 하면서 갑자기 프론트 강의 보니까 또 재밌음… 백엔드 강의 들으면서 백엔드로 갈까? 하다가 지금은 ㄹㅇ 백엔드와 프론트에서 어디로 가야 할지 아직도 모르겠어~~
