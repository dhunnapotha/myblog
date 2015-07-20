---
layout: post
title: Setting up a custom domain website with Jekyll
---

I recently setup this [website + blog](http://www.theox.in) using Jekyll. Tried to capture the important information in this blog. I added some useful references at the bottom.

*This blog assumes that the reader has basic knowledge of Git repos, HTML & Markdown*

----

1. Install Jekyll -> __gem install jekyll__
2. Following the instructions in jekyll website looked a bit complex to me. So, I would suggest you can skip it for now.
3. As a starting point, go to [Solo](http://chibicode.github.io/solo/). Follow the instructions and play around with the content to get a feel of what Jekyll can do.
4. Next go to [this awesome blog](http://joshualande.com/jekyll-github-pages-poole/) and follow the instructions.

----

### Few things I want to highlight
* Creating custom domain is pretty simple. Just follow these steps
  1. Create a file named CNAME, add the custom domain, you want to map this webpage to (in my case, it was www.theox.in)
  2. Go to your domain service provider (in my case, [godaddy](http://godaddy.com)) and add a CNAME record for __www__ subdomain to __your-username__.github.io (in my case, it was __dhunnapotha.github.io__)

  *Please note that Github has pages for profiles and repositories. The instructions given above are for mapping custom domain to repository web pages. The instructions might slightly vary for profile pages. Please check [this](https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages/) for more details*
* If you want to highlight code snippets with language formatting, use the below syntax (ruby, in the below example)
    {% raw %}
      {% highlight ruby %}
      def foo
        puts 'foo'
      end
      {% endhighlight %}
    {% endraw %}
  _Please find more details in the [code-snippet-highlighting](http://jekyllrb.com/docs/templates/#code-snippet-highlighting) section._
* I added Follow Me buttons using [addthis](https://www.addthis.com/)
* If you are starting with an already built jekyll theme, I would suggest you clone into a new repo instead of working with a forked repo, as commits done to forked repos do not get reflected in your github's account acitivity.


__This blog is built using the approach given above. You can get the source code [here](https://github.com/dhunnapotha/myblog).__

----


### References:
* [http://joshualande.com/jekyll-github-pages-poole/](http://joshualande.com/jekyll-github-pages-poole/)
* [http://chibicode.github.io/solo/](http://chibicode.github.io/solo/)
* [http://jekyllrb.com/](http://jekyllrb.com/)


<a href="https://github.com/dhunnapotha/myblog"><img style="position: fixed; top: 0; left: 0; border: 0;" src="https://camo.githubusercontent.com/c6625ac1f3ee0a12250227cf83ce904423abf351/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6769746875622f726962626f6e732f666f726b6d655f6c6566745f677261795f3664366436642e706e67" alt="Fork me on GitHub" data-canonical-src="https://s3.amazonaws.com/github/ribbons/forkme_left_gray_6d6d6d.png"></a>
