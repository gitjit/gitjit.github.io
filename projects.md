--- 
    layout: project
    title: 
--- 
  <ul class="list-group">
      {% for member in site.data.members %}
      <li>
         <span class="list-group-item">
            <h3> Project 1 </h3>
            <a  href="https://github.com/{{ member.github }}">
               {{ member.name }}
            </a>
         <span>
      </li>
      {% endfor %}
   </ul>

   <!-- <ul class="list-group">
  {% for post in site.tags[tag] %}
     <a class="list-group-item"  href="{{site.baseurl}}{{post.url}}" rel="bookmark" title="Permanent Link to {{site.baseurl}}{{post.url}}">
            {{post.title}} &nbsp;&nbsp;| &nbsp; &nbsp; <small>{{post.date | date: "%b %d, %Y" }}</small> </a>
  {% endfor %} -->

