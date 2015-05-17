---
layout: post

type: "article"

title: "[] is not JSON serializable (django)."
subtitle: "Serializing Django values_list."
cover_image: posts/binary.jpg
cover_image_caption: "Binary"

excerpt: "Serializing Django values_list."

author:
  name: Mourad Mourafiq.
  twitter: mmourafiq.
  bio: Maths, Technology, Philosophy, Startups, ...
  image: logo.png
---

Not very explicit error, especially if you re trying to serialize Django values_list.

{% highlight python %}
data = MyModel.object.all().values_list('id', flat=true)
{% endhighlight %}

This indeed returns a list: `[‘1’, '2’, '3’, …]`, So if you intend to serialize it you’ll likely get errors; that it’s not serializable.

This is actually not quite the full story. Because you are trying to serialize a ValuesListQuerySet and not a List.

{% highlight python  %}
type(data) <class 'django.db.models.query.ValuesListQuerySet'>
{% endhighlight %}

Practically, you have two solutions:

 1. Convert to a python List:

{% highlight python  %}
data = list(MyModel.object.all().values_list('id', flat=true))
{% endhighlight %}

But again, there is no guarantee that it will work! It depends if the returned value of values_list is iterable or not.

 2. Serialize before passing it to json.dumps:

You can choose what to serialiaze Django has a built-in way to serialize a `QuerySet`. And if you only want the `IDs`, you may use the fields `kwarg`.

{% highlight python linenos %}

from django.core import serializers

data = serializers.serialize('json', MyModel.objects.all(), fields=('id',))
{% endhighlight %}

Hope it helps!
