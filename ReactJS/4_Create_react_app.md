# #4 Create react app

- node.js 설치
- create-react-app 폴더 생성
- 터미널에서 `npx create-react-app my app` 입력, y 선택 후 설치
- 생성된 앱에서 package.json 파일 확인, `npm start` 실행

![image](https://user-images.githubusercontent.com/106129152/224240317-7ce51687-cf3c-4fd8-bb39-8c527e31f0fe.png)

자동으로 브라우저에서 열린 첫 화면! 성공적이다.

기초부터 시작하기 위해 기존 `index.js` 와 `App.js` 에서 불필요한 부분은 지우고 기본적인 코드만 남겨뒀다.

```jsx
// index.js
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

```jsx
// App.js
function App() {
  return (
    <div>
        <h1>Welcome back!</h1>
    </div>
  );
}

export default App;
```

이렇게 하면 제법 멋있었던 초기 화면에서 글자만 출력되는 단순한 화면으로 바뀐다.

![image](https://user-images.githubusercontent.com/106129152/224240355-ceeb6386-0e16-4f37-b15e-badd545ce2a9.png)

그리고 src 폴더의 다른 파일들은 다 지우고, 이 두 파일만 남겨둔다.

![image](https://user-images.githubusercontent.com/106129152/224240385-5d062acf-4bd5-473b-bef0-60ac25f3b7be.png)

### ****Tour of CRA****

src 폴더에 Button.js 를 만든다.

```jsx
function Button({ text }) {
    return <button>{text}</button>;
}
export default Button;
```

App.js 에 Button.js 를 import 한다.

```jsx
import Button from "./Button";

function App() {
  return (
    <div>
        <h1>Welcome back!</h1>
        <Button text={"Continue"} />
    </div>
  );
}

export default App;
```

여기서 create react app의 장점을 느낄 수 있는데, 저장만 해도 **브라우저에 자동으로 반영**이 된다는 점이다! 이렇게 바로 버튼이 추가된 것을 볼 수 있다.

![image](https://user-images.githubusercontent.com/106129152/224240414-43fa9446-8f4e-404f-ba61-79816d26e053.png)

다음으로는 propTypes를 설치하는 것!

```
$ npm i prop-types
```

설치 후, Button.js에 import 하고 Button의 propTypes를 설정해준다.

```jsx
import PropTypes from "prop-types";

function Button({ text }) {
    return <button>{text}</button>;
}
Button.propTypes = {
    text: PropTypes.string.isRequired,
}

export default Button;
```

### css 적용

여러가지 방법이 있는데, 아래 방법은 css 파일을 index.js에 import 하는 것이다.

```css
/*style.css*/
button {
    color: white;
    background-color: tomato;
}
```

```jsx
// index.js
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import "./style.css";

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

![image](https://user-images.githubusercontent.com/106129152/224240455-bc15738a-4970-436a-aac2-e077c9fbfc03.png)

강의에서는 이 방식을 사용하지 않으려고 한다.

global css 스타일을 원치 않기 때문에 css module을 사용할 것이다.

위에서 만든 style.css의 이름을 **Button.module.css**로 바꿔준다.

그 다음, btn 이라는 클래스를 생성한다.

```css
/*Button.module.css*/
.btn {
    color: white;
    background-color: tomato;
}
```

그리고 Button.js에 import한 다음, Button 태그에 클래스를 적용한다.

```jsx
import PropTypes from "prop-types";
import styles from "./Button.module.css";

function Button({ text }) {
    return <button className={styles.btn}>{text}</button>;
}

Button.propTypes = {
    text: PropTypes.string.isRequired,
}

export default Button;
```

create-react-app이 **css코드를 javascript 오브젝트로 변환**해주는 것!

이렇게 css를 그대로 사용할 수 있다는 건 정말 편한 것 같다! 자바스크립트 오브젝트 형식으로 css를 작성하는 것은 정말 낯설고 불편했기 때문이다… 

**App.module.css**를 만들어서 title 클래스를 만들어보자.

```css
/*App.module.css*/
.title {
    font-family: Arial, Helvetica, sans-serif;
    font-size: 18px;
}
```

적용하는 방식은 방금과 같다.

```jsx
import Button from "./Button";
import styles from "./App.module.css";

function App() {
  return (
    <div>
        <h1 className={styles.title}>Welcome back!</h1>
        <Button text={"Continue"} />
    </div>
  );
}

export default App;
```

![image](https://user-images.githubusercontent.com/106129152/224240538-5ac0d8fc-b865-46e9-a969-61129c870fa7.png)

이때, 콘솔을 살펴보면 각각 **랜덤한 클래스명**이 부여된 것을 확인할 수 있다.

![image](https://user-images.githubusercontent.com/106129152/224240573-91fd844e-ac3b-4d90-b911-c9738457608b.png)

이것또한 create-react-app의 기능이다!

컴포넌트나 스타일을 독립적이게 유지시켜주고 있다. (진짜 대박)
