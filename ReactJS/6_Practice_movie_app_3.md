# #6 Practice movie app (3)

### ****Movie App****

맨 처음 API를 가져오기 위해 짠 코드는 아래와 같다.

```jsx
// App.js
import { useState, useEffect } from "react";

function App() {
  const [loading, setLoading] = useState(true);
  const [movies, setMovies] = useState([]);
  useEffect(() => {
    fetch(
      `https://yts.mx/api/v2/list_movies.json?minimum_rating=9&sort_by=year`
    )
      .then((response) => response.json())
      .then((json) => {
        setMovies(json.data.movies);
        setLoading(false);
      });
  }, []);
  console.log(movies);
  return <div>{loading ? <h1>Loading...</h1> : null}</div>;
}

export default App;
```

하지만 요즘은 `.then`을 쓰기 보다는 **async-await**를 많이 사용한다고 한다.

→ async-await : [https://ko.javascript.info/async-await](https://ko.javascript.info/async-await)

```jsx
import { useState, useEffect } from "react";

function App() {
  const [loading, setLoading] = useState(true);
  const [movies, setMovies] = useState([]);
  const getMovies = async() => {
    const json = await (
      await fetch(
      `https://yts.mx/api/v2/list_movies.json?minimum_rating=9&sort_by=year`
      )
    ).json();
    setMovies(json.data.movies);
    setLoading(false);
  };
  useEffect(() => {
    getMovies()
  }, []);
  console.log(movies);
  return <div>{loading ? <h1>Loading...</h1> : null}</div>;
}

export default App;
```

![image](https://user-images.githubusercontent.com/106129152/224485709-783b44fc-7f55-45a8-895b-c268573f9f43.png)

잘 가져와진 것을 확인했다.

이제 데이터를 화면으로 가져오자!

```jsx
import { useState, useEffect } from "react";

function App() {
  const [loading, setLoading] = useState(true);
  const [movies, setMovies] = useState([]);
  const getMovies = async() => {
    const json = await (
      await fetch(
      `https://yts.mx/api/v2/list_movies.json?minimum_rating=9&sort_by=year`
      )
    ).json();
    setMovies(json.data.movies);
    setLoading(false);
  };
  useEffect(() => {
    getMovies()
  }, []);
  console.log(movies);
  return (
    <div>
      {loading ? (
        <h1>Loading...</h1>
      ) : (
        <div>
          {movies.map(movie => (
            <div key={movie.id}>{movie.title}</div>
          ))}
        </div>
      )}
    </div>
  );
}

export default App;
```

![image](https://user-images.githubusercontent.com/106129152/224485719-0e767505-a0aa-4fa7-86ca-e4ee0698b8eb.png)

여기서 조금더 필요한 정보를 가져와보면!

```jsx
import { useState, useEffect } from "react";

function App() {
  const [loading, setLoading] = useState(true);
  const [movies, setMovies] = useState([]);
  const getMovies = async() => {
    const json = await (
      await fetch(
      `https://yts.mx/api/v2/list_movies.json?minimum_rating=9&sort_by=year`
      )
    ).json();
    setMovies(json.data.movies);
    setLoading(false);
  };
  useEffect(() => {
    getMovies()
  }, []);
  console.log(movies);
  return (
    <div>
      {loading ? (
        <h1>Loading...</h1>
      ) : (
        <div>
          {movies.map(movie => (
            <div key={movie.id}>
              <img src={movie.medium_cover_image} />
              <h2>{movie.title}</h2>
              <p>{movie.summary}</p>
              <ul>
                {movie.genres.map((g) => (
                  <li key={g}>{g}</li>
                ))}
              </ul>
            </div>
          ))}
        </div>
      )}
    </div>
  );
}

export default App;
```

![2023_3_11_10](https://user-images.githubusercontent.com/106129152/224485725-e02eaef5-bab4-48d5-9568-7f6a75578d3c.gif)


지금은 한 스크린에서 모든 것이 작동되기 때문에,

리액트 앱에서 **페이지를 전환**하는 방법을 배워보려고 한다.

우선! 다소 지저분해진 App.js를 위해 **Movie**라는 컴포넌트를 따로 만들어보자.

정리된 코드는 다음과 같다.

```jsx
// App.js
import { useState, useEffect } from "react";
import Movie from "./Movie";

