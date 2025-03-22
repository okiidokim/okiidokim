# 📌 Vanilla JS로 Virtual dom을 구현한다면 고려해야할 내용 (hint : Reconciliation)

## ▶️ Reconciliation : 재조정

[참고 자료]<br>
https://ko.legacy.reactjs.org/docs/reconciliation.html<br>
https://namansaxena-official.medium.com/react-virtual-dom-reconciliation-and-fiber-reconciler-cd33ceb0478e

- virtual DOM : 실제 DOM의 가벼운 가상 복제본
    - 실제 DOM 조작은 비용이 많이 들기 때문에 가상 DOM을 사용하여 변경사항만 실제 DOM에 반영하도록 계산한다.
- 재조정 : 가상 DOM을 기반으로 current virtual DOM (기존 화면에 보이는 DOM)과 Work In Process, 작업 후 새로운 virtual DOM을 비교하여 변경된 부분만 최소한으로 업데이트하는 과정
    - 예시
        
        ```jsx
        <h1>Hello</h1>
        
        //돔 트리 구조
        ElementNode: h1
        └── TextNode: "Hello"
        
        //변경 후
        <h1>Hi</h1>
        
        ElementNode: h1
        └── TextNode: "Hi"
        ```
        
        ElementNode부터 전체가 바뀌는 게 아닌 변경되는 최소한의 부분 TextNode만 변경
        
        리페인트와 리플로우는 발생하지 않음
        

## ▶️ 고려해야 할 내용

1. 기존 html을 vnode로 변환
    
    자바스크립트 객체 꼴로 변환하는 함수를 이용하여 변환하거나 처음부터 js 객체 꼴로 vnode를 작성
    
    type, props, children으로 구성
    
2. createElement 함수를 이용하여 vnode를 실제 DOM으로 변환
    
    맨처음 페이지를 화면에 렌더링할 때 사용되는 함수
    
3. diff 함수
    
    리액트에서는 재조정을 위해 효율적인 비교 알고리즘이 사용됨
    
    type, props, children을 순서대로 비교하여 해당 부분만 변경
    
    이때 type은 한 가지만 들어가고
    
    props는 id, class를 비교해야 하며
    
    children은 재귀적으로 비교하여 top-down으로 들어가야 함 (변경 자체는 최소한만 하위에서 이루어짐)
    
4. diff 결과를 DOM에 반영

<br><br>

# 📌 SPA에서 SEO 이슈, 대응 방법

https://www.startupcode.kr/company/blog/archives/11

https://chatgpt.com/share/67dcf05a-6af4-8012-b359-e4024ee0672a

SPA : Single Page Application

한 번에 모든 페이지를 하나로 렌더링하는 어플리케이션 - 리액트, vue.js 등에서 지원

버추얼 돔 특징 : 변경되는 부분에 있어서만 해당 노드를 업데이트 함 (페이지를 전체 새로고침하지 않음)

<br>

## ▶️ SEO 이슈

### 기존 정적 html 방식

```jsx
//page a
<html>
    <head>
         <meta>
            <title>PAGE A</title>
         </meta>
    </head>
    <body>
         <div> <a href="/page-b">b로 이동</a></div>
    </body>
</html>

//page b
<html>
    <head>
         <meta>
            <title>PAGE A</title>
         </meta>
    </head>
    <body>
         <div> <a href="/page-a">a로 이동</a></div>
    </body>
</html>
```

- 한 번에 페이지 렌더링을 하나씩 하기 때문에 애초에 돔 트리가 새로 만들어지고 head 태그 안에 있는 내용이 바로 저장되기 때문에 검색엔진에 반영됨
- 돔 트리는 아예 새로 생성됨

### 현재 동적으로 페이지를 불러오는 방식

모든 페이지를 하나로 렌더링하고 페이지에 따라 나머지를 동적으로 렌더링하기 때문에 head 태그 내부 요소는 처음에 저장되지 않아 비어있어 SEO 검색엔진에 적용되지 않음

