## ✏️ Spring

자바 플랫폼을 위한 모듈형 아키텍처 프레임워크

- 주요 모듈
    - AOP (관점 지향 프로그래밍)
    - Core 컨테이너 → IOC를 제공하여 Bean의 생명 주기 관리
    - Data Access / Integration - JDBC 같은 거 이용할 때 용이
    - Web
    - Messaging
    - Test → `@ContextConfiguration`, `@MockBean`과 같은 어노테이션으로 단위 → 통합 테스트 가능
- 어노테이션
    - 자동으로 모듈 간 매핑을 지원해 주는 기능
    - `@` 사용
- 객체 지향성
    - 상속
        - Bean 정의 상속 → XML 설정
        - 클래스 extends 상속
    - 다형성
        - 다양한 어노테이션을 사용하여 인터페이스의 다양한 동작을 지원
        - 스프링 부트는 인터페이스 기반 언어(?)
    - 추상화
        - 인터페이스로 내부 구현체 숨김
        - `@Transactional` 어노테이션 사용 → 데이터 트랜잭션 관리
    - 캡슐화
        - Bean의 사용 → 재사용성 향상

### ▶️ Spring과 Spring Boot의 차이점에 대해서 설명해주세요.

- Spring
    - 수동으로 의존성 세팅 - 리액트에서 매번 import 해줘서 모듈 간 의존성 주입하는 것과 유사
    - Start POM 없음 - React에 비교하자면 npx create-react-app 없이 하나하나 dependencies에 추가해야 함
- Spring Boot
    - 더욱 구현에 집중할 수 있도록 의존성 세팅이 더욱 편리
    - @SpringBootApplication 어노테이션으로 모듈 간 의존성 자동 주입
    - Start POM - `spring-boot-starter-*` 제공 ) npx create-react-app으로 package.json에 자동으로 라이브러리 추가되는 메커니즘과 유사
- DI, AOP, IOC 컨테이너는 동일하게 동작

**[GPT 질문 내역이 좀 괜찮았던 것 같음 ㅎ]**

https://chatgpt.com/share/67c982d0-ebb4-8012-82a1-73e6fd497342

## ✏️ MVC 패턴

- 정의 : 모델(데이터), 뷰(UI), 컨트롤러(처리), 역할별로 기능을 분리하여 구현하는 개발 방식
- 특징
    - 뷰는 일반적으로 프론트엔드로 구분됨
    - 컨트롤러를 통해 모델과 프론트엔드 처리를 연결(?)할 수 있음
- 동작 흐름
    1. 클라이언트 요청
    2. 컨트롤러에서 요청 받아서 필요한 데이터 처리를 위해 모델 호출
    3. 모델에서 데이터 처리 후 컨트롤러로 반환(비즈니스 로직)
    4. 논리적 로직 수행 후 컨트롤러에서 클라이언트로 응답
    5. 화면에 렌더링 / 사용자 확인

## ✏️ Servlet

- 정의 : 사용자의 요청을 받아 처리하는 객체
- 특징
    - 순수 자바에서는 HttpRequest 객체를 사용하여 요청을 받음
    - 별도의 RequestDispatcher 객체를 이용하여 다른 서블릿이나 뷰로 포워딩
    - 스프링부트에서는 자동으로 @RestController나 @Controller가 요청을 받아 처리

## ✏️ Spring MVC

- 동작 과정
    1. 사용자 요청
    2. DispatcherServlet : 사용자 요청을 받는 컨트롤러
    3. HandlerMapping : GetMapping, RequestMapping 어노테이션으로 url 매핑
    4. HandlerAdapter : 실제로 요청을 처리할 컨트롤러 호출 (Mapping 어노테이션으로 매핑 후 자동 수행)
    5. 컨트롤러 → 모델(비즈니스 로직/데이터 처리) → 컨트롤러
    6. RestController 어노테이션에 의해 json으로 변환하여 사용자에게 응답
    - Controller 어노테이션의 경우 컨트롤러 → 뷰 리졸버(뷰 url 찾기) → 뷰
- DispatcherServlet : 사용자 요청을 받음
- HandlerMapping : 처리할 메서드/컨트롤러 찾기
- HandlerAdapter : 어떻게 컨트롤러를 실행할지 결정

## ✏️ Bean

- 정의 : 비즈니스 로직을 처리하는 역할
- 특징
    - 스프링부트에서 @Component (+ @Service + @Repository) 같은 역할
    - 객체지향적으로 구현하기 위해 세 역할을 분리하는 게 일반적
- 알고 가기
    - @Service : 실질적인 비즈니스 로직 처리
    - @Repository : CRUD (DAO 역할)
    - @Component : @Service + @Repository
    - @Entity : 데이터 구조 정의된 Model / @Data : json DTO 데이터 구조 Model (선호)

**[두 번째 지피티 프롬프트]**

