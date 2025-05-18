---
title: "리액트 설치하기 (1)"
date: 2024-10-29T15:49:29+09:00
draft: true
categories: ["React"]
tags: ["React", "리액트", "설치"]
---
# 1. NVM 설치
React와 같은 웹 UI 라이브러리를 사용하는 데 Node.js가 필수인 것은 아니다. 별도의 설치 없이도 충분히 개발은 할 수 있다. 그러나 프로젝트가 커지고 복잡해지다 보면 순수 React만으로는 부족해지는 순간이 온다. 프로젝트의 성능 때문일 수도 있고, 개발자 경험을 위해서일 수도 있다.

필요한 라이브러리가 브라우저 상에서 동작하는 것이 아니라 CLI로 실행시켜야 할 수도 있다. 그럴때 Node.js가 필요해진다. 다양한 노드 버전은 nvm을 설치후 적절하게 골라가며 사용가능하고 노드로 만들어진 모듈은 npm을 통해 패키지를 설치하고 실행할 수 있다.

# 2. NPM 설치
상기 작성된 내용처럼 노드에서 사용가능한 모듈을 패키지화해 모아둔 프로그램이므로 설치한다.

# 3. 프로젝트 생성
작업용 폴더 만들고 `Terminal`에서

```bash
npx create-react-app 프로젝트명
```
   
   
{{< details title="npx란?" >}}
- npx는 Node Package eXecute의 줄임말이며 Package eXecute = 패키지 실행을 의미한다. `npm 5.2.0` 버전 이상부터 npm을 설치하면 자동으로 npx가 설치된다.
    - 즉, 새로운 패키지 관리 모듈이 아닌 npm의 5.2.0 버전부터 새롭게 추가된 `도구` 이다. 따라서 npm과 비교대상이 아니라 npm을 좀 더 편하게 사용하기 위해 최신 npm이 제공해주는 하나의 도구이다.
    - 패키지를 설치하지 않고도 npm 레지스트리에서 원하는 패키지를 실행(Execute)할 수 있다.
      - 실행, 즉 `npm package runner` 로써 npx는 일회용 패키지로써 사용된다.
      - node.js 를 이미 설치해서 사용하고 있다면, 왠만하면 npm이 5.2.0 버전 이상일테고, 그럼 npx 가 기본적으로 설치되어 있을 것이다.
      {{< /details >}}
   
{{< details title="npm registry란?" >}}
- ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0f97fd66-2ffc-4534-a9c8-04982497d8ed/92f16d5c-19bd-4068-89c4-1fb74ec3a399/Untitled.png)
- NPM Registry는 **누구나 Node.js에서 사용할 수 있는 모듈을 일정한 양식만 갖춘다면 등록할 수 있고, npm 명령을 통해서 Module Registry 에서 필요한 모듈을 설치할 수 있다.**
{{< /details >}}
   
   
그럼에도 확인해보려 한다면

```bash
$ which npx
```

그리고 만약 npx가 설치되어 있지 않다면 수동으로 설치

```bash
$ npm i -g npx
```

npx로 패키지 실행하려면

```bash
$ npx your-package-name
```

### 왜 등장했는가?

npm으로 패키지를 설치할 때, 두 가지 케이스가 있다.

1. 전역으로 패키지를 설치하여 의존성 라이브러리들을 전체적으로 관리하는 방법
2. 특정 프로젝트에만 의존성 라이브러리를 설치하는 방법

의존성 라이브러리(Dependency)들이 전역이나 로컬에 설치된 채 관리가 되면 어떤 문제가 있을까?

1. 패키지가 업데이트 된다면?
2. 전역으로 관리되고 있는 패키지도 따로 업데이트 해주고, 로컬로 관리되고 있는 패키지도 다로 업데이트하면 굉장히 번거로울 것이다.
3. 변화무쌍한 JS 생태계에서는 더욱 관리가 쉽지 않을 것이다.

이러한 문제를 해결할 수 있는 도구가 바로 npx이다.

> ***내가 패키지를 설치하고 업데이트를 하지 않더라도, npm 레지스트리에 올라가 있는 
> 최신 버전을 실행시키고 설치만 시키면 끝이다. 굉장히 간편하다 !***

### 언제 사용하는 가?

- **npm run-script**를 사용하지 않고 **로컬에 설치된 패키지를 사용할 경우**
- **일회성 명령으로 패키지를 실행할 경우**
- gist-based scripts 를 실행할 경우
- 특정 노드 버전의 스크립트를 실행할 경우

## NPM vs NPX

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0f97fd66-2ffc-4534-a9c8-04982497d8ed/84e45249-cd29-4df7-9db8-c532c338505f/Untitled.png)   
![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0f97fd66-2ffc-4534-a9c8-04982497d8ed/3251d7a9-8b47-406b-9125-930e48644636/Untitled.png)   

## 결론

포인트만 정리하자면

- `npm` : 패키지 **관리자**, 전역적
- `npx` : 패키지 **실행자**, 일회성

# 4. 프로젝트 실행(미리보기 띄우기)

`Terminal`에서 프로젝트 폴더로 이동하여

```bash
npm start
```

# 5. App.js가 메인페이지임

src 폴더 안에 있는 App.js 이게 메인 페이지니까 거기다가 코드짜면 됨

# 6. 내 사이트를 브라우저로 미리보기 띄우고 싶으면

프로젝트 폴더에서 `Terminal` 로 명령어 `npm start` 입력후 엔터치면 미리보기 뜸.

[localhost:3000](http://localhost:3000) 이라고 크롬 브라우저 열고 입력해주면 미리보기 가능(혹은 자동으로 뜸)