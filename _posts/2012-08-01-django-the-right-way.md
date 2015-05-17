---
layout: post

type: "article"

title: "Django the right way."
subtitle: "How to start a correct django project."
cover_image: posts/django.png
cover_image_caption: "Django"

excerpt: "One of the things I wish I had known when I begun developing in Django, is how to start a django the right way. I am not saying that the official documentation lacks pedagogic approach, but mostly it doesn’t reflect the real life; a Django project from developing phase to deploying phase."

author:
  name: Mourad Mourafiq.
  twitter: mmourafiq.
  bio: Maths, Technology, Philosophy, Startups, ...
  image: logo.png
---

One of the things I wish I had known when I begun developing in Django, is how to do it the right way. I am not saying that the official documentation lacks pedagogic approach, but mostly it doesn’t reflect the real life; a Django project from developing phase to deploying phase.

*N.B.* Some prior experience with Django is highly appreciated, but not strictly mandatory.

You need to have [virtualenv](https://pypi.python.org/pypi/virtualenv) to set your your python development environment, and [git](http://www.git-scm.com/) for version control.

Let’s start by he first thing first, installing Django. But, not yet, installing it directly in your site-package area is not a good idea, especially if you consider working on different projects that need different versions of Django or Django apps. Which means you’ll run into dependencies issues. So, what you will need to do is set a virtual environment to manage your packages installation, one of the practices of good python development.

Create a virtual environment for your Django development, and make it active:

{% highlight console %}
> virtualenv env
> source ./env/bin/activate
{% endhighlight %}

And to stop using this current virtualenv:

{% highlight console %}
>  deactivate
{% endhighlight %}

To install django:

{% highlight console %}
>  pip install django
{% endhighlight %}

To install some interesting apps, they will make you life easier, check this post [What makes django amazing](/2012/07/15/best-of-django.html).

Now, you can start your project:

{% highlight console %}
>  django-admin.py startproject myproject
{% endhighlight %}

Before you go any further, you need to track your Django project using git.

{% highlight console %}
>  cd myproject
>  git init
{% endhighlight %}

Also, you should know that you don’t have to track all files in your project, for this:

{% highlight console %}
>  $ vi .gitignore

*.pyc
*~
local_settings.py
wsgi.py
{% endhighlight %}

This tells git not to track all files finishing with .pyc, any temp file, you will see why we don’t track local_settings.py in the very end fo the post.

{% highlight console %}
>  git add .
>  git commit -m ’initial commit of myproject’
{% endhighlight %}

If you are interested by using a service like Github or Bitbucket, or otherwise using a personal server, it is time to make your first push.

{% highlight console %}
>  git psuh origin master
{% endhighlight %}

Now it’s time to manage database, if you didn’t followed the steps from the [What makes django amazing!](/2012/07/15/best-of-django.html), you should install South:

{% highlight console %}
>  pip install south
{% endhighlight %}

Add south to the list of your INSTALLED_APP in your “settings.py” file, and execute:

{% highlight console %}
>  python manage.py syncdb
{% endhighlight %}

You can commit to git this changes, in case something goes wrong you can go back;

{% highlight console %}
>  git add .
>  git commit -m ’added south to the list of installed apps’
{% endhighlight %}

To start a new app;

{% highlight console %}
>  python manage.py startapp myapp
{% endhighlight %}

add it to your INSTALLED_APP in your ’settings.py’ file.

To tell south that you want to manage this app;

{% highlight console %}
>  python manage.py schemamigration myapp –initial
{% endhighlight %}

Which creates a migration file that will allow you to make changes or revert them, in case of any changes in your model;

{% highlight console %}
>  python manage.py migrate myapp
{% endhighlight %}

To tell south that it should manage your model automatically;

{% highlight console %}
>  python manage.py schemamigration myapp –auto
{% endhighlight %}

Sometimes, you will need to work on different issues or anomalies in your project, the best way to track all of them is by creating branches, git is very tolerant and you can create as much branches as issues that you have;

{% highlight console %}
>  pgit checkout -b <brachnme>
{% endhighlight %}

Finally, comes the deployment, if you don’t have already [fabric](http://docs.fabfile.org/en/1.4.3/index.html), then you need to install it:

{% highlight console %}
>  pip install fabric
{% endhighlight %}

Fabric expects a fabfile.py in you myproject directory to define for it all actions that it needs to execute, add this to your fabfile.py:

{% highlight python linenos %}
from fabric.api import local
from fabric.api import lcd

def prepare_deployment(branch_name):
    local('python manage.py test myapp')
    local('git add -p && git commit')
    local('git checkout master && git merge ' + branchname)


def deploy():
    with lcd('/path/to/my/prod/area/'):
        local('git pull /my/path/to/dev/area/')
        local('python manage.py migrate myapp')
        local('python manage.py test myapp')
        local('/my/command/to/restart/webserver')
{% endhighlight %}

This will pull your changes from the development master branch, run any migrations you’ve made, run your tests, and restart your webserver. All in one simple command from the command line. If one of those steps fails, the script stops and reports what happened. Once you fix the issue, there is no need to run the steps manually. Simply rerun the deploy command and all will be well.

So now that we have our fabfile.py created, how do we actually deploy? Simple. Just run:

{% highlight console %}
>  fab prepare_deployment
>  fab deploy
{% endhighlight %}

Finally, you could separate your development settings from your production settings, especially passwords, apis keys and ids and so on. You only need to take all your confidential elements and put them in a file, ‘local_settings.py’ (this file should be different depending on the environment) and import everything from this file in your settings.py file.

Et voila, you have a working Django project ready to serve your project.
