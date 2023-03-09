# #2 State

강의의 첫번째에서 HTML, Javascript로만 짜봤던 코드를 React로 똑같이 만들어보자!

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
        let counter = 0;
        function countUp() {
            counter = counter + 1;
            render();
        }
        function render() {
            ReactDOM.render(<Container />, root);
        }
        const Container = () => (
            <div>
                <h3>Total clicks: {counter}</h3>
                <button onClick={countUp}>Click me</button>
            </div>
        );
        render();
    </script>
</html>
```

→ 하지만 이 방법은 업데이트가 필요한 구간마다 render를 입력해야 하기 때문에 탁월한 선택이 아니다.

이제 `useState` 함수를 통해 더욱 간단하게 구현해보자.

`useState`는 **컴포넌트에서 상태를 관리**할 수 있는 함수로, 상태의 기본값을 파라미터로 넣어서 호출한다.

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
        function App() {
            const [counter, setCounter] = React.useState(0);
            const onClick = () => {
                setCounter(counter + 1);
            };
            return (
                <div>
                    <h3>Total clicks: {counter}</h3>
                    <button onClick={onClick}>Click me</button>
                </div>
            );
        }
        ReactDOM.render(<App />, root);
    </script>
</html>
```

`const [counter, setCounter] = React.useState(0);` 바로 이 부분인데, 

***배열의 첫번째에 있는 `counter`는 현재의 값, 두번째의 `setCounter`는 그 값을 바꿔주는 역할***을 한다. 

보통 데이터 이름을 지으면 “set데이터”명 으로 짓는다.

버튼을 누를 때마다 문제없이 리렌더링이 되는 것을 확인할 수 있다.

`setCounter`로 state를 변경할 때 컴포넌트가 재생성되는 것 → 새로운 값을 가지고 리렌더링됨.

`return` 위에 아래 콘솔 로그를 추가해보면 확인이 가능하다.

```
console.log("rendered");
console.log(counter);
```

