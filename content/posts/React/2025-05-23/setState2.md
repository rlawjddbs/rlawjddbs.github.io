---
title: "array, object state 변경하는 법"
date: 2025-05-23T06:38:37+09:00
draft: true
categories: ["React"]
tags: ["React", "리액트", "setState", "참조 자료형의 state값 변경"]
---
[**정리**] 참조 자료형의 state값 변경 시에는 깊은복사를 활용하라
# 일단 글 수정 버튼 만들기
```jsx
function App(){
    
    let [글제목, 글제목변경] = useState( ['남자코트 추천', '강남 우동맛집', '파이썬 독학'] );
    
    return (
        <button onClick={ ()=>{ ??? } }> 수정버튼 </button>
    )
}
```
버튼을 하나 만든 뒤 그 버튼을 누르면 첫 번째 글이 수정되는 기능을 만들어보자.  
물음표 안에 무엇이 들어가야 할까?

```jsx
function App(){
    
    let [글제목, 글제목변경] = useState( ['남자코트 추천', '강남 우동맛집', '파이썬 독학'] );  
  
    return (
        <button onClick={ ()=>{ 
          글제목변경(['여자코트 추천', '강남 우동맛집', '파이썬 독학'])
        } }> 수정버튼 </button>
    )
}
```
이러면 숙제 끝일까?

> ### Q. 글제목변경('여자코트 추천') 이건 왜 안됩니까
> A. 기존 state를 갈아치워 주기때문에 안된다.  
> 현재 글제목이라는 state의 자료타입은 배열이다. 여기에 문자열을 넣으면 해당 state는 이를  
> char 타입의 배열로 인식하는 것이다. 따라서  
> ['남자코트 추천', '강남우동 맛집', '파이썬 독학'] 이걸  
> ['여자코트 추천', '강남우동 맛집', '파이썬 독학'] 이걸로 바꾸고 싶으면  
> 배열 형식을 유지하여 state 변경함수에 넣어야한다.

# 약간 프로그래머 스타일로 다시 만들어보면
현재 코드는 확장성이 부족하다.  
[] 안에 글이 3개밖에 없지만 글이 100개라면 어떻게 코딩할 것인가?  
현재 코드라면 onClick 안의 코드도 매우 길어질 것이다.  
따라서 기존 state를 다 복붙하지말고 기존 state 중 첫 글만 살짝 바꿔 state 변경함수에
집어넣는 식으로 개발해보자.

```jsx
function App(){
  
    let [글제목, 글제목변경] = useState( ['남자코트 추천', '강남 우동맛집', '파이썬 독학'] );  
    
    return (
        <button onClick={ ()=>{
            글제목[0] = '여자코트 추천';
            글제목변경(글제목)
        } }> 수정버튼 </button>
    )
}
```
이래도 된다.  
array 자료안의 X번째 항목을 변경하고 싶으면  
array자료[X] = '바꿀값' 이렇게 하면 된다.  
그래서 자료를 바꾸고 저기 state변경함수에 집어넣은 것이다.  
근데 이런거보다 더 나은 방법이 하나 있는데

```jsx
function App(){
  
    let [글제목, 글제목변경] = useState( ['남자코트 추천', '강남 우동맛집', '파이썬 독학'] );  
    
    return (
        <button onClick={ ()=>{ 
            let copy = 글제목;
            copy[0] = '여자코트 추천';
            글제목변경(copy)
        } }> 수정버튼 </button>
    )
}
```
array, object 자료 다룰 때는 원본 데이터를 직접 조작하는 것 보다는  
**기존값은 보존해주는 식으로 코드짜는게** 좋은관습이다.  
(왜냐면 원본 막 바꿔버렸을 때 갑자기 원본이 필요해지면?)  
그래서 let copy 같은 변수에다가 기존 array를 복사해놓고  
그걸 조작하는 식으로 코드짜면 조금 더 안전하다.  
근데 수정버튼 눌러도 안바뀌는데...?

```jsx
function App(){
  
    let [글제목, 글제목변경] = useState( ['남자코트 추천', '강남 우동맛집', '파이썬 독학'] );  
  
    return (
        <button onClick={ ()=>{ 
            let copy = [...글제목];
            copy[0] = '여자코트 추천';
            글제목변경(copy)
        } }> 수정버튼 </button>
    )
}
```
이러면 제대로 동작한다.  
근데 동작원리가 궁금하지않은가?  
동작원리같은거 많이 알고 있으면 나중에 혼자 코드짤 때 많은 응용이 가능하다.

# state 변경함수 동작원리
state 변경함수를 쓸 때  
`기존state === 신규state` 이렇게 먼저 비교연산자로 일치하는지를 검사해본다.  
그래서 같으면 state 변경을 해주지 않는다.  
그래서 위 코드에서도 `글제목변경(copy)` 해도  
copy라는 변수가 기존 state와 같아서 변경을 안해준 것이다.

