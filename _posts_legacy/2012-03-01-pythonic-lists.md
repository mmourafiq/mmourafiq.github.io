---
layout: post

type: "article"

title: "Python List"
subtitle: "Using Lists in Python."
cover_image: posts/python-power.jpg
cover_image_caption: "Write code."

excerpt: "Using Lists in Python."

author:
  name: Mourad Mourafiq.
  twitter: mmourafiq
  bio: Maths, Technology, Philosophy, Startups, ...
  image: logo.png
---
**list.append(element)**
{% highlight python linenos %}
#appen an element to a list
li = [1, 2, 3, 4, 5]
li.append(6)
print li # [1, 2, 3, 4, 5, 6]
{% endhighlight %}

**Sets**
{% highlight python linenos %}
a.issubset(b)
#<==>
a & b == a

a.union(b)
#<==>
a | b

a.intersect(b)
#<==>
a & b

a.difference(b)
# <==>
a - b

a.symmetric_difference(b)
#<==>
a ^ b

#set ==> list
list(ensemble)

s1 = set([1, 2, 3, 4, 5, 6])
s2 = set([2, 3, 4])
s3 = set([6, 7, 8, 9])

print s1 & s2 # set([2, 3, 4])
print s1 | s3 # set([1, 2, 3, 4, 5, 6, 7, 8, 9])
print s1 & s3 # set([6])
print s1 ^ s3 # set([1, 2, 3, 4, 5, 7, 8, 9])
print s1 - s2 # set([1, 5, 6])
{% endhighlight %}

**list.extend(other_list)**
{% highlight python linenos %}
#extend list and append to it element from other_list
li = [1, 2, 3, 4, 5]
li.extend([6, 7, 8]) # Same as li += [6, 7, 8]
print li # [1, 2, 3, 4, 5, 6, 7, 8]
{% endhighlight %}

**list.remove(other_list)**
{% highlight python linenos %}
#remove the first value equal to v from list
li = [1, 2, 3, 4, 5]
li.remove(1)
print li # [2, 3, 4, 5]
{% endhighlight %}

**list.reverse()**
{% highlight python linenos %}
#reverse the list
li = [1, 2, 3, 4, 5]
li.reverse()
print li # [5, 4, 3, 2, 1]

print reversed(li) # [1, 2, 3, 4, 5]
print li # [5, 4, 3, 2, 1] <-- not modified
{% endhighlight %}

**list.sort()**
{% highlight python linenos %}
#sort list
li = [3, 1, 4, 2, 5]
li.sort()
print li # [1, 2, 3, 4, 5]

li = [3, 1, 4, 2, 5]
print sorted(li) # [1, 2, 3, 4, 5]
print li # [3, 1, 4, 2, 5] <-- not modified
{% endhighlight %}

**map(callback, list)**
{% highlight python linenos %}
#use of callbacks in map
# <==>
# [a, b, c] -> [callback(a), callback(b), callback(c)]
def map(callback, l):
    new_l = []
    for element in l:
       new_l.append(callback(element))
    return new_l

def sqr(x): return x ** 2
def pair(x): return not bool(x % 2)

print map(sqr [1, 2, 3, 4, 5]) # [1, 4, 9, 16, 25]

print map(pair, [1, 2, 3, 4, 5]) # [False, True, False, True, False]
{% endhighlight %}

**filter(callback, list)**
{% highlight python linenos %}
#useof callback in filter
# <==>
# [a, b, c] -> [a if callback(a) == True, b if callback(b) == True, c if callback(c) == True]
def filter(callback, l):
    new_l = []
    for element in l:
       if callback(element): new_l.append(element)
    return new_l

#e.g
def sqr_less_16(x): return x ** 2 < 16
def pair(x): return not bool(x % 2)

print filter(sqr_less_16, [1, 2, 3, 4, 5]) # [1, 2, 3]

print filter(pair, [1, 2, 3, 4, 5]) # [2, 4]


# pair element
print [x for x in l if x % 2 == 0] # <==> filter + easy to use.

# use of both pair sqr
print [x ** 2 for x in l if x ** 2 % 2 == 0]
# <==>
print [x for x in [a ** 2 for a in l] if x % 2 == 0]
{% endhighlight %}

**reduce(callback, list, init_value)**
{% highlight python linenos %}
#use of callback in reduce
# <==>
# [a, b, c] -> callback(callback(a, b), c)
def reduce(callback, l, initial_value):
    if l == []: return initial_value
    else: return callback(l[0], reduce(callback, l[1:], initial_value))

#e.g
def add(x, y): return x + y
def prod(x, y): return x * y

print reduce(add, [1, 2, 3], 0) # ((0 + 1) + 2) + 3
print reduce(prod, [1, 2, 3], 1) # ((1 * 1) * 2) * 3
{% endhighlight %}

**list <==> string**
{% highlight python linenos %}
print ", ".join(["a", "b", "c", "d"]) # print list element + ", " in between, <==> "a, b, c, d".
print "a b c d".split(" ") # <==> [a, b, c, d]

l = [
    (105, "d"),
    (21, "z"),
    (0, "v")
]
l.sort() # sort by first element then second
{% endhighlight %}
