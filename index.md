## Welcome to my Personal Website

Hi! I'm building this site using Jekyll and GitHub Pages. This is the main landing page where I'll share my projects and thoughts.

### Quick Links
* [My Projects](https://github.com/mbrakic?tab=repositories)
* [About Me](#)

### Recent Posts
<!-- # note that files must follow YYYY-MM-DD-title.md -->
<ul>
  {% for post in site.posts %}
    <li><a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>