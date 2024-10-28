---
title: "인텔리제이_단축키"
date: 2024-05-07T09:59:27+09:00
draft: true
categories: ["Intellij"]
tags: ["Intellij"]
---
aa
## Mac OS 인텔리제이 단축키 모음
아래 작성된 단축키의 대부분은 윈도우의 경우 Command = Ctrl, Option = Alt 키로
대체하여 사용될 수 있고 최초 윈도우 환경에 맞춰 사용되다 Mac OS로 개발환경을 바꾸었기
때문에 검증되지 않은 단축키가 있을 수 있음...
- `Option + Shift + 방향키` 줄내렸다올렸다
- `Command + D` 한줄복사
- `Command + Y` / `Shift + Del` 한줄삭제
- `Command + Z` 되돌리기, `Command + Shift + Z` 되살리기
- `Command + Shift + F` 프로젝트 내 문자열 전체 검색, `Shift x 2` 파일검색
- `Command + G` 해당 라인으로 이동
- `Command + R` 문자열바꾸기
- `Command + Shift + F12` 창 크게/작게 하기
- `CTRL + ALT + O` 소스 내 빠진 패키지, 불필요한 패키지 등을 자동 임포트 및 삭제
- `Ctrl + F11` 북마크 이동하는 창 표시
- `Ctrl + 1` : 북마크 지정한 곳으로 이동
- `Ctrl + W`, `Option + ↑` 현재 커서에 걸쳐진 문자열 점점 확장 선택
  - `Option + ↓` 선택된 문자열 점점 축소 선택 
- `Ctrl + Shift + \`, `Ctrl + Shift + 백스페이스` 최근 작업한곳 왔다갔다
- `Ctrl + Alt + Shift + S` project structure셋팅(빌드패쓰..)
- `Ctrl + Shift + A` 바로 Action으로 검색
- `Ctrl + Shift + U` 대소문자 변경
- `Alt + Shift + Insert` 편집모드
- `Alt + MouseLeftButton` 세로블록
- `Command + Option + T` if문등 감싸기
- `Ctrl + Shift + ]` / `Ctrl + Shift + [` 괄호 시작,끝
- 파일선택후 `Ctrl + Alt + A` unversioned file svn에 추가
- `Ctrl + Shift + C` 현재파일경로 copy
- `project root and select from the menu > "Subversion > Show History"` svn history 보기
- -> 뻘짓하지말고 우측상단에 아이콘클릭
- ctrl + Shift + A , File Status Colors
- ->svn 색상 의미
- javadoc
- ctrl + alt + shift + g
- ->전체 주석생성
- ctrl + alt + shift + z
- ->전체 주석제거
- maven .m2에 라이브러리 생성안될때
- pom.xml 우클릭 maven -> reload project
- 외부라이브러리 추가할때
- File > Project Structure
- Modules > 프로젝트 > Dependencies > + 클릭 > JARs or directories...
- 추가할 라이브러리 선택 > OK 클릭
- 이미지새로 생성했는데 target이 리프레쉬 안될때
- 메뉴상단 build -> build Artifacts -> 원하는 빌드형태 선택 후 clean -> 재시작
- (프로젝트가 run일때는 반드시 stop후 clean)
- `Ctrl + Alt + Shift + Insert` 스크래치파일 생성 가능
- pom.xml에서 Ctrl+Shift+O
- ->load the changes.(자동로드?)
- Alt + Enter
- ->exception
- ->클래스생성(변수만들고 클래스없을때)
- Alt + Insert
- ->자동 override, get,set...
- 파일검색안될때
- File -> Invalidate Caches -> INVALIDATE AND RESTART
- ctrl + shift + T
- ->테스트코드 생성
- Split Right
- Setting - Keymap - Split Right - ` + ->
- Move To Opposite Group
- Setting - Keymap - Move To Opposite Group
- ` + <-
  vim 명령어
  gg=G 문서 전체 정렬 (편집됨) (편집됨)