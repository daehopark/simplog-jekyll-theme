---
layout: post
title: "CDN(Contents Delivery Network) 활용법"
tags: ["Network", "Web"]
comments: true
---

안녕하세요, 오늘은 `CDN(Content Delivery Network)`에 대해 얘기해보려고 합니다. 회사에서 인프라 관련 업무를 맡다보니 자연스럽게 `CDN`을 접할 수 있게 되었는데,
`CDN`이 무엇인지 그리고 웹 애플리케이션을 운영함에 있어 `CDN`을 어떻게 활용할 수 있는지 알아보겠습니다.

## CDN(Content Delivery Network)

> A content delivery network, or content distribution network (CDN), is a geographically distributed network of proxy servers and their data centers. The goal is to provide high availability and performance by distributing the service spatially relative to end users.

위키피디아 정의에 따르면 `CDN`은 지리적으로 분산된 프록시 서버의 네트워크이며 이를 활용하여 최종 사용자에게 고가용성과 성능을 제공할 수 있다고 되어있습니다.

아래 그림은 `CDN`이 없는 경우(왼쪽 그림)와 `CDN`이 있는 경우(오른쪽 그림)를 나타내고 있습니다.

![](\assets\images\20201110\NCDN_CDN.png)

왼쪽 그림에서 하얀색 서버는 `Origin Server`를 나타내며 모든 최종 사용자는 `Origin Server`와 통신하여 컨텐츠를 응답받을 수 있습니다. 때문에 `Origin Server`와의 거리가 먼 최종 사용자는 늦은 응답을 받을 수도 있습니다.

오른쪽 그림에서 주황색 서버는 `Edge Server`를 나타내며 지리적으로 넓게 분포되어 있습니다. 최종 사용자는 `Origin Server`와 직접 통신하지 않고 지리적으로 가장 가까운 `Edge Server`와 통신하여 컨텐츠를 응답받을 수 있습니다.

## CDN 활용법

`CDN`을 통해 많은 기능을 사용할 수 있지만 저는 크게 3가지 기능을 사용하고 있습니다.

- Path Forwarding
- Static Caching
- Dynamic Caching

### Path Forwarding

웹 애플리케이션이 둘 이상의 시스템으로 구성되어 있는 경우 `URI` 경로에 따라 다른 `Origin Server`를 바라보도록 설정할 수 있습니다.

```
www.service.com/a -> service-a-origin.com
www.service.com/b -> service-b-origin.com
```

### Static Caching

`Static Caching`은 주로 정적 파일(`html`, `css`, `js`)들을 대상으로 사용합니다. `Edge Server`는 `Origin Server`로부터 미리 정적 파일을 요청하여 가지고 있다가 사용자가 요청하면 즉시 응답하여 `Origin Server`의 부하를 줄여줍니다.

`Static Caching`을 사용할 때 특정 파일을 지정할 수도 있지만 주로 확장자(`.html`, `css`, `.js`)를 지정하여 많이 사용합니다. 또한 주기를 설정할 수도 있고 주기가 끝나지 않더라도 `Purge API`를 통해 언제든 파일을 최신본으로 유지할 수 있습니다.

### Dynamic Caching

`Dynamic Caching`은 주로 동적으로 변하는 컨텐츠를 대상으로 사용합니다. 최종 사용자가 최초로 컨텐츠를 요청하면 `Edge Server`는 사용자를 대신하여 `Origin Server`로부터 컨텐츠를 요청하게 되는데, `Origin Server`로부터 응답받은 컨텐츠는 설정된 주기 동안 `Edge Server`에서 캐싱하고 있기 때문에 다음 사용자부터는 자체적으로 컨텐츠를 응답할 수 있게 됩니다.

`Dynamic Caching` 또한 `Static Caching`처럼 특정 경로나 패턴을 지정할 수 있고 주기도 조절할 수 있습니다. 다만 일반적으로 동적으로 변하는 컨텐츠의 경우 자주 업데이트가 발생하는 영역이기 때문에 `Static Caching` 대상보다는 주기를 짧게 가져가는 편입니다.

## References

- [Content delivery network](https://en.wikipedia.org/wiki/Content_delivery_network)
- [CDN이란 무엇인가요?](https://cdn.hosting.kr/cdn%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80%EC%9A%94/)
