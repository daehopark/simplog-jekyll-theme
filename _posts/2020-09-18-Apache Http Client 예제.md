---
layout: post
title: "Apache Http Client 예제"
tags: ["Java", "Web"]
comments: true
---

`Apache`에서 제공하는 `HttpClient` 객체를 통해 `HTTP` 통신하는 예제를 구현합니다. 요청 데이터와 응답 데이터 모두 `JSON` 형태이며 `POST` 방식을 살펴보겠습니다.

가장 먼저 `HttpPost` 객체를 생성합니다. `HTTP Method`에 따라 `HttpGet`, `HttpPut`, `HttpDelete` 등의 객체로 바꿔주시면 됩니다.

```java
HttpPost httpPost = new HttpPost();
```

추가해야 하는 헤더가 있다면 `addHeader()`를 통해 추가할 수 있습니다.

```java
httpPost.addHeader("Accept", "application/json");
httpPost.addHeader("Content-Type", "application/json");
```

`URI`는 `URIBuilder` 객체를 통해 생성할 수 있습니다.

```java
URI uri = new URIBuilder("http://localhost:3000/todos")
    .addParameter("param1", "value1")
    .addParameter("param2", "value2")
    .build();

httpPost.setURI(uri);
```

다음으로는 `HTTP` 통신에 필요한 환경 설정을 하는데 저는 `Connection Timeout`을 설정하기 위해 주로 사용합니다.

```java
RequestConfig requestConfig = RequestConfig.custom()
    .setConnectionRequestTimeout(10 * 1000)
    .setConnectTimeout(10 * 1000)
    .setSocketTimeout(10 * 1000)
    .build();

httpPost.setConfig(requestConfig);
```

마지막으로 다른 서버에 전달한 `Body` 데이터를 추가합니다.

```java
String body = new StringBuilder()
    .append("{")
    .append("\"title\": \"Java\",")
    .append("\"done\": false")
    .append("}")
    .toString();

httpPost.setEntity(new StringEntity(body));
```

이제 `HttpPost` 객체가 완전해졌으니 `HttpClient` 객체를 생성하고 요청을 처리합니다.

```java
HttpClient httpClient = HttpClientBuilder.create().build();
HttpResponse httpResponse = httpClient.execute(httpPost);
```

`Apache`에서 제공하는 `EntityUtils` 객체를 통해 응답 `Entity`를 문자열로 치환할 수 있습니다.

```java
String resultJson = EntityUtils.toString(httpResponse.getEntity());
System.out.println(resultJson);
```

실행하면 다음과 비슷한 결과를 얻을 수 있습니다.

```
{
  "title": "Java",
  "done": false,
  "id": 4
}
```

## Full Source Code

```java
HttpPost httpPost = new HttpPost();

httpPost.addHeader("Accept", "application/json");
httpPost.addHeader("Content-Type", "application/json");

URI uri = new URIBuilder("http://localhost:3000/todos")
    .addParameter("param1", "value1")
    .addParameter("param2", "value2")
    .build();

httpPost.setURI(uri);

RequestConfig requestConfig = RequestConfig.custom()
    .setConnectionRequestTimeout(10 * 1000)
    .setConnectTimeout(10 * 1000)
    .setSocketTimeout(10 * 1000)
    .build();

httpPost.setConfig(requestConfig);

String body = new StringBuilder()
    .append("{")
    .append("\"title\": \"Java\",")
    .append("\"done\": false")
    .append("}")
    .toString();

httpPost.setEntity(new StringEntity(body));

HttpClient httpClient = HttpClientBuilder.create().build();
HttpResponse httpResponse = httpClient.execute(httpPost);

String resultJson = EntityUtils.toString(httpResponse.getEntity());
System.out.println(resultJson);
```
