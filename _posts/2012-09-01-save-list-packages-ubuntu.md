---
layout: post

type: "article"

title: "save a list of your favourite packages in Ubuntu."
subtitle: "tip to save a list of all packages that you wish to have every time you run a ubuntu system!"
cover_image: posts/ubuntu.png
cover_image_caption: "Ubuntu"

excerpt: "Just reinstalled your ubuntu system, you must be thinking about all your favourite packages, but it’s really hard to start all over again. Well next time use this tip to save a list of all packages that you wish to have every time you run a ubuntu system!"

author:
  name: Mourad Mourafiq.
  twitter: mmourafiq.
  bio: Maths, Technology, Philosophy, Startups, ...
  image: logo.png
---

Just reinstalled your ubuntu system, you must be thinking about all your favourite packages, but it’s really hard to start all over again. Well next time use this tip to save a list of all packages that you wish to have every time you run a ubuntu system!

{% highlight console %}
> apt-cache --installed pkgnames
{% endhighlight %}

Save them in a file:

{% highlight console %}
>  apt-cache --installed pkgnames > installed.packages.lst
{% endhighlight %}

To install all packages in a file:

{% highlight console %}
>  sudo apt-get install `cat installed.packages.lst`
{% endhighlight %}
