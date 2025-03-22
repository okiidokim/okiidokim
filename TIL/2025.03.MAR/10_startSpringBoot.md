## 1️⃣ https://start.spring.io/ 접속해서 스프링부트 프로젝트 만들기 / intelliJ에서 프로젝트 생성

이전에 깃 레포지토리 만들고 클론 → 새로 만들기 → "모듈…" 클릭해서 내부에 프로젝트 생성하기

![](https://velog.velcdn.com/images/okiidokim/post/bba426a4-0d0c-4195-b4f0-24b2ef4463e7/image.png)

1. 서버 url 보면 start.spring.io인 걸 볼 수 있음 → 결국 둘이 똑같은 거임
    그래서 그냥 intelliJ에서 시작하는 게 편하다~
    “다음” 누르기
    
![](https://velog.velcdn.com/images/okiidokim/post/d5fd726b-ae58-4572-bcd4-cb60b18891df/image.png)

2. 이후 종속성 선택 (이 정도 5개는 선택해 줘야..~~) → “생성” 누르면 아래 사진 뜸

![](https://velog.velcdn.com/images/okiidokim/post/f96c4308-a500-454f-9cb0-c9454e84c51a/image.png)

<br>

## 2️⃣ SpringBoot - DB 연결

- .env 설정
    
    ```bash
    DB_URL=jdbc:mysql://localhost:3306/db-name?useSSL=false&serverTimezone=Asia/Seoul
    DB_USERNAME=username
    DB_PASSWORD=password
    ```
    
    프로젝트 루트 디렉토리에 `.env` 파일 생성
    
    useSSL : 보안 설정 같은 건데 로컬 DB일 경우에는 필요 없으니까 false
    
    serverTimezone은 한국으로 설정 ㄱㄱ

    .gitignore에 .env 추가

    application.properties에 아래 내용 추가

  ```bash
  spring.application.name=springBootStarter

  spring.config.import=optional:file:.env[.properties]

  server.port=8080
  spring.devtools.livereload.enabled=true
  spring.devtools.restart.enabled=true
  spring.jpa.hibernate.naming.physical-strategy=org.hibernate.boot.model.naming.CamelCaseToUnderscoresNamingStrategy

  spring.datasource.url=jdbc:mysql://localhost:3306/ktbWeek6DB?useSSL=false&serverTimezone=Asia/Seoul
  spring.datasource.username=ktbWeek6DB
  spring.datasource.password=rlaehgus1213
  spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

  spring.jpa.hibernate.ddl-auto=update
  spring.jpa.properties.hibernate.format_sql=true
  spring.jpa.show-sql=true
  
  spring.sql.init.mode=always
  ```
  ${DB_URL}처럼 .env파일에 있는 변수명 가져다 쓰면 스프링에서는 자동으로 매핑 함
        
⇒ 근데 env 파일 properties에 적용 잘 안 되는 경우도 있어서 인텔리제 자체 환경변수 설정 기능 이용하면 훨씬 빠름
    
<br>

## ▶️ 디렉토리 구조!

```bash
projectName
 ├── src/main/java/com/example/projectName
 │    ├── controller    
 │    ├── service       
 │    ├── repository    
 │    ├── entity        
 │    ├── config      
 │    ├── dto          
 │    ├── SpringBootStarterApplication.java  
 ├── src/main/resources
 │    ├── application.properties 
 ├── build.gradle  
 ├── settings.gradle
 ├── .gitignore  
 ├── .env  
```

- `controller` → 클라이언트 요청을 처리
- `service` → 비즈니스 로직을 담당
- `repository` → DB와의 데이터 처리
- `entity` → DB 테이블과 매핑되는 클래스
- `dto` → 클라이언트와 데이터 주고받을 객체
- `config` → 설정 관련 클래스

만약에 위에서 프로젝트 만들 때 적절한 dependencies 모듈을 추가하지 않았다면,<br>
빼먹은 게 있다면 build.gradle에 수동으로 입력해서 넣은 후<br>
intelliJ 우측 Gradle 아이콘 누르고 새로고침(refresh) 하기<br>

<br>

## 3️⃣ DTO, Repository, Service, Controller 작성
   - **Entity**
   	- `import jakarta.persistence.*`
     - `@Entity`
     - `@Table(name = "실제 테이블 이름")`
     - DB 테이블 속성 그 자체로 실행 시 자동으로 DB table을 생성함
     - `@Column`을 이용해서 PK, auto_increment 등 도메인 설정 가능
     - 카멜 형식으로 정의하였더라도 자동 snake("_")방식으로 컬럼 정의됨
     - jpa로 DB 연결할 경우`application.properties`에 아래 내용 추가
    
   ```java
   spring.jpa.hibernate.ddl-auto=update //없으면 생성 있으면 업데이트
   spring.jpa.properties.hibernate.format_sql=true
   spring.jpa.show-sql=true
   ```        
   
   ```java
   //User.java 예시
   package com.example.demo.entity;
   import jakarta.persistence.*;
   
   @Entity
   @Table(name = "users")  // 실제 DB 테이블명
   public class User {
        
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY) // 자동 증가 (Auto Increment)
        private int userId;
        
        @Column(nullable = false)
        private String userName;
        
        @Column(nullable = false)
        private String userPassword;
        
        @Column(nullable = false, unique = true)
        private String userEmail;
        
        @Column(nullable = false)
        private String userProfileImgUrl;
    }
   ```
   - **Dto**
       - ㄹㅇ 클래스 속성, getter 그 잡채
       - json 형식
       - Restful한 api 요청/응답에 필수
     
   ```java
   package com.example.demo.dto;
   
    public class UserDto {
    	private int userId;
        private String userName;
        private String userPassword;
        private String userEmail;
        private String userProfileImgUrl;
        
    }

  ```

   - **Repository**
       - `@Repository`
       1. jdbcTemplate 선언
       2. 생성자
       3. sql문 처리 및 반환 (1~3 비추, 4번으로 ㄱㄱ)
       4. JpaRepository를 상속 받아 interface로 구현하면 다 필요없고 걍 선언만 하면 알아서 flush() 호출해서 동작함
       
	```java
	package com.example.demo.repository;

	import com.example.demo.entity.User;
	import org.springframework.data.jpa.repository.JpaRepository;
	import org.springframework.stereotype.Repository;

	@Repository
	public interface UserRepository extends JpaRepository<User, Integer> {
	}

    ```


  - **Service**
       - `@Service`
       1. Repository 객체 생성
       2. 생성자
      3. 적절히 처리 후 반환 / **비즈니스 로직**
           - entity → dto 변환
           - controller에 반환

 ```java
 package com.example.demo.service;

 import com.example.demo.dto.UserDto;
 import com.example.demo.entity.User;
 import com.example.demo.repository.UserRepository;
 import org.springframework.stereotype.Service;

 import java.util.List;
 import java.util.stream.Collectors;

 @Service
 public class UserService {
     private final UserRepository userRepository;
     
     public UserService(UserRepository userRepository) {
     	this.userRepository = userRepository;
     }

	// User Entity를 UserDto로 변환
    	private UserDto convertToDto(User user) {
        	return new UserDto(user.getUserId(), user.getUserName(), user.getUserPassword(),
                user.getUserEmail(), user.getUserProfileImgUrl());
    	}

    // 전체 사용자 조회 (User -> UserDto 변환 후 반환)
    public List<UserDto> getAllUsers() {
        List<User> users = userRepository.findAll();
        return users.stream().map(this::convertToDto).collect(Collectors.toList());
    }
}

   ```

   - **Controller**
       - `@RestController`
        1. Service 객체 생성
        2. 생성자
        3. uri 매핑 `@RequestMapping`, `@GetMapping`
        4. 적절히 처리 후 반환
        
   ```java
   package com.example.demo.controller;
   
   import com.example.demo.dto.UserDto;
import com.example.demo.service.UserService;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

  import java.util.List;

 @RestController
@RequestMapping("/user")
public class UserController {
	private final UserService userService;
    	public UserController(UserService userService) {
        	this.userService = userService;
    	}
    	@GetMapping("/lists")
    	public List<UserDto> list() {
        	return userService.getAllUsers();
    	}
}

   ```

<br>

## ▶️ 자주 쓰이는 어노테이션
- `@Getter`
  속성 중 id가 정의돼있다면 자동으로 `void getId() { return id; }` 함수를 만들어주며 개발자가 직접 작성할 필요 없음<br>
  속성을 정의한 위치 바로 위에 쓰거나 클래스 전체에 대해서 클래스 위에 작성하면 됨
  
- `@Setter`
  `int setId(int id) { this.id = id; }` 함수를 자동으로 만들며 개발자가 직접 작성할 필요 없음<br>
  `@Getter`와 마찬 가지로 속성을 정의한 위치 바로 위에 쓰거나 클래스 전체에 대해서 클래스 위에 작성하면 됨

- `@NoArgsContructor`
  기본 생성자
  클래스 전체에 대해서 클래스 위에 작성<br>
  클래스 이름이 `Car`일 경우<br>
  개발자가 작성할 필요 없이 `public Car() {}`를 자동으로 생성

- `@AllArgsConstructor`
  모든 매개변수를 가진 생성자<br>
  마찬가지로 클래스 위에 작성<br>
  클래스가 User이고 속성으로 name, gender, id, age가 있다고 할 때 아래 코드를 자동으로 생성
  ```java
  public User(int id, string name, string, gender, int age) {
    this.id = id;
    this.name = name;
    this.gender = gender;
    this.age = age;
  }
  ```

  
