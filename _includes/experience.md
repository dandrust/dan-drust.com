{% for experience in include.experiences %}

{% if experience.print == false %}
<div class='noprint' markdown='1'>
{% endif %}

### {{ experience.title }}
#### {{ experience.organization }} ({{ experience.date_string }})
<ul>
{% for detail in experience.description %}
<li>{{ detail }}</li>
{% endfor %}
</ul>

{% if experience.print == false %}
</div>
{% endif %}


{% endfor %}
