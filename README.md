# gitjit.github.io
My user page blog !

commands

$ gem install jekyll 

$ gem install jekyll-paginate


#### Blog design notes

On a high level, I started with a basic blog template specified in this tutorial using bootstrap. 
The home page (index.html) consists of following in order
<http://prideparrot.com/blog/archive/2014/4/blog_template_using_twitter_bootstrap3_part1>
1. Menu
1. Body
2. Footer
3. Sidebar

The template for index.html is in _layouts folder called default.html. Which is based of bootstrap architecture.

Top menu is designed based on bootstrap nav-bar, used nav-bar inverted for darker design. The entire menu is
in a source called "menu.html" which is included in default.html page as shown below.  The menu is part of 
all pages hence can be reused. The main html design is based on this tutorial from "pride parrot"



```html
default.html
<body>
    <!--header-->
        {% include menu.html %}
    <!--end header-->
    <div id="body">
        <div class="container">
            <div class="row">
                <div class="col-md-8">
                    {{content}}
                </div>
            </div>
        </div>
    </div>
<footer>
    {% include footer.html %}
</footer>

```
Body is divided into 8 **col-md-8** and 4 **col-md-4** columns(bootstrap divides screen into 12 columns) .
The actual content comes from index.html for home page. Side bar is defined in a seperate file "sidebar.html"
in _includes folder. The "{{content}}" is replaced from index.html where it loops through all posts , extracts
the tags, date of post and excerpt of posts and creates the body. Pagination is also handled in index.html and 
finally sidebar.html is also injected. Footer.html contains footer content.

In a similar way, posts are based on a template called post.html. I have referenced 'font-awsome' icons
as well as glyph icons all over website.

The main font is defined using google fonts. Some bootstrap css is overridden in blog.css file. 

For SEO, I am using jekyll seo plugin and for search I am using lunr.js. For syntax highlight I am
using prism.js. For comments I am using disqus. Please refer this tutorial ..


<http://prideparrot.com/blog/archive/2014/4/blog_template_using_twitter_bootstrap3_part1>

<https://nikinath.github.io/>

#### Search
https://learn.cloudcannon.com/jekyll/jekyll-search-using-lunr-js/

<img src="/images/lang-cli.png" class="img-responsive">

#### Prism.js languages

<pre class="line-numbers" ><code class="language-csharp">
    stfd int32 Point::Y
    ldc.i4 0xc8
</code></pre>


