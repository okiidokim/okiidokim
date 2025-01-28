# ✏️ 자바의 메모리 영역과 컴파일 과정을 서술하시오.

- 메모리 영역
    - 스레드 별로 할당되는 세 개의 영역과 아닌 두 개의 영역이 있다. 스레드 별로 할당되는 영역은 Heap, JVM Stack, Native Method 영역이다. 나머지 두 개는 PC Register, Method 영역이다.
    - Heap 영역 : 객체, 문자열이 할당되는 영역으로 메모리에서 가장 큰 부분을 차지하며 가변적 크기를 가진다. 가비지 컬렉터의 관리를 받는다.
    - JVM Stack 영역 : 임시 데이터, 멤버 변수의 정보가 저장되는 영역으로 스레드별 고유하게 하나씩 존재한다.
    - Native Method 영역 : 자바로 작성되지 않은 코드의 정보가 저장되는 영역으로 C, C++, 기계코드 등이 저장된다.
    - PC Register : 어떤 부분을 어떤 명령으로 실행할지의 정보가 저장돼있는 영역
- 컴파일 과정
    
    💡 이 부분은 확실히 공식 문서에 작성된 클래스들과 설명을 읽고 분석하는 게 최고인 듯
    
    1. 소스코드(.java 파일)를 실행시킨다.
    2. javac 컴파일러에 의해 바이트 코드 파일(.class)로 변환된다.
    3. 변환된 바이트 코드 파일은 JVM의 클래스 로더로 간다.
    4. JVM의 클래스 로더는 실행시킬 수 있는 파일인지 검증 및 분석 후 JVM 메모리 영역에 로드한다.
    5. execution engine에 의해 바이트 코드는 인터프리터 방식과 JIT 방식, 2가지 번역 방식 중 하나로 실행된다.

# ✏️ 가비지 컬렉션(Garbage Collection)에 대해 정의하고 어떻게 동작하는지 서술하시오.

- Java 외에 다른 언어 한가지를 정하고 어떻게 Garbage Collection이 동작하는지 서술하고 비교해보세요.
- Java GC도 여러 종류가 있습니다. 어떤 차이점이 있는지 간단하게 서술하시오.

## 가비지 컬렉션이란?

- JVM 힙 영역의 메모리에 동적 할당되는 객체가 사용되는지 주기적으로 메모리를 확인하여 동적 할당을 해제하고 메모리에서 제거하는 기능
    - 직역하면 쓰레기 수집이라는 뜻인데 쓰레기는 참조되지 않는 메모리를 말하고 수집은 말 그대로 모은다는 의미로 한 번 가비지 컬렉션 실행될 때마다 사용하지 않는 걸 하나씩 모은다고 보는 것

---

https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html

https://docs.oracle.com/en/java/javase/11/gctuning/garbage-collector-implementation.html#GUID-16166ED9-32C6-402D-BB22-FD85BCB04E57

## Automatic GC

> An in use object, or a referenced object, means that some part of your program still maintains a pointer to that object.
> 
- 사용 중인 객체(참조된 객체)라는 것은 프로그램 중 일부가 해당 객체의 포인터, 주소값을 가진다는 의미

> In a programming language like C, allocating and deallocating memory is a manual process. In Java, process of deallocating memory is handled automatically by the garbage collector. The basic process can be described as follows.
> 

C → 메모리 할당/해제는 개발자의 영역

자바 → 해제는 가비지 컬렉터의 영역 (할당은 new 예약어)

1. 마킹
    - 가비지 컬렉션 수행을 시작하면 참조하는 메모리를 표시한다.
    - 가비지 컬렉션을 수행하는 타이밍
        - 일정 주기
        - 힙 메모리가 충분하지 않을 경우
    
    ![스크린샷 2025-01-24 오후 5.12.34.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6ccd7f16-2c35-4552-9db0-2cc8aa49354a/eacbe07c-022d-4938-8431-8f6d37ba8d58/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-01-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.12.34.png)
    

1. 삭제 후 압축
    - 1번에서 참조되지 않은 객체는 삭제된다.
    - 빈 공간을 한 쪽으로 몰아 추가로 순차적으로 메모리에 할당될 수 있게 한다.
    
    ![스크린샷 2025-01-24 오후 5.12.48.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6ccd7f16-2c35-4552-9db0-2cc8aa49354a/5bfe205c-59d9-4172-af39-4e8f0323f1cb/0fac6faa-45e1-481a-b38f-c43030d97c4a.png)
    

