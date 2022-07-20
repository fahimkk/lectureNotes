## To run on server

---

Change the directory into the newly created jekyll folder and run the commands in terminal.

for first time use _bundle exec_,

```
bundle exec jekyll serve
```

after run this the first time then you can just type out
_`jekyll serve`_.

it's gonna start a website on our localhost with some default contents.

If we changes the content inside the \__config.yml_ file or the _date_ (date changes the url), then we have to restart the server.

## Folder Structure

---

### **\_post** folder

Its basically the folder where you store all of your blog posts. By default jekyll has given us like this

> 2020-12-30-welcome-to-jekyll.markdown

### **\_site** folder

This is basically like the final output of your website so it continuously get updated, and it used to host the website.

### **\_config.yml** file

This is just like the settings for your jekyll website.
Inside it there are different attributes like title, description, email, baseurl, etc., that we can store about our sites.
So in here we can configure different settings and variables in order to affect the way that our website gonna run.

### **Gemfile** file

This is used with Ruby to store all of the dependencies for our website.

We can add `Themes` or `Plugins` by adding the version inside this Gemfile.

---

### **Front Matter**

Front Matter is basically just information that we store about the _pages_ of our site. All the pages in our jekyll website have front matter, which is at the top of the file.
It contains the information like titles, date, or auther etc.,

Front Matter can be written in two languages either in **YAML** or **JSON**

```
---
layout: post
title: "Welcome To Jekyll!"
date: 2020-12-30 21:02:48 +0530
categories: jekyll update
---
```

By default title is the file name and date is the date on which the file is created. We can redefine or edit these variables by providing custom values.
The jekyll theme that we are using is grabbing the information from the front matter and using it to display our blog posts on our website. So by modifying the front matter in these pages we can change the way that they show up on the website.

Front matter also plays a part in the actual URL of the page. If you click on the blog the url shows is

> localhost:port/jekyll/category/filename_with_date

`localhost:4000/jekyll/update/2020/12/30/welcome-to-jekyll.html`

we can change the term update by changing the _categories_ section.

We can define custom front matter variables, and we can also access these variables in the jekyll layouts.

```
author: "Name"
```

---

### **Default Front Matter**

Default Front Matter is basically the Front Matter that we can define in our `_config.yml` file, that front matter will apply to either all of our pages or some of our pages by default.

```
defaults:
  -
    Scope:
      path:""
      # path is tell us which files the default front matter woult be applied to.
      # emplty string means all of the files.
      type: "posts"
      # if we don't give layout to a page, it uses the default, to avoid that we can mention the type
    values:
      # values we want to store.
      layout: "post"
      title: "Default title"
```

Whenever you modified the \_config.yml file, you should restart the jekyll server

---

### **Writing Posts**

We can create the blog posts directly inside the \_posts folder or the sub-folder inside the \_posts folder to organise them. By default when we created a jekyll file there will be a sample blog post.

`2020-12-30-welcome-to-jekyll.markdown`

> Naming convention: The front of the name is the date that we want to show in our blog post. After that give the title of the post. And delinate the words by hyphen.

For posts we can use both **Markdown** and **HTML** format.

```
---
add Front Matter
---

Some Contents

```

---

### **Working with Draft**

When we write a post inside the \_post folder that will shows in the website. Here the usage of `_drafts` folder comes.
\_drafts folder is used to save the post that are'nt mean to be shows in the website.
And when you ready to publish them just move into `_posts` folder.

There is no need to follow the naming convention to save a file in \_draft folder.

To show the files inside the \_draft folder in the web page run the serer with `--draft` flag

```
jekyll serve --draft
```

When you served up a draft it will shows the default date (current date) in both the URL and below the title.

---

### **Creating Pages**

When you are creating a jekyll blog, you gonna have two type of webpages in your website.

Blog post: Which are more commenly refered to as posts, and also have general pages like About, Contact, etc. Basically these pages in your website are'nt blogs.

Pages will create inside the root directory of our project just like markdown file or html file.

```
---
Front matter
---

Contents

```

To visit the page type the page name after the post name in the url

> localhost:port_number/page_name

We can also create pages inside a sub-folder inside the root directory.

