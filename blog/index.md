---
title: "Blog"
---

# 📝 Blog

Writing about cybersecurity, homelab automation, AI workflows, and the path from Air Force to tech.

---

<ul class="post-list">
{% for post in site.posts %}
  <li>
    <a href="{{ post.url }}">{{ post.title }}</a>
    <br><small>{{ post.date | date: "%B %-d, %Y" }}</small>
  </li>
{% endfor %}
</ul>