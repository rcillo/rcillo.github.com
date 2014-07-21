---
layout: post
comments: true
title: "The Role of Algorithms in Computing"
description: "Study notes and solutions to some of the exercises from the CLRS tome."
category:
tags: []
---
{% include JB/setup %}

These are the answers to the first chapter problems and exercises of the
[Introduction to Algorithms](http://mitpress.mit.edu/books/introduction-algorithms),
better know as *CLRS*. I expect this to be the first of series of posts, one per week.

I'm going to be using [MathJax](http://www.mathjax.org/) for mathematical symbols,
*Ruby* and *C* for implementing solutions.

## Algorithms

### 1.1-1

Give a real-world example in which one of the following computational problems
appears: sorting, determining the best order for multiplying matrices, or finding
the convex hull.

<hr>

**Answer:** Sorting algorithms are broadly used in database softwares. Sorting results by a
specific criteria is a basic feature from theses systems. But in practice, sorting
 is implemented with the help of a data structure, which is know by the *index*.
But when an index is not available, sorting algorithms comes to the rescue.

### 1.1-2

Other than speed, what other measures of efficiency might one use in a real-world
setting?

<hr>

**Answer:** Memory usage. Due to hardware limitations, one may be required to build algorithms
that favors low memory usage to low CPU time.

### 1.1-3

Select a data structure that you have seen previously, and discuss its strengths
and limitations.

<hr>

**Answer:** Hash tables. Besides been a great data structure for key-value storage (get and put
operations in constant time), most hash table implementations do not provide access
to the lowest key or any sorting capability, like listing keys in ascending order.

### 1.1-4

How are the shortest-path and traveling-salesman problems given above similar?
How are they different?

<hr>

**Answer:** Both yield as answer the shortest path, an ordered list of vertices and arrows to travel by,
but the TSP has one more criteria: the origin vertice must be equal to the destination
vertice.

### 1.1-5

Come up with a real-world problem in which only the best solution will do. Then
come up with one in which a solution that is "approximately" the best is good
enough.

<hr>

**Answer:** [Bin packing](http://en.wikipedia.org/wiki/Bin_packing_problem) is known to be a
NP-complete problem where approximation algorithms are a desirable approach.

But when it comes to cryptography, only the exact solution would be of value, since
providing the wrong answer could lead to disastrous consequences, like changing
that "I love you" message to your girlfriend into a "I love her". That would probably
cost you some very long time to explain the approximation algorithm rate error.

## Algorithms as a technology

### 1.2-1

Give an example of an application that requires algorithmic content at the application
level, and discuss the function of the algorithms involved.

<hr>

**Answer:** Flight scheduling optimization. This is a problem of real interest to flight
companies where keeping the costs lower then their competitors may be a key factor
to not get out of business.

### 1.2-2

Suppose we are comparing implementations of insertions sort and merge sort on the
same machine. For inputs of size $n$, insertion sort runs in $8 n^2$ steps, while
merge sort runs in $64 n \lg n$ steps. For which values of $n$ does insertion sort
beat merge sort?

<hr>

**Answer:** Using some basic equation cancelation and number inspection, it is easy to see
that it happens between $n = 32$ and $n = 64$. But testing all the 32 possibilites
would be tedious, so using this script ruby:

<pre>
def merge_time(n)
  64 * n * Math.log2(n)
end

def insertion_time(n)
  8 * (n**2)
end

n = 32

while insertion_time(n) <= merge_time(n); n = n + 1; end
</pre>

I concluded that for values of $n$ lower than 44 it is faster to use the insertion
sort algorithm.

### 1.2-3

What is the smallest value of $n$ such that an algorithm whose runing time is $100n^2$
runs faster than an algorith whose running time is $2^n$ on the same machine?

<hr>

Using some math it was easy to find out that it was between 10 and 20, but with
simple script,

<pre>
while 2 * (100 ** n) < 2 ** n; n = n + 1; end
</pre>

Yields $n = 15$.

## Problems

### 1-1 Comparison of running times

For each function $f(n)$ and time $t$ in the following table, determine the
largest size $n$ of a problem that can be solved in time $t$, assuming that the
algorithm to solve the problem takes $f(n)$ microseconds.

<table width="100%" border="1">
  <tr>
    <th></th>
    <th>1 second</th>
    <th>1 minute</th>
    <th>1 hour</th>
    <th>1 day</th>
    <th>1 month</th>
    <th>1 year</th>
    <th>1 century</th>
  </tr>
  <tr>
    <td>$\lg n$</td>
    <td>$2^{10^6}$</td>
    <td>$2^{6.0 \times 10^7}$</td>
    <td>$2^{3.6 \times 10^9}$</td>
    <td>$2^{8.64 \times 10^{10}}$</td>
    <td>$2^{2.6 \times 10^{12}}$</td>
    <td>$2^{3.1 \times 10^{13}}$</td>
    <td>$2^{3.1 \times 10^{15}}$</td>
  </tr>
  <tr>
    <td>$\sqrt{n}$</td>
    <td>$10^{12}$</td>
    <td>$3.6 \times 10^{15}$</td>
    <td>$1.3 \times 10^{17}$</td>
    <td>$7.4 \times 10^{19}$</td>
    <td>$6.7 \times 10^{22}$</td>
    <td>$9.6 \times 10^{27}$</td>
    <td>$9.6 \times 10^{31}$</td>
  </tr>
  <tr>
    <td>$n$</td>
    <td>$10^6$</td>
    <td>$6.0 \times 10^7$</td>
    <td>$3.6 \times 10^8$</td>
    <td>$8.6 \times 10^9$</td>
    <td>$2.6 \times 10^{11}$</td>
    <td>$3.1 \times 10^{13}$</td>
    <td>$3.1 \times 10^{15}$</td>
  </tr>
  <tr>
    <td>$n \lg n$</td>
    <td>$6.2 \times 10^3$</td>
    <td>$2.8 \times 10^6$</td>
    <td>$1.5 \times 10^7$</td>
    <td>$3.0 \times 10^8$</td>
    <td>$7.9 \times 10^9$</td>
    <td>$7.8 \times 10^{11}$</td>
    <td>$6.7 \times 10^{13}$</td>
  </tr>
  <tr>
    <td>$n^2$</td>
    <td>$10^3$</td>
    <td>$7.7 \times 10^3$</td>
    <td>$6 \times 10^4$</td>
    <td>$3 \times 10^5$</td>
    <td>$1.6 \times 10^6$</td>
    <td>$5.5 \times 10^6$</td>
    <td>$5.5 \times 10^7$</td>
  </tr>
  <tr>
    <td>$n^3$</td>
    <td>$10^2$</td>
    <td>$3.9 \times 10^2$</td>
    <td>$7.1 \times 10^2$</td>
    <td>$2.0 \times 10^3$</td>
    <td>$6.3 \times 10^3$</td>
    <td>$3.1 \times 10^4$</td>
    <td>$1.4 \times 10^5$</td>
  </tr>
  <tr>
    <td>$2^n$</td>
    <td>$19$</td>
    <td>$25$</td>
    <td>$28$</td>
    <td>$33$</td>
    <td>$37$</td>
    <td>$44$</td>
    <td>$51$</td>
  </tr>
  <tr>
    <td>$n!$</td>
    <td>9</td>
    <td>11</td>
    <td>11</td>
    <td>13</td>
    <td>14</td>
    <td>16</td>
    <td>17</td>
  </tr>
</table>

In order to calculate some of the results in the table, I used an [iterative method](http://rosettacode.org/wiki/Roots_of_a_function#Ruby)
for finding the root of a function in a given range.

<pre>
def sign(x)
  x <=> 0
end

def find_roots(f, range, step=0.001)
  sign = sign(f[range.begin])
  range.step(step) do |x|
    value = f[x]
    if value == 0
      puts "Root found at #{x}"
    elsif sign(value) == -sign
      puts "Root found between #{x-step} and #{x}"
    end
    sign = sign(value)
  end
end

f = lambda { |n| n * Math.log2(n) - (3.1 * 10**15) }
find_roots(f, 100000000000..1000000000000000, 1000000000)

# Root found between 67480000000000 and 67481000000000

"%E" % 67480000000000

# => "6.748000E+13"
</pre>

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}
});
</script>

<script type="text/javascript"
  src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