function App() {
  const [loading, setLoading] = useState(true);
  const [movies, setMovies] = useState([]);
  const getMovies = async() => {
    const json = await (
      await fetch(
      `https://yts.mx/api/v2/list_movies.json?minimum_rating=9&sort_by=year`
      )
    ).json();
    setMovies(json.data.movies);
    setLoading(false);
  };
  useEffect(() => {
    getMovies()
  }, []);
  console.log(movies);
  return (
    <div>
      {loading ? (
        <h1>Loading...</h1>
      ) : (
        <div>
          {movies.map((movie) => (
            <Movie 
              key={movie.id}
              coverImg={movie.medium_cover_image}
              title={movie.title}
              summary={movie.summary}
              genres={movie.genres}
            />
          ))}
        </div>
      )}
    </div>
  );
}

export default App;
```

```jsx
// Movie.js
function Movie({coverImg, title, summary, genres}) {
    return (
        <div>
            <img src={coverImg} alt={title} />
            <h2>{title}</h2>
            <p>{summary}</p>
            <ul>
                {genres.map((g) => (
                    <li key={g}>{g}</li>
                ))}
            </ul>
        </div>
  );
}

export default Movie;
```

Movie component가 정보들을 부모 component로부터 받아오는 것이다.

**key**는 React.js 에서 **map 안의 component들을 render할 때만 사용**하는 것!

그리고 Movie.js에 proptypes를 추가해준다.

```jsx
// Movie.js
import PropTypes from "prop-types";

function Movie({coverImg, title, summary, genres}) {
    return (
        <div>
            <img src={coverImg} alt={title} />
            <h2>{title}</h2>
            <p>{summary}</p>
            <ul>
                {genres.map((g) => (
                    <li key={g}>{g}</li>
                ))}
            </ul>
        </div>
  );
}

Movie.propTypes = {
    coverImg: PropTypes.string.isRequired,
    title: PropTypes.string.isRequired,
    summary: PropTypes.string.isRequired,
    genres: PropTypes.arrayOf(PropTypes.string).isRequired,
}

export default Movie;
```

이제 **React Router**에 대해 배울 것이다.

React Router의 역할은 **페이지를 전환(이동)**하는 것이다.

`$ npm i react-router-dom@5.3.0`

(현재 6버전의 라우터는 Switch컴포넌트가 Routes컴포넌트로 대체되었다고 함)

터미널에서 설치를 하고, routes 폴더 추가 후 Home.js 생성, components 폴더 추가 후 Movie.js 이동.

현재 src 폴더는 이 상태이다.

![image](https://user-images.githubusercontent.com/106129152/224485734-ef9ef12b-9433-4655-ae01-a4299d1cf5a4.png)

그리고 App.js에 있던 코드들을 Home.js에 옮겨주고, routes 폴더에 Detail.js를 생성했다.

App.js에는 Home을 렌더링하기 위한 Route를 적어주었다.

```jsx
import { BrowserRouter as Router, Switch, Route, } from "react-router-dom";
import Detail from "./routes/Detail";
import Home from "./routes/Home";

function App() {
  return <Router>
    <Switch>
      <Route>
        <Detail path="/movie">
          <Detail />
        </Detail>
      </Route>
      <Route path="/">
        <Home />
      </Route>
    </Switch>
  </Router>;
}

export default App;
```

```jsx
// routes/Home.js
import { useState, useEffect } from "react";
import Movie from "../components/Movie";

function Home() {
    const [loading, setLoading] = useState(true);
    const [movies, setMovies] = useState([]);
    const getMovies = async() => {
        const json = await (
            await fetch(
            `https://yts.mx/api/v2/list_movies.json?minimum_rating=9&sort_by=year`
            )
        ).json();
        setMovies(json.data.movies);
        setLoading(false);
    };
    useEffect(() => {
        getMovies()
    }, []);
    console.log(movies);
    return (
        <div>
            {loading ? (
                <h1>Loading...</h1>
            ) : (
                <div>
                {movies.map((movie) => (
                    <Movie 
                    key={movie.id}
                    coverImg={movie.medium_cover_image}
                    title={movie.title}
                    summary={movie.summary}
                    genres={movie.genres}
                    />
                ))}
                </div>
            )}
        </div>
    );
}
export default Home;
```

```jsx
// routes/Detail.js
function Detail() {
    return <h1>Detail</h1>;
}

