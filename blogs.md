---
layout: default
---

<div class="home">
  <div class="post">
    <header class="post-header">
      <h1 class="post-title">Blogs</h1>
    </header>
    <ul class="post-list">
        {% for post in site.posts %}
          <li>
            <h3>
              <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
            </h3>
            <div class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</div>
          </li>
        {% endfor %}
    </ul>
  </div>
</div>