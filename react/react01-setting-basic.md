# React! 웹서비스 만들기 우선 기초~!

### 필요 Tool
**Webpack**
- Module bundler, 트랜스포머, 트랜스파일러
- 브라우저가 이해할 수 있는 코드로 변경해준다.
- 리엑트 코드 -> 자바스크립트 코드로 변경해준다.
- 또한 ES6로 된 코드를 모든 브라우저에서 실행되도록 변경
- 기타 등등

**create-react-app** 설치
- 기초로 습득하기 위해 우선 [여기](https://github.com/facebookincubator/create-react-app)에서 설치하자.
- 웹팩 등의 툴이 포함되어있어 쉽게 리액트 앱을 만들어 준다.
- yarn설치가 안되어 있다면 설치하도록 한다.

```
$npm install -g create-react-app // 전역에 설치
$create-react-app my-app         // 리엑트 앱 만들기
```

- 설치 후 4개의 script : start, build, test, eject

#### 실행
```
$yarn start
```
- react-scripts를 실행시킨다.
- http://localhost:3000 확인!

## React, ReactDOM, reactNative
- 리엑트는 UI라이브러리
- 리엑트돔은 리엑트를 웹사이트에 출력(render)하는걸 도와주는 모델
 - 리엑트돔은 한개의 컴포넌트를 reander
 `ReactDOM.render(<App />, document.getElementById('root'));`
  : App컴포넌트를 html의 id가 root인 엘리먼트에 출력한다.
- 리엑트네이티브는 리엑트를 모바일 앱에 render해줌

## Component 생성
- Movie.js

```
import React, { Component } from 'react';
import './Movie.css';

class Movie extends Component {  // 컴포넌트 생성
    render() {                   // 렌더(필수)
        return (                // return
            <h1>Hello~!!</h1>   // JSX
        )
    }
}

export default Movie;
```

- App.js에서 Movie컴포넌트 사용
```
import React, { Component } from 'react';
import './App.css';
import Movie from './Movie';

class App extends Component {
  render() {
    return (
      <div className="App">
        <Movie />
      </div>
    );
  }
}

export default App;

```

## Component Lifecycle 제공 functions
#### Render시
1. componentWillMount(): ex) api에 작업 요청
2. render()
3. componentDidMount(): ex) : 데이터 관련 작업

#### Update시
1. componentWillReceiveProps(): 컴포넌트가 새로운 props를 받았다.
2. shouldComponentUpdate(): 기존의 props와 새로운 props를 비교해 다르다면 update = true
3. componentWillUpdate(): update할 것이다. ex) spinner (돌아가는 아이콘)
4. render()
5. componentDidUpdate(): ex) spinner 숨기기

## Data flow with Props, 유효성 검사
#### App.js
- 각 children component에게 데이터 전달
- 아래 코드에서는 변수 movies 배열 데이터를 title, poster라는 props로 Movie 컴포넌트에 전달한다.
```
class App extends Component {
  render() {
    return (
      <div className="App">
        {movies.map((movie, i) => {  // movies 배열 데이터
            return <Movie title={movie.title} poster={movie.poster} key={i}/>
        })}
      </div>
    );
  }
}

```
#### Movie.js
- Movie 컴포넌트에서 부모 컴포넌트로 부터 받은 props를 받아 사용한다
```
import PropTypes from 'prop-types';
...
class Movie extends Component {
    // 유효성검사
    static propTypes = {
        title: PropTypes.string.isRequired, // isRequired: 필수
        poster: PropTypes.string.isRequired
    }
    render() {
        // console.log(this.props);
        return (
            <div>
                <h2>{this.props.title}</h2>
                <MoviePoster poster={this.props.poster}/>
            </div>
        )
    }
}
```

## State, setState
- React Component 안에 있는 Object

#### 규칙
- state가 바뀔 때 마다, 컴포넌트는 다시 render한다. (새로운 state와 함께)
- this.state.movies = 'xxx'; 처럼 state는 직접 변경은 할 수 없다. state를 업데이트 하려면 setState를 사용해야한다.

```
this.setState({
    movies: [
        {
            title: "시카리오: 데이 오브 솔다도",
            poster: "http://photo.jtbc.joins.com/news/2018/05/07/20180507173210620.jpg"
        },
        ...this.state.movies
    ]
})
```
- 위 코드에서 es6문법: spread문법 (https://github.com/sharryhong/full-stack/blob/master/javascript/es6/es6-spread.md)

#### State 사용 예
- infinite scroll: 페이지 로딩시 스크롤을 아래로 내릴 수록 더 많은 영화가 로딩되는 효과