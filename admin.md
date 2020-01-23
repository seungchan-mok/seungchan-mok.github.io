---
layout: page
---

Run this under **tag** folder in order to generate a page for each of your tags. If you added a new tag you need to come to this page and run the script again.

{% highlight bash %}
# 날짜 삽입하기
echo $NOW >>_posts/2020-01-23-vscode-ros-extension.md
{% for tag in site.tags %}
echo $'---\nlayout: tag_index\ntag: {{ tag[0] }} \n---' > '{{ tag[0] }}.md' &{% endfor %}
{% endhighlight %}
