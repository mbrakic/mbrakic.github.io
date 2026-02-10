## Welcome to my Personal Website

Hello! My name's Milan, I'm a physicist turned AI safety researcher based in
London. 

I'm using this site as a kind of landing page for my work. If you're
interested in what I'm doing please see my work here! 

For blogs and musings, please go here! 

### Quick Links
* [My github](https://github.com/mbrakic?tab=repositories)
* [About Me]({{ '/about/' | relative_url }})
* [Portfolio]({{ '/portfolio/' | relative_url }})

### Recent Posts
<!-- # note that files must follow YYYY-MM-DD-title.md -->
<ul>
  {% for post in site.posts %}
    <li><a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>