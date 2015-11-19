---
layout: post
title: Colorizing output strings
comments: true
---

I was curious on how to colorize output (as is done in [colorize](https://github.com/fazibear/colorize) gem). So, here is a brief info of my findings.

Any colorized string in general looks like the below

{% highlight ruby %}
  "\x1B[n1;n2….m<your text goes here>\x1B[0m"
{% endhighlight %}

where 

* \x1B is an escape identifier to mark the beginning of colored string
* n1, n2, n3 are numbered options separated by semicolons. The important numbered options are
  * **Color options**
    * 30-37 foreground colors
    * 40-47 background colors
      * for eg: 
        * 30 is foreground black, 40 is background black
        * 31 is foreground red, 41 is background red and so on..
      * Get the complete list [here](https://en.wikipedia.org/wiki/ANSI_escape_code#Colors)
  * **Other Styling Options**
    * 1 - bold
    * 4 - underline
    * 0 - default attributes
    * Get the entire list in SGR (Select Graphic Rendition) parameters section of the [wiki](https://en.wikipedia.org/wiki/ANSI_escape_code).

So, any text that follows **\x1B[0m** will have default attributes set because of 0.

And to show you an example, 

{% highlight ruby %}
  puts “\x1B[1;32;43;4m bold green on yellow\x1B[0mdefault text”
{% endhighlight %}
results in 
  <img src="{{site.url}}/img/colorize-example.png" class="colorize_img2" />

And just fyi, the same works with 'printf' on bash