> ### Q. 잉 copy라는 변수랑 기존 state랑 안에 있는 자료가 다른데 왜 같다고함
> 기존 state는 '남자코트 추천'  
> copy에는 '여자코트 추천'이 들어있지만  
> 실은 기존state === copy 비교해보면 같다고 나온다.  
> 왜일까?
> ### array/object 동작원리
> 1. 자바스크립트는 array/object 자료를 하나 만들면  
> 예를 들어서 let arr = [1,2,3] 이렇게 만들면  
> [1,2,3] 자료는 램이라는 가상공간에 몰래 저장이 되고  
> let arr 변수엔 그 자료가 어디있는지 가리키는 화살표(주소값)만 담겨있다.    
>    
> 
> 2. 그래서 array/object 자료를 복사하면 이상한 일이 일어나는데  
> 예를 들면
> ```jsx
> let data1 = [1,2,3];
> let data2 = data1;   //복사문법임 
> ```
> 이런 식으로 사용하면 복사가 된다.  
> data1에 있던 자료를 data2에 복사한다는 뜻이다.  
> 그럼 data2 출력해보면 [1,2,3] 이게 잘 나온다.  
> 근데 data1과 data2는 각각 [1,2,3]을 별개로 저장하는게 아니라  
> data1과 data2는 똑같은 값을 공유한다.  
> data1을 변경하면 data2도 자동으로 변경되고 그렇다. (충격)  
>   
>   
> 왜냐면 변수에는 화살표(주소값)만 저장된다.  
> 그래서 방금 님들 화살표(주소값)를 복사한 것임  
> 그래서 data1, data2는 똑같은 화살표(주소값)를 가지게 된다. 같은 자료를 가리키는 것이다.
>   
> 3. 그래서 같은 화살표(주소값)를 가지고 있는 변수끼리는  
> 등호로 비교해도 똑같다고 나온다.
> ```jsx
> let data1 = [1,2,3];
> let data2 = data1;  //복사
> data2[0] = 1000;  //data2 내부 변경
> console.log(data2 === data1)   //true 나올듯
> ```
> 그렇다.
> 자세한건 javascript reference data type이라고 검색 시 추가학습 가능하다.
>   
>   
> ```jsx
> let copy = 글제목;
> copy[0] = '여자코트 추천';
> 글제목변경(copy)
> ```
> 그래서 아까처럼 이렇게하면  
> 컴퓨터는 copy와 기존 글제목 state는 똑같다고 생각하기 때문에 (화살표가 똑같아서)  
> state 변경을 안해준다. 
> ```jsx
> let copy = [...글제목];
> copy[0] = '여자코트 추천';
> 글제목변경(copy)
> ```
> 이렇게 하면 잘된다. 화살표(주소값)가 달라지는 문법이라 그렇다.
> 
> ### 저기요 점3개 뭐임
> pread operator 라고하는 문법인데  
> array나 object 자료형 왼쪽에 붙일 수 있으며  
> 뜻은 별거없고 괄호를 벗겨주세요~ 라는 뜻이다.  
> `...[1,2,3]` 이렇게 쓰면  
> 그 자리에 1,2,3 이 남는다. 걍 괄호 벗기기용 연산자다.  
> 근데 두번째 용도도 있는데 array나 object 자료형을 복사할 때 많이 사용한다.  
> ```jsx
> let data1 = [1,2,3];
> let data2 = [...data1];
> console.log(data1 === data2) //false 나올듯
> ```
> 그냥 data1에 있던 자료들을 괄호 벗긴담에 다시 array로 만들어주세요~ 라고 사용하면  
> 화살표가 달라진다. 새로운 array로 인식한다.  
> 그래서 그렇게 하면 화살표가 다른 완전 독립적인 array 복사본을 생성해줄 수 있습니다.
> object 자료형도 마찬가지다.
> 그리고 독립적인 사본을 shallow copy 아니면 deep copy 라고 한다.
>> # 정리
>> 이전까지가 코딩애플님 강의에 나온 내용이고 쉽게 말하면 state값이 
>> 참조 자료형일 경우에는 얕은복사 또는 원본 state의 값을 변경해서 
>> setState() 하면 이전과 값을 === 비교연산자로 비교하여 true가 
>> 나와버린다(참조 자료형의 경우 변수 자체가 가진값은 주소값이므로).  
>> 따라서 `이전 state 값 === 이후 state 값`이 false가 나오게 하려면 
>> 해당 state값을 깊은복사하여 setState() 함수의 parameter로 넘겨야 하는 것이다.

# 숙제
![숙제](https://github.com/rlawjddbs/rlawjddbs.github.io/blob/9991a24b5a2cca7aab07aed712b6dc857bb78b4e/content/posts/React/imgs/IMG_023257120684-1.jpeg)

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