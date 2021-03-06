---
title: spring security 로그인 인증
updated: 2019-11-07 09:40
category: Java
---
**NOTE** : spring-security 파헤치기 <a href="https://minwan1.github.io/2017/04/22/2017-04-22-spring-security-implement/" target="_new">(참고 문서)</a>

### Maven 의존성 추가 (pom.xml)
```xml
<dependency>
	<groupId>org.springframework.security</groupId>
	<artifactId>spring-security-core</artifactId>
	<version>3.1.3.RELEASE</version>
</dependency>

<dependency>
	<groupId>org.springframework.security</groupId>
	<artifactId>spring-security-taglibs</artifactId>
	<version>3.1.3.RELEASE</version>
</dependency>

<dependency>
	<groupId>org.springframework.security</groupId>
	<artifactId>spring-security-config</artifactId>
	<version>3.1.3.RELEASE</version>
</dependency>
```

### web.xml 설정
```xml
<filter-name>springSecurityFilterChain</filter-name>
	<filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class> 
	<!-- filter 처리를 spring security로 위임하는것 -->
</filter>

<filter-mapping>
	<filter-name>springSecurityFilterChain</filter-name>
	<url-pattern>/*</url-pattern>
	</filter-mapping>
<filter>

<!-- ========================= -->
<!-- spring-security 설정 파일 -->
<!-- ========================= -->
<context-param>
	<param-name>contextConfigLocation</param-name>
	<param-value>/WEB-INF/spring/security-context.xml</param-value>
</context-param>
<listener>
```

### spring security xml파일 설정
```xml
<security:http auto-config="true" use-expressions="true">
	<!-- expressions를 사용할 수 있게 설정해야 hasRole,permitAll등의 다양한 옵션들을 쓸수 있음 -->
	<security:intercept-url pattern="/login/login.do" access="isAnonymous()" />

	<security:intercept-url pattern="/images/**" access="permitAll" />
	<security:intercept-url pattern="/css/**" access="permitAll" />
	<security:intercept-url pattern="/lib/**" access="permitAll" />
	<security:intercept-url pattern="/**/*.js" access="permitAll" />
	<!-- 누구나 다들어갈수 있음 -->

	<security:intercept-url pattern="/**" access="hasAnyRole('ROLE_SYS, ROLE_ADMIN, ROLE_USER')" />

	<security:intercept-url pattern="/admin/**" access="hasAuthority('ROLE_ADMIN')"/>
	<!-- 관리자만 들어갈수있음 -->
	<security:intercept-url pattern="/manager/**" access="hasRole('ROLE_MANAGER')"/> 
	<!-- 위랑 같은뜻 매니저만 들어갈수있음 -->

	<security:form-login login-page="/loginForm"/> 
	<!-- 로그인 url -->
	<!-- <security:form-login login-page="/loginForm" authentication-failure-url="성공url "/> -->
	<security:intercept-url pattern="/member/**" access="isAuthenticated()"/>
	<!-- 인증한사람만 들어갈수있음 -->
	<security:intercept-url pattern="/user" access="hasRole('ROLE_USER')" />
	<security:intercept-url pattern="/welcome" access="hasRole('ROLE_ADMIN')" />

</security:http>
```
`<security:form-login>` 의 `login-page` 속성값을 설정하면 로그인 하지 않은 상태에서 권한이 없는 url로 접근 시 자동으로 `login-page` 옵션에 설정된 url로 이동.


