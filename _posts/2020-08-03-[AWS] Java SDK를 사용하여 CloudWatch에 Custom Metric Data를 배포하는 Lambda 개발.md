---
layout: post
title: "[AWS] Java SDK를 사용하여 CloudWatch에 Custom Metric Data를 배포하는 Lambda 개발"
tags: ["aws"]
comments: true
---

`Lambda`를 개발하게 된 배경은 이렇다. 서비스를 운영하는데 `DB`가 자꾸 죽는다. `Auto Scaling`을 걸고 싶은데 어떤 지표를 사용해야하는지 잘 모르겠다. 우선 각 `WAS` 별로 DB에 붙는 `Active Connection`의 개수를 모니터링하기로 했다.

개발자는 1초마다 각 서버의 `Active Connection`과 `Max Active Connection`의 개수를 가지고 `Lambda`를 호출한다. 그러면 나는 `Lambda`를 통해 각 서버의 `Active Connection`과 `Max Active Connection`을 `CloudWatch`에서 모니터링할 수 있도록 `Metrics`를 그린다.

개발자가 보내주는 데이터의 형식은 다음과 같다.

```json
{
    "serverName": "{serverName}",
    "activeConn": "{activeConn}",
    "maxActiveConn": "{maxActiveConn}"
}
```

`Lambda`에서 `Map`으로 데이터를 받기 위해서는 아래와 같이 `RequestHandler`를 구현하면 된다.

```java
public class ActiveConnectionHandler implements RequestHandler<Map<String, String>, String> {

    public ActiveConnectionHandler() {
    }

    @Override
    public String handleRequest(Map<String, String> input, Context context) {
        return "OK";
    }
}
```

`Metrics`를 그리기 위해서는 `CloudWatchClient` 객체가 필요하다. `Region` 객체는 시스템 변수를 통해 할당하는데 시스템 변수는 `Lambda`의 구성 탭에서 관리할 수 있다.

```java
private final CloudWatchClient cloudWatchClient;

public ActiveConnectionHandler() {
    Region region = Region.of(System.getenv("REGION"));
    cloudWatchClient = CloudWatchClient.builder()
            .region(region)
            .build();
}
```

로그를 사용하고 싶다면 `Context` 객체에서 `getLogger()` 메소드를 불러와 남길 수 있다.

```java
@Override
public String handleRequest(Map<String, String> input, Context context) {
    LambdaLogger logger = context.getLogger();
    logger.log("handleRequest() started.");

    return "OK";
}
```

`Metric` 데이터를 찍기 위해서는 `Dimension`이 필요하다. `Dimension`은 고유한 `Row`를 남기기 위해 필요하다. `(Dimension, Metric)`이 하나의 고유한 `Row`가 된다.

```java
Dimension dimension = Dimension.builder()
        .name("Server")
        .value(serverName)
        .build();

MetricDatum activeMetricData = MetricDatum.builder()
        .dimensions(dimension)
        .metricName("Active")
        .timestamp(Instant.now())
        .storageResolution(1)// 고단위 지표 사용 (1초마다)
        .value(Double.parseDouble(activeConn))
        .build();

MetricDatum maxActiveMetricData = MetricDatum.builder()
        .dimensions(dimension)
        .metricName("MaxActive")
        .timestamp(Instant.now())
        .storageResolution(1)// 고단위 지표 사용 (1초마다)
        .value(Double.parseDouble(maxActiveConn))
        .build();
```

1초마다 데이터를 찍고 싶으면 `storageResolution`의 값을 1로 설정해야 한다. 이를 고단위 지표를 사용한다고 하는데 60초 미만으로 데이터를 찍을 경우 최대 3시간 동안 데이터를 보존할 수 있다. 단, 3시간이 지나면 데이터가 삭제되는 것은 아니고 3시간 이후부터는 60초 단위의 평균으로 데이터를 집계할 수 있다.

우리가 원하는 데이터 `Row`를 생성했으니 이제 데이터를 `List`에 담아 메트릭스를 그려보자.

```java
List<MetricDatum> metricData = Arrays.asList(activeMetricData, maxActiveMetricData);

PutMetricDataRequest pmdReq = PutMetricDataRequest.builder()
        .namespace("Server/JdbcActiveConnection")
        .metricData(metricData)
        .build();

PutMetricDataResponse pmdRes = cloudWatchClient.putMetricData(pmdReq);
logger.log(pmdRes.responseMetadata().requestId() + " request is completed");
```

`Namespace`는 메트릭스를 관리하는 디렉토리 정도 되겠다. 제일 좋은 건 한 번 구현해서 `Metric`을 그려보고 `CloudWatch`에서 어떤 식으로 표현되는지 확인하는 것이다.

완성된 코드는 다음과 같다.

```java
public class ActiveConnectionHandler implements RequestHandler<Map<String, String>, String> {

    private final CloudWatchClient cloudWatchClient;

    public ActiveConnectionHandler() {
        Region region = Region.of(System.getenv("REGION"));
        cloudWatchClient = CloudWatchClient.builder()
                .region(region)
                .build();
    }

    @Override
    public String handleRequest(Map<String, String> input, Context context) {
        LambdaLogger logger = context.getLogger();
        logger.log("handleRequest() started.");

        String serverName = input.get("serverName");
        String activeConn = input.get("activeConn");
        String maxActiveConn = input.get("maxActiveConn");

        Dimension dimension = Dimension.builder()
                .name("Server")
                .value(serverName)
                .build();

        MetricDatum activeMetricData = MetricDatum.builder()
                .dimensions(dimension)
                .metricName("Active")
                .timestamp(Instant.now())
                .storageResolution(1)// 고단위 지표 사용 (1초마다)
                .value(Double.parseDouble(activeConn))
                .build();

        MetricDatum maxActiveMetricData = MetricDatum.builder()
                .dimensions(dimension)
                .metricName("MaxActive")
                .timestamp(Instant.now())
                .storageResolution(1)// 고단위 지표 사용 (1초마다)
                .value(Double.parseDouble(maxActiveConn))
                .build();

        List<MetricDatum> metricData = Arrays.asList(activeMetricData, maxActiveMetricData);

        PutMetricDataRequest pmdReq = PutMetricDataRequest.builder()
                .namespace("Server/JdbcActiveConnection")
                .metricData(metricData)
                .build();

        PutMetricDataResponse pmdRes = cloudWatchClient.putMetricData(pmdReq);
        logger.log(pmdRes.responseMetadata().requestId() + " request is completed");

        return "OK";
    }
}
```
