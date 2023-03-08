# #1 The basics of react

**HTML과 Javascript만 사용했을 때의 코드.**

이후 React를 사용해서 이 코드와 비교를 해볼 것임!

```jsx
<!DOCTYPE html>
<html>
    <body>
        <span>Total clicks: 0</span>
        <button id="btn">Click me</button>
    </body>
    <script>
        let counter = 0;
        const button = document.getElementById("btn");
        const span = document.querySelector("span");
        function handleClick() {
            counter = counter + 1;
            span.innerText = `Total clicks: ${counter}`;
        }
        button.addEventListener("click", handleClick);
    </script>
</html>
```

일단 React의 작동방식을 이해하기 위해 복잡한 방식으로 짧은 코드를 만들었다.

React JS는 웹사이트에 간단한 방법으로 상호작용을 만들어주고, React dom은 element를 HTML에 두는 역할을 한다.

```jsx
<!DOCTYPE html>
<html>
    <body>
        <div id="root"></div>
    </body>
    <script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
    <script>
        const root = document.getElementById("root");
        const span = React.createElement(
            "span", 
            {id:"hi-span", style: {color:"red"}}, 
            "Hello, I'm a span."
        );
        ReactDOM.render(span, root);
    </script>
</html>
```

React JS는 결과물인 HTML을 업데이트할 수 있다. (유저에게 보여질 내용을 컨트롤할 수 있음!)

**Javascript가 element를 생성 → React JS가 HTML로 번역**

```jsx
<!DOCTYPE html>
<html>
    <body>
        <div id="root"></div>
    </body>
    <script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
    <script>
        const root = document.getElementById("root");
        const h3 = React.createElement(
            "h3", 
            {
                onMouseEnter: () => console.log("mouse enter"),
            }, 
            "Hello, I'm a title."
        );
        const btn = React.createElement(
            "button", 
            {
                onClick: () => console.log("im clicked"),
                style: {
                    backgroundColor: "tomato",
                },
            }, 
            "Click me"
        );
        const container = React.createElement("div", null, [h3, btn]);
        ReactDOM.render(container, root);
    </script>
</html>
```

여기까지 맛보기로 React의 작동 방식에 대해 알아봤다.

### JSX

**JSX**는 createElement를 대체할 수 있는 방법! **Javascript를 확장한 문법**이다.

JSX를 사용할 때 브라우저가 이해할 수 있도록 **Babel**을 사용해야 한다.

참고로 **컴포넌트의 첫 글자는 반드시 대문자로 작성**해야 하는데, 그냥 소문자로 쓰면 HTML 태그로 인식하기 때문이다.

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
        const root = document.getElementById("root");
        const Title = () => (
            <h3 id="title" onMouseEnter={() => console.log("mouse enter")}>
                Hello I'm a title
            </h3>
        );
        const Button = () => (
            <button 
                style={{
                    backgroundColor: "tomato"
                }} 
                onClick={() => console.log("I'm clicked.")}>
                Click me!
            </button>
        );
        const Container = () => (
            <div>
                <Title />
                <Button />
            </div>
        );
        ReactDOM.render(<Container />, root);
    </script>
</html>
```