export default Detail;
```

App.js에는 저 코드만 있음에도 불구하고, Home.js가 잘 렌더링되는 것을 확인하였다!

다만 링크를 통해 /movie로 들어가는 순간 뒤로가기를 눌러도, 새로고침을 눌러도 계속 Detail 화면에 머물러 있었다.

 버전 관련 이슈인 것 같았는데, 강의 댓글창에 있는 코드를 참고하고, react-router-dom 을 최신버전으로 업그레이드했다. 해결!

```jsx
// App.js
import { BrowserRouter, Routes, Route} from "react-router-dom";
import Detail from "./routes/Detail";
import Home from "./routes/Home";

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />}></Route>
        <Route path="/movie" element={<Detail />}></Route>
      </Routes>
    </BrowserRouter>
    );
}

export default App;
```

```jsx
// components/Movie.js
import PropTypes from "prop-types";
import { Link } from "react-router-dom";

function Movie({coverImg, title, summary, genres}) {
    return (
        <div>
            <img src={coverImg} alt={title} />
            <h2><Link to="/movie">{title}</Link></h2>
            <p>{summary}</p>
            <ul>
                {genres.map((g) => (
                    <li key={g}>{g}</li>
                ))}
            </ul>
        </div>
  );
}

Movie.propTypes = {
    coverImg: PropTypes.string.isRequired,
    title: PropTypes.string.isRequired,
    summary: PropTypes.string.isRequired,
    genres: PropTypes.arrayOf(PropTypes.string).isRequired,
}

export default Movie;
```

강의 내용이 현 버전과 맞지 않아 어려움이 있지만, 이 부분은 최근 글을 참고해서 적용하면 될 것 같다.

![2023_3_11_13](https://user-images.githubusercontent.com/106129152/224485753-954d330a-88ef-40ae-bb69-dce0d3a30c3d.gif)


이제 동적 URL 작업이 필요하다. 모든 영화가 한 Detail 주소로만 가면 의미가 없기 때문이다.

movie route 부분을 아래와 같이 수정해준다.

`<Route path="/movie/:id" element={<Detail />}></Route>`

그러면 movie component에는 id가 필요해질 것이다.

Home.js에 `id={movie.id}` (movies의 map안)을 추가해주고, 

Movie.js도 수정해준다.

```jsx
import PropTypes from "prop-types";
import { Link } from "react-router-dom";

function Movie({ id, coverImg, title, summary, genres }) {
    return (
        <div>
            <img src={coverImg} alt={title} />
            <h2><Link to={`/movie/${id}`}>{title}</Link></h2>
            <p>{summary}</p>
            <ul>
                {genres.map((g) => (
                    <li key={g}>{g}</li>
                ))}
            </ul>
        </div>
  );
}

Movie.propTypes = {
    id: PropTypes.number.isRequired,
    coverImg: PropTypes.string.isRequired,
    title: PropTypes.string.isRequired,
    summary: PropTypes.string.isRequired,
    genres: PropTypes.arrayOf(PropTypes.string).isRequired,
}

export default Movie;
```

이렇게 하면 영화의 링크를 눌렀을 때, 각 영화의 id가 경로의 마지막에 붙어있는 것을 확인할 수 있다!

이 id를 가져오기 위해 **useParams**를 사용할 것이다.

```jsx
// Detail.js
import { useEffect } from "react";
import { useParams } from "react-router-dom";

function Detail() {
    const { id } = useParams();
    const getMovie = async () => {
        const json = await (
            await fetch(`https://yts.mx/api/v2/movie_details.json?movie_id=${id}`)
            ).json();
            console.log(json);
    };
    useEffect(() => {
        getMovie();
    }, []);
    return <h1>Detail</h1>;
}

export default Detail;
```

바로 이렇게 사용하는 것인데, 정말 간단하다.

강의에서는 console.log로 json을 확인하는 것까지만 하고 끝마쳤지만, 

- Home에서 해줬던 loading을 Detail에 해주기
- Detail에서 json을 받아왔지만 아무것도 안 하고 있는 상태 → json을 state에 넣어 이전에 했던 것처럼 불러오기

등의 코드챌린지를 권장하셨다. 시간관계상 일단 강의는 여기에서 마치고, 여유가 생길 때 마저 들어야할 것 같다! 짧은 시간동안 빠르게 들었지만, 리액트에 대해 더 이해하고, 어떤 원리로 흘러가는지 알게 되었다.
