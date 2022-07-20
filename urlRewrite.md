# How to Remove the .html extension from the URL

This blog post is about creating a URL rewrite for a website,
which means that if you were to go to `example.net/gallery.html` then instead it should say `example.net/gallery`.
The `.html` extension can be easily removed by editing `.htaccess` file inside our server.

> This is not supported officially in nginx. If you need this kind of functionality you will need to use Apache or some other http server which supports it.

**.htaccess** file is created with a text editor like notepad.
It is a simple ASCII file that lets the server know what configuration changes are to be made on a pre-directory basis.

Delete the .html tag from the file name inside the link path in the index.html (or other html files).

For example inside your document if you have

```html
<a href="gallery.html" class="">Gallery</a>
```

it became

```html
<a href="gallery" class="">Gallery</a>
```

Create a `.htaccess` file inside the your root folder inside the online version (server) of the website.

We can use `hash(#)` to comment in .htaccess file.
And it's very important that you write the letters as same as below, otherwise it's not gonna work.

The first thing we are going to do inside the .htaccess page is to turn on this rewrite function.

```htaccess
RewriteEngine on
```

The next thing is to write a bunch of **conditions** and **rule** that is going to take care of this URL rewrite.

The **rule** is the thing that actually changes what we see inside the website even though we go to internet for `example.net/gallery` without the extension `example.net/gallery.html` that still shows the content form the *gallery.html* page.

The **condition** is something that has to be true in order for the rule to actually run.
This is very important to have otherwise you might end up getting errors when we try to do this inside our website.
So you first create a bunch of conditions that has to be true and then we can create the rule.

Now the first condition inside the .htaccess file is to make sure that, if you have a folder with the same name as the document(html page) we are trying to access then it's not going to make an error.
Because if you have a folder inside your website like *gallery* in this case, then it's going to give us an error because you are trying to access the *gallery* folder when you just write *gallery* inside the URL like `example.net/gallery`.

```htaccess
RewriteCond %{REQUEST_FILENAME} !-d
```

here `-d` stands for directory and `exclamation(!)` is for negation.

So if this true then continue with a new condition.
Now the second condition is to check that if the file we are trying to show inside the website doesn't exist then it's not going to run the rule.
That means if you don't have `gallery.html` file inside the server then it's going to ignore this script.

```htaccess
RewriteCond %{REQUEST_FILENAME}\.html -f 
```

here `-f` stands for file name and `backslash(\).html` is to tell it that the file name in this case *gallery* inside our URL has to have a `.html` extension behind it.
That means it needs to add `.html` behind it if that file exists inside our directory or inside our website.
Then its going to continue the rule that is going to run.

Now we are going to create a rule.

```htaccess
RewriteRule ^(.*)$ $1.html [NC,L]
```

Here `^(.*)$` is the regular expression to grab the whole URL, and we used the brackets to group the url and used it with `$1`.
And `NC` is stands for no capital means inside the URL can be uppercase and lowercase so that's doesn't really matter.
And the last `L` means that the condition before this specific rule is only going to applied this specific rule.
That means if you latter add any rule then these conditions are not going to apply for that rule.

If you follow this post from beginning to end, then your code almost look like this

```htaccess
RewriteEngine on

# Check if a directory exists with this filename
RewriteCond %{REQUEST_FILENAME} !-d

# Check for file with .html extension
RewriteCond %{REQUEST_FILENAME}\.html -f

# Add .html extension to the URL
RewriteRule ^(.*)$ $1.html [NC,L]
```