[리액트 예시]

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import { RouterProvider } from 'react-router-dom';
import router from './router';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>
);
```

- `document.getElementById('root')` : 동적으로 실행되게 하는 함수
    - 얘를 매 페이지마다 세팅하면 비효율적이니까 프레임워크에서는 버츄얼 돔이라는 개념을 사용하게 됨
    - 버츄얼 돔은 나중에 더 알아보는 걸로…
- 프레임워크가 없으면 얘를 보통 <script>태그 안에서 매번 이벤트마다 함수를 쓰게 됨
- router 객체에 있는 모든 걸 root에 한 번에 하나로 렌더링하는 게 spa

```jsx
import { createBrowserRouter } from 'react-router-dom';
import Login from './pages/user/Login';
import Signup from './pages/user/Signup';
import BoardList from './pages/board/boardList';
import BoardDetail from './pages/board/boardDetail';
import BoardPost from './pages/board/boardPost';
import InfoEdit from './pages/user/infoEdit';
import PwEdit from './pages/user/pwEdit';

const router = createBrowserRouter([
  {
    path: '/',
    element: <Login />,
  },
  {
    path: '/signup',
    element: <Signup />,
  },
  {
    path: '/board',
    element: <BoardList />,
  },
  {
    path: '/board/:boardId',
    element: <BoardDetail />,
  },
  {
    path: '/board/post',
    element: <BoardPost />,
  },
]);

export default router;

```

- 이렇게 되면 한 파일에서 정의한 SEO 정보가 검색엔진에 적용되지 않음
    
    ```jsx
    <Helmet>
    	<title>검색엔진 타이틀</title>
    	<meta name="description" content="검색엔진 내용" />
    </Helmet>
    ```
    
    - 리액트에서는 Helmet이 document.head로 변환되며 head 태그 내부 내용이 동적으로 업데이트 됨
    - SEO에서 불리한 이슈 발생

<br>

## ▶️ SPA에서 SEO 대응 방법

1. SSR(server side rendering)
    - next.js에서 지원 - 리액트는 client side rendering framework
        - 어제 미니가 보여준 예시가 생김새 느낌이 대응됨
            - 함수명이 동일한 건 아닌데 next.js 특성상 로직같이 동작하는 부분이 들어감
            - 그래서 next.js가 서버의 특성을 가짐
        - https://github.com/PhantomOfLINUX/POL_Front/blob/develop/src/store/problemStageStore.ts
        - https://github.com/PhantomOfLINUX/POL_Front/blob/develop/src/hooks/useGetXtermUrl.tsx
    - SPA가 아님
    - `getServerSideProps()`
    - 매 요청마다 새로운 html, DOM 생성 - 정적
    - 요즘 개인화된 서비스 같은 경우에는 ssr이 유리
2. SSG
    - next.js에서 지원
    - 배포할 때만 유효
        - 배포 환경에서는 배포할 때 모든 html을 생성해 둠
        - 새로운 페이지에서 html을 가져와 렌더링
    - 개발 환경에서는 새 페이지에 접속할 때마다 새 DOM, 새 html을 생성 - 정적 html
    - SPA가 아님
    - `geteStaticProps()`
    - 변경되는 내용이 잦으면 매번 새로 빌드해야 하기 때문에 불리 → ISR이라는 걸 만듦
        - ISR은 시간 상 조사 생략…

초기 페이지 렌더링에서만 유효하므로 get 메소드에서만 유효함

나머지는 useEffect를 사용하든 리액트에서 버추얼 돔을 사용하여 일부가 업데이트되는 것은 동일함

3. Pre-rendering
    - 리액트에서 사용 가능 (CSR)
    - `npm i react-snap`
    - 배포환경에서 적용
    - html 스냅샷을 만들어줌 (html 텍스트 구조)
    - seo 적용을 위해 스냅샷을 배포 환경에 미리 올려둘 뿐 - meta 태그, title 태그를 반영하게 됨
    - 동작은 그냥 리액트랑 똑같이 동작함
    
    [기존 package.json]
    
    ```jsx
      "scripts": {
        "start": "react-scripts start",
        "build": "react-scripts build",
        "test": "react-scripts test",
        "eject": "react-scripts eject"
      },
    ```
    
    [개발자의 수정이 필요한 package.json]
    
    ```jsx
    {
      "scripts": {
        "build": "react-scripts build",
        "postbuild": "react-snap" //직접 추가
      },
      "reactSnap": { //직접 추가
        "inlineCss": true,
        "routes": ["/", "/signup", "/login", "/board", ...~] //페이지별로 라우트 추가
      }
    }
    ```
