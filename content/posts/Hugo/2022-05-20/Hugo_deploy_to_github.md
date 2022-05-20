---
title: "Github pages 변경"
date: 2022-05-20T21:11:27+09:00
draft: false
---

# 1. Github 프로젝트 생성

![Github 프로젝트 생성](posts/Hugo/2022-05-20/images/create.png)   

- Project name: `닉네임`.github.io
gitlab은 private 가능한데 github는 public으로 해야 pages 설정이 가능하다.. 무료 사용자 기준


# 2. hugo 설치

참고 사이트
[hugo official](https://hugo.io) - 휴고 공식 사이트  

`homebrew`를 이용하여 설치한다.

```shell
brew install hugo
```

# 3. hugo로 새 사이트 만들기

hugo 설치 완료 후 사이트를 배포하고 싶은 디렉토리를 생성하고 해당 디렉토리로 이동 후

```shell
hugo new site 생성할사이트명 -f yml
```

여기서 `-f yml` 옵션은 선택적인 것인데 설정 파일을 `.yml` 포맷으로 쓰겠다는 뜻이다. 안쓰면 그냥 `.toml` 형식을 씀.


# 4. git init

```shell
# 3번 수행 후 생성된 사이트 디렉토리로 이동한 후
git init
```

3번 과정에서 생성된 디렉토리로 이동 후 `git init` 명령어를 입력한다. hugo의 테마를 적용하기 위해 `git`의 `submodule`을 이용해야 하는데
`git init`이 수행되지 않으면 당연히 서브 모듈을 못 쓴다.


# 5. hugo의 테마 고르기

[hugo theme](https://themes.gohugo.io)  
각 테마별로 사이트 데모, 설치 방법등이 상이하지만 모든 테마는 깃 서브 모듈을 이용하여 적용된다.


# 6. git sub-module 적용

```shell
# git init한 디렉토리에서
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
git submodule update --init --recursive
```

위 명령어를 통해 themes 디렉토리에 Paper Mod 테마를 서브 모듈로 `clone` 했다.


# 7. config.yml에 테마 설정

```yml
baseURL: https://사용자닉네임.github.io
languageCode: en-us
title: 제목
themes: "PaperMod"
...
```

실제 작성될 내용은 더 길다. `baseURL`, `title`을 수정하고 `theme` 속성을 추가한 뒤 속성값으로 사용할 테마명을 적어준다.

# 8. github pages, actions 설정

레포지토리의 settings 탭 메뉴로 이동 후 pages와 actions 서브 메뉴로 이동하여 각각 아래와 같이 세팅 해주었다.

![Github Actions](/posts/Hugo/2022-05-20/images/actions_setting1.png)  

![Github Actions](/posts/Hugo/2022-05-20/images/actions_setting2.png)  

# 9. github actions 작성

![Github Actions](/posts/Hugo/2022-05-20/images/action.png)  

1번 과정에서 만든 깃허브 repository의 Actions 탭 메뉴로 이동하면 위처럼 액션을 세팅할 수 있다. `main.yml`을
기본적으로 만들어 주는데 아래와 같은 세팅을 한다. 

```yml
name: github pages

on:
  push:
    branches:
      - main  # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          # extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

