---
layout: post
title: "[Web] CDN(Contents Delivery Network)의 정의와 기능"
tags: ["web"]
comments: true
---

회사에서 아키텍처 관련 업무를 진행하다보니 자연스럽게 `CDN`을 접하게 되었습니다. 저희는 `Origin` 서비스의 부하를 줄이고 사용자의 요청에 빠른 응답을 제공하기 위해 `CDN`을 사용하고 있습니다. 오늘은 `CDN`이 동작하는 방식에 대해 간단히 설명하고 서비스를 운영하면서 사용하는 `CDN` 기능에 대해 정리해보려고 합니다.

## CDN(Contents Delivery Network)

글로벌 서비스를 운영하기 위해서 전 세계에 서버를 구축하면 좋겠지만, 일반적으로 비용 문제가 있기 때문에 `CDN`을 사용하여 서비스를 운영합니다. `CDN` 서비스를 제공하는 회사는 전 세계에 `Edge Server`를 구축하여 전 세계의 사용자의 요청에 빠른 응답을 제공할 수 있도록 합니다.

![CDN Edge Server Example](/assets/images/20200914/cdn_edge_server_example.png)

그렇다면 `CDN`은 어떻게 `Origin Server`에 부하를 주지 않고 응답을 제공할 수 있을까요? 정답은 `Cache`에 있습니다. 서비스 운영자는 정적 자원을 미리 `Edge Server`에 배포하거나 동적 컨텐츠를 일정 주기로 캐싱하여 사용자의 요청에 빠르게 응답합니다.

![How CDN works flow](/assets/images/20200914/How-CDN-works-flow.png)

일반적으로 `CDN`에서 제공하는 `Cache`의 종류는 정적 캐싱과 동적 캐싱이 있습니다.

### 정적 캐싱(Static Caching)

정적 캐싱은 웹 페이지를 제작하는데 필요한 정적 자원(`css`, `js`)들을 미리 `Edge Server`에 배포하여 `Origin` 서비스를 거치지 않고 `Edge Server`에서 바로 응답할 수 있도록 합니다. 일반적으로 정적 캐싱은 주기를 가지고 있으며 일정 주기가 지나면 `Origin` 서비스에서 다시 자원을 당겨옵니다. 만약 캐싱 주기가 끝나기 전에 파일의 변경이 일어난다면 반드시 `Purge`를 사용하여 `Edge Server`가 `Origin` 서비스에서 다시 자원을 가져올 수 있도록 해야합니다.

### 동적 캐싱(Dynamic Caching)

동적 캐싱은 정적 자원과는 다르게 실시간으로 컨텐츠의 내용이 바뀌는 페이지를 캐싱하고 싶을 때 사용합니다. `End User`가 페이지에 최초로 접근할 경우에는 `Origin` 서비스에서 해당 컨텐츠를 가져오고 이후에는 `Edge Server`
에 해당 컨텐츠를 캐싱하게 됩니다. 이후에 같은 `Edge Server`로 요청하는 `End User`에게는 캐싱된 컨텐츠를 응답하게 됩니다.

### Origin Forwarding

부가적으로 `CDN`은 사용자 요청 경로에 따라 각기 다른 `Origin` 서비스로 요청을 분기 처리할 수 있습니다. 예를 들어 독립적으로 개발된 3개의 서비스(`serviceA`, `serviceB`, `serviceC`)가 하나의 도메인을 공유하고 요청 경로에 따라 처리한다고 가정했을 때 아래처럼 요청을 분기 처리할 수 있습니다.

```
www.service.com/serviceA -> serviceA.service.com
www.service.com/serviceB -> serviceB.service.com
www.service.com/serviceC -> serviceC.service.com
```

## References

- [CDN이란 무엇인가요?](https://cdn.hosting.kr/cdn%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80%EC%9A%94/)
