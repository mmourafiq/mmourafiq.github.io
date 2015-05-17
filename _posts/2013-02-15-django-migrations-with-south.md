---
layout: post

type: "article"

title: "Django migrations with South."
subtitle: "Intelligent schema and data migrations for django project."
cover_image: posts/django.png
cover_image_caption: "Django"

excerpt: "Intelligent schema and data migrations for django project."

author:
  name: Mourad Mourafiq.
  twitter: mmourafiq.
  bio: Maths, Technology, Philosophy, Startups, ...
  image: logo.png
---

South is an “intelligent schema and data migrations for django projects”

In this post we will go through the steps required in order to set up south in your django project.

### installation

To install south:

{% highlight console %}
>  pip install south
{% endhighlight %}

To see the list of management functions offered by south, just type ./manage.py  and go to [south]


In this post we will cover the following three functionalities of south:

 1. schemamigration
 2. migrate
 3. datamigrate : changing data within our database to match the new structure of schema

### Schema migration

Suppose we have a posts app with a model: Post

The first thing to do is tell south to manage and builld history migrations for this app. This is called initial migration:

{% highlight console %}
>  ./manage schemamigration posts --initial
{% endhighlight %}

South will show us that we can apply this first migration with migrate.

Now, we are not going to apply this migration yet, first, we are going to add a new model and apply the migrationschema, but this time with auto instead of initial. Auto tells south to automatically detect any changes in our models and update our schema and the migration appropriately.

### migrate

To apply migrations we need to use the command :

{% highlight console %}
>  ./manage.py migrate posts
{% endhighlight %}

N.B. if you are updating tables that are already in the database, the initial migration should be followed by the statement –fake.

Now, suppose we want to change the model. All we need to do is to go back and run:

{% highlight console %}
>  ./manage.py schemamigration posts --auto
{% endhighlight %}

Depending on the changes you have made. You will be asked to specify a default value.

 * You can quit and specify the default values in your django model.
 * Specify a one off value to use for existing columns

Now run the command :

{% highlight console %}
>  ./manage.py migrate posts
{% endhighlight %}

Suppose we missed something, or we are not completely happy with last changes, to roll back the migration we run:

{% highlight console %}
>  ./manage.py migrate posts 000X
{% endhighlight %}

(where “000X” is the migration number, e.g: “0002”)

In case you changed your mind another time and you want to move forward, you only need to run:

{% highlight console %}
>  ./manage.py migrate posts
{% endhighlight %}

If on the other hand you happened to think that migration “x” isn’t what I want, you just need to go ahead a delete it physically from the migrations folder.

### Data migration

Now suppose we want to get rid of a field and migrate into two different fields, but keep the data and make some changes. In this case we need to go through 3 stages. Of course these 3 stages are only required when we have data in the initial field and we want to keep them.

 1. We need to create the new fields in our model, without deleting the initial field.
 2. Then we need a data migration; which will process whatever is in the initial field and populate the newly created fields with the appropriate data.
 3. Finally we need a schemamigration to get rid of the old field and leave the newly created fields there.

-> Go then and create two new fields. run:

{% highlight console %}
>  ./manage.py schemamigration posts -- auto
{% endhighlight %}

-> We need now a datamigration. run :

{% highlight console %}
>  ./manage.py datamigration posts convert_names
{% endhighlight %}

(where “convert_names” is the name of the data we need to migrate, it creates a file in the migrations folder with an empty forwards and backwards function that we need to fill appropriately)

-> What we need to do is access the models in the database using the orm. e.g:

{% highlight python linenos %}
def forwards(self, orm):
    for a in orm.Author.object.all():
        try:
            a.first_name, a.second_name = a.name.split(" ")
        except:
            a.first_name, a.second_name = a.name, ""
        c.save()

 def backwards(self, orm):
    for a in orm.Author.object.all():
        a.name = ""
        a.save()
{% endhighlight %}

-> Now we can create another migration to delete the older field. Go to your model and delete the initial field. and then run:

{% highlight console %}
>  ./manage.py schemamigration posts --auto
{% endhighlight %}
