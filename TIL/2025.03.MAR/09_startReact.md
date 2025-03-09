### 1️⃣ 깃허브 organization/레포지토리 파기 (생략 가능)
   - 로컬로 클론하기
    
### 2️⃣ 리액트 환경 세팅하기

- 기본 환경 세팅
  - 터미널 (Cmd + ~) 켜서 아래 명령어 입력 
  ```bash
    npx create-react-app [app-name]
    # 원하는 app-name으로 프로젝트 생성
    ```
    
- 리액트를 처음 사용 or 세팅된 거 클론했는데 안 되는 사람의 경우
    
    ```bash
    npm i react react-dom
    # react, react-dom 라이브러리 설치
    ```
    
- 추가로 필요한 라이브러리 설치 (개인적으로 자주 사용)
    
    ```bash
    npm i react-router-dom #가상 돔 - 라우팅에 용이 (거의 필수)
    npm i styled-components #css 가독성 향상
    npm i react-icons #아이콘 사용에 편리
    ```
    
###  3️⃣ 기본 세팅 변경하기 (개취)
 - public 폴더에 있는 index.html 수정 (나중에 해도 됨)
      - 원하는 로고 사진 넣기
      - 첫 link태그 속성 중 사진 변경하기
      - 크롬 상단에 페이지 타이틀, 아이콘 반영되는 거임
  - src 폴더 내부 정리하기
      1. 쓸 데 없는 js 폴더 지우기
      	- index.\*, App.* 제외 거의 다 지우는 듯
        - App.\*도 필요 없는 경우 삭제함
      2. index.js → 가상 돔 세팅하는 파일로 수정 필요
            
           ```javascript
            import React from 'react';
            import ReactDOM from 'react-dom/client';
            import './index.css';
            import { RouterProvider } from 'react-router-dom';
            import router from './router';
            
            ReactDOM.createRoot(document.getElementById('root')).render(
              <React.StrictMode>
              	<RouterProvider router={router} />
              </React.StrictMode>
            );
           ```
            
      3. router.jsx 새로 파일 만들어서 작성
            
           ```javascript
            import { createBrowserRouter } from "react-router-dom";
            import PageOne from "./pages/PageOne";
            import PageTwo from "./pages/PageTwo";
            
            const router = createBrowserRouter([
              {
                path: "/one",
                element: <PageOne />,
              },
              {
              	path : "/two",
                element : <PageTwo/>,
              }
            ]);
            
            export default router;
           ```
            
          - router를 export하고 index.js에서 (import)불러와서 사용
          - Context에 필요한 태그를 삽입할 수도 있음 (비추)
        
      4. router path에 children 넣고 싶으면 App.jsx에서 변하는 부분의 가장 하단에 `<Outlet/>` 추가하기
       	- App.jsx 
        ```javascript
            import React from 'react';
            import './App.css';
            import { Outlet } from 'react-router-dom';
            
            function App() {
            	return(
                	<div>
                  		<header>헤더</header>
                  		<main>
                  			//구현
                  			<Outlet/>
                  		</main>
                  	</div>
                );
            }
            export default App;
           ```

           - router.jsx
            
            ```javascript
            import { createBrowserRouter } from "react-router-dom";
            import App from "./App";
            import Main from "./pages/Main";
            
            const router = createBrowserRouter([
              {
                path: "/",
                element: <App />,
                children: [ //App에서 Outlet 사용했을 때만 가능
                	{
                		path : "main",
                		element : "<Main/>",
              		},
                ]
              },
            ]);
            
            export default router;
            ```
        

### 4️⃣ 내가 흔히 쓰는 디렉토리 구조 (src 내부)
  - asset 폴더
      - fonts
      	- font.css
      - images
      	- *.svg
        - *.png
  - components 폴더
  - pages 폴더
  - App.jsx
  - App.css
  - index.js
  - index.css 
  - router.jsx

마찬가지로 App.jsx, App.css는 필요에 따라 필요하지 않을 수도 있음<br>
api나 상태 관리를 위한 파일 및 폴더는 나중에 구현하면서 추가함

---

https://velog.io/@okiidokim/React-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0<br>
벨로그 처음 써봤다.<br>
첫 벨로그!!!<br>
뭔가 좀 구리긴 하지만 그래도 신기함

6주차 과제 하다가 나중에 api 연결하기 전에 마이그레이션 하는 게 편하겠는데?<br>
싶어서 미리 환경 세팅하다가 정리해 봤다.<br>
사실 스프링 환경 세팅도 제대로 안 해서 마이그레이션 벌써 생각하는 게 웃기긴 하지만<br>
뭔가 스프링 잘 안 돼서 잠시 숨 돌리려고 리액트 좀 끄적여봤다.

router 설정할 때 children 없이도 대충 잘 해 오긴 했는데<br>
router에 children 넣을 때 오류 나는 이유를 알고 싶어서<br>
알아보다가 Outlet의 존재를 이제 알아버림

암튼 다시 DB랑 스프링... 제대로 해 보자고우...
