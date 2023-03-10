# #5 Effects

현재 다음과 같은 App.js 파일이 있다.

```jsx
import { useState } from "react";

function App() {
  const [counter, setValue] = useState(0);
  const onClick = () => setValue((prev) => prev + 1);
  console.log("call an api");
  return (
    <div>
        <h1>{counter}</h1>
        <button onClick={onClick}>click me</button>
    </div>
  );
}

export default App;
```

이 코드에서 버튼을 누를 때마다 state가 변경되므로, state가 변경될 때마다 모든 code들이 재실행될 것이다. 

하지만 이걸 원치 않는다면? component가 처음 렌더링될 때만 특정 코드가 실행되기를 원한다면?

만약 API를 통해 데이터를 가져온다면, state가 변경될 때마다 계속해서 API에게서 데이터를 가져와야 할 것이다!

어떻게 해야할까? 🤔

### useEffect

이럴 때 바로 **useEffect** 를 사용하는 것이다.

useEffect는 **2개의 argument를 가지는 function**이다.

```jsx
import { useState, useEffect } from "react";

function App() {
  const [counter, setValue] = useState(0);
  const onClick = () => setValue((prev) => prev + 1);
  console.log("i run all the time");
  const iRunOnlyOnce = () => {
    console.log("i run only once.");
  }
  useEffect(iRunOnlyOnce, []);
  return (
    <div>
        <h1>{counter}</h1>
        <button onClick={onClick}>click me</button>
    </div>
  );
}

export default App;
```

직접 확인을 해보면, useEffect를 사용한 `iRunOnlyOnce`가 맨 처음에만 렌더링되는 것을 확인할 수 있다.

