---
title: "리액트 React 설치와 개발환경 셋팅(新)"
date: 2024-10-30T15:49:29+09:00
draft: true
categories: ["React"]
tags: ["React", "리액트", "설치", "vite 사용"]
---

**[정리]** vite를 사용하여 
# 1. Node 프로젝트 생성하기
`ESM`방식의 번들러인 `Vite`를 사용하여 리액트 프로젝트 빌드 시도
```bash
npm create vite@latest
Need to install the following packages:
  create-vite@latest
Ok to proceed? (y) y
│
◇  Project name:
│  blog
│
◆  Select a framework:
│  ○ Vanilla
│  ○ Vue
│  ● React
│  ○ Preact
│  ○ Lit
│  ○ Svelte
│  ○ Solid
│  ○ Qwik
│  ○ Angular
│  ○ Marko
│  ○ Others
◆  Select a variant:
│  ○ TypeScript
│  ○ TypeScript + SWC
│  ● JavaScript
│  ○ JavaScript + SWC
│  ○ React Router v7 ↗
│  ○ TanStack Router ↗
│  ○ RedwoodSDK ↗
└
```
1. 방향키로 `React` 찾아서 `Enter`
2. 두 번째 선택지에서 `Javascript`
   
이후에 프로젝트 빌드가 완료되면 아래와 같은 안내가 나타난다.
```bash
◇  Project name:
│  blog
│
◇  Select a framework:
│  React
│
◇  Select a variant:
│  JavaScript
│
◇  Scaffolding project in /Users/zeongyun/Desktop/test_2/blog...
│
└  Done. Now run:

  cd blog
  npm install
  npm run dev
```


# 2. npm install 실행
```bash
cd blog # blog 프로젝트 경로
npm install # Node Package Manager으로 리액트 프로젝트에 필요한 라이브러리 설치 
```
`npm install` 실행하면 해당 경로에 리액트 프로젝트에 필요한 라이브러리가
설치된다. 그 후에 에디터에서 프로젝트 폴더를 오픈하면 개발 준비가 완료된다.

# 3. App.js(jsx)가 메인 페이지
`src` 폴더 내 App.jsx가 메인 페이지라고 생각하면 된다.
가장 먼저 실행되는 `main.jsx` 파일이 `App.jsx` 를 `App`이라는
컴포넌트로 `import` 한 뒤 실행시켜준다.

# 4. npm run dev 실행
```bash
npm run dev
```
미리보기를 지원해준다. 오류가 발생하면 사용중인 Node 버전의 문제일 수 있다.
성공적으로 실행된다면 아래와 같은 결과 메시지가 뜬다.

```bash
npm run dev

> blog@0.0.0 dev
> vite


  VITE v6.3.5  ready in 1358 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help
```

