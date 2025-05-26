---
title: "Component : 많은 div들을 한 단어로 줄이고 싶으면"
date: 2025-05-23T06:38:37+09:00
draft: true
categories: ["React"]
tags: ["React", "리액트", "component", "fragment", "fragment 문법"]
---
[**정리**] component를 사용하여 모달 만들어보기
# 상세페이지 겸 모달창 UI 만들어보기
```jsx
<div className="modal">
    <h4>제목</h4>
    <p>날짜</p>
    <p>상세내용</p>
</div>
```
```css
.modal{
    margin-top : 20px;
    padding : 20px;
    background : #eee;
    text-align : left;
}
```
당연히 css 코드는 css 파일에 넣어야 한다.
모달창 겸 상세페이지 완성 끝

# fragment 문법
```jsx
return (
    <div></div>
    <div></div>
)
```
리액트에서 html 코드 짤 때 유의점이 있는데  
`return ()` 안에 두 개 이상의 최상위 html 요소가 존재하면 안된다.  
`return ()` 내부는 하나의 태그로 시작해서 하나의 태그로 끝나야 한다.

```jsx
return (
    <div>
        <div></div>
        <div></div>
    </div>
)
```
그래서 굳이 \<div\> 두개를 나란히 적고 싶으면 저렇게 하나의 div로 또 감싸거나 해야한다.  
의미없는 div 쓰기 싫다면 \<\>\</\> 이걸로 감싸도 된다.  
fragment 문법이라고 한다.
```jsx
// fragment 사용 
return (
    <>
        <div></div>
        <div></div>
    </>
)
```

# 복잡한 html을 한 단어로 치환할 수 있는 Component 문법
리액트는 긴 html을 한 단어로 깔끔하게 치환해서 넣을 수 있는 문법을 제공한다.  
Component라고 한다.  
이걸 이용하면 함수 만들듯, 변수 만들듯 html을 깔끔하게 한 단어로 치환해서
원하는 곳에 꽂아 넣을 수 있다.  
방금 만든 모달도 깔금하게 한 단어로 치환해보자.

```jsx
function App (){
  return (
    <div>
      (생략)
      <Modal></Modal>
    </div>
  )
}

function Modal(){
  return (
    <div className="modal">
      <h4>제목</h4>
      <p>날짜</p>
      <p>상세내용</p>
    </div>
  )
}
```
이렇게 하면 원하는 html을 한 단어로 줄일 수 있다.  
줄이는 법은
1. function을 이용해서 함수를 하나 만들어주고 작명한다.
2. 그 함수 안에 return () 안에 축약을 원하는 HTML을 담으면 된다.
3. 그럼 원하는 곳에서 \<함수명\>\</함수명\> 사용하면 아까 축약한 HTML이 등장한다.
  
이렇게 축약한 HTML 덩어리를 Component 라고 부른다.  
앞으로 HTML 깔끔하게 축약해서 쓰고싶으면 Component 이런 식으로 많이 만들어 쓰자.  
\<div\> 뭉텅이보다 깔끔하게 \<Modal\> 이렇게 되어있으니   
남이 봤을 때 & 미래의 내가 봤을 때 레이아웃을 바로 파악이 가능하니 나중에 관리하기도 좋을것이다.

# Component 만들 때 주의점
1. component 작명할 땐 영어대문자로 보통 작명한다.
2. return () 안엔 html 태그들이 평행하게 여러개 들어갈 수 없다.
3. function App(){} 내부에서 만들면 안된다.  
왜냐면 **function App(){} 이것도 알고보면 컴포넌트 생성문법**이다.  
**component 안에 component 를 만들진 않는다.**
4. \<컴포넌트\>\</컴포넌트\> 이렇게 써도 되고 \<컴포넌트/\> 이렇게 써도 된다.

# arrow function 써도 된다


```jsx
function Modal(){
    return ( <div></div> )
}
let Modal = () => {
    return ( <div></div> )
}
```
함수 만드는 문법이 하나만 있는게 아니라 저렇게 써도 된다.  
let 대신 const 변수 쓰는 사람들도 있다.  
마음에 드는거 쓰도록 하자.

# 어떤 HTML들을 Component 만드는게 좋을까

기준은 없지만 관습적으로 어떤 부분을 주로 Component화 하냐면  
- 사이트에 반복해서 출현하는 HTML 덩어리들은 Component로 만들면 좋다.
- 내용이 매우 자주 변경될 것 같은 HTML 부분을 잘라서 Component로 만들면 좋다.
- 다른 페이지를 만들고 싶다면 그 페이지의 HTML 내용을 하나의 Component로 만드는게 좋다.
- 또는 다른 팀원과 협업할 때 웹페이지를 Component 단위로 나눠서 작업을 분배하기도 한다.
  
여러분들 함수문법 언제쓰는가
1. 긴 코드 축약할 때
2. 다른 곳에서 코드 재사용할 때 
3. 복잡한 코드를 작은 기능으로 나눌 때 쓰지 않는가  
컴포넌트는 그냥 함수 문법이랑 똑같아서 용도도 똑같다.

# Component의 단점
일단 HTML 깔끔하게 쓰려고 Component를 수백개 만들면 그것 만으로도 관리가 힘들다.  
예를 들어서 function Modal 안에서 글제목 state를 쓰고싶어서 {글제목} 
이렇게 쓰면 잘 안되는데  
왜냐면 당연히 자바스크립트에선  
한 function 안에 있는 변수를 다른 function에서 맘대로 쓸 수 없어서 그렇다.  
props라는 문법을 이용해 state를 <Modal>까지 전해줘야 비로소 사용가능하다.  
  
props를 배우진 않았지만 듣기만 해도 귀찮지 않은가?  
그러니까 리액트 갓 배운 초보처럼 막 온갖 잡다한걸 
Component로 쪼개지 말고 꼭 필요한 곳만 Component로 쪼개길 바란다.

# 숙제
오늘의 숙제 :
연습삼아 다른 컴포넌트 몇개 더 만들어보자.
그리고 지우고 다음강의 넘어가자. 