![image](https://user-images.githubusercontent.com/106129152/224279974-3e1753ba-f01f-4090-a7c6-50a3e02304cf.png)

아래 코드로 실행해도 똑같이 맨 처음 한번만 실행되는 것을 확인할 수 있다!

```jsx
import { useState, useEffect } from "react";

function App() {
  const [counter, setValue] = useState(0);
  const onClick = () => setValue((prev) => prev + 1);
  console.log("i run all the time");
  useEffect(() => {
    console.log("CALL THE API....")
  }, []);
  return (
    <div>
        <h1>{counter}</h1>
        <button onClick={onClick}>click me</button>
    </div>
  );
}

export default App;
```

![image](https://user-images.githubusercontent.com/106129152/224280027-bb471bdf-1faa-437f-9348-ce05aeec53df.png)

***→ useEffect : 코드가 딱 한번만 실행될 수 있도록 보호***

검색 API를 사용하고 싶은데, 글자를 타이핑할때마다 API가 새로 호출되는 것도 정말 원치않는 일일 것이다!

```jsx
import { useState, useEffect } from "react";

function App() {
  const [counter, setValue] = useState(0);
  const [keyword, setKeyword] = useState("")
  const onClick = () => setValue((prev) => prev + 1);
  const onChange = (event) => setKeyword(event.target.value);
  console.log("i run all the time");
  useEffect(() => {
    console.log("CALL THE API....");
  }, []);
  console.log("SEARCH FOR", keyword);
  return (
    <div>
        <input 
          value={keyword} 
          onChange={onChange} 
          type="text" 
          placeholder="Search here..." 
        />
        <h1>{counter}</h1>
        <button onClick={onClick}>click me</button>
    </div>
  );
}

export default App;
```

이렇게 search bar에 입력된 값을 가져오려고 할 때, 콘솔을 확인해보면…

![image](https://user-images.githubusercontent.com/106129152/224280102-7e079a83-fd97-4aa6-80b7-92f9896aa1f9.png)

우려했던 일이 일어나고 있다.

- counter가 변화할 때에도 검색하고 싶지는 않음
- 서치 키워드에 변화가 있을 때에만! 검색하고 싶음

### Deps

지금 원하는 것은, **코드의 특정한 부분만 변화했을 때, 원하는 코드를 실행하고 싶은 것**이다.

이럴 때 바로 **useEffect의 비어있던 배열을 사용**하는 것이다.

```jsx
useEffect(() => {
    console.log("SEARCH FOR", keyword);
  }, [keyword]);
```

→ `keyword`가 변화할 때 코드를 실행할 거라고 react에게 알려주는 것

여기에 조건문을 덧붙여서, 키워드가 공백이 아니고, 길이가 5글자 이상일 때 작동하게 해보자.

```jsx
import { useState, useEffect } from "react";

function App() {
  const [counter, setValue] = useState(0);
  const [keyword, setKeyword] = useState("")
  const onClick = () => setValue((prev) => prev + 1);
  const onChange = (event) => setKeyword(event.target.value);
  console.log("i run all the time");
  useEffect(() => {
    console.log("CALL THE API....");
  }, []);
  useEffect(() => {
    if (keyword !== "" && keyword.length > 5){
      console.log("SEARCH FOR", keyword);
    }
  }, [keyword]);
  return (
    <div>
        <input 
          value={keyword} 
          onChange={onChange} 
          type="text" 
          placeholder="Search here..." 
        />
        <h1>{counter}</h1>
        <button onClick={onClick}>click me</button>
    </div>
  );
}

export default App;
```

![image](https://user-images.githubusercontent.com/106129152/224280169-7b89859e-d493-46e4-a032-ae4551b15ec4.png)

아무것도 손대지 않은 맨 처음의 상태이다.

![image](https://user-images.githubusercontent.com/106129152/224280201-cdb1859a-386d-46c4-80f0-3be54ec51eb3.png)

marve 까지만 입력해도 아무런 반응이 없다.

![image](https://user-images.githubusercontent.com/106129152/224280245-e2e6582c-93c7-40b1-91ba-3053ecca467e.png)

marvel 까지 입력되는 순간 작동되는 것을 확인할 수 있다!

이런 식으로 어떻게 작동하는지 눈으로 확인할 수도 있다. 

```jsx
useEffect(() => {
    console.log("I run only once.");
  }, []);
  useEffect(() => {
    console.log("I run when 'keyword' changes." )
  }, [keyword]);
  useEffect(() => {
    console.log("I run when 'counter' changes." )
  }, [counter]);
  useEffect(() => {
    console.log("I run when keyword & counter changes." );
  }, [keyword, counter]);
```

**→ useEffect의 첫번째 argument는 실행할 코드(effect), 두번째 argument는 react가 지켜봐야할 것(deps).**

useEffect에 deps가 없으면 코드가 맨 처음 한번만 실행될 것을 의미한다. deps는 array이기 때문에, 여러개가 들어갈 수 있다.

### Cleanup

```jsx
import { useState, useEffect } from "react";

function Hello(){
  useEffect(() => {
    console.log("created :)");
    return () => console.log("destroyed :(");
  }, []);
  return <h1>Hello</h1>;
}

function App() {
  const [showing, setShowing] = useState(false);
  const onClick = () => setShowing((prev) => !prev);
  return (
    <div>
      {showing ? <Hello /> : null}
      <button onClick={onClick}>{showing ? "Hide" : "Show"}</button>
    </div>
  );
}

export default App;
```

이 코드는 Show 상태일 때 Hello 라는 글씨가 출력되고, 콘솔창에 created :) 라는 문구가 출력된다. 

반대로 버튼을 다시 눌러 Hide 상태가 되면, Hello가 사라지고 콘솔창에 destroyed :( 라는 문구가 출력된다.

여기서 `return () => console.log("destroyed :(");` 이 부분을 Cleanup function이라고 부른다.

다시 같은 동작을 하는 코드를 만들어보면,

```jsx
import { useState, useEffect } from "react";

function Hello(){
  function byFn() {
    console.log("bye :(");
  }
  function hiFn() {
    console.log("created :)");
    return byFn;
  }
  useEffect(hiFn, []);
  return <h1>Hello</h1>;
}

function App() {
  const [showing, setShowing] = useState(false);
  const onClick = () => setShowing((prev) => !prev);
  return (
    <div>
      {showing ? <Hello /> : null}
      <button onClick={onClick}>{showing ? "Hide" : "Show"}</button>
    </div>
  );
}

export default App;
```

![2023_3_10_7](https://user-images.githubusercontent.com/106129152/224279840-a0288c58-0085-4beb-a95c-fa183b52ceb5.gif)


이렇게 작동한다. 

hiFn이 실행되고, component가 사라질 때 hiFn이 return한 byFn이 실행되는 것.

위 코드의 Hello function 부분을 react 사용자들이 주로 작성하는 방법으로 다시 쓰면 아래와 같다.

(보통 useEffect 안에 모든 것을 작성한다고 한다!)

```jsx
function Hello(){
  useEffect(() => {
    console.log("hi :)");
    return () => console.log("bye :(");
  }, []);
  return <h1>Hello</h1>;
}
```

이것으로 리액트에 대한 이론은 끝!