> localhost:port_number/sub-folder/page_name

Unlike the blog post, pages don't have default title or date. So to add title or date, add inside the front matter.

---

### **Permalinks**

Permalink is basically a permanent link or permanent URL that can be assigned to all the pages and posts in the site.

By default the url of a blog post (post inside the \_posts folder) is

> localhost:port/categories/year/month/day/filename

It means that the file is stored in that whole folder structure inside the `_site` folder.

When you change the categories or date in the Front matter section the URL will change.
We need to restart the server to see the changes. Here comes the importants of `permalinks`.
permalinks will add inside the front matter.

```
layout: post
date: 2020-12-30 21:02:48 +0530
categories: jekyll

permalink: "/new-url"
```

Now the URL looks like

> localhost:port/new-url

We can access certain variables inside this permalinks.

```
permalinks: /:categories
```

The `collen (:)` signifies that we want use a variable.
Now the URL using the category variable, here 'jekyll' is the category variable. Spaces between categories will convert into `/` in URL.

> localhost:port/jekyll

Lets see an example with variables and custom extension for title

```
title: "blog"
layout: post
date: 2020-12-30 21:02:48 +0530
categories: jekyll new-cat

permalink: "/:categories/:year/:month/:title.html"
```

This will gives the URL as

> localhost:port/jekyll/new-cat/2020/12/blog.html

---

### **Themes**

By default the theme is `minima`. We can check this in `_config.yml` file under theme section.

To find the theme, visit [rubygems.org]() and search for `jekyll-theme`.
Once you select a theme and you want to know more about it select `Homepage` on left side bar (this will leads into a github repo), to see a preview of the page, select preview in th readme file.

Usage:

1. Under the `Usage` section the readme section copy the theme name,

```
theme: theme_name
```

2. Open up the `Gemfile` in a text editor, (this file allows you to specify the dependencies) and paste the theme name

```
gem "theme_name"
```

3. Open the `terminal`, close the server and run

```
bundle install
```

this will install the all of the gems inside the Gemfile.

3. Open `_config.yml`, update the theme variable

```
theme: theme_name
```

4. restart the jekyll server

```
bundle exec jekyll server
```

> This may show some errors, because the theme may not have the layout (post, page, home) that we used. These layout are from the default minima theme.

> To know the availble layout, under the github page of the theme, select `_layouts` file.
> Replace the new layouts with our old layouts in our posts and pages.

---

### **Layouts**

Layouts are basically just skeltons of the html code that you can used to define the looks and the feel of the different types of pages on your sites or just the entire site in general.

### Create a new layout

1. create a folder `_layouts` under the project folder.
2. create a html file inside this folder. If you use the same name(name of the layout form the theme, for minima posts, page) for the html file then this will overwrite the default.
3. Once you have define layout in html to grab the content inside the html use {{ content }} variable, this will serve a placeholder for all of the content in the current blog post.

see an example
```
<h1>This is a post</h1>
<hr>
{{ content }}
``` 

Different levles of layout:
we can insert another layout inside of a layout.
1. create a layout, let's say `wrapper.html`
2. create another layout, and add Front matter `layout: "wrapper"` inside the new layout.

wrapper.html
```
<html>
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
  <body>
    Wrapper <br>
    {{ content }}
    </br> Wrapper
  </body>
</html>
```
We can use this wrapper layout inside another layout
```
---
layout: "Wrapper"
---

<h1>This is a post</h1>
<hr>
{{ content }}

```
We can use this layout (latter) for our blog posts.

---
### **Varaibles**
We can define variable in the front matter section and access the down part, `{{ layout.variable }}` - when defining a layout.

`{{ page.title }}` will access the title of the current page.

`{{ site.title }}` to access the site - it will grap the title inside the `_config.yml` file.

