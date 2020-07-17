---
layout: post
title: "[Jekyll] Disqus 설치"
tags: ["jekyll"]
comments: true
---

만약 댓글 기능이 없는 웹 사이트나 블로그를 운영하고 있다면 [Disqus](https://disqus.com/)를 설치하여 간단히 댓글 시스템을 구축할 수 있습니다. 저 역시 `Disqus`를 사용하여 댓글 시스템을 구축했으며 간단한 실습을 통해 그 과정을 공유하려고 합니다.

## Disqus 설치

우선 `Disqus` 계정을 생성해야 합니다. 계정 생성을 완료하고 뜬 화면에서 `I want to install Disqus on my site`가 적힌 버튼을 클릭합니다. `Create a new site` 화면이 뜨면 간단한 정보를 입력하여 사이트를 생성합니다.

사이트 생성이 완료되면 좌측 상단의 `Install Disqus` 메뉴를 클릭한 다음 `Jekyll` 플랫폼을 클릭합니다. `Jekyll` 플랫폼에서 `Disqus`를 설치하는 방법이 나오는데 아래쪽에 `Universal Embed Code` 링크를 클릭합니다.

아래와 비슷한 코드가 보이면 `Jekyll` 프로젝트의 `_includes` 디렉토리 하위에 `disqus.html` 파일을 하나 만들고 복사합니다. 바로 아래에 보이는 제 코드를 복사하려면 `s.src` 할당문의 경로를 자신의 `Disqus` 사이트 경로로 수정해야 합니다. 

```html
<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
    var d = document, s = d.createElement('script');
    s.src = 'https://my-awesome-site.disqus.com/embed.js';
    s.setAttribute('data-timestamp', +new Date());
    (d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
```

코드 중간에 `disqus_config`라는 변수가 주석 처리되어 있는데 주석을 제거하고 아래처럼 코드를 수정합니다.

{% raw %}
```js
var disqus_config = function () {
    this.page.url = `{{ page.url | absolute_url }}`;  // Replace PAGE_URL with your page's canonical URL variable
    this.page.identifier = `{{ page.url | absolute_url }}`; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
```
{% endraw %}

위의 코드가 있던 페이지에서 스크롤을 살짝 내리면 아래와 같은 코드가 보입니다.

```html
<script id="dsq-count-scr" src="//my-awesome-site.disqus.com/count.js" async></script>
```

레이아웃을 정의한 페이지의 `</body>` 태그 바로 위에 추가합니다.

이제 거의 다 왔습니다. 레이아웃을 정의한 페이지의 적당한 위치에 아래와 같이 코드를 작성하여 `disqus.html`을 삽입합니다.

{% raw %}
```html
<!-- disqus -->
{% if page.comments == true %}
{% include disqus.html %}
{% endif %}
<!-- disqus -->
```
{% endraw %}

조건문을 사용한 이유는 댓글 기능을 사용할 포스트나 페이지를 선택할 수 있게 하기 위한 것입니다. 포스트나 페이지를 작성할 때 댓글 기능을 사용하고 싶으면 아래처럼 코드를 작성해주면 됩니다.

```markdown
---
layout: post
comments: true # comments can be disabled by setting false
---
```

## References

- [Install instructions for Jekyll](https://disqus.com/admin/install/platforms/jekyll/)
- [Disqus를 이용하여 Jekyll에 댓글기능 추가하기](https://17billion.github.io/jekyll/disqus/reply/2017/06/01/jekyll_disqus.html)
