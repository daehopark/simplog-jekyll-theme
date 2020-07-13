---
layout: post
title: "[Jekyll] Tag Archive 페이지 개발"
tags: ["jekyll"]
comments: true
---

블로그를 운영할 때 포스트를 일정한 기준에 따라 분류하기 위해 태그를 사용합니다. 대부분의 블로그 플랫폼은 태그들만 모아서 따로 보여주거나 태그별로 포스트를 분류한 페이지를 제공합니다. 저 역시 `Jekyll` 블로그를 시작하면서 태그별로 포스트를 분류한 페이지를 별도록 제작하고 싶었고 그 과정을 공유하려고 합니다.

## Tag Archive

사용자가 `/tags`로 접근하면 `Tag Archive` 페이지가 나올 수 있도록 프로젝트의 루트에 `tags.md` 파일을 하나 생성하고 아래처럼 작성합니다.

```markdown
---
layout: tags
title: "Tags"
---
```

이제 `_layouts` 디렉토리 하위에 `tags.html` 페이지를 만들고 아래 실습을 진행하면 됩니다.

`Jekyll` 프로젝트에서는 `site.tags`로 사이트의 태그 정보에 접근할 수 있습니다. `site.tags`는 얼핏 모든 태그를 담고 있는 배열처럼 보이지만 실제 데이터는 그보다 조금 복잡합니다. 우선 사이트의 모든 태그를 출력해보겠습니다.

{% raw %}
```html
{% for tag in site.tags %}
<h2>{{ tag[0] }} </h2>
{% endfor %}
```
{% endraw %}

`site.tags`는 배열이며 그 원소 또한 배열입니다. 원소의 첫 번째 인덱스에는 태그 이름이 저장되어 있습니다. 그렇다면 두 번째 인덱스에는 무엇이 있을까요? 첫 번째 인덱스에 저장된 태그를 가지고 있는 포스트 리스트가 배열로 담겨 있습니다.

이번엔 태그별 포스트 리스트를 출력해보겠습니다.

{% raw %}
```html
{% for tag in site.tags %}
<h2>{{ tag[0] }} </h2>
<ul>
    {% for post in tag[1] %}
    <li>
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
    {% endfor %}
</ul>
{% endfor %}
```
{% endraw %}

이제 거의 다 왔습니다. 한 가지 아쉬운 점은 태그를 `Alphabetical`하게 정렬하고 태그별 포스트 리스트를 출력하고 싶었기 때문입니다. [Liquid](https://jekyllrb.com/docs/liquid/)의 `Filter` 기능을 사용하여 `site.tags`를 `Alphabetical`하게 정렬한 다음 포스트 리스트를 출력하도록 해보겠습니다.

{% raw %}
```html
{% assign tags = site.tags | sort %}
{% for tag in tags %}
<h2>{{ tag[0] }} </h2>
<ul>
    {% for post in tag[1] %}
    <li>
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
    {% endfor %}
</ul>
{% endfor %}
```
{% endraw %}

## References

- [Liquid template language](https://shopify.github.io/liquid/)