https://chatgpt.com/share/67c99c13-6de4-8012-8ddc-50871b14aeaf + 빈까지

## ✏️ Interceptor

- 정의 : Spring에서 처리 로직 중간에 가로채서 다른 작업을 처리하는 기능의 컴포넌트
- 목적 : 컨트롤러 전후 요청을 가로채 추가 로직을 처리하기 위해
- 사용 방법
    - `HandlerInterceptor`를 구현
    - `WebMvcConfigurer`를 구현하고 `@Autowired`를 사용하여 의존성 주입
    - 특정 경로에만 추가할 수도, 제외할 수도 있음
    - 이때 관련 요청이 오면 `preHandle()`, `postHandle()`, `afterCompletion()` 함수를 호출하여 가로챔
- 예시 ) 사용자 인증, 권한 확인 등

## ✏️ Filter

- 정의 : 서블릿 컨테이너에서 요청을 가로채는 기능의 컴포넌트
- 사용 방법
    - `import javax.servlet.Filter;`
    - `OncePerRequestFilter` → 요청 당 한 번만 실행됨

### ▶️ 인터셉터와 필터의 차이를 설명해주세요.

1. 인터셉터는 일반적으로 자바에서 설정, 필터는 일반적으로 web.xml에서 설정
2. 인터셉터는 컨트롤러 전후, 필터는 서블릿 실행 전후에 호출됨
3. 인터셉터는 HandlerInterceptor 인터페이스를 구현하고 필터는 javax.servlet.Filter를 구현하여 사용
4. 인터셉터는 `WebMvcConfigurer`를 구현하여 `@Autowired`로 의존성을 주입한다면 필터는 `@Component`나 `@Bean`에 등록하여 의존성 주입하지만 기본적으로는 불가능함

## ✏️ POJO (Plain Old Java Object)

- 정의 : 특정 프레임워크에 종속되지 않는 순수 자바 객체
- 스프링 부트는 기본적으로 POJO 기반이라고 함

## ✏️ Gradle

- 정의 : 자바 및 스프링부트에서 빌드와 의존성을 관리하는 빌드 자동화 도구
- 목적 : XML 기반의 Maven보다 간결하게 빌드, 테스트, 배포를 자동화하고 쉽게 의존성을 관리하기 위함
- 종류
    - `build.gradle` - 자바
    - `build.gradle.kts` - 코틀린
- 실행 방법
    - `build.gradle` : 필요한 라이브러리 정의 (import문 같은 거, dependencies에 있는 거)
    - `gradle build` : 실행 전 필요한 라이브러리나 도구를 다운로드하며 실행 준비
    - `gradle run` : 어플리케이션 실행

---

## 👀 느낀점

https://github.com/okiidokim/okiidokim/tree/main/TIL/2025.03.MAR<br>
5주차 DB 과제도 리액트 마이그레이션을 제외하고 진행 완료

스프링 재밌다.<br>
저 어노테이션 매핑 기능이 진짜 신기한 것 같다.<br>
웬만한 건 다 돌아가게 해 주는 것 같다. (제이도 그렇게 말함)<br>
뭔가 의존성 주입이랑 어노테이션이랑 좀 헷갈리긴 하는데 암튼 재밌음

지금 백엔드 6주차부터 할지 5주차 리액트 마이그레이션부터 할지 너무 고민됨<br>
뭔가 저번주에 해커톤 하면서 아 나 걍 프론트엔드 해야겠다 싶었는데<br>
지금 스프링 하니까 이거 너무 재밌는 것 같음<br>
개인 플젝 해 보고 빨리 정해야 할텐데... 걱정이 이만 저만..~~<br>
그리고 아직은 이론만 좀 공부한 거라 구현해 봐야 알 것 같다.

그리고 역할 분리가 생각보다 MVC 세 단계 이상으로 더 되는 걸 보니 막상 구현하면서 헷갈릴 것 같음<br>
생각보다 자바 그 자체고, 생각보다 지원하는 역할 분리가 굉장한 것 같고<br>
생각보다 객체지향에서 가져와지는 구현된 개념이 많다는 거?<br>

그리고 자바 프로젝트 뭐 만들면 있는 gradle이 뭔지 몰랐는데 이제 알았다.<br>
또 해커톤이 생각보다 도움이 많이 됐던 게<br>
클라우드에서 배포할 때 빌드 명령어 달라고 했는데<br>
gradle도 덕분에 보자마자 어느 정도 와닿았다.<br>
아 이게 도커 파일에 작성하는 서비스 빌드/실행문이구나~ 하는 게 이해됐다.

그리고 생각보다 속도도 좀 붙는 듯? (아닐 수도... ㅎㅎ)<br>
암튼 오늘 5주차 과제도 하고 개념 정리도 하고 야무진 하루를 끝냈다.<br>
내일 좀 맘 편할 것 같아 다행이다. 후후~~
