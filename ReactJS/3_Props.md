# #3 Props

**Props**는 부모 컴포넌트로부터 자식 컴포넌트에 데이터를 보낼 수 있게 해준다.

컴포넌트는 어떤 JSX를 반환하는 함수라고 생각하면 된다.

가장 기본적인 style 적용 방법

→ 태그 내의 style 변경

```jsx
<!DOCTYPE html>
<html>
    <body>
        <div id="root"></div>
    </body>
    <script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script type="text/babel">
        function SaveBtn() {
            return (
                <button 
                    style={{
                        backgroundColor: "tomato",
                        color: "white",
                        padding: "10px 20px",
                        border: 0,
                        borderRadius: 10,
                    }}
                >
                    Save Changes
                </button>
            );
        }
        function ConfirmBtn() {
            return (
                <button
                    style={{
                        backgroundColor: "tomato",
                        color: "white",
                        padding: "10px 20px",
                        border: 0,
                        borderRadius: 10,
                    }}
                >
                    Confirm
                </button>
            );
        }
        function App() {
            return (
                <div>
                    <SaveBtn />
                    <ConfirmBtn />
                </div>
            );
        }
        const root = document.getElementById("root");
        ReactDOM.render(<App />, root);
    </script>
</html>
```

![image](https://user-images.githubusercontent.com/106129152/224219373-b44f2f0a-a56d-47aa-8625-438cbcfdc5a4.png)

하지만 각각의 컴포넌트마다 이런식으로 스타일을 지정해줘야하는 건 상당히 비효율적이다.

이럴때 **Props를 이용해 하나의 컴포넌트를 재사용**한다!

```jsx
<!DOCTYPE html>
<html>
    <body>
        <div id="root"></div>
    </body>
    <script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script type="text/babel">
        function Btn({ text }) {
            return (
                <button 
                    style={{
                        backgroundColor: "tomato",
                        color: "white",
                        padding: "10px 20px",
                        border: 0,
                        borderRadius: 10,
                    }}
                >
                    {text}
                </button>
            );
        }
        function App() {
            return (
                <div>
                    <Btn text="Save Changes" />
                    <Btn text="Continue" />
                </div>
            );
        }
        const root = document.getElementById("root");
        ReactDOM.render(<App />, root);
    </script>
</html>
```

보다시피 Btn을 두 번 렌더링하되, text라는 key에 원하는 값을 넣고, `Btn({ text })`와 같이 props를 넣는다. 그리고 text가 출력될 자리에 `{text}` 처럼 코드를 작성하면 Btn하나만으로도 아래와 똑같이 나타낼 수 있다.

![image](https://user-images.githubusercontent.com/106129152/224219422-22d664b7-5f5b-4144-95f6-1280421e5f34.png)

또, 아래처럼 else if 를 이용해 변화를 줄 수도 있다.

```jsx
<!DOCTYPE html>
<html>
    <body>
        <div id="root"></div>
    </body>
    <script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script type="text/babel">
        function Btn({ text, big }) {
            return (
                <button 
                    style={{
                        backgroundColor: "tomato",
                        color: "white",
                        padding: "10px 20px",
                        border: 0,
                        borderRadius: 10,
                        fontSize: big ? 18 : 16,
                    }}
                >
                    {text}
                </button>
            );
        }
        function App() {
            return (
                <div>
                    <Btn text="Save Changes" big={true} />
                    <Btn text="Continue" big={false} />
                </div>
            );
        }
        const root = document.getElementById("root");
        ReactDOM.render(<App />, root);
    </script>
</html>
```

![image](https://user-images.githubusercontent.com/106129152/224219451-397a8483-6d57-4cef-9c9c-b3623009ed9c.png)

Btn에 big이라는 키와 값을 넣고, `fontSize: big ? 18 : 16` 로 서로 다른 폰트 사이즈를 확인할 수 있다. 

```jsx
<!DOCTYPE html>
<html>
    <body>
        <div id="root"></div>
    </body>
    <script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script type="text/babel">
        function Btn({ text, changeValue }) {
            return (
                <button 
                    onClick={changeValue}
                    style={{
                        backgroundColor: "tomato",
                        color: "white",
                        padding: "10px 20px",
                        border: 0,
                        borderRadius: 10,
                    }}
                >
                    {text}
                </button>
            );
        }
        function App() {
            const [value, setValue] = React.useState("Save Changes");
            const changeValue = () => setValue("Revert Changes");
            return (
                <div>
                    <Btn text={value} changeValue={changeValue} />
                    <Btn text="Continue" />
                </div>
            );
        }
        const root = document.getElementById("root");
        ReactDOM.render(<App />, root);
    </script>
</html>
```

이건 changeValue라는 prop을 추가해서 onClick에 적용해본 것인데, 버튼을 클릭하면 입력된 값으로 텍스트가 바뀌는 것을 확인할 수 있다.

주의해야할 점은, `<Btn text={value} changeValue={changeValue} />` 여기에 써둔 값들이 자동적으로 return 안에 들어가지 않는다는 것이다! `function Btn({ text, changeValue })` 처럼 꼭 props로 넣어줘야 적용이 가능하다.

위의 코드는 onClick 이벤트가 작동될 때, 값이 업데이트되면서 컴포넌트가 리렌더링되는데, **아무런 값도 업데이트되지 않는 Continue 버튼까지 다시 렌더링**이 된다.  

이럴 때 우리는 **React.memo**를 통해 Continue 버튼이 다시 그려지는 걸 원치 않는다고 리액트에게 알려줄 수 있다(prop이 변경되지 않는다는 선에서)!

```jsx
<!DOCTYPE html>
<html>
    <body>
        <div id="root"></div>
    </body>
    <script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script type="text/babel">
        function Btn({ text, changeValue }) {
            console.log(text, "was rendered.")
            return (
                <button 
                    onClick={changeValue}
                    style={{
                        backgroundColor: "tomato",
                        color: "white",
                        padding: "10px 20px",
                        border: 0,
                        borderRadius: 10,
                    }}
                >
                    {text}
                </button>
            );
        }
        function App() {
            const [value, setValue] = React.useState("Save Changes");
            const changeValue = () => setValue("Revert Changes");
            return (
                <div>
                    <MemorizedBtn text={value} changeValue={changeValue} />
                    <MemorizedBtn text="Continue" />
                </div>
            );
        }
        const MemorizedBtn = React.memo(Btn)
        const root = document.getElementById("root");
        ReactDOM.render(<App />, root);
    </script>
</html>
```

`const MemorizedBtn = React.memo(Btn)` 을 입력하고 Btn을 MemorizedBtn으로 바꿔주기만 하면, 리렌더링될 때 prop의 변화가 없는 Continue 버튼은 리렌더링되지 않는 것을 확인할 수 있다!!

![image](https://user-images.githubusercontent.com/106129152/224219495-adfa3ae3-4d05-4f5b-9a67-088ffbe6c40f.png)

이렇게 불필요한 요소까지 전부 리렌더링되는 것을 막음으로써, 어플이 느려지는 것을 방지할 수 있다.

### ****Prop Types****

prop types는 어떤 타입의 prop을 받고 있는지 체크해주는 패키지로, prop 이름과 그 type을 적어주면 된다! 

```jsx
<!DOCTYPE html>
<html>
    <body>
        <div id="root"></div>
    </body>
    <script src="https://unpkg.com/react@17.0.2/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/prop-types@15.7.2/prop-types.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script type="text/babel">
        function Btn({ text, fontSize }) {
            console.log(text, "was rendered.")
            return (
                <button 
                    style={{
                        backgroundColor: "tomato",
                        color: "white",
                        padding: "10px 20px",
                        border: 0,
                        borderRadius: 10,
                        fontSize,
                    }}
                >
                    {text}
                </button>
            );
        }
        Btn.propTypes = {
            text: PropTypes.string,
            fontSize: PropTypes.number,
        };
        function App() {
            const [value, setValue] = React.useState("Save Changes");
            const changeValue = () => setValue("Revert Changes");
            return (
                <div>
                    <Btn text="Save Changes" fontSize={18} />
                    <Btn text={18} fontSize="Save Changes" />
                </div>
            );
        }
        const root = document.getElementById("root");
        ReactDOM.render(<App />, root);
    </script>
</html>
```

위와 같이 propTypes를 알려주고, Btn의 prop을 알려준 타입과 다르게 입력해보면 

![image](https://user-images.githubusercontent.com/106129152/224219523-9c759ebc-42de-457a-b516-03f948f3a3d1.png)

이렇게 콘솔창에 경고가 뜨는 것을 확인할 수 있다!

파라미터를 잘못 넘겨도 확인할 수 없는 리액트의 문제점을 이 방법으로 해소할 수 있다.
