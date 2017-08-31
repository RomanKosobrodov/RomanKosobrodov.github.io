---
layout: post
title:  "Four-liner anagram - the STL way"
date:   2016-09-26
categories: "C++"
---

In my experience Python beats C++ in terms of development time with a ratio 3 to 1.
The same is true for the code size. There are a few examples though where the gap is less obvious.   

Here is a four-liner to check if two strings are anagrams:
{% highlight cpp %}
bool is_anagram(const std::string& s1, const std::string& s2)
{
    std::map<char, int> m;

    std::for_each(begin(s1), end(s1), [&m](char a) mutable { if (!isspace(a)) ++m[tolower(a)]; });
    std::for_each(begin(s2), end(s2), [&m](char a) mutable { if (!isspace(a)) --m[tolower(a)]; });

    return std::all_of(begin(m), end(m), [](auto &e){return e.second == 0;});
}
{% endhighlight %}

As usual, it's hard to beat Python code below which we could make [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)'er with a lambda, or even [put in one line](https://www.youtube.com/watch?v=DsUxuz_Rt8g). 

{% highlight python %}
from collections import Counter

def is_anagram(s1, s2):
    a = s1.replace(' ','').lower()
    b = s2.replace(' ','').lower()

    return Counter(a) == Counter(b)             

{% endhighlight %}

OK, clarity is probably not the strongest side of the C++ snippet above. So what is it doing?

The first for_each creates a map of symbols in the first string. Spaces are ignored, characters are converted to lower case and the count is incremented. The trick with ++m[a] is that map creates a default key-value pair if you call operator[] for a non-existing key. The rest is pretty usual: we capture the map by reference and let the lambda modify it by declaring it mutable.
The second call to for_each performs a similar operation using symbols of the second string, this time decrementing the symbol counts in the map.
Finally, if all the counts in the map are zeros we have an anagram!

And here is my favourite:

{% highlight python %}
assert is_anagram('public relations', 'crap built on lies')
{% endhighlight %}
