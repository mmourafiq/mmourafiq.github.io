---
layout: post

type: "article"

title: "Splitting Django models."
subtitle: "How to start a correct django project."
cover_image: posts/django.png
cover_image_caption: "Django"

excerpt: "Sometimes, it just makes sense to split up your models over multiple files instead of populating one models.py file. Allowing simple organization, easy maintenance clarity for new coder."

author:
  name: Mourad Mourafiq.
  twitter: mmourafiq
  bio: Maths, Technology, Philosophy, Startups, ...
  image: logo.png
---

Sometimes, it just makes sense to split up your models over multiple files instead of populating one `models.py` file. Allowing simple organization, easy maintenance clarity for new coder.

The operation is quit easy but requires some extra steps, that we shall walk through in this post.

In this example we will work with two models, `Musician` and `Album`, each model is defined in its own file within an app called `myapp`.

Let’s assume that `Album` has a ForeignKey to `Musician`;

 * myapp
    * models
        - \_\_init\_\_.py
        - musicians.py
        - albums.py

The contents of `musicians.py`:

{% highlight python linenos %}
class Musician(models.Model):
    first_name = models.CharField(max_length=50)
    last_name = models.CharField(max_length=50)
    instrument = models.CharField(max_length=100)

    class Meta:
        app_label = 'myapp'
{% endhighlight %}

The contents of `albums.py`:

{% highlight python linenos %}
class Album(models.Model):
    artist = models.ForeignKey('Musician')
    name = models.CharField(max_length=100)
    release_date = models.DateField()
    num_stars = models.IntegerField()

    class Meta:
        app_label = 'myapp'
{% endhighlight %}

Notice the definition of the app_label property in the inner Meta classes for each model. This is very important to let Django’s `syncdb` command know that these split up model classes belong to the same application.

We’re not done yet. You’ll also need to explicitly import each model class in the model module’s

{% highlight python linenos %}
from myapp.models.musicians import Musician
from myapp.models.albums import Album
{% endhighlight %}

And that’s it. Run `syncdb` and you should be all set.

NOTE: One thing to note about `ForeignKey`: either you don’t specify the import; you put the model then between ' ', or when you do the `import`, the order in the file `__init__.py` must be: first the model who doesn’t have the foreign key then the model who has the foreign key.

NOTE: If you are splitting up the contents of an existing `models.py` file, make sure to delete the original `models.py` file when you are done otherwise syncdb may get confused.
