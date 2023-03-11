# #6 Practice movie app (1)

### To Do List (1)

첫 연습은 To Do List 만들기이다. 

```jsx
// App.js
import { useState } from "react";

function App() {
  const [toDo, setToDo] = useState("");
  const onChange = (event) => setToDo(event.target.value);
  const onSubmit = (event) => {
    event.preventDefault();
    console.log(toDo);
  };
  return (
    <div>
      <form onSubmit={onSubmit}>
        <input 
          onChange={onChange}
          value={toDo} 
          type="text" 
          placeholder="Write your to do..." 
        />
        <button>Add To Do</button>
      </form>
    </div>
  );
}

export default App;
```

기본이 될 뼈대로, input에 값(`toDo`)이 들어가면 state(`setToDo`)가 타겟의 값을 받아오는 구조.

![image](https://user-images.githubusercontent.com/106129152/224466926-d3345183-dc7f-4a04-9efc-b375a2e6c5aa.png)

이제 여기에서 추가하고 싶은 기능들은 대강 이렇다.

- Add 버튼을 누를 때 input에 값이 없으면 작동을 하지 않게
- input에 값이 있으면 리스트에 추가
- input을 비우는 기능 (리스트에 추가하면 input에 값이 남아있을 이유가 없으므로)

to do를 array에 추가해야 하는데, 주의해야할 점이 있다.

예를 들어 `const food = [1, 2, 3, 4]`일 때, food에 6만 추가하고 싶다면!

❌ `[6, food]` → `[6, array(4)]` 

⭕ `[6, …food]` → `[6, 1, 2, 3, 4]`

```jsx
import { useState } from "react";

function App() {
  const [toDo, setToDo] = useState("");
  const [toDos, setToDos] = useState([]);
  const onChange = (event) => setToDo(event.target.value);
  const onSubmit = (event) => {
    event.preventDefault();
    if (toDo === "") {
      return;
    }
    setToDo(""); // 값이 입력되면 input 에 썼던 값을 리셋
    setToDos((currentArray) => [toDo, ...currentArray]);
  };
  console.log(toDos);
  return (
    <div>
      <form onSubmit={onSubmit}>
        <input 
          onChange={onChange}
          value={toDo} 
          type="text" 
          placeholder="Write your to do..." 
        />
        <button>Add To Do</button>
      </form>
    </div>
  );
}

export default App;
```

![2023_3_11_49](https://user-images.githubusercontent.com/106129152/224466935-6cfdfad7-4e64-4919-8d34-602493fbcb70.gif)


이렇게 값을 입력하면 배열에 입력값이 잘 들어가고, 값이 들어가면 input창이 비워지는 것을 확인할 수 있다.

다음 파트로 넘어가기 전에 `<h1>My To Dos ({toDos.length})</h1>` 로 제목을 추가해주면 리스트에 몇 개가 들어있는지 제목에서 바로 확인이 가능하다.

![image](https://user-images.githubusercontent.com/106129152/224466951-0c4aae33-3c53-4b9f-a0a7-db76b58c53ea.png)


### To Do List (2)

array에 있는 값들을 각 컴포넌트로 렌더링하고 싶다.

여기서 `map()`을 사용하는데, `map()`은 하나의 array에 있는 item을 원하는 값으로 바꿔주고, 새로운 array로 return한다. → `map()`으로 **component를 return**시키고 싶음

```jsx
import { useState } from "react";

function App() {
  const [toDo, setToDo] = useState("");
  const [toDos, setToDos] = useState([]);
  const onChange = (event) => setToDo(event.target.value);
  const onSubmit = (event) => {
    event.preventDefault();
    if (toDo === "") {
      return;
    }
    setToDo(""); // 값이 입력되면 input 에 썼던 값을 리셋
    setToDos((currentArray) => [toDo, ...currentArray]);
  };
  return (
    <div>
      <h1>My To Dos ({toDos.length})</h1>
      <form onSubmit={onSubmit}>
        <input 
          onChange={onChange}
          value={toDo} 
          type="text" 
          placeholder="Write your to do..." 
        />
        <button>Add To Do</button>
      </form>
      <hr />
      <ul>
        {toDos.map((item) => (
          <li>{item}</li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```

`{toDos.map((item) => (<li>{item}</li>))}` : 여기서 함수의 첫번째 argument(`item`)는 각 todo를 의미

![2023_3_11_42](https://user-images.githubusercontent.com/106129152/224466958-aafed75d-6d0c-403f-8980-03a9d710eacf.gif)


다만 콘솔창에 아래와 같은 경고 문구가 출력되고 있다.

![image](https://user-images.githubusercontent.com/106129152/224466977-c34c92d4-8817-443f-b1fb-004c6f117642.png)

같은 component의 list를 render할 때 **key라는 prop이 필요**하다는 뜻.

→ 기본적으로 react가 list에 있는 모든 item들을 인식하기 때문이다.

이렇게 key에 index를 넣어서 수정해주면 된다.

```jsx
{toDos.map((item, index) => (
          <li key={index}>{item}</li>
        ))}
```

### To Do List 최종 코드

```jsx
import { useState } from "react";

function App() {
  const [toDo, setToDo] = useState("");
  const [toDos, setToDos] = useState([]);
  const onChange = (event) => setToDo(event.target.value);
  const onSubmit = (event) => {
    event.preventDefault();
    if (toDo === "") {
      return;
    }
    setToDo(""); // 값이 입력되면 input 에 썼던 값을 리셋
    setToDos((currentArray) => [toDo, ...currentArray]);
  };
  return (
    <div>
      <h1>My To Dos ({toDos.length})</h1>
      <form onSubmit={onSubmit}>
        <input 
          onChange={onChange}
          value={toDo} 
          type="text" 
          placeholder="Write your to do..." 
        />
        <button>Add To Do</button>
      </form>
      <hr />
      <ul>
        {toDos.map((item, index) => (
          <li key={index}>{item}</li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```
