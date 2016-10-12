---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: default
---
<main class="page-content" aria-label="Content">
      <div class="wrapper">
        <div class="home">

  <h1 class="page-heading">All Posts</h1>
  
  


  <ul class="post-list">
    {% for post in site.posts %}
      <li>
        <span class="post-meta">Oct 11, 2016</span>

        <h2>
          <a class="post-link" href="{{site.baseurl}}{{post.url}}">{{post.title}}</a>
        </h2>
      </li>
    {% endfor%}
  </ul>

  <p class="rss-subscribe">subscribe <a href="/feed.xml">via RSS</a></p>

</div>

      </div>
    </main>