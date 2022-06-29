---
title: "Procedure"
date: 2022-06-29T11:02:42+09:00
draft: false
---

# Procedure

```SQL
CREATE [OR REPLACE] PROCEDURE 프로시저명 (변수명 [IN/OUT] 데이터형, ...)

IS
    변수선언

BEGIN
    코드작성

END;
/
```

- `IN`: in parameter, 외부값(내부, 값을 저장 X)
- `OUT`: out parameter, 내부값(외부, 값을 저장 O)


# Cursor

```SQL
CURSOR 커서명 IS
    SELECT
        ZIPCODE,
        ...
    FROM
        ZIPCODE
    WHERE
        DONG LIKE I_DONG% -- error
```

> 단지 연산만 할거라면 함수를 쓰고, 쿼리까지 사용해야 한다면 프로시저를 사용한다.


# Package

- 관련있는 `function`, `procedure`를 묶어 관리하는 객체
- `header`와 `body`로 구성
- `user_procedures` 테이블에서 조회가능
- package로 묶여있는 function, procedure를 호출할 떄 

> - header: `package`에 포함될 `function`, `procedure`의 목록 정의(`body`가 없으면 일을 할 수 없다.)
> - body: `header`에 포함된 `function`, `procedure`의 실제 동작되는 코드를 정의
