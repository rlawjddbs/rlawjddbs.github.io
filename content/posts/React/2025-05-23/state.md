---
title: "중요한 데이터는 변수말고 state에"
date: 2025-05-23T06:38:37+09:00
draft: true
categories: ["React"]
tags: ["React", "리액트", "state", "state 사용하는 이유", "destructuring"]
---
[**정리**] 자주 변경될 것 같은 데이터들은 state에 저장했다가 html에 {데이터 바인딩} 하라
# 블로그 글 레이아웃 만들기
```jsx
function App(){
 
  let posts = '강남 우동 맛집';
  return (
    <div className="App">
      <div className="black-nav">
        <div>개발 blog</div>
      </div>
      <div className="list">
        <h4>글제목</h4>
        <p>2월 17일 발행</p>
      </div>
    </div>
  )
}
```

```css
(App.css)

div {
    box-sizing : border-box
}
.list {
    text-align : left;
    padding-left : 20px;
    border-bottom : 1px solid grey;
}
```

# state 만든는 법
```jsx
import { useState } from 'react';
import './App.css'

function App(){
 
  let [a,b] = useState('남자 코트 추천');
  let posts = '강남 우동 맛집';
  return (
    <div className="App">
      <div className="black-nav">
        <div>개발 blog</div>
      </div>
      <div className="list">
        <h4>글제목</h4>
        <p>2월 17일 발행</p>
        <hr/>
      </div>
    </div>
  )
}
```
맨 윗줄에 `import {useState} from 'react'` 하고 원하는 곳에서
`useState('보관할 자료')`를 쓰면 state에 자료를 잠깐 저장할 수 있다.   
그리고 저장한 자료를 나중에 쓰고싶으면   
`let [a, b] = useState('남자 코트 추천')'`   
`a` 자리에 `state 이름`을 자유롭게 작명한 다음 나중에 자유롭게 사용하면 된다.

> ### let 뒤에 저거 무슨 문법임
>    
> 자바스크립트 destructuring 문법으로   
> `array` 안에 있는 데이터들을 변수로 쉽게 저장하고 싶으면 쓰는 문법.   
>    
> 예를 들어서 `['Kim', 20]` 이렇게 생긴 `array 자료`를 만들어놨는데   
> 그 내부에 저장된 'Kim'과 20을 각각 변수에 저장하려면
> ```js
> let array = ['Kim', 20];
>    
> let name = array[0];
> let age = array[1];
> ```
> 해도 되지만
> ```js
> let [name, age] = ['Kim', 20];
> ```
> 요즘은 더 간결하고 예쁘게 작성이 가능하다. 한줄로 각각 `name = 'Kim'`, `age = 20`
> 이라는 변수가 생성된다. 귀찮게 등호 여러번 쓸 필요 없이 **왼쪽 오른쪽 형식을 똑같이 맞춰주면**
> 자동으로 알아서 변수가 생성된다.
> 이게 변수만들 때 자주 사용되는 **destructuring 문법**이다.
>    
> ### `useState()`함수는 방 두개짜리 배열을 반환한다.
> ```jsx
> const [state, setState] = useState(initialValue);
> ```
> ### 반환값 구성
>    
> | 항목       | 설명                             |
> |----------|--------------------------------|
> | `state`    | 현재 상태 값(초기값은 initialValue)     |
> | `setState` | 상태를 변경하는 함수 (호출 시 컴포넌트 리렌더링 됨) |
> `state`는 자료값이고 `setState`는 state 변경을 도와주는 함수다.   
>    
> 그 데이터들을 각각 변수로 빼고 싶으면   
> `let [a, b] = useState('남자 코트 추천')` 이렇게 작성하면 되는 것 뿐이다.

# 변수 말고 state에 저장해서 쓰는 이유
state 용도는 그냥 변수랑 똑같다.   
하지만 **state는 변동사항이 생기면 state쓰는 html도 자동으로 재랜더링 해준다.**   
기존 바닐라 자바스크립트에서는 기존 데이터값이 변경되면 해당 자료를 표출하고있는 html에 
반영하고 싶다면 직접 재렌더링 소스코드를 작성해야 한다.    
state 자료를 사용한다면
그럴 필요가 없다.   
개발자가 재렌더링 소스코드를 작성하지 않아도 자동으로
html도 바뀐다. state는 변경이 일wq:어나면 state가 포함된 html을 자동으로 재렌더링 해줘서 그렇다.

# 숙제
![숙제](https://github.com/rlawjddbs/rlawjddbs.github.io/blob/9991a24b5a2cca7aab07aed712b6dc857bb78b4e/content/posts/React/imgs/IMG_C1783B056DC4-1.jpeg)
   
### 숙제 결과
```jsx
function App() {

    const [posts, setPosts] = useState([
        {title: '강남 우동 맛집', content: '모름', likes: 0},
        {title: '남자 코트 추천', content:'본문', likes: 0},
        {title: '글제목', content:'신남', likes: 0}
    ]);

    return (
        <div className="App">
            <div className="black-nav">
                <h4>ReactBlog</h4>
            </div>
            {
                posts.map((item, index) => {
                    return (
                        <div className="list" key={index}>
                            <h4>
                                {item.title}
                                <span>
                                    <button onClick={() => {
                                        const copyPosts = [...posts];
                                        copyPosts[index].likes += 1;
                                        setPosts(copyPosts);
                                    }}>👍</button>
                                    {item.likes}
                                </span>
                            </h4>
                            <p>{item.content}</p>
                        </div>
                    )
                })
            }
        </div>
    )
}
```
- state 구조에 대한 고민이 필요함!