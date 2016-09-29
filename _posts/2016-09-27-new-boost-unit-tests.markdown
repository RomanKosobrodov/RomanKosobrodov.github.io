---
layout: post
title:  "Starting a new project with Boost Unit Tests Framework"
date:   2016-09-27
categories: "C++"
---

Boost Unit Tests is a great testing framework. Unfortunately, it is somewhat lacking in the documentation department and I always struggle when I have to set up a new unit tests project. So I decided to put together some step-by-step instructions on how to do this.

### Step 1 - Development environment and prerequisites
The procedure is for a Linux development enviroment (Ubuntu 16.0 and optionally Eclipse CDT). Installing and configuring Eclipse is a different saga, so let's assume you somehow figured this out.

If you don't have the development version of boost you need to install it by running 

{% highlight bash %}
sudo apt-get install libboost-dev
{% endhighlight %}

This will install complete Boost library. You can also install just the Boost Unit Tests Framework:

{% highlight bash %}
sudo apt-get install libboost-test-dev
{% endhighlight %}

### Step 2 - Create a new project
In Eclipse create a new C++ project and set up the following properties (Properties > C/C++ General > Paths and Symbols):

<table border="0" width="100%" cellspacing="2" cellpadding="0">
<tbody>
<tr>
<td>include path</td>
<td>/usr/include/boost</td>
</tr>
<tr>
<td>libraries</td>
<td>boost_unit_test_framework</td>
</tr>
<tr>
<td>library paths</td>
<td>/usr/lib/x86_64-linux-gnu/boost</td>
</tr>
</tbody>
</table>

### Step 3 - Write automated tests
This step is a bit tricky because Boost Unit Test is very flexible when it comes to your code structure: there are automated and manual suites/tests and various combinations of them. We are going to create a fully automated unit tests module.

First, we need to define a unit test module and an empty test suite in one cpp file: 

{% highlight cpp %}
// file boost_auto_tests_main.cpp
#define BOOST_TEST_DYN_LINK
#define BOOST_TEST_MODULE sample_test_module

#include <boost/test/auto_unit_test.hpp>

BOOST_AUTO_TEST_SUITE(sample_test_suite)
// (so far) empty test suite
BOOST_AUTO_TEST_SUITE_END()
{% endhighlight %}

Now we can start adding tests to our sample_test_suite. Add a new cpp file to the project:

{% highlight cpp %}
// file unit_tests_00.cpp
#include <boost/test/auto_unit_test.hpp>

BOOST_AUTO_TEST_SUITE(sample_test_suite)

BOOST_AUTO_TEST_CASE(unit_test_00)
{
	BOOST_CHECK(true);
}

BOOST_AUTO_TEST_SUITE_END()
{% endhighlight %}

Now the project can be compiled, built and run. It produces the following optimistic output:
{% highlight bash %}
*** No errors detected
Running 1 test case...
{% endhighlight %}
For some reason the lines are out of order in the Eclipse console.

New suites and tests cases can be created by adding cpp files to the project. For example,
the code below adds a second unit test to the sample_test_suite, creates another suite named widget_test_suite and adds a test (widget_create_test) to it. 

{% highlight cpp %}
// file unit_tests_01.cpp
#include <boost/test/auto_unit_test.hpp>

BOOST_AUTO_TEST_SUITE(sample_test_suite)
// add another test to sample_test_suite
BOOST_AUTO_TEST_CASE(unit_test_01)
{
	BOOST_CHECK(4==4);
}

BOOST_AUTO_TEST_SUITE_END()

// create a new test suite 
BOOST_AUTO_TEST_SUITE(widget_test_suite)

BOOST_AUTO_TEST_CASE(widget_create_test)
{
	int widget = 0;
	BOOST_CHECK(widget<1);
}

BOOST_AUTO_TEST_SUITE_END()
{% endhighlight %}

This happily builds and runs informing us (somewhat randomly) of the results:
{% highlight bash %}
Running 3 test cases...

*** No errors detected
{% endhighlight %}

That's it. As you write more code you keep adding files to your unit test project.
I prefer to put each test suite in its own directory to keep things organised.

Happy testing! 