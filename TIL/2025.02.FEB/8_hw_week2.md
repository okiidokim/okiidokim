## ☝️ 주제 정하기

CLI 프로그램이기 때문에 다른 사람과의 상호작용을 이용하지 않는 선에서 주제를 생각해야 했다. 그러다 보니 생각나는 거라고는 단순 시간 출력 밖에 없었다. 하지만 기존 1주차와 연결 지으려니 설계도 안 되고 아무리 과제라 해도 서비스라고 생각하고 맥락은 있어야 하니까 적어가면서 진행했다.
![Image](https://github.com/user-attachments/assets/8a7b0d28-c88b-477a-aff3-d513936d199d)

- 수량 감소
    - 다른 사람의 주문이 중간에 접수돼야 하는데 배포하지 않고 한 사용자에 대해서만 실행 가능한 CLI 특성 상 그럴 수 없어서 제외
- 쿠폰 발급 알림
    - 쿠폰도 결국 관리자의 동시 실행이 필요한데 마찬가지로 CLI 특성상 제외
    - 근데 쿠폰도 결국 기간 제한이 존재하는 아이템이기 때문에 적용해 보자 하는 생각으로 가게 됨
- 앱 사용 시간 경과
    - 너무 뻔해서 아이디어를 넣고 싶었음

→ 갑자기 Temu 어플이 떠오름<br>
  : 룰렛을 돌리고 24시간 내 결제하면 이래저래 할인 및 무료 선택 혜택을 주는 쇼핑 어플

### [쿠폰 발급 + 시간 경과를 적용하자!!]
결제하는 시점의 스레드와 엮어서 상호작용을 구현할 수도 있을 것이라고 생각함

<br>

## 설계
![Image](https://github.com/user-attachments/assets/335a480f-9218-4ea6-8569-3d643cf7623c)

스레드를 적용하는 부분은 감이 잘 오지 않는다.
<br>시간 감소에 사용은 해야 할텐데 아직 코딩과 개념의 융합이 잘 되지 않아 구현하면서 적용해야 할 듯하다.<br>
우선 새로운 기능이 추가되는 부분에 대해서만 클래스 다이그램을 그려봤다.
물론 구현하면서 더 추가되긴 했지만 이번엔 그래도 함수까지 어느 정도 써봤다.<br>
그리고 설계까지 하는데 시간이 오래 걸리지 않았다.<br>
그새 한 번 적용해 봤다고 투자하는 시간이 확 줄어든 것이 체감될 정도였다.<br>
여기서 반복의 중요성을 다시 한 번 깨닫는다..~~<br>

<br><br>

## ☝️ 현 상황

우선 쿠폰 관련해서는 구현이 끝났다. 스레드로 시간 제한도 걸어두었고 이래저래 잘 출력되는 것 같다.<br>
결제 부분에 스레드 적용하는 걸 구현해야 할 단계인데 이제 막 좀 감이 오는 중<br>
사실 처음에 쿠폰 부분 구현하면서는 이걸 다른 스레드랑 어떻게 연결하지 이러고 있었는데 생각보다 결제 부분에서 해 주면 되겠다는 생각이 바로 들었다.<br>
암튼 결제 부분 60%(?) + 쿠폰 스레드 잠시 중단(?) 이렇게 남은 듯하다.<br>

밤 11시 반까지만 해도 그랬는데 지금은(새벽 12시 30분) 또 interrupt() 함수 써서 예외 처리 시키니까 또 스레드 간 상호 작용이 필요 없어 보인다.<br>
사실 상 상호작용 생각 안 하면 여기서 구현을 끝내도 될 것만 같다.<br>
그래도 금액을 건드리는 것과 결제되는 시점과 사용 시간 만료되는 시점에 대한 고민을 좀 더 해봐야겠다.<br>
암튼 그래도 큰 산은 넘었으니 오늘은 이만 자고 내일 생각해 보려고 한다.

아 그리고 지금 새벽 1시 반...<br>
갑자기 생각난 게 지금 결제되는 전체 금액을 cart 클래스를 수정했는데 Payment 클래스에서 수정해야 하는 건가? 급 고민...
근데 사실 결제 금액 자체를 건드리는 건 cart 클래스가 맞는데...<br>
이것도 암튼 내일 생각해 보는 걸로

<br>

## 📚 구현 참고 자료
- [참고 자료] https://oscarstory.tistory.com/19<br>
- [참고 자료] https://parkadd.tistory.com/48<br>
- [참고 자료] https://javachoi.tistory.com/186<br>

<br><br>

---

<br>

## 👀 느낀점

저번주에 자바 프로그래밍 한 번 했다고 추가 구현 부분의 클래스 다이어그램도 훨씬 잘 그려지고 코딩도 술술 되는 느낌이어서 너무 신기했다.<br>
그리고 1주차 때 InputCheck 클래스를 만들어놨었는데 이번에 진짜 사용자 입력 유효검사할 때 함수만 아무렇지 않게 가져다 쓰는 나의 손을 보며 '아...!!! 이게 유지보수성이구나!!!!'를 진짜 처음 느껴봐서 너무 급 행복했다.<br>
난 정말 프론트엔드보다 백엔드가 하고 싶은 게 맞는 것 같다. 새로운 배움의 시작이 늦다는 생각도 들지만 과제의 고통도 있지만 덕분에 또 한 번 이렇게 기대되는 기분을 선사받았다.<br>
그리고 저번에 팩토리 패턴 쓰고 싶은데 Map을 생각하지 못했어서 GPT를 썼었는데 이번에는 처음에 스레드 어떻게 써야 하는지 실습 했는데도 진짜 감 안 와서<br>
검색 키워드 막 바꿔가면서 검색하니까 아이디어도 떠오르고 갑자기 좀 수월하게 흘러가서 기분이 좋았다.

근데 오늘 14시간을 잤다.<br>
어제 저녁에 친구 만나고 집 왔는데 잠 잔 기억이 없다.(술 안 마심;;) 일어나니 폰은 꺼져있고 ㄹㅇ 체력 이슈로 걍 뻗어버린 것이다.<br>
그래도 목금 컨디션 안 좋다 느끼고 있긴 했는데 진짜 14시간 자고 좀 띵가띵가 쉬니까 다 괜찮아진 기분이다.<br>
근데 월, 금 개념 정리 안 된 거 지금 댕 사고인데 내일 빨리 과제 끝나고 구현할 수 있기를..!!!<br>
2주차인데 벌써 이렇게 밀리는 게 생기다니!!!!!!!!<br>

그리고 exp 매달 랭킹 리셋되던데<br>
1월에 뭣도 모르고 학습 계획 무한 생성하고 무한 피드백 남겼었는데 아 살짝 수치스러웠다.<br>
그래서 진짜 이건 계획이다 싶은 것만 큰 묶음으로 묶어서 하나씩 실행해 나가고 있다.<br>
투두메이트만 주구장창 쓰다가 뭔가 세미장기 계획에 사소한 할 일을 하나씩 전부 채워넣고 하나씩 해결해 나가는 게 더 뿌듯한 기분도 든다.<br>
원래 계획이란 건 내 인생에 없었지만 뭐 이참에 J력 10퍼센트 정도만 올려보자고~ (그래도 P인 게 함정^^)

진짜 컨디션 조절 너무 중요하다고 느꼈고 오늘 잠 와방 자서 어느 정도 해결했으니 앞으로 잘 해보자고~<br>
(다짐 특 : 다음 날 일어나면 리셋됨)

### 🕰️ 오늘의 라이즈
<img width="423" alt="Image" src="https://github.com/user-attachments/assets/ee463ad4-10fa-4bdc-9045-07e1a69336cc" />