### 하지만 Automatic GC는 효율적이지 못함 왜?

- 가비지 컬렉션 주기가 돌아올 때마다 수행할텐데 우선 메모리 세그먼트는 배열처럼 순차적으로 있음
    - 메모리에 할당되는 객체가 많아질수록 하나씩 순차적으로 참조되는 객체인지 검사할텐데 시간이 너무 오래 걸림
    - 대부분 객체의 생존 시간은 짧은데 그걸 전부 검사
    - 그래서 나온 게 Genenerational GC

## Generational GC

![스크린샷 2025-01-24 오후 7.21.00.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6ccd7f16-2c35-4552-9db0-2cc8aa49354a/5c86e844-4f36-4f24-a0e9-216d9440f546/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-01-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.21.00.png)

- Permanent 영역
    - 위 그림에는 있지만 현재 Heap 영역에 있지 않아 GC의 관여를 받지 않음
    - MetaSpace라는 이름으로 변경되고 현재는 Native Method 영역에 존재

### ⬇ 현재 GC 구조

![스크린샷 2025-01-24 오후 7.46.58.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6ccd7f16-2c35-4552-9db0-2cc8aa49354a/ef024b8e-bcfe-4730-989d-d65fb869fd27/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-01-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.46.58.png)

- Young Generation
    - 새로운 객체, 생성된지 얼마 안 된 객체들이 존재 하는 영역
    - 마이너 가비지 컬렉션 발생 타이밍 : YoungGeneration이 가득 찼을 때
    - 대부분의 객체가 이곳에서 생성되고 죽음
- Old Generation
    - Young Generation에 있는 객체 중 특정 나이(?) 이상의 객체가 보내지는 영역
    - 메이저 가비지 컬렉션 → 모든 살아있는 객체에 대해 참조 여부 확인
    - 대부분 객체의 생존 시간은 짧기에 많은 객체가 이곳으로 넘어오지는 않음
- 가비지 컬렉션 = Stop the World 이벤트 → 모든 스레드 중지

1. 메모리에 객체가 할당되면 `eden`에 세그먼트가 생성되어 채운다.
2. `eden`이 가득 차면 stop the world 이벤트가 발생한다. (마이너 가비지 컬렉션)
3. 참조되는 객체는 `s0` 영역으로 이동하고 더이상 참조되지 않는 객체는 `eden`에서 삭제된다.
    - 처음 `eden`에 할당된 객체의 나이는 전부 0이며 `s0`으로 이동한 객체의 나이는 +1, 즉 1이 된다.
    - `eden`에는 이제 아무 객체도 남지 않는다.
4. 2번 과정을 반복한다. 
    - `s0`에서도 마이너 가비지 컬렉션이 발생하는데 이때 s1으로 넘어오는 객체를 구분하기 위해 이동할 때마다 객체의 나이를 1씩 증가한다.
    - 이때 `s0`과 `eden`은 비어있게 된다.
    - 마이너 가비지 컬렉션이 발생할 때마다 young generation의 `eden`, `s0`와 `s1` 중 하나는 항상 클리어된다.
5. 2 ~ 4의 과정을 반복한다.
    - 이때 `s0`/`s1`에서 특정 나이 임계값을 넘는 객체는 old generation으로 이동한다.

💡 케빈이 기능이 생긴 문제점과 해결책을 정리한다고 했는데 실제로 문제점과 해결책을 아니 해당 기능에 대한 이해가 확 올라가는 기분이다.

## Java 외에 다른 언어 한가지를 정하고 어떻게 Garbage Collection이 동작하는지 서술하고 비교해보세요.

[javascript]

https://developer.mozilla.org/ko/docs/Web/JavaScript/Memory_management

자바스크립트 하려 했는데 급 너무 지쳐버림…

[C/C++] (간략)

- 메모리 관리를 위해 `malloc()` 과 `free()`를 사용
- 자바에서는 메모리 할당을 위해 `new` 예약어를 사용

→ 개발자가 메모리 할당에 있어 코드로 구현하는 점은 공통

→ 해제에 있어서 unmanaged 언어들은 직접 `free()`, `delete()` 함수를 통해 해제/삭제 해줘야 하지만 managed 언어는 가비지 컬렉션을 통해 개발자의 코드 없이 자동으로 객체가 삭제된다.

💡사실 처음 C언어를 배울 때 `free()` 함수를 사용하고 자바스크립트에서는 해제하지 않는 부분에 대해 의문을 품어본 적이 없는데 (메모리 할당, 객체 선언 방식이 달라 동일 선상에 두어 생각하지 못했음) 이렇게 보니 나는 정말 패션 컴공이 맞구나 싶고 나름 자바, 자바스크립트는 최신 언어인 듯 느껴진다.

