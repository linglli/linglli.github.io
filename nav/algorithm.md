---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: default
---
<main class="page-content" aria-label="Content">
    <div class="home">
        <h2 class="page-heading">Posts about Algorithm</h2>
        <ul class="post-list">
            {% for post in site.posts %}
            {% if post.tag == "algorithm" %}
            <li>
                <h3><a class="post-link" href="{{site.baseurl}}{{post.url}}">{{post.title}}</a></h3>
                <span class="post-meta">{{post.date}}</span>
                <div class="summary"><p>{{ post.excerpt }}<a href="{{site.baseurl}}{{post.url}}">...</a></p></div>
            </li>
            {% endif %}
            {% endfor%}
        </ul>
    </div>
</main>