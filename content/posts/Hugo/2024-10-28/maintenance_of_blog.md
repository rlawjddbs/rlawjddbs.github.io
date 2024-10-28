---
title: "블로그 재정비"
date: 2024-10-28T18:37:29+09:00
draft: true
categories: ["Hugo"]
tags: ["Hugo"]
---

### ???
![???](/posts/Hugo/2024-10-28/images/confused.gif)   
   
코딩애플님의 리액트 강의를 정리하기전 간단히 `hugo server`명령어로 세팅한 블로그를 로컬환경에서 가동했는데 뭔가 
잘못된걸 느꼈다.

```error
...
found no layout file for “HTML” for “section”: You should create a template file which matches Hugo Layouts Lookup Rules for this combination.
found no layout file for “HTML” for “section”: You should create a template file which matches Hugo Layouts Lookup Rules for this combination.
found no layout file for “HTML” for “taxonomyTerm”: You should create a template file which matches Hugo Layouts Lookup Rules for this combination.
...
```
대략 이런류의 에러가 뜨는데 참조해야할 `themes` 폴더 내 PaperMod 폴더가 비어있기 때문이었다.
따라서 로컬 환경에서 부가적으로 git 서브 모듈을 사용하기 위한 작업을 진행했다.

```shell
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
git submodule update --init --recursive # needed when you reclone your repo (submodules may not get cloned automatically)
```

명령어 파라미터로 입력한 경로에 해당 서브모듈이 잘 내려받아진 것을 확인하고 다시 한 번 
`hugo server`를 입력해본다.

```error
Watching for changes in /.../rlawjddbs.github.io/{archetypes,content,layouts,static,themes}
Watching for config changes in /.../rlawjddbs.github.io/config.yml
Start building sites … 
hugo v0.136.5+extended darwin/arm64 BuildDate=2024-10-24T12:26:27Z VendorInfo=brew
ERROR deprecated: .Site.Social was deprecated in Hugo v0.124.0 and will be removed in Hugo 0.137.0. Implement taxonomy 'social' or use .Site.Params.Social instead.
Built in 32 ms

Error: error building site: logged 1 error(s)
zeongyun@zeongyun-macbook rlawjddbs.github.io % hugo server
Watching for changes in /.../rlawjddbs.github.io/{archetypes,content,layouts,static,themes}
Watching for config changes in /.../rlawjddbs.github.io/config.yml
Start building sites … 
hugo v0.136.5+extended darwin/arm64 BuildDate=2024-10-24T12:26:27Z VendorInfo=brew

ERROR deprecated: .Site.Social was deprecated in Hugo v0.124.0 and will be removed in Hugo 0.137.0. Implement taxonomy 'social' or use .Site.Params.Social instead.
Built in 34 ms
Error: error building site: logged 1 error(s)
```
`음... 안되구요...`
기존 PaperMod 템플릿에서 `.site.Social` 이라는 Context 변수를 사용하는 듯 하여 확인해보았다.
그리고 실제 템플릿 내에서 `.site.Social`을 사용중인 파일이 있어 해당 변수를 위 경고문에서
안내한대로 `.site.Params.Social`로 수정한 후 `hugo server` 해보았더니...

```shell
hugo server
Watching for changes in /.../rlawjddbs.github.io/{archetypes,content,layouts,static,themes}
Watching for config changes in /.../rlawjddbs.github.io/config.yml
Start building sites … 
hugo v0.136.5+extended darwin/arm64 BuildDate=2024-10-24T12:26:27Z VendorInfo=brew

                   | EN  
-------------------+-----
  Pages            | 41  
  Paginator pages  |  0  
  Non-page files   |  7  
  Static files     |  3  
  Processed images |  0  
  Aliases          | 10  
  Cleaned          |  0  

Built in 43 ms
Environment: "development"
Serving pages from disk
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1) 
Press Ctrl+C to stop
```
결과적으론 손쉽게 해결된듯하나 방치되다시피한 hugo 블로그 다시 세팅하려니 처음 세팅할적 들였던
시간이 똑같이 소비된다...