---

## Java GC도 여러 종류가 있습니다. 어떤 차이점이 있는지 간단하게 서술하시오.

https://velog.io/@mirrorkyh/GC-%EC%A2%85%EB%A5%98%EC%99%80-%ED%8A%B9%EC%A7%95

https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EA%B0%80%EB%B9%84%EC%A7%80-%EC%BB%AC%EB%A0%89%EC%85%98GC-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC

1. Serial GC
    - 서버의 CPU 코어가 1개일 때 사용하기 위해 개발된 가장 단순한 GC
        - CPU 코어 : 연산 작업을 수행하는 핵심적인 부분 (이게 많을수록 성능 귯)
    - 가비지 컬렉션을 수행하는 스레드가 하나이기 때문에 stop the world 시간이 가장 길다. (n개의 스레드 처리 시간)
    - 실무에서 잘 사용하지 않음
    - 실행 명령어 : `java -XX:+UseSerialGC -jar Application.java`
2. Parallel GC (Throughput GC)
    - Java8 이전에 주로 사용
    - 마이너 GC를 멀티 스레드로 수행하지만 메이저 GC는 단일 스레드로 수행하는 GC
    - 기본적인 처리 과정은 Serial GC와 동일하지만 여러 개의 스레드를 동시에 작업할 수 있다. (cpu 개수만큼 스레드 처리 시간 절감)
    - 실행 명령어 : `java -XX:+UseParallelGC -jar Application.java`
3. Concurrent Mark-Sweep(CMS) GC
    - 위 두 개의 Old Generation에서는 단일 스레드로 가비지 컬렉션을 수행하였기 때문에 2번에서도 대규모로 갔을 때 중단되는 문제는 피할 수 없었음
    - Old Generation에서도 멀티 스레드로 가비지 컬렉션을 수행하는 GC
    - 문제점
        - 힙이 크거나 코어가 많은 시스템에서는 성능 문제 가능성 → 메모리 파편화 문제
        - 높은 CPU 사용량
        - 복잡한 GC 과정
    - Java9 부터 사용하지 않다가 Java14부터 삭제
    - 실행 명령어 : `java -XX:+UseConcMarkSweepGC -jar Application.java`
4. G1(Garbage First) GC
    - 힙을 작은 영역으로 나누고 각 영역에 대해 별도의 가비지 컬렉션을 수행하는 GC
    - 단순히 s0 ↔ s1이 아닌 효율적인 영역으로 객체 재할당
    - 기본 GC 알고리즘으로 Java9 이후부터 적용
    - 맥북에서 사용 중인 거 확인 함 후후
        
        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6ccd7f16-2c35-4552-9db0-2cc8aa49354a/aa85938b-6879-4c5d-b8a0-934081a1027f/image.png)
        
    - 신기한 점은 사용자가 GC 알고리즘을 선택할 수 있긴 함
    - 실행 명령어 : `java -XX:+UseG1GC -jar Application.java`
5. Shenandoah GC → 사실 여기부턴 뭐라는 건지 모르겠음… 특히 이 놈… (냅다 차나핑 됨)
    - CMS GC, G1 GC의 문제를 해결한 GC
    - Java12에 등장 → 최신 GC라 비교적 정보가 적음
    - 실행 명령어 : `java -XX:+UseShenandoahGC -jar Application.java`
6. Z GC
    
    https://d2.naver.com/helloworld/0128759
    
    - 힙의 크기가 증가해도 stop the world의 시간이 절대 10ms을 넘지 않도록 하는 아주 빠른 GC
    - Java15에 등장 → 최신 GC라 비교적 정보가 적음
    - G1과 유사하게 영역 개념을 사용하지만 고정적 영역을 사용하는 G1과 달리 2배수로 동적 영역으로 운영됨 (small, medium, large 세 타입으로 나뉨)
    - 빈 영역을 찾기보다 새 영역을 생성해 채우는 방식 → 하나의 영역에서 제거되고 남은 건 생성되는 새 영역에 들어감(재할당)
    - 실행 명령어  : `java -XX:+UnlockExperimentalVMOptions -XX:+UseZGC -jar Application.java`

