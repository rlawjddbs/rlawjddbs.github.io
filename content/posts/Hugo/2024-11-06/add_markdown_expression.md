---
title: "Add_markdown_expression"
date: 2024-11-06T12:21:15+09:00
draft: false
categories: ["Hugo"]
tags: ["Hugo", "테마 커스터마이징", "Customize a theme"]
---
# 블로그에 \<detail\> 요소 사용할 수 있게 추가하기
어찌된 영문인지 github markdown 에서는 `<detail>`을 사용할 수 있었는데 `hugo blog`
에서는 사용이 되지 않는다. `stack overflow`에서 검색좀 해보니 hugo에서는 따로 표현식을
허용하는 과정이 필요했다.

# details.html 추가하기
`layouts/shortcodes/` 디렉토리 내부에 `details.html` 파일을 추가한다.
추가한 파일에 아래 내용을 추가한다.

```jsx
<details>
    <summary>{{ (.Get "title") | default "펼침" | markdownify }}</summary>
    <div>{{ .Inner | markdownify }}</div>
</details>
```
`.Get "title"`은 `<detail>`태그의 `title`속성값을 `<summary>`태그 내용으로 사용
함을 의미한다. 