[more](https://jekyllrb.com/docs/variables/ "www.jekyllrb.com/docs/variables/")

---
### **Includes**
Includes allows you to take certain components of your website like a header or footer or a navigation list and abstract them out into their own html files. So that you have an another html file that defines all the codes for the header of your website and then you can include all these codes inside of the layout
of the jekyll website.

### Create Includes
1. create a new folder `_includes` inside the root folder.
2. Inside this folder we can create all of our includes.

#### Example
create a new html file header.html inside the _includes folder 
```
<h1>{{ site.title }}</h1>
<hr><br>
```
Add this header.html insidet the highest level layout (here wrapper.html).
```
<body>
  {% include filename.html %}
  # here filename is header
</body>
```
We can also pass the information, like color etc,
put the below code inside the highest level layout (wrapper.html)
```
<body>
{% include header.html color="blue" %}
{{ content }}
</body>
```
to access this color variable inside the include
```
<h1 style="color: {{ include.color }}">{{ site.title }}</h1>
<hr><br>
```
### **for Loop**
Loop through some of the pages of the site to buid the navigation list or to show the users what contents you have etc.

This example show how to loop through all of the post and display them on the home page

create a file home.html inside the _layouts
```
{% for post in site.posts %}
  <li><a href="{{ post.url }}>{{ post.title }}</a></li>
{% endfor %}
    
# instead of looping through posts, we can also loop through pages.
```
---
### **Conditional**
```
---
layout: "wrapper"
---

{% if page.title == "name" %}
  Do
{% else if page.title = "anther name" %}
  Do
{% else %}
  Do
{% endif %}

<h2>{{ page.title }}</h2>
<hr>
{{ content }}

```
```
{% for post in site.posts %}

 <li><a style="{% if page.url == post.url %}color:red;{% endif %}" href="{{ post.url }}">{{ post.title }}</a></li>

{% endfor %}
```
---
### **Data Files**
Data files are used to store all sorts of information in the website. 
You can create JSON, YAML, or CSV data files and store a bunch of information inside it and access that information from jekyll layouts and display it on website.

> Its very useful when you want to store large amount and you dont want to put the data inside of blog front matter or layout front matter.

### Create Data files
1. create a folder `_data` inside the root folder.

#### Example 
create a file people.yml inside the _data folder
```
-name: "XXX"
occupation: "Programmer"

-name: "YYY"
occupation: "coder"
```
To access the data from the people.yml file inside a layout called home.html
```
{{ site.data.datafile_name }}
# here datafile_name is people
```
We can also loop through the data file
```
{% for person in site.data.datafile_name %}
  {{ person.name }}, {{ person.occupation }} <br>
{% endfor %}
```
---
### **Static Files**
Static files are basically any files on our website that aren't getting rendered or used by jekyll.

> Static files don't have front matters. eg: javascript, css, pdf, image, etc

You can store static files in a folder called assets. But it's not compulsory, it doesn't care about the location.

To use a static file in a layout
```
{% for file in site.static_files %}
  {{ file.path }}<br>
{% endfor%}

# file.name
# file.basename
# file.extname
```
You can give a front matter to the static files.For example if we create a folder called img inside the assent folder, and want to tell the jekyll that all the files inside the img folder are images. To do so open _config.yml
```
defaults:
  -scope:
    path: "assets/imag"
   values:
    image: true
```
Whenever you modified _config.yml file, restart the jekyll server.

Over the layout html file we can use that front matter
```
{% for file in site.static_files %}

  {% if file.image %}
    <img src="{{ file.path }}" alt="{file.name}">
  {% endif %}

{% endfor%}

```
---
### **Hosting on Github Pages**
Github pages is a service that github offers, and it basically allows you to serve and host a static website completly free.
1. Create a new repository - make sure that you dont initialize with a readme.
2. Modify a variable inside the `_config.yml` file
```
baseurl: "name of the repository"
# if you have a custom domain name put that.
# it acts as the base url for our website.
```
3. Open up terminal

to initialize the git inside the root directory.
```
git init
```
checkout to the gh-pages branch. 
When you create a Github pages website, all the files for that website get stored on the gh-pages branch 
```
git checkout -b gh-pages
```
Add all of the files to commit
```
git add .
```
Commit all of the files to our repository
```
git commit -m "initial commit"
```
Link our local git repository with the Github repository.
```
git remote add origin repository_link
```
Push all the files into Github repository
```
git push origin gh-pages
```
4. Open Github - settings - Github pages section you will get a link


