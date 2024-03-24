## lesson6

### 참고 레포지토리

- https://github.com/gitdagray/react_redux_toolkit/tree/main/06_lesson

### 초기 세팅

1. 새로운 프로젝트를 만든다.
2. 이 친구들 설치하기. 폰트와 아이콘 관련 패키지

```
    "@fortawesome/fontawesome-svg-core": "^6.5.1",
    "@fortawesome/free-solid-svg-icons": "^6.5.1",
    "@fortawesome/react-fontawesome": "^0.2.0",
```

1. data/db.json 생성하기.
   - 참고 레포지토리에서 복사해오기
2. npm i json-server -g
   - 빠르게 rest api 서버 구축해주는 라이브러리.
   - 글로벌로 설치하는 이유는 패키지에 남기지 않기 위해서임. 어디에서나 접근해서 사용할 수 있기 때문이기도 함.
   - json-server --watch data/db.json --port 3500
     - data/db.json 데이터로 3500번 포트에 포그라운드로 서버 열기.
3. features/todos/TodoList.jsx 생성
   - 코드
     ```jsx
     // add imports
     import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
     import { faTrash, faUpload } from "@fortawesome/free-solid-svg-icons";
     import { useState } from "react";

     const TodoList = () => {
       const [newTodo, setNewTodo] = useState("");

       const handleSubmit = (e) => {
         e.preventDefault();
         //addTodo
         setNewTodo("");
       };

       const newItemSection = (
         <form onSubmit={handleSubmit}>
           <label htmlFor="new-todo">Enter a new todo item</label>
           <div className="new-todo">
             <input
               type="text"
               id="new-todo"
               value={newTodo}
               onChange={(e) => setNewTodo(e.target.value)}
               placeholder="Enter new todo"
             />
           </div>
           <button className="submit">
             <FontAwesomeIcon icon={faUpload} />
           </button>
         </form>
       );

       let content;
       // Define conditional content

       return (
         <main>
           <h1>Todo List</h1>
           {newItemSection}
           {content}
         </main>
       );
     };
     export default TodoList;
     ```
4. index.css 복사
   - 코드
     ```jsx
     @import url('https://fonts.googleapis.com/css2?family=Nunito&display=swap');

     body {
       font-family: 'Nunito', sans-serif;
       font-size: 1.5rem;
     }

     input[type="text"], button {
       font: inherit;
     }

     main {
       margin: auto;
       max-width: 600px;
     }

     h1 {
       margin-bottom: 0.5rem;
     }

     article {
       padding: 1rem;
       display: flex;
       justify-content: space-between;
       align-items: center;
       border: 1px solid hsl(0, 0%, 58%);
     }

     .todo {
       display: flex;
       justify-content: flex-start;
       align-items: center;
     }

     input[type="checkbox"] {
       min-width: 30px;
       min-height: 30px;
       margin-right: 1rem;
     }

     button {
       min-width: 50px;
       min-height: 50px;
       border: 1px solid #333;
       border-radius: 10%;
       cursor: pointer;
     }

     .trash {
       background-color: #fff;
       color: mediumvioletred;
     }

     .trash:focus, .trash:hover {
       filter:brightness(120%)
     }

     form {
       padding: 1rem;
       display: flex;
       justify-content: space-between;
       align-items: center;
       border: 1px solid #333;
       margin-bottom: 1rem;
     }

     form label {
       position: absolute;
       left: -10000px;
     }

     .new-todo {
       width: 100%;
       padding-right: 30px;
     }

     input[type="text"] {
       width: 100%;
       padding: 0.5rem;
       border-radius: 10px;
       border: 0.5px solid #333;
     }

     .submit {
       background-color: gray;
       color: #fff;
     }

     .submit:focus, .submit:hover {
       background-color: limegreen;
     }
     ```
5. features/api/apiSlice.js 생성
   - 참고 레포지토리에서 복사해오기

### 새로운 데이터 반영

- 새로운 리스트를 추가하거나 업데이트, 딜리트 하면 화면에 바로 반영이 되지 않는다. api서버에서 이전에 캐시해 둔 버전의 데이터를 여전히 가져오기 때문이다.
- 태그를 캐시에 전달해서 캐시를 무효화하고 새롭게 데이터를 fetch해오게 해야한다.
- 처음 쿼리를 할 때, 태그를 지정한다. 데이터를 builder.mutation하면서 지정해 놓은 invalidatesTag를 걸어두면 새롭게 데이터를 fetch해온다.
