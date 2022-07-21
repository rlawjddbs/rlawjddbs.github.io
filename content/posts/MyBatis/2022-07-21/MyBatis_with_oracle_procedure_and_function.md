---
title: "MyBatis에서 오라클 Procedure, Function 호출 및 반환값 받기"
date: 2022-07-21T19:03:31+09:00
draft: false
---

\[[**프로시저에 대한 약식 설명**](https://rlawjddbs.github.io/posts/oracle/2022-06-29/procedure/)\] 


# 1. 프로시저 생성하기

```SQL
create or replace (EDITIONABLE||NONEDITIONABLE) PROCEDURE GET_CODE_LIST (
    IN_PARAM IN CLOB,
    OUT_CURSOR OUT SYS_REFCURSOR
)
IS
    P_GRP_CD VARCHAR(30) := JSON_VALUE(IN_PARAM, '$.GRP_CD');
BEGIN
    OPEN OUT_CURSOR FOR
        SELECT
            DATA_CD,
            DATA_NM,
            GRP_CD
        FROM 
            SYS_CD_MGMT
        WHERE
            GRP_CD = P_GRP_CD;
END GET_CODE_LIST;
```

- EDITIONABLE || NONEDITIONABLE 옵션: 