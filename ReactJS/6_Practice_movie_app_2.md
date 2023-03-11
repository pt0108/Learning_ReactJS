# #6 Practice movie app (2)

### ****Coin Tracker****

암호화폐들과 그 가격을 나열하는 기능을 만들 것이다.

로딩을 위한 state, 코인 리스트를 잠시 갖고 있기 위한 state 두 개가 필요함!

```jsx
// App.js
import { useState } from "react";

function App() {
  const [loading, setLoading] = useState(true);
  return (
    <div>
      <h1>The Coins!</h1>
      {loading ? <strong>Loading...</strong> : null}
    </div>
  );
}

export default App;
```

![image](https://user-images.githubusercontent.com/106129152/224469158-4db2d3b1-9bbc-4810-84d9-f1ad115a81e3.png)

문구가 잘 출력되는 것을 확인.

그리고 API를 가져오고 싶은데, **컴포넌트가 가장 처음으로 render 되었을 때만 실행**시키고 싶음.

→ **useEffect**를 사용

```jsx
import { useState, useEffect } from "react";

function App() {
  const [loading, setLoading] = useState(true);
  useEffect(() => {
    fetch("https://api.coinpaprika.com/v1/tickers")
      .then((response) => response.json())
      .then((json) => console.log(json));
  }, []);
  return (
    <div>
      <h1>The Coins!</h1>
      {loading ? <strong>Loading...</strong> : null}
    </div>
  );
}

export default App;
```

`fetch()`는 첫번째 인자로 URL, 두번째 인자로 옵션 객체를 받고, Promise 타입의 객체를 반환한다.

API를 불러오고, response.json을 콘솔 로그로 가져오면 아래처럼 잘 가져와진 것을 확인할 수 있다.

![image](https://user-images.githubusercontent.com/106129152/224469175-389b8203-cd48-45bf-a44c-4c5be3f92d84.png)

이제 이 데이터들을 직접 눈으로 보기 위해서 state를 만들면 된다.

```jsx
import { useState, useEffect } from "react";

function App() {
  const [loading, setLoading] = useState(true);
  const [coins, setCoins] = useState([]);
  useEffect(() => {
    fetch("https://api.coinpaprika.com/v1/tickers")
      .then((response) => response.json())
      .then((json) => {
        setCoins(json);
        setLoading(false);
      });
  }, []);
  return (
    <div>
      <h1>The Coins!({coins.length})</h1>
      {loading ? <strong>Loading...</strong> : null}
      <ul>
        {coins.map((coin) => <li>{coin.name} ({coin.symbol}): ${coin.quotes.USD.price} USD</li>)}
      </ul>
    </div>
  );
}

export default App;
```

json 파일을 setCoins에 넣고, 로딩이 끝나면 loading은 false로 바뀐다.

전의 todolist를 만들었던 것처럼 `map()` 함수를 사용해서 데이터를 출력할 것인데,

**coins라는 변수에 코인들의 array가 담겨져 있음**을 잊지 말아야 함! 

리스트로 coin의 name, symbol, price를 가져오면 아래처럼 가져온 값들이 출력된다.

![image](https://user-images.githubusercontent.com/106129152/224469181-b42c6a43-c485-490e-a81c-0f30c6f38e1f.png)

### ****Coin Tracker**** 최종 코드

coins의 length도 로딩 후에 나타나게 하고, coin들은 쭉 리스트로 나열되는 것이 아닌 select의 option으로 나타나게 만들었다.

```jsx
import { useState, useEffect } from "react";

function App() {
  const [loading, setLoading] = useState(true);
  const [coins, setCoins] = useState([]);
  useEffect(() => {
    fetch("https://api.coinpaprika.com/v1/tickers")
      .then((response) => response.json())
      .then((json) => {
        setCoins(json);
        setLoading(false);
      });
  }, []);
  return (
    <div>
      <h1>The Coins!{loading ? "" : `(${coins.length})`}</h1>
      {loading ? (
        <strong>Loading...</strong>
      ) : (
        <select>
        {coins.map((coin) => (
          <option key={coin.id}>
            {coin.name} ({coin.symbol}): ${coin.quotes.USD.price}
          </option>
        ))}
      </select>
      )}
    </div>
  );
}

export default App;
```

![2023_3_11_38](https://user-images.githubusercontent.com/106129152/224469185-f89b0b00-b6ff-43db-9b34-8d4b3f813c4b.gif)