| 이름 | 차이 |
| --- | --- |
| Serial GC | - Young Generation : 단일 스레드
- Old Generation : 단일 스레드
- 실행 : `java -XX:+UseSerialGC -jar Application.java` |
| Parallel GC | - Young Generation : 멀티 스레드
- Old Generation : 단일 스레드
- 실행 : `java -XX:+UseParallelGC -jar Application.java` |
| Concurrent Mark-Sweep(CMS) GC | - Young Generation : 멀티 스레드
- Old Generation : 멀티 스레드
- 삭제됨 (현재 사용하지 않음)
- 메모리 파편화 문제, 복잡한 과정
- 실행 : `java -XX:+UseConcMarkSweepGC -jar Application.java` |
| G1 GC | - Young, Old보다 영역의 개념으로 변경
- 단순 s0 ↔ s1이 아닌 효율적인 영역으로 객체 재할당
- 현재 디폴트 GC
- 실행 : `java -XX:+UseG1GC -jar Application.java` |
| Shenandoah GC | - CMS GC, G1 GC의 문제 해결
- 실행 : `java -XX:+UseShenandoahGC -jar Application.java` |
| Z GC | - G1과 유사한 편이지만 G1과 달리 영역의 크기가 2배수로 동적으로 운영 (3가지 타입)
- 실행 : `java -XX:+UnlockExperimentalVMOptions -XX:+UseZGC -jar Application.java` |

---

### 3. JVM관점에 static을 왜 조심해서 써야하는지 서술하시오

- static 변수는 우선 자바 Method 영역에 저장된다. 하지만 전부 static으로 선언하거나 무분별하게 static 변수를 많이 사용할 경우 메모리 부족 현상이 발생할 수 있다(Out of Memory). Heap 영역이 아니기 때문에 GC의 영향을 받지 않아 프로그램이 끝나지 않는다면 데이터는 계속 메모리에 적재되기 때문이다.

--- 

# 👀 느낀 점

한 4시까지는 너무 정신이 없어서 제대로 정리를 못 했다. 1, 3번만 했다가 새로운 팀 빌딩을 하고 자체적으로 뇌의 쉬는 시간을 잠시 주었다가 다시 재개를 했다. 부캠 시작하고 스스로 공부하는 이 과정이 좀 재밌게 느껴지는데 공부할 주제가 데일리로 명확한 것 같아서 그런 것 같다. 생각해 보니 대학생이 되고 규칙적으로 잔잔히 길게 공부한 적이 잘 없다. 시험 기간에 닥쳐서 공부하는 게 일반적이거나 억지로 스터디를 모아서 일주일에 한 번이라도 진행하는 것도 6개월이 최대였다. 매일 이렇게 할 내용이 정해지니 무엇을 해야할지 명확하다는 점이 너무 마음에 든다.

모든 개념에 공식 문서를 보는 게 좋겠지만 사실 쉽지 않은 부분이다. 그래서 오늘은 최대한 구글 검색 결과 첫 페이지는 다 보려고 했다. 우선 나의 답부터 전부 써내려나간 후 인터넷 조사를 시작했다. 답이 쉽게 안 나오는 경우에는 우선 냅다 찾아보긴 했지만 자료 조사는 이렇게 하는 거구나 싶다. ‘신빙성 있는 자료 조사? 그거 어떻게 하는 건데? 나무 위키만 안 쓰면 되는 거 아니야? 많은 정보를 보고 그러다보면 그 중에 맞는 정보만 눈에 보이는 게 어쩌면 당연하겠구나’ 하는 게 나의 결론이다.

다만 한 가지 아쉬운 점은 어디까지 가야 깊은 건지 잘 모르겠다. 어제 공식 문서 보기에 갖혀 막상 중요한 파고들기를 잘하지 못한 느낌이다. 스스로에게 질문하고 답하는 방식을 세세하고 깊게 하면 될 것 같긴 한데 잘 되는 것 같지 않은 느낌? 다른 분들의 한 줄 정리를 보았는데 내가 하는 자료 조사는 좀 얕다는 느낌을 받았다. 아직 많이 부족한 듯? 맥북 첨 써보는 게 신나서 좀 덜 진지하지 않았나? 싶다.

그리고 자료조사에 있어 이리 갔다 저리 갔다 계속 하는데 한 블로그 내에 있는 링크를 또 타고 들어가고 또 타고 들어가다 보면 양이 많아지기도 하고 틀린 내용도 보여서 지우고 그러다가 또 돌아오고 이러다 보니 너무 시간을 오래 잡아 먹는데 딥다이브 자체는 취지가 좋지만 그걸 효율적으로 잘 활용하지 못 하는 기분도 좀 든다.