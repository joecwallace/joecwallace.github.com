---
layout: default
use: ["posts"]
---

Hi. My name is Joe. I write code for a living, and - right now - I live in an RV with my wife and our three kids. Here are some ways to contact and/or keep up with me:

<ol>
    <li><a href="mailto:joe.c.wallace@gmail.com">joe.c.wallace@gmail.com</a></li>
    <li><a href="https://www.twitter.com/joecwallace">www.twitter.com/joecwallace</a></li>
    <li><a href="https://www.github.com/joecwallace">www.github.com/joecwallace</a></li>
    <li><a href="https://www.facebook.com/joecwallace">www.facebook.com/joecwallace</a></li>
</ol>

Occasionally, I feel inclined to memorialize my thoughts on this web site. When I do, you'll find them below, in reverse chronological order:

<ol class="reverse-chron" reversed>
    {% for post in data.posts %}
        <li><a href="{{ site.url }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ol>
