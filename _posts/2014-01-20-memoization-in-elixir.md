---
layout: post
comments: true
title: "Memoization in Elixir"
description: ""
category:
tags: []
---
{% include JB/setup %}


This post presents an investigation over implementations of the [memoization](http://en.wikipedia.org/wiki/Memoization) technique in the context of functional programming.

Besides being straighforward and broadly used by programmer of imperative languages, implementing memoization in a functional programming language was a non obvious task and presented lots of opportunities to learning some theory and running some experiments, which I compiled in this post.

Regardless the choice of language, [Elixir](http://elixir-lang.org/), the results may be useful for other functional languages as well.

## The problem

To illustrate the problem, lets take a look at a [fibonacci algorithm](http://en.wikipedia.org/wiki/Fibonacci_number) implementation.

{% include code/fibonacci-exponetial.html %}

This implementation has a very well known performance problem, as we can see in the profiling chart bellow.

<figure>
  <img width="100%" src="/images/exponetial-fibonacci.png" alt="Fibonacci with exponential growth curve">
  <figcaption>
    Computation time in microseconds. Calculating the 40th number in the sequence takes 1 minute. But just 5 numbers ahead, the 45th, takes slow 12 minutes. It’s just unfeasible to go much further without optimizing it.
  </figcaption>
</figure>

To understand why such thing happens, lets draw a diagram with the calculation of the 5th element in the fibonacci sequence (each node represents a function call), where we can see that the same fibonacci number is calculated many times as identical subproblems repeat.

<figure>
  <img width="75%" src="/images/identical_subproblems.png" alt="Identical Subproblems of a Fibonacci sequence">
  <figcaption>
    Each node represents a call to fibonacci function, so in order to calculate f(5) we calculate f(4) and f(3). As you can see in the blue circle, the calculation for f(3) is done twice and f(2) is done three times.
  </figcaption>
</figure>

We can drastically improve over that. Let’s by now ignore the bottom-up solution, which is linear in complexity and memory usage, and instead try to improve over what we already have, without changing the algorithm too much. An idea would be to store these subproblems in a cache that we may consult before calculating the same function again.

## Adding a cache

One of the benefits of using a functional programming language is the clean flow of data. It’s just easy to reason about it. But, take a look at the fibonacci algorithm after adding the cache.

{% include code/fibonacci-partial-cache.html %}

This is not ready yet. We still have some duplicated computations. Can you spot the problem?

Fully memoization is achieved with the following

{% include code/fibonacci-fully-memoized.html %}

Which surely solved the complexity problem.

<figure>
  <img width="100%" src="/images/cached-fibonacci.png" alt="Fibonacci with linear growth curve">
  <figcaption>
    Computing the 1000th element in the sequence takes less time than computing the 25th with our inneficient algorithm.
  </figcaption>
</figure>

Unfortunatelly, our beatiful fibonacci algorith is gone. Having to handle the cache messed up our once clear function. A really simple change in the behavior required a considerable change in code. Well designed code should not behave like that. Wouldn’t it be nice if fewer modifications were needed and the solution could be used for memoizing other functions as well? It feels that there is a better solution for this. Some things are easy to do once, but as every programmer, I’m lazy and don’t want to repeat it every time I face this problem again. I wanted something that solved not only this problem, but the whole category of problems. Something easy to use. Something like this:

{% include code/wanted.html %}

## Taking a deep breath

I couldn't find such a generic memoization implementation straight away, so I kept this process running in the background, waiting for an idea that whould shed some light in the problem. That's exactly what happened while I was reading about [Y combinator](http://en.wikipedia.org/wiki/Fixed-point_combinator#Y_combinator) in The Little Schemer<sup><a href="#fn-item1" id="fn-return1">1</a></sup> book, during my vacations.

The idea was to add a layer of inderection in the function call, problem solved by the Y combinator. Instead of calling the `fib` function directly, we would call a function that looks like our original fib, but internally implements caching strategy. This way we could keep our algorithm as is.

So, first of all, lets understand what a Y combinator is by implementing it.

## Y combinator

I'm gonna let the computer science definition to the wikipedia article, and instead focus on the aspect that is important to our problem. The Y combinator is a way of implementing recursion without named functions. So, for a moment, lets consider the `def` construction no longer exists and we can only use anonnymous functions.

We are going to build it using a trial and error approach, just like the referenced book.

In order to call `fib` recursively, we need to receive the `fib` function as a parameter, since we lost the ability to define functions.

{% include code/y-combinator-1.html %}

Don't worry about the `foo` function. It's just a dummy function that raises an error when called.

Our anonnymous function receives as an argument a function that was suposed to be the `fib` function. But, how can we pass the function we just defined to itself?

As we do not know how to do it yet, lets pass a function that raises an error.

This function works for the first value in the sequence, but raises an error for the second.

So in order to be able to calculate the value for the second number, we define the function again and pass it as a parameter.

{% include code/y-combinator-2.html %}

This function works for values 1 and 2 but not 3. Defining the funciton again and againg whithout an end is not the way to go.

In the next code, we pass our annonymous function to yet another function that calls it and pass it to itself.

At this point, I stoped reading the book and just returned the next day<sup><a href="#fn-item2" id="fn-return2">2</a></sup>.



We've just bootstraped our function with itself as an argument. Another key point to notice is that we do not call `fib` directly, in the body of the function. Instead, we call `fib` passing `fib` as a parameter, because thats what `fib` expects to receive (itself as an argument).

{% include code/y-combinator-3.html %}

Explaining it again. First, we define a function in line 46 that receives as argument a function f, calls that function f with f as argument. Then in line 52 we need to keep passing this function as parameter to itself. This are the two points to understand: bootstraping and maintaining.

But we want our pure `fib` function back, so lets extract this recursion generator code to a function.

{% include code/y-combinator-4.html %}

As we can see, we have our `fib` function back (64-72). Extracting the rest of the function, we have the y-combinator separated from the `fib` function.

{% include code/y-combinator-5.html %}

{% include code/make-fib.html %}

We can use it as follows.

{% include code/y-fib-use.html %}

It may seem a lot of work just to get our original functionality back. But the fact that our new `fib` function receives a function as an argument, is going to provide the entry point to inject the memoization functionality.

## Memoization with Y combinator and ETS

Now that we understand what a Y combinator is, lets use it to implement memoization of the fibonacci function in a trasparent way.

The goal is to provide a way to plug-in memoization, without messing around with our `fib` function. Also we want to make it in a generic fashion, so we can memoize any function (any function with one integer argument, by now).

In order to do that, we are going to use [Erlang ets](http://www.erlang.org/doc/man/ets.html).

The code bellow is the main result of this research. It provides a generic memoization function. I'm gonna credit Paul Mineiro, the author of the [Erlang version](http://erlang.org/pipermail/erlang-questions/2007-July/028057.html) of this code.

{% include code/memoization.html %}

We can use this code like

{% include code/memoization-use.html %}

As we can see, its pretty fast.

<figure>
  <img width="100%" src="/images/memoized-fibonacci.png" alt="Memoized fibonacci with linear growth curve">
  <figcaption>
    The memoized version is faster than the cached version. Thats because the ets is has constant (xxx) access lookup time and dictionaries have logarithm (xxx) time.
  </figcaption>
</figure>

## Conclusion

Sometimes, when learning something completely different, like functional programming, we may feel ourselves limited by the lack of some features we are used to. But facing these "shortages" can take you to a the next level of programming languages compreehension. Theory is valuable.

## Next steps

I believe that there are other ways of solving this with pure functional features. I'm reading the article [Monads for functional programming](http://homepages.inf.ed.ac.uk/wadler/papers/marktoberdorf/baastad.pdf) and think that it may present yet another solution to this problem. But I'm not feeling confident to write about it yet.

Also, it seems that the Haskell comunity already knows about a [solution](http://www.haskell.org/haskellwiki/Memoization) to this problem, but I don't know Haskell yet.

To make using the `memo` function easier, maybe we could use some macro to generate `fib` functions, those that receive a function as paramenter, from function that receive integers.

### Notes

<footer>
  <ol class="foot-notes">
    <li id="fn-item1">The Little Schemer - 4th Edition, Daniel P. Friedman and Matthias Felleisen. Chapter 9. Many thanks to <a href="https://twitter.com/fabioyamate">@fabioyamate</a> for borrowing the book. I'm gonna return it.<a href="#fn-return1">↩</a></li>
    <li id="fn-item2">To be honest, the book was not enough to make me understand it. So I've read another <a href="http://cs.brown.edu/~sk/Publications/Books/ProgLangs/2007-04-26/">awesome reference</a>, Programming Languages: Application and Interpretation, by Shriram Krishnamurthi. Read the section 22.4, "Eliminating Recursion".<a href="#fn-return2">↩</a></li>
  </ol>
</footer>


<!-- <div id="chart_div" style="width: 900px; height: 300px;"></div> -->

<!--script type="text/javascript">
  google.load("visualization", "1", {packages:["corechart"]});
  google.setOnLoadCallback(drawChart);

  function drawChart() {
    var data = google.visualization.arrayToDataTable([
      ['Size', 'Time'],
      // Memoized
      ['100', 355],
      ['200', 564],
      ['300', 675],
      ['400', 927],
      ['500', 1187],
      ['600', 1335],
      ['700', 1644],
      ['800', 1636],
      ['900', 1899],
      ['1000', 2204]

      // Cached
      // ['100', 518],
      // ['200', 935],
      // ['300', 1274],
      // ['400', 1809],
      // ['500', 2185],
      // ['600', 2110],
      // ['700', 2209],
      // ['800', 2553],
      // ['900', 2752],
      // ['1000', 3045]

      // Exponential times
      // ['1', 1],
      // ['5', 1],
      // ['10', 7],
      // ['15', 69],
      // ['20', 612],
      // ['25', 4879],
      // ['30', 59597],
      // ['35', 614081],
      // ['36', 1009343],
      // ['37', 1620680],
      // ['38', 2635064],
      // ['39', 4264705],
      // ['40', 6903349],
      // ['41', 11154273],
      // ['42', 18039415],
      // ['43', 29249938],
      // ['44', 47774353],
      // ['45', 196758967]
    ]);

    var options = {
      // curveType: 'function'
    };

    var chart = new google.visualization.LineChart(document.getElementById('chart_div'));
    chart.draw(data, options);
  }
</script-->
