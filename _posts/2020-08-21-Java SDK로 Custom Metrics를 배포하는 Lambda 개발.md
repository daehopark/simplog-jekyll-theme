---
layout: post
title: "Java SDK로 Custom Metrics를 배포하는 Lambda 개발"
tags: ["AWS", "Lambda", "Java"]
comments: true
---

시스템을 운영하다가 서비스의 연속성을 위해 각 `WAS`와 `DB` 간의 `Active Connection` 개수를 실시간으로 모니터링할 일이 생겼다. 각 `WAS`에서 실시간으로 `Active Connection`의 개수를 가지고 `Lambda`를 호출하면 `Lambda`는 `Custom Metrics`를 배포해서 `CloudWatch`에서 데이터를 실시간으로 모니터링할 수 있지 않을까.

## Lambda 호출

각 `WAS`에서는 [MBeanInfo](https://docs.oracle.com/javase/8/docs/api/javax/management/MBeanInfo.html)을 사용해서 `Active Connection`의 개수를 가지고 `Lambda`를 1초마다 호출한다. 대략적인 코드는 다음과 같다.

```java
@Scheduled(fixedDelay = 1000)
public void sendActiveConn() {
  Map<String, String> payloadMap = new HashMap<>();
  payloadMap.put("serverName", serverName);
  payloadMap.put("activeConn", activeConn);

  ObjectMapper mapper = new ObjectMapper();
  SdkBytes payload = SdkBytes.fromUtf8String(mapper.writeValueAsString(payloadMap));

  InvokeRequest invokeRequest = InvokeRequest.builder()
      .functionName(lambdaName)
      .payload(payload)
      .build();

  AwsBasicCredentials credentials = AwsBasicCredentials.create(accessKey, secretKey);
  LambdaClient lambdaClient = LambdaClient.builder()
      .credentialsProvider(StaticCredentialsProvider.create(credentials))
      .region(Region.AP_NORTHEAST_2)
      .build();

  InvokeResponse invokeResponse = lambdaClient.invoke(invokeRequest);
  String result = invokeResponse.payload().asUtf8String();
  System.out.printf("Result: %s", result);// SUCCESS or FAIL
}
```

## Lambda

데이터를 `Map<String, String>` 형태로 받고 결과값을 `String`으로 반환해야되기 때문에 `RequestHandler<Map<String, String>, String>`를 상속해서 `Handler` 클래스를 구현한다. `Lambda`가 호출을 받으면 `CloudWatchClient` 객체를 사용해서 `Custom Metrics`를 배포할 수 있따.

```java
public class Handler implements RequestHandler<Map<String, String>, String> {

  CloudWatchClient cloudWatchClient = CloudWatchClient.builder()
      .region(Region.AP_NORTHEAST_2)
      .build();

  @Override
  public String handleRequest(Map<String, String> input, Context context) {
    LambdaLogger logger = context.getLogger();
    logger.log("handleRequest() started");

    String serverName = input.get("serverName");
    double activeConn = Double.parseDouble(input.get("activeConn"));

    Dimension dimension = Dimension.builder()
        .name("Server")
        .value(serverName)
        .build();

    MetricDatum metricDatum = MetricDatum.builder()
        .dimensions(dimension)
        .metricName("Active Connection")
        .value(activeConn)
        .timestamp(Instant.now())
        .storageResolution(1)
        .build();

    PutMetricDataRequest putMetricDataRequest = PutMetricDataRequest.builder()
        .namespace("Server/ActiveConnection")
        .metricData(metricDatum)
        .build();

    try {
      cloudWatchClient.putMetricData(putMetricDataRequest);
    } catch (Exception e) {
      logger.log(e.getMessage());
      return "FAIL";
    }

    return "SUCCESS";
  }
}
```

`MetricDatum`은 기본적으로 60초 동안 집계된 데이터의 통계를 보여주게 되는데 1초마다 데이터를 찍고 싶으면 반드시 `Storage Resolution`의 값을 `1`로 설정해야 한다.

## References

- [AWS Lambda function handler in Java](https://docs.aws.amazon.com/lambda/latest/dg/java-handler.html)
- [Publishing Custom Metric Data](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/examples-cloudwatch-publish-custom-metrics.html)
