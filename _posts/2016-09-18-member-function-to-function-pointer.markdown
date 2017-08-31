---
layout: post
title:  "Impossible C++: Converting a pointer to object member to a function pointer"
date:   2016-09-11
categories: "C++"
---

As a C++ programmer I used to think that there are no tasks that could not be
tackled by the language. People do all sorts of fascinating things with it including
rendering web pages and processing text.

Recently I came across a simple task that apparently could not be solved in C++.
This is conversion from a pointer to an object member function to raw C function 
pointer. Below is the code that illustrates the problem.

First, we define a pointer to a function that takes two parameters: an integer
and a double and returns a double value:

{% highlight cpp %}
typedef double (*func_ptr) (int, double);
{% endhighlight %}

Next, let's define a function that takes that pointer as an argument and uses it to calculate
a value:

{% highlight cpp %}
double third_party_function(func_ptr f)
{
	return f(3, 25.5);
}
{% endhighlight %}

We'll call it a third-party function. In my application this was a non-linear least squares solver
from Intel MKL library.

We want to use this third-party code with our own function of type func_ptr which we define as
follows:

{% highlight cpp %}
double standalone(int n, double x)
{
	return n*x;	
}
{% endhighlight %}

This function can now be used with our third-party code like this:

{% highlight cpp %}
std::cout << third_party_function(standalone) << "\n";
{% endhighlight %}

My problem was a bit more difficult than multiplying two numbers so I had to use a class to
maintain state between function calls (it also helps with reducing the memory footprint of your
code). So here is a function object:

{% highlight cpp %}
struct functor
{
	int offset;	
	double member(int n, double x) 
	{
		return n + x + offset;
	}		
};
{% endhighlight %}

Notice that the member function has the same signature as standalone, so it is reasonable to 
expect that with some C++11 magic you should be able to use it as an input to third_party_function.
Apparently, this is not the case.

I did a bit of research and tried several things:

1. Implicit conversion does not compile for obvious reasons
{% highlight cpp %}
func_ptr foo = std::bind(&functor::member, &worker, _1, _2);
{% endhighlight %}

1. Function object
{% highlight cpp %}
std::function<double(int,double)> function_object = std::bind(&functor::member, &worker, _1, _2);
func_ptr fp = *function_object.target<func_ptr>();
y = third_party_function(fp);
{% endhighlight %}
That compiles but for unknown reasons causes a segmentation fault (on Ubuntu 16.04 with g++ 5.4.0)

I also tried
{% highlight cpp %}
*function_object.target<double(int,double)>()
{% endhighlight %}
with equally devastating results. 

Sad as it is but this conversion seems impossible in C++.

And how did I go about solving my optimisation problem with MKL? The old-school way - I wrote more code!

