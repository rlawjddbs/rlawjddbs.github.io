---
title: "Maven Dependency scope"
date: 2022-06-24T11:33:12+09:00
draft: false
categories: ["Maven"]
tags: ["Maven"]
---

## compile

- `컴파일` 할 경우 필요


## runtime

- `런타임` 때 필요


## provided

- `컴파일` 때 필요하긴 하지만 `런타임` 때는 `JDK`, 또는 `컨테이너`가 제공
- `서블릿`이나 `JSP 관련 API` 같은 것
- 즉 `WAS`에서 제공하는 `servlet-api.jar`를 사용하는 경우에 해당
- 만약 `운영환경`에서 servlet-api.jar 중복으로 인한 문제가 발생한다면 꼭 `provided`로 바꿔줘야 함


## system

- `system`은 provided와 유사하지만 **`JAR 파일`을 직접 사용**
- 이 때는 JAR 파일의 위치를 지정하는 systemPath 속성을 지정해야함!!!


### ex(pom.xml)
```xml

...

<dependencies>
    <dependency>
        <groupId>...</groupId>
        <artifactId>...</artifactId>
        <version>...</version>
        <scope>...</scope>
        <systemPath>...</systemPath>
    </dependency>
</dependencies>

...

```



