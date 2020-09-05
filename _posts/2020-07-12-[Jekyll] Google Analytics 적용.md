---
layout: post
title: "[Jekyll] Google Analytics 적용"
tags: ["Jekyll"]
comments: true
---

[Google Analytics](https://marketingplatform.google.com/about/analytics/?hl=en_US)는 웹 사이트 트래픽을 추적하고 보고하는 웹 분석 서비스입니다. `Google Analytics`는 데이터를 분석하기 위한 다양한 도구를 제공하지만 저는 제 블로그에 얼마나 많은 사람들이 방문하는지, 어떤 포스트가 방문율이 높은지를 보고 싶어 사용하게 되었습니다.

이번 장은 `Jekyll` 프로젝트에 `Google Analytics`를 어떻게 적용할 수 있는지 알아보려고 합니다. 당연히 `Google Analytics`를 사용하기 위해서는 [Google](https://www.google.com/) 계정이 필요합니다. `Google` 계정이 없다면 새로 생성한 뒤 [Google Analytics](https://analytics.google.com/)에 접속하여 `Google Analytics` 계정을 생성합니다. 계정과 더불어 웹 사이트에 대한 정보를 적당히 기입합니다.

모든 과정이 끝나면 `Tracking ID`에 대한 정보와 삽입할 수 있는 `Tracking Code`가 보입니다. 만약 보이지 않는다면 좌측 하단에 있는 `Admin` 버튼을 클릭한 다음 `Property > Tracking Info > Tracking Code` 경로에서 확인할 수 있습니다.

## Google Analytics 적용

`Tracking ID`를 변수처럼 활용하기 위해 `_config.yml` 파일을 열고 다음 줄을 추가합니다.

```yaml
google-analytics: [tracking-id]
```

`_includes` 디렉토리 하위에 `google-analytics.html` 파일을 생성하고 제공받은 `Tracking Code`를 복사합니다. `Tracking Code` 중에서 `Tracking ID`가 할당된 부분은 위에서 선언한 환경 변수로 바인딩해줍니다.

{% raw %}

```html
<!-- Global site tag (gtag.js) - Google Analytics -->
<script
  async
  src="https://www.googletagmanager.com/gtag/js?id={{ site.google-analytics }}"
></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag() {
    dataLayer.push(arguments);
  }
  gtag("js", new Date());

  gtag("config", "{{ site.google-analytics }}");
</script>
```

{% endraw %}

`Google Analytics`는 `Tracking Code`의 삽입 위치를 `<head>` 태그의 가장 윗부분에 넣을 것을 권장하고 있습니다.

{% raw %}

```html
<head>
  {% include google-analytics.html %}
  <!-- more -->
</head>
```

{% endraw %}

이제 `Jekyll` 프로젝트를 실행하고 웹 사이트에 접속하면 `Tracking Code`가 적용된 것을 확인할 수 있습니다. 만약 운영 환경 데이터만 측정하길 원하신다면 아래와 같이 조건문을 사용할 수도 있습니다.

{% raw %}

```html
<head>
  {% if jekyll.environment == 'production' %} {% include google-analytics.html
  %} {% endif %}
  <!-- more -->
</head>
```

{% endraw %}

모든 작업이 완료되었습니다. 이제 `Google Analytics` 페이지에서 ~~전세계~~ 사람들이 제 블로그에 방문하는 것을 확인할 수 있습니다!

## References

- [Google Analytics setup for Jekyll](https://michaelsoolee.com/google-analytics-jekyll/)
- [Jekyll을 사용한 blog에 Google Analytics 적용하기](https://rextarx.github.io/jekyll/2017/02/03/Applying_Google_Analytics_to_a_blog_using_Jekyll/)
