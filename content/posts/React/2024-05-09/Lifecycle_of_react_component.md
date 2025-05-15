---
title: "Lifecycle과 useEffect1"
date: 2024-05-09T08:56:55+09:00
draft: true
categories: ["React"]
tags: ["React", "라이프사이클", "Lifecycle", "useEffect"]
---
## 컴포넌트의 라이프사이클
- 컴포넌트는 사람처럼 태어나고 죽음
- 상세 페이지로 이동 시 Detail 컴포넌트가 보인다
  - 페이지에 장착되기도 하고(mount)
  - 가끔 업데이트도 되고(update)
  - 메인 페이지로 이동 시 필요없으면 제거된다(unmount)

## 라이프사이클을 알아야 하는 이유
- 위 표기된 라이프사이클 중간중간에 간섭을 할 수 있음
  - 라이프사이클 중간중간 코드실행가능

## 라이프사이클훅
- 컴포넌트 라이프사이클에 갈고리를 건다고 연상하면 이해가 쉬움
- 훅에 실행할코드를 담아 라이프사이클에 훅을 건다 생각하면 쉬움

## 컴포넌트에 갈고리 다는 법
### 클래스 방식
```js
class Detail extends React.Component {
    componentDidMount() {
        
    }
    componentDidUpdate() {
        
    }
    componentWillUnmount() {
        
    }
}
```

### 함수방식
```js
import { useEffect } from "react";

function Detail() {
    useEffect(() => {
        // mount, update 시 여기 코드 실행됨
    });
}
```

> ### Q. 컴포넌트 렌더링 시 내부 함수 실행되는데 라이프 사이클 쓰는 이유
> #### A. 라이프사이클훅 내부의 코드는 html 렌더링 후에 동작함

> ### Q. 왜 이름이 Effect~~ 인지
> #### Side Effect: 함수의 핵심기능과 상관없는 부가기능. 여기에서 따온 이름


## useEffect 안에 적는 코드들은
- 어려운 연산
- 서버에서 데이터가져오는 작업
- 타이머 장착하는거