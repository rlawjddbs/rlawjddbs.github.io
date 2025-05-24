---
title: "버튼에 기능개발을 해보자 & 리액트 state변경하는 법"
date: 2025-05-23T06:38:37+09:00
draft: true
categories: ["React"]
tags: ["React", "리액트", "setState", "터미널/브라우저 콘솔창에 warning", "react return() 내부에 if문"]
---
[**정리**] state를 변경하려면 state 변경함수를 사용하라
# 터미널/브라우저 콘솔창에 warning이 뜨는 이유
eslint라는 라이브러리가 잘못된 코딩에 대한 지적을 하는 것.   
`/*eslint-disable*/` 이라는 주석을 jsx 파일 최상단에 
추가해주면 eslint 기능을 잠시 끌 수 있다.
```jsx
/*eslint-disable*/

...

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

# state 변경하는 방법
```jsx
function App(){
// useState(state 초기값)는 [state 초기값, state 변경함수]를 반환값으로 return 해준다.
let [ 따봉, 따봉변경 ] = useState(0); 
return (
    <h4> { 글제목[0] } <span onClick={ ()=>{ 따봉 = 따봉 + 1 } } >👍</span> { 따봉 }</h4>
  )
}
```
state 만들 때 2개까지 작명할 수 있는데, 두 번째 작명한건 state 변경을 도와주는 함수다.
이걸 써야 state 변경이 가능하다.

사용법은
```jsx
따봉변경(새로운state)
```
와 같이 작성하면 된다.
왜냐면 state 함수는 () 안에 넣은걸로 state를 갈아치워주는 함수이기 때문에 그렇다.   
   
`따봉변경(1)`이라고 사용하면 따봉이라는 state가 1로 변경된다.  
`따봉변경(100)`이라고 사용하면 따봉이라는 state가 100으로 변경된다.  
`따봉변경(따봉 = 따봉 + 1)` 이런식으로는 사용할 수 없다. 깔끔하게 새로운 state만 집어넣으면 된다.

# 좋아요를 눌렀을 때 따봉이라는 state를 1 증가하려면
```jsx
function App(){
  
  let [ 따봉, 따봉변경 ] = useState(0);
  return (
      <h4> { 글제목[0] } <span onClick={ ()=>{ 따봉변경(따봉 + 1) } } >👍</span> { 따봉 }</h4>
  )
}
```
**따봉이라는 기존 state에 1을 더한 값**을 따봉변경 함수에 집어넣었다.  
그럼 버튼을 누를 때 마다 그 값으로 대체된다.

# 숙제
![숙제](https://github.com/rlawjddbs/rlawjddbs.github.io/blob/9108453b64e7a5581221c8f1689a4624e2f09096/content/posts/React/imgs/IMG_023257120684-1.jpeg)

### 숙제 결과
```jsx
function App() {

    const [posts, setPosts] = useState([
        {title: '남자 코트 추천', content:'본문'},
        {title: '강남 우동 맛집', content: '모름'},
        {title: '글제목', content:'신남'}
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
                            <h4>{item.title}</h4>
                            <p>{item.content}</p>
                            {
                                0 === index
                                    ? <button style={{'float': 'right'}} onClick={() => {
                                        const copyPosts = [...posts];
                                        copyPosts[index].title = '여자 코트 추천';
                                        setPosts(copyPosts);
                                    }}>제목 수정</button>
                                    : null
                            }
                        </div>
                    )
                })
            }
        </div>
    )
}
```
- jsx return문 내부에 if 문을 사용할 수 없다고 한다(!!!)
- 따라서 삼항연산자를 써야한다.