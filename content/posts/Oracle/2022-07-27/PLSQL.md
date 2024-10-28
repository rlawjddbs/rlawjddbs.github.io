---
title: "PL/SQL1-변수, 연산자, 제어문"
date: 2022-07-27T13:53:00+09:00
draft: false
---

# PL/SQL

- `Procedural Language`
- `DBMS`에서 제공하는 언어적인 요소


## 1. 작성구조

```SQL
DECLARE
    변수선언
BEGIN
    코드작성
END;
/
```

- PL/SQL 코드의 끝은 항상 슬래쉬`/`로 끝이 나야함


## 2. 저장 형식

- `파일명.sql`


## 3. 실행

- `cmd/편집기` → `@파일명.sql`
    - error 발생: 왜?

## 4. 화면출력

```SQL
sqlplus -- 계정연결
set serveroutput on -- 출력설정
dbms_output.put_line(출력할 내용); -- 출력 후 줄 변경
dbms_output.put(출력할 내용) -- 출력 후 줄 변경 X
```
- 출력할 내용에는 변수, 문자열, 연산식, '문자열' 등등이 들어감


