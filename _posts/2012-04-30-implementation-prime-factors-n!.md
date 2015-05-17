---
layout: post

type: "article"

title: "Finding prime factors of n! and their multiplicities"
subtitle: "A small algorithm to compute all prime factors of n!"
cover_image: posts/blueprint-math.jpg
cover_image_caption: "math."

excerpt: "The purpose of this post is to write a small algorithm to compute all prime factors of n!, given a positive integer n."

author:
  name: Mourad Mourafiq.
  twitter: mmourafiq.
  bio: Maths, Technology, Philosophy, Startups, ...
  image: logo.png
---

In this post, we will write a small algorithm to compute all prime factors of x!, given a positive integer. x The theoretical background behind the algorithm we are going to write could be found in this post.

First, we have to find all prime numbers less than x:

{% highlight python linenos %}
def primes_less(x):
    non_prime = set()
    i = 2
    while i*i <= x:
        if i not in non_prime:
            j = i
            while j*i <= x:
                non_prime.add(j*i)
                j += 1
        i += 1

    return set(range(2, x+1)) - non_prime
{% endhighlight %}

Then we  can easily calculate all their multiplicities for x!:

{% highlight python linenos %}
def get_primes_multiplicities(x):
    """
    Returns factors for x!
    """
    p_multi = {}  # factors multiplicities
    for p in primes_less(x):
        temp = x  
        mult = 0
        while temp!= 0:
            temp /= p
            mult += temp
        p_multi[p] = mult
    return p_multi

def nbr_factors(x):
    nbr = 0
    for f in get_primes_multiplicities(x).values():
        nbr += (f + 1)
    return nbr
{% endhighlight %}

The product of the multiplicities of this prime factors is thus the possible divisors for x.

Let $$p_i$$ and $$e_i$$ be the prime and it's exponents, we can write:

$$
\begin{align*}
x! = p_1^{e_1} * p_2^{e_2} * ... * p_n^{e_n}
\end{align*}
$$

We can conclude also that, there are:

$$
\begin{align*}
(1 + 2*e_1) * (1 + 2*e_2) * ... * (1 + 2*e_n)\ possible\ divisors\ for\ x!^2
\end{align*}
$$

The only changes we need to bring to our code:

{% highlight python linenos %}
def nbr_factors_sqr(x):
    nbr = 1
    for f in get_primes_multiplicities(x).values():
        nbr *= (2*f + 1)
    return nbr
{% endhighlight %}
