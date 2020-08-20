---
layout: post
title: "[Java] Builder Pattern을 사용하여 CookieBuilder 클래스 설계"
tags: ["java"]
comments: true
---

웹 서비스를 개발하다보면 쿠키를 활용한 로직을 구현할 때가 많은데, `Servlet`에서 제공하는 `Cookie` 클래스는 `Builder Pattern`을 제공하지 않아서 불편함이 있다. 일반적으로 쿠키를 생성할 때는 다음과 같은 코드를 사용해서 구현할 수 있는데,

```java
Cookie cookie = new Cookie("name", "value");
cookie.setDomain("domain.com");
cookie.setPath("/path");
cookie.setMaxAge(3600);
cookie.setHttpOnly(true);
cookie.setSecure(true);
```

이를 아래처럼 `Builder Pattern`을 사용해서 구현하고 싶다는 생각이 들었다.

```java
Cookie cookie = new CookieBuilder("name", "value")
        .domain("domain.com")
        .path("/path")
        .maxAge(3600)
        .httpOnly(true)
        .secure(true)
        .build();
```

분명 좋은 `Builder Pattern`을 제공하는 라이브러리가 있겠지만, 공부도 할겸 한 번 `CookieBuilder`를 구현해보기로 했다.

## CookieBuilder Class

기본적으로 `CookieBuilder` 클래스는 `Cookie` 클래스와 동일한 필드를 가진다. 필수값은 `final`로 설정하고 생성자를 호출할 때 무조건 값이 할당될 수 있도록 했다.

```java
public class CookieBuilder {
    private final String name;
    private final String value;
    private String domain;
    private String path;
    private int maxAge = -1;
    private boolean secure;
    private boolean httpOnly;

    public CookieBuilder(String name, String value) {
        this.name = name;
        this.value = value;
    }

    /* method */
}
```

각각의 메소드의 반환 객체는 `CookieBuilder` 클래스가 되어야 한다. 그래야만 `CookieBuilder` 클래스가 가지고 있는 메소드를 연달아 호출할 수 있다.

```java
public CookieBuilder domain(String domain) {
    this.domain = domain;
    return this;
}

public CookieBuilder path(String path) {
    this.path = path;
    return this;
}

public CookieBuilder secure(boolean secure) {
    this.secure = secure;
    return this;
}

public CookieBuilder httpOnly(boolean httpOnly) {
    this.httpOnly = httpOnly;
    return this;
}

public CookieBuilder maxAge(int maxAge) {
    this.maxAge = maxAge;
    return this;
}
```

마지막으로 `build()` 메소드의 반환 객체는 우리가 원하는 `Cookie` 객체가 되어야 한다. 쿠키를 동적으로 생성하고 이전에 설정했던 값들을 할당한 다음 반환해주면 된다.

```java
public Cookie build() {
    Cookie cookie = new Cookie(name, value);
    cookie.setDomain(domain);
    cookie.setPath(path);
    cookie.setMaxAge(maxAge);
    cookie.setSecure(secure);
    cookie.setHttpOnly(httpOnly);
    return cookie;
}
```

완성된 `CookieBuilder` 클래스는 다음과 같다.

```java
public class CookieBuilder {
    private final String name;
    private final String value;
    private String domain;
    private String path;
    private int maxAge = -1;
    private boolean secure;
    private boolean httpOnly;

    public CookieBuilder(String name, String value) {
        this.name = name;
        this.value = value;
    }

    public CookieBuilder domain(String domain) {
        this.domain = domain;
        return this;
    }

    public CookieBuilder path(String path) {
        this.path = path;
        return this;
    }

    public CookieBuilder secure(boolean secure) {
        this.secure = secure;
        return this;
    }

    public CookieBuilder httpOnly(boolean httpOnly) {
        this.httpOnly = httpOnly;
        return this;
    }

    public CookieBuilder maxAge(int maxAge) {
        this.maxAge = maxAge;
        return this;
    }

    public Cookie build() {
        Cookie cookie = new Cookie(name, value);
        cookie.setDomain(domain);
        cookie.setPath(path);
        cookie.setMaxAge(maxAge);
        cookie.setSecure(secure);
        cookie.setHttpOnly(httpOnly);
        return cookie;
    }
}
```

`CookieBuilder` 클래스가 정상적으로 동작하는지 다시 한 번 사용해보자.

```java
Cookie cookie = new CookieBuilder("name", "value")
        .domain("domain.com")
        .path("/path")
        .maxAge(3600)
        .httpOnly(true)
        .secure(true)
        .build();
```

## References

- [Design Patterns: The Builder Pattern](https://dzone.com/articles/design-patterns-the-builder-pattern)
- [Implementing the builder pattern in Java 8 - Tutorial](https://www.vogella.com/tutorials/DesignPatternBuilder/article.html)
