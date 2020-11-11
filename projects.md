--- 
    layout: project
    title: 
--- 
  <ul class="list-group">
      {% for project in site.data.projects %}
      <li class="list-group-item" style="background-color: transparent;">
         <span>
            <!-- <h3> {{project.name}}</h3> -->
            <i class="fa fa-github" aria-hidden="true"></i>
            <a  target="_blank" href="{{project.github}}">
                 <h3> {{project.name}}</h3>
            </a>
            <p> {{project.description}}</p>
             
             <span style=" color:red" class="glyphicon glyphicon-bookmark"></span> {{project.tag}}
         </span>
      </li>
      {% endfor %}
   </ul>

<!-- project: Expense Tracker
  github: konklone
  tag: Xamarin, C#
  description: This project is to explore the Xamarin capability. -->

   <!-- <ul class="list-group">
  {% for post in site.tags[tag] %}
     <a class="list-group-item"  href="{{site.baseurl}}{{post.url}}" rel="bookmark" title="Permanent Link to {{site.baseurl}}{{post.url}}">
            {{post.title}} &nbsp;&nbsp;| &nbsp; &nbsp; <small>{{post.date | date: "%b %d, %Y" }}</small> </a>
  {% endfor %} -->

