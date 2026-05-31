---
layout: single
title: Blog
permalink: /blog/
author_profile: false
classes: wide
share: false
---

<style>
.blog-list { list-style: none; padding: 0; margin: 2em 0 0; }

.blog-item {
  display: flex;
  gap: 2.6rem;
  align-items: flex-start;
  padding: 2.5rem 0;
  border-bottom: 1px solid #e8e8e8;
  transition: opacity 0.25s;
}
.blog-item:last-child { border-bottom: none; }
.blog-item:hover { opacity: 0.82; }

.blog-img {
  flex-shrink: 0;
  width: 220px;
  height: 155px;
  overflow: hidden;
  background: #f5f5f5;
}
.blog-img img { width: 100%; height: 100%; object-fit: cover; display: block; transition: transform 0.55s ease; }
.blog-item:hover .blog-img img { transform: scale(1.05); }
.blog-img-empty {
  width: 100%; height: 100%; background: #f0f0f0;
  display: flex; align-items: center; justify-content: center;
  font-size: 22px; color: #ccc;
}

.blog-body { flex: 1; min-width: 0; display: flex; flex-direction: column; gap: 0.6rem; }

.blog-cats { display: flex; gap: 8px; flex-wrap: wrap; }
.blog-cat {
  font-size: 0.7rem; font-weight: 600; letter-spacing: 0.18em;
  text-transform: uppercase; color: #111; border: 1px solid #111;
  padding: 2px 10px; text-decoration: none; transition: background 0.2s, color 0.2s;
}
.blog-cat:hover { background: #111; color: #fff; }

.blog-title {
  font-size: clamp(1.05rem, 2vw, 1.35rem);
  font-weight: 500; line-height: 1.4; color: #111;
  text-decoration: none; display: block;
}
.blog-title:hover { color: #444; }

.blog-excerpt {
  font-size: clamp(0.9rem, 1.5vw, 1.05rem);
  font-weight: 300; color: #888; line-height: 1.9;
  display: -webkit-box; -webkit-line-clamp: 3;
  -webkit-box-orient: vertical; overflow: hidden;
}

.blog-meta { font-size: 0.78rem; color: #bbb; display: flex; align-items: center; gap: 0.5rem; }
.blog-meta-sep { color: #ddd; }

.blog-footer {
  display: flex; align-items: center; justify-content: space-between;
  flex-wrap: wrap; gap: 0.5rem;
  margin-top: 0.3rem; padding-top: 0.8rem; border-top: 1px solid #efefef;
}

.blog-tags { display: flex; gap: 8px; flex-wrap: wrap; }
.blog-tag { font-size: 0.7rem; letter-spacing: 0.07em; color: #ccc; text-decoration: none; }
.blog-tag:hover { color: #111; }

.blog-readmore {
  font-size: 0.7rem; font-weight: 600; letter-spacing: 0.2em;
  text-transform: uppercase; color: #111; text-decoration: none;
  border-bottom: 1px solid #111; padding-bottom: 1px;
  white-space: nowrap; transition: opacity 0.2s;
}
.blog-readmore:hover { opacity: 0.4; }

@media (max-width: 640px) {
  .blog-item { flex-direction: column; gap: 1.2rem; }
  .blog-img { width: 100%; height: 200px; }
  .blog-footer { flex-direction: column; align-items: flex-start; }
}
</style>

<ul class="blog-list">
  {% for post in site.posts %}
  <li class="blog-item">
    <div class="blog-img">
      {% if post.header.teaser %}
        <img src="{{ post.header.teaser }}" alt="{{ post.title }}">
      {% else %}
        <div class="blog-img-empty">✦</div>
      {% endif %}
    </div>
    <div class="blog-body">
      {% if post.categories.size > 0 %}
      <div class="blog-cats">
        {% for cat in post.categories limit:2 %}
        <a href="{{ '/categories/#' | prepend: site.baseurl }}{{ cat | slugify }}" class="blog-cat">{{ cat }}</a>
        {% endfor %}
      </div>
      {% endif %}
      <a href="{{ post.url | relative_url }}" class="blog-title">{{ post.title }}</a>
      <div class="blog-excerpt">{{ post.excerpt | strip_html | truncate: 200 }}</div>
      <div class="blog-meta">
        <time>{{ post.date | date: "%d %b %Y" }}</time>
        {% if post.author %}<span class="blog-meta-sep">·</span><span>{{ post.author }}</span>{% endif %}
      </div>
      <div class="blog-footer">
        {% if post.tags.size > 0 %}
        <div class="blog-tags">
          {% for tag in post.tags limit:3 %}
          <a href="{{ '/tags/#' | prepend: site.baseurl }}{{ tag | slugify }}" class="blog-tag"># {{ tag }}</a>
          {% endfor %}
        </div>
        {% endif %}
        <a href="{{ post.url | relative_url }}" class="blog-readmore">Leggi tutto &rarr;</a>
      </div>
    </div>
  </li>
  {% endfor %}
</ul>
