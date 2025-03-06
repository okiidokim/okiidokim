## ✏️ 과제 내용
### ⬇️ 3, 4주차 커뮤니티 과제의 ERD 그려보기<br>
https://www.erdcloud.com/d/dZ8rMnXWZQavdgmD9

<img width="941" alt="image" src="https://github.com/user-attachments/assets/36911898-caa2-4e00-885d-c323ce5c2c8a" />

<br><br>

### ▶️ MySQL 설치하기<br>
XAMPP 설치가 안 되길래 그냥 인터넷에서 MySQL 설치하는 방법을 따로 찾아서 설치했다.<br>
https://velog.io/@cyseok123/MySQL-%EC%84%A4%EC%B9%98-%EB%B0%8F-%EC%8B%A4%ED%96%89-for-Mac<br>
여기 링크 참고해서 연결 성공까지 도달했다.<br>
<img width="264" alt="스크린샷 2025-03-06 오후 6 52 27" src="https://github.com/user-attachments/assets/5bb2f652-3780-4a00-938b-31adbf328b56" />
<br>
근데 항상 느끼지만 환경 세팅할 때 제일 겁난다...<br>
뭐 안 될 때마다 노트북 초기화해야 될까봐 겁나는...<br>
그래도 잘 해내서 다행

<br>

---

<br>

## 👀 회고
1. 자료형 이슈 [int, datetime]<br>
   이번 수업 때 DB 이론 하면서 자료형도 배웠는데 이 부분 정리하는 걸 깜빡했다.<br>
   키워드 정리까지는 하는데 추가질문까지 정리하면 항상 새벽까지 많이 넘어가서 잊고 있었다.<br>
   varchar나 datetime, int로 대충 항상 다 표시가 됐어서 지금까지 대충 그려왔었는데<br>
   이번에 추가 질문 중에 varchar vs char 질문이 있던 게 기억나서 그것도 알아보는 겸, DB 자료형에 대해 알아보았다.<br>
   우선 둘 차이가 있다는 것도 놀랐고 항상 varchar만 썼었는데 가변이라는 점에서 그 이유를 알았다.<br>
   그리고 사실 int도 number로 기억하고 있었는데 다시 알아보니 int였던 것...ㅎㅎ<br>

   그리고 timestamp와 datetime도 다르게 있다는 걸 알게 되었다.<br>
   이것도 거의 항상 datetime을 익숙하게 봐와서 그냥 썼는데 실제로도 datetime을 더 많이 사용한다고 한다.<br>
   범위의 차이가 있는데 timestamp는 1970~2038년이라고 한다.<br>
   그리고 기본적으로 CURRENT_TIME을 저장하면 utc 기준으로 저장하고 나중에 한국 기준으로 변환해야 한다.<br>
   반면 datetime은 mysql에서 아래 sql문으로 시간 기준을 설정할 수 있다고 한다.<br>
   
   ```sql
   SET GLOBAL time_zone = 'Asia/Seoul';
   SET time_zone = 'Asia/Seoul';
   ```
   이렇게 설정하면 datetime을 사용할 때는 NOW()로 저장할 때 그냥 한국 기준으로 된다는 게 아주 신기했다.
   
2. 긍정적인 부분<br>
   생각보다 금방 했다.<br>
   이래저래 생각보다 익숙했고 항상 erdcloud만 썼었어서 그것도 익숙했다.<br>
   근데 자동으로 sql문 다 뽑아주는 게 너무 꿀이라서 sql 공부 안 해도 될 듯?이라 생각했던... 과거의 나... 반성해...<br>
   그리고 백엔드 해보고 싶다고 느낀 것도 DB 좀 재밌던 기억이 있어서 관심 간 것도 없지 않아 있었기 때문에<br>
   암튼 순탄하게 잘 흘러가서 아주 좋았다.

3. ERD 작성 흐름(?)<br>
   처음에 BoardLike 테이블은 안 그리려고 했다.<br>
   우선 머리 속에서 대충 구상할 때 보드, 댓글, 유저만 있으면 되겠다고 생각했었는데<br>
   사용자가 좋아요 취소하고 다시 누르고 이걸 효율적으로 하려면 속성이 필요하겠다고 생각을 했는데<br>
   다른 테이블에 넣는 건 아니겠다는 결론을 내림<br>
   그래서 그냥 like 테이블 만들고 isLike 불리언 타입 속성도 넣어야 하나 했는데<br>
   그냥 post, delete로 해서 안 넣기로 했다.<br>
   api 작성한 거에는 수정으로 들어가게 했는데 그러면 좋아요 삭제 후에도 계속 튜플이 남아있어야 하고 로직도 조건 걸아야 하고<br>
   굳이 그럴 필요 없을 것 같아서 그냥 외래키만 연결시켰다.<br>

   그리고 처음에 Board 테이블에 likeCount, viewCount, commentCount를 넣어놨었는데 생각해 보니
   select count(*) from Comment 해서 반환해 주면 되니까 viewCount 빼고 속성 두 개는 과감히 지워버렸다.<br>

4. json과 DB의 차이<br>
   이번 실습 전까지 큰 차이가 없을 거라고 생각했다.<br>
   프론트에서 반환받는 json이나 DB나 걍 똑같은 거 아닌가 하고 4주차 프로젝트 때 작성한 json 파일을 보았는데<br>
   완전 내가 사용하고 싶은 관점에서 반환한다는 가정 하에 작성한 게 보였다.<br>
   
   comment 배열이 board 안에 그냥 들어가있다든가, <br>
   내 글, 댓글인지 아닌지를 백엔드에서 처리한다고 가정하고 반환한다든가<br>
   뭐 생각보다 백엔드의 처리를 가정하고 프론트 맛대로 작성했구나 하는 게 보여서 좀 신기했다.<br>

   근데 또 웃긴 건 erd 그릴 때 당연하게 Board랑 Comment 테이블을 같이 둘 생각이 전혀 없었다.<br>
   이미 무의식에서는 둘을 별개로 본 것 같은데 하나의 개인 프로젝트로 순차적으로 해보니 인식된다는 점이 재밌다.<br>
      