![image](https://user-images.githubusercontent.com/106129152/223947609-0216e5e2-e9b4-4476-ad68-dbf81aab8744.png)

→ 이번 강의의 핵심은, **state 값이 바뀌면 새로운 값을 가지고 컴포넌트가 리렌더링** 되는 것!

하지만 위 코드를 그대로 사용하면 생길 수 있는 문제가 있다.

counter가 다른 곳에서 변경되는 문제가 생길수도 있다는 것이다. 

지금 **이전 값을 이용해서 현재 값을 계산하는 방식**으로 코드를 썼는데,

현재 코드 : `setCounter(counter + 1);`

더 안전한 방식의 코드 : `setCounter((current) ⇒ current + 1);`

확실하게 현재값을 보장해주는 함수를 사용하는 방식이 더 안전하다.

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
        function App() {
            const [counter, setCounter] = React.useState(0);
            const onClick = () => {
                // setCounter(counter + 1);
                setCounter((current) => current + 1);
            };
            return (
                <div>
                    <h3>Total clicks: {counter}</h3>
                    <button onClick={onClick}>Click me</button>
                </div>
            );
        }
        ReactDOM.render(<App />, root);
    </script>
</html>
```

여기서 `current`는 current라는 이름의 특정 함수가 있는게 아니라, 현재 state값을 호출하는 static 변수라고 생각하면 된다.

### Unit Conversion - 단위변환앱 만들기

❗*주의사항 : React에서 사용하는 것은 HTML이 아니라 JSX이기 때문에, **Javascript의 예약어와 겹치는 단어는 사용하지 않는 것**으로! (ex- for → htmlFor, class → className 등으로 바꿔서 사용)* 

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
        function App() {
            const [minutes, setMinutes] = React.useState(0);
            const onChange = (event) => {
                setMinutes(event.target.value);
            };
            return (
                <div>
                    <h1>Super Converter</h1>
                    <label htmlFor="minutes">Minutes</label>
                    <input 
                        value={minutes} 
                        id="minutes" 
                        placeholder="Minutes" 
                        type="number"
                        onChange={onChange}
                    />
                    <h4>You want to convert {minutes}</h4>
                    <label htmlFor="hours">Hours</label>
                    <input id="hours" placeholder="Hours" type="number"/>
                </div>
            );
        }
        const root = document.getElementById("root");
        ReactDOM.render(<App />, root);
    </script>
</html>
```

여기까지의 코드만으로 아래와 같이 작동한다. (Hours는 미완성 상태)

![image](https://user-images.githubusercontent.com/106129152/223947694-41aaec13-a414-4d29-aac4-be89a66fdac0.png)

1. useState의 결과는 array인데, 첫번째 요소는 데이터, 두번째 요소는 데이터를 수정하기 위한 함수이다.
2. input의 value를 state로 연결한다.
3. onChange 함수를 만든다. (데이터를 업데이트해주는 역할)
    
    → change 이벤트가 일어났을 때(사용자가 input에 입력) onChange 함수가 실행됨.
    
4. event.target.value를 얻음 (= input value) → input이 업데이트됨.

그리고 아래와 같이 코드를 작성해보면, minutes에 입력하고 있는 값이 hours에 동시에 입력되는 것을 확인할 수 있다. minutes과 hours 둘 다 input value를 minutes로 주었기 때문!

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
        function App() {
            const [minutes, setMinutes] = React.useState(0);
            const onChange = (event) => {
                setMinutes(event.target.value);
            };
            return (
                <div>
                    <h1>Super Converter</h1>
                    <div>
                        <label htmlFor="minutes">Minutes</label>
                        <input 
                            value={minutes} 
                            id="minutes" 
                            placeholder="Minutes" 
                            type="number"
                            onChange={onChange}
                        />
                    </div>
                    <div>
                        <label htmlFor="hours">Hours</label>
                        <input 
                            value={minutes} 
                            id="hours" 
                            placeholder="Hours" 
                            type="number"
                        />
                    </div>
                </div>
            );
        }
        const root = document.getElementById("root");
        ReactDOM.render(<App />, root);
    </script>
</html>
```

![image](https://user-images.githubusercontent.com/106129152/223947750-7bb677ad-43d5-49ff-83ef-e5b4b86694ab.png)

이제 여기서 hours의 input value값을 minutes에서 **minutes/60**으로 바꿔주기만 하면!

![image](https://user-images.githubusercontent.com/106129152/223947853-f25b2b4d-6ddf-4071-a5ba-adf01d078322.png)

입력된 분 단위를 시간 단위로 출력해주는 것을 확인할 수 있다.

소수점으로 길게 나오는게 싫다면, *`value*={Math.round(minutes/60)}` 로 함수를 적용해주면 된다.

그리고 `const reset = () => setMinutes(0);` 를 버튼에 적용하면 리셋 버튼도 완성이다!

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
        function App() {
            const [minutes, setMinutes] = React.useState(0);
            const onChange = (event) => {
                setMinutes(event.target.value);
            };
            const reset = () => setMinutes(0);
            return (
                <div>
                    <h1>Super Converter</h1>
                    <div>
                        <label htmlFor="minutes">Minutes</label>
                        <input 
                            value={minutes} 
                            id="minutes" 
                            placeholder="Minutes" 
                            type="number"
                            onChange={onChange}
                        />
                    </div>
                    <div>
                        <label htmlFor="hours">Hours</label>
                        <input 
                            value={Math.round(minutes/60)} 
                            id="hours" 
                            placeholder="Hours" 
                            type="number"
                        />
                    </div>
                    <button onClick={reset}>Reset</button>
                </div>
            );
        }
        const root = document.getElementById("root");
        ReactDOM.render(<App />, root);
    </script>
</html>
```

다음으로는 **Flip** 기능을 만들어보는 것이다.

- 디폴트는 minutes 활성화, hours 비활성화
- Flip 버튼을 누르면 minutes 비활성화, hours 활성화

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
        function App() {
            const [minutes, setMinutes] = React.useState(0);
            const [flipped, setFlipped] = React.useState(false);
            const onChange = (event) => {
                setMinutes(event.target.value);
            };
            const reset = () => setMinutes(0);
            const onFlip = () => setFlipped((current) => !current);
            return (
                <div>
                    <h1>Super Converter</h1>
                    <div>
                        <label htmlFor="minutes">Minutes</label>
                        <input 
                            value={minutes} 
                            id="minutes" 
                            placeholder="Minutes" 
                            type="number"
                            onChange={onChange}
                            disabled={flipped}
                        />
                    </div>
                    <div>
                        <label htmlFor="hours">Hours</label>
                        <input 
                            value={Math.round(minutes/60)} 
                            id="hours" 
                            placeholder="Hours" 
                            type="number"
                            disabled={!flipped}
                        />
                    </div>
                    <button onClick={reset}>Reset</button>
                    <button onClick={onFlip}>Flip</button>
                </div>
            );
        }
        const root = document.getElementById("root");
        ReactDOM.render(<App />, root);
    </script>
</html>
```

`const [flipped, setFlipped] = React.useState(false);` 

여기서 flipped는 단순히 true 혹은 false인 변수이다. 

Flip 버튼을 누르면 `onFlip` 함수가 실행되고, 

`const onFlip = () => setFlipped((current) => !current);` 현재값의 반대값을 내놓는다.

![2023_3_9_7](https://user-images.githubusercontent.com/106129152/223949057-6fae9e60-6acb-4254-8117-0fe24001b33c.gif)

여기까지도 완벽해보이겠지만, hours가 활성화된 상태에서 입력을 하려고 하면 불가능하다.

이때, 삼항연산자를 이용해서 hours의 input value에 다음과 같은 코드를 작성해준다.

`value={flipped ? minutes : Math.round(minutes/60)}`

- flipped 상태라면 state에 있는 값을 그대로 보여줌
- flipped 상태가 아니라면 변환된 값을 보여줌

같은 방식으로 minutes의 input value에도 `value={flipped ? amount*60 : amount}`를 더해준다.

여기서 Flip 버튼을 누를 때 리셋하는 과정이 필요하므로 **onFlip에 reset()함수를 추가**해준다.

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
        function App() {
            const [amount, setAmount] = React.useState(0);
            const [flipped, setFlipped] = React.useState(false);
            const onChange = (event) => {
                setAmount(event.target.value);
            };
            const reset = () => setAmount(0);
            const onFlip = () => {
                reset();
                setFlipped((current) => !current);
            };
            return (
                <div>
                    <h1>Super Converter</h1>
                    <div>
                        <label htmlFor="minutes">Minutes</label>
                        <input 
                            value={flipped ? amount*60 : amount} 
                            id="minutes" 
                            placeholder="Minutes" 
                            type="number"
                            onChange={onChange}
                            disabled={flipped}
                        />
                    </div>
                    <div>
                        <label htmlFor="hours">Hours</label>
                        <input 
                            value={flipped ? amount : Math.round(amount/60)} 
                            id="hours" 
                            placeholder="Hours" 
                            type="number"
                            onChange={onChange}
                            disabled={!flipped}
                        />
                    </div>
                    <button onClick={reset}>Reset</button>
                    <button onClick={onFlip}>Flip</button>
                </div>
            );
        }
        const root = document.getElementById("root");
        ReactDOM.render(<App />, root);
    </script>
</html>
```

### Unit Conversion - 최종 코드

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
        function App() {
            const [amount, setAmount] = React.useState(0);
            const [inverted, setInverted] = React.useState(false);
            const onChange = (event) => {
                setAmount(event.target.value);
            };
            const reset = () => setAmount(0);
            const onInvert = () => {
                reset();
                setInverted((current) => !current);
            };
            return (
                <div>
                    <h1>Super Converter</h1>
                    <div>
                        <label htmlFor="minutes">Minutes</label>
                        <input 
                            value={inverted ? amount*60 : amount} 
                            id="minutes" 
                            placeholder="Minutes" 
                            type="number"
                            onChange={onChange}
                            disabled={inverted}
                        />
                    </div>
                    <div>
                        <label htmlFor="hours">Hours</label>
                        <input 
                            value={inverted ? amount : Math.round(amount/60)} 
                            id="hours" 
                            placeholder="Hours" 
                            type="number"
                            onChange={onChange}
                            disabled={!inverted}
                        />
                    </div>
                    <button onClick={reset}>Reset</button>
                    <button onClick={onInvert}>
                        {inverted ? "Turn back" : "Invert"}
                    </button>
                </div>
            );
        }
        const root = document.getElementById("root");
        ReactDOM.render(<App />, root);
    </script>
</html>
```

![2023_3_9_58](https://user-images.githubusercontent.com/106129152/223949118-e02622de-be8e-428c-bba2-fa15eeb1598b.gif)


### ****Final Practice****

→ 분/시간 변환기, 마일/킬로미터 변환기를 유저가 선택할 수 있는 메뉴 만들기

- 기존의 App()은 MinutesToHours()로 이름 수정
- MinutesToHours()를 복사해서 KmToMiles 생성
- App()은 아래와 같이 작성해서 유저가 선택할 수 있는 option 생성

```jsx
function App() {
            const [index, setIndex] = React.useState("xx");
            const onSelect = (event) => {
                setIndex(event.target.value);
            }
            return (
                <div>
                    <h1>Super Converter</h1>
                    <select value={index} onChange={onSelect}>
                        <option value="xx">Select your units</option>
                        <option value="0">Minutes & Hours</option>
                        <option value="1">Km & Miles</option>
                    </select>   
                    <hr />
                    {index === "xx" ? "Please select your units." : null}
                    {index === "0" ? <MinutesToHours /> : null}
                    {index === "1" ? <KmToMiles /> : null}
                </div>
            );
        }
```

→ index의 값이 0이면 분/시간 변환기를 렌더링하고, 1이면 마일/킬로미터 변환기를 렌더링한다.

완성 코드!

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
        function MinutesToHours() {
            const [amount, setAmount] = React.useState(0);
            const [inverted, setInverted] = React.useState(false);
            const onChange = (event) => {
                setAmount(event.target.value);
            };
            const reset = () => setAmount(0);
            const onInvert = () => {
                reset();
                setInverted((current) => !current);
            };
            return (
                <div>
                    <div>
                        <label htmlFor="minutes">Minutes</label>
                        <input 
                            value={inverted ? amount*60 : amount} 
                            id="minutes" 
                            placeholder="Minutes" 
                            type="number"
                            onChange={onChange}
                            disabled={inverted}
                        />
                    </div>
                    <div>
                        <label htmlFor="hours">Hours</label>
                        <input 
                            value={inverted ? amount : Math.round(amount/60)} 
                            id="hours" 
                            placeholder="Hours" 
                            type="number"
                            onChange={onChange}
                            disabled={!inverted}
                        />
                    </div>
                    <button onClick={reset}>Reset</button>
                    <button onClick={onInvert}>
                        {inverted ? "Turn back" : "Invert"}
                    </button>
                </div>
            );
        }
        function KmToMiles() {
            const [amount, setAmount] = React.useState(0);
            const [inverted, setInverted] = React.useState(false);
            const onChange = (event) => {
                setAmount(event.target.value);
            };
            const reset = () => setAmount(0);
            const onInvert = () => {
                reset();
                setInverted((current) => !current);
            };
            return (
                <div>
                    <div>
                        <label htmlFor="km">Km</label>
                        <input 
                            value={inverted ? (amount*1.609).toFixed(3) : amount} 
                            id="km" 
                            placeholder="Km" 
                            type="number"
                            onChange={onChange}
                            disabled={inverted}
                        />
                    </div>
                    <div>
                        <label htmlFor="miles">Miles</label>
                        <input 
                            value={inverted ? amount : (amount/1.609).toFixed(3)} 
                            id="miles" 
                            placeholder="Miles" 
                            type="number"
                            onChange={onChange}
                            disabled={!inverted}
                        />
                    </div>
                    <button onClick={reset}>Reset</button>
                    <button onClick={onInvert}>
                        {inverted ? "Turn back" : "Invert"}
                    </button>
                </div>
            );
        }
        function App() {
            const [index, setIndex] = React.useState("xx");
            const onSelect = (event) => {
                setIndex(event.target.value);
            }
            return (
                <div>
                    <h1>Super Converter</h1>
                    <select value={index} onChange={onSelect}>
                        <option value="xx">Select your units</option>
                        <option value="0">Minutes & Hours</option>
                        <option value="1">Km & Miles</option>
                    </select>   
                    <hr />
                    {index === "xx" ? "Please select your units." : null}
                    {index === "0" ? <MinutesToHours /> : null}
                    {index === "1" ? <KmToMiles /> : null}
                </div>
            );
        }
        const root = document.getElementById("root");
        ReactDOM.render(<App />, root);
    </script>
</html>
```

![2023_3_9_58 1](https://user-images.githubusercontent.com/106129152/223949218-b466b8d2-a2b6-4d88-99a3-324560349d4f.gif)
