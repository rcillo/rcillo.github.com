---
layout: post
title: "Getting Started"
description: ""
category:
tags: []
---
{% include JB/setup %}

## 2.1 Insertion sort

### 2.1-1

Using Figure 2.2 as a model, illustrate the operation of INSERTION-SORT on the
array $A = \langle 31, 41, 59, 26, 41, 58 \rangle$.

<hr>

<figure>
  <img width="100%" src="/images/clrs/ch2/exercise-2_1-1-insertion-sort-diagram.png" alt="Insertion sorte operations illustrated">
</figure>

### 2.1-2

Rewrite the INSERTION-SORT procedure to sorte into nonincreasing instead of
nondecreasing order.

<hr>

<div class="code">
INSERTION-SORT($A$)<br>
1&emsp;<b>for</b> $j \leftarrow 2$ to $length[A]$<br>
2&emsp;&emsp;<b>do</b> $key \leftarrow A[j]$<br>
3&emsp;&emsp;&emsp;$\rhd$ Insert $A[j]$ into the sorted sequence $A[1..j - 1]$.<br>
4&emsp;&emsp;&emsp;$i \leftarrow j - 1$<br>
5&emsp;&emsp;&emsp;<b>while</b> $i > 0$ and $A[i] < key$<br>
6&emsp;&emsp;&emsp;&emsp;<b>do</b> $A[i + 1] \leftarrow A[i]$<br>
7&emsp;&emsp;&emsp;&emsp;&emsp;$i \leftarrow i - 1$<br>
8&emsp;&emsp;&emsp;$A[i + 1] \leftarrow key$<br>
</div>

The only difference is in line 5 where the comparison $A[i] > key$ has been replaced
by $A[i] < key$.

### 2.1-3

Consider the **searching problem**:

**Input:** A sequence of $n$ numbers $A = \langle a_1, a_2, ..., a_n \rangle$ and a value $v$.

**Output:** An index i such that $v = A[i]$ or the special value $NIL$ if $v$ does
not appear in $A$.

Write pseudocode for **linear search**, which scans through the sequence, looking
for $v$, Using a loop invariant, prove that your algorithm is correct. Make sure
that your loop invariant fulfills the three necessary properties.

<hr>

<div class="code">
LINEAR-SEARCH($A$, $v$)<br>
1&emsp;$j \leftarrow 1$<br>
2&emsp;<b>while</b> $j \leq length(A)$<br>
3&emsp;&emsp;<b>do if</b> $A[j] = v$<br>
4&emsp;&emsp;&emsp;&emsp;<b>then return</b> $j$<br>
5&emsp;&emsp;&emsp;$j \leftarrow j + 1$<br>
6&emsp;<b>return</b> $NIL$<br>
</div>

<br>

#### Proof of correctness of LINEAR-SEARCH

**Invariant:** At the beginning of each iteration of the loop at line 3, the subarray
$A[1 .. j - 1]$ does not contain $v$.

**Initialization:** Before the first iteration $j = 1$, so the subarray $A[1 .. j - 1]$
is the empty array. So the invariant holds, since $v$ does appear in the empty array.

**Maintenance:** The loop scans the array from left to right checking at every
index $j$ if $v$ appears at that position (line 3). When that happens, the position
$j$ is returned. If $v$ is not in position $j$ we have that $v$ is not in the subarray
$A[1 .. j].

**Termination:** By the end of the loop, $j = length(A) + 1$, so replacing this in the
invariant expression, we have that $v$ has not appeared in the subarray $A[1 .. length(A)] = A$.
Otherwise, $v$ has been found and the algorithm terminated before the end of the loop. $\blacksquare$

### 2.1-4

Consider the problem of adding two $n$-bit binary integers, stored in two $n$-element
arrays $A$ and $B$. The sum of the two integers should be stored in binary form in
an $(n + 1)$-element array $C$. State the problem formally and write pseudocode for
adding the two integers.

<hr>

**Input:** Two $n$-element arrays $A = \langle a_1, a_2, ..., a_n \rangle$ and
$B = \langle b_1, b_2, ..., b_n \rangle$, with $a_i, b_i \in \lbrace 0, 1 \rbrace$, representing
two binary integers $a = a_n \times 2^{n-1} + a_ {n-1} \times 2^{n-2} + \dots + a_1 \times 2^0$
and $b = b_n \times 2^{n-1} + b_ {n-1} \times 2^{n-2} + \dots + b_1 \times 2^0$ respectively.

**Output:** An $(n+1)$-element array $C = \langle c_1, c_2, ..., c_ {n + 1} \rangle$
representing the number $c = a + b$.

#### Full adder circuit solution

The algorithm bellow uses what is know as the full adder circuit for adding bits.
It takes as input three bits $a_i, b_i$ and $c'$ (also know as carry) and calculates
the $c_i$ (sum) and the $c''$ (carry-out).

<figure>
  <img width="100%" src="/images/clrs/ch2/exercise-2_1-4-full-adder-circuit.png" alt="Full adder circuit diagram">
  <figcaption>
    Full adder circuit
  </figcaption>
</figure>

<table width="50%" border="1">
  <caption>Truth table for the full adder circuit</caption>
  <tr>
    <th>$a_i$</th>
    <th>$b_i$</th>
    <th>$c'$</th>
    <th>$c_i$</th>
    <th>$c''$</th>
  </tr>
  <tr>
    <td>0</td>
    <td>0</td>
    <td>1</td>
    <td>0</td>
    <td>0</td>
  </tr>
  <tr>
    <td>0</td>
    <td>0</td>
    <td>1</td>
    <td>1</td>
    <td>0</td>
  </tr>
  <tr>
    <td>0</td>
    <td>1</td>
    <td>0</td>
    <td>1</td>
    <td>0</td>
  </tr>
  <tr>
    <td>0</td>
    <td>1</td>
    <td>1</td>
    <td>0</td>
    <td>1</td>
  </tr>
  <tr>
    <td>1</td>
    <td>0</td>
    <td>0</td>
    <td>1</td>
    <td>0</td>
  </tr>
  <tr>
    <td>1</td>
    <td>0</td>
    <td>1</td>
    <td>0</td>
    <td>1</td>
  </tr>
  <tr>
    <td>1</td>
    <td>1</td>
    <td>0</td>
    <td>0</td>
    <td>1</td>
  </tr>
  <tr>
    <td>1</td>
    <td>1</td>
    <td>1</td>
    <td>1</td>
    <td>1</td>
  </tr>
</table>

<br>

<div class="code">
BINARY-SUM(A, B, C)<br>
1&emsp;$c' \leftarrow 0$<br>
2&emsp;<b>for</b> $i \leftarrow 1$ <b>to</b> $length(A)$<br>
3&emsp;&emsp;<b>do</b> $d \leftarrow A[i] \oplus B[i]$<br>
4&emsp;&emsp;&emsp;$C[i] \leftarrow d \oplus c'$<br>
5&emsp;&emsp;&emsp;$c' \leftarrow (d \land c') \lor (A[i] \land B[i])$<br>
6&emsp;$C[i] \leftarrow c'$<br>
</div>


## Exercises 2.2

### 2.2-1

Express the function $\frac{n^3}{1000} - 100 n^2 - 100n + 3$ in terms of $\Theta$-notation.

<hr>

After removing the constants and lower order terms, what's left is $\Theta (n^3)$.

### 2.2-2

Consider sorting $n$ numbers stored in array $A$ by first finding the smallest element
of $A$ and exchanging it with the element in $A[1]$. Then find the second smallest
element of $A$, and exchange it with $A[2]$. Continue in this manner for the first $n - 1$
elements of $A$. Write pseudocode for this algorithm, which is know as *selection sort*.
What loop invariant does this algorithm maintain? Why does it need to run for only the
first $n - 1$ elements, rather than for all $n$ elements? Give the best-case and worst-
case runnint times of selection sort in $\Theta$-notation.

<hr>

<div class="code">
SELECTION-SORT(A)<br>
1&emsp;<b>for</b> $i \leftarrow 1$ <b>to</b> $length[A] - 1$<br>
2&emsp;&emsp;<b>do</b> $min \leftarrow i$<br>
3&emsp;&emsp;&emsp;<b>for</b> $j \leftarrow i + 1$ <b>to</b> $length[A]$<br>
4&emsp;&emsp;&emsp;&emsp;<b>do if</b> $A[j] < A[min]$<br>
5&emsp;&emsp;&emsp;&emsp;&emsp;<b>then</b> $min \leftarrow j$<br>
6&emsp;&emsp;&emsp;$\rhd$ swap $A[i]$ and $A[min]$<br>
</div>

<br>

#### Proof of correctness

Before proving the correctness of the algorithm, its convenient to prove the following.

**Lemma:** the inner loop from lines $2 - 5$ finds the index of the smallest element in the
subarray $A[i .. length[A]]$.

**Invariant:** at the start of the loop at line $3$, the variable $min$ holds the index of
the smallest element in the subarray $A[i .. j - 1]$.
**Initialization:** it's trivial, since $min = i$ which is the index of the smallest element in
the subarray $A[i .. (i + 1) - 1] = A[i]$.
**Maintenance:** since $min$ is the index of the smallest element in $A[i .. j - 1]$,
if the next element to check $A[j]$ is smaller than $A[min]$ (line 4), we may assing $j$ to
$min$ (line 5), and $min$ would hold the index of smallest element in the subarray $A[i .. j]$.
**Termination:** as $j$ is incremented to $length[A]$, by the end of the loop, we have
that $min$ is the index of the smallest elment in $A[i .. length[A]]$.

<br>

Back to the main proof at the start of the iteration of the outer loop $1 - 6$, the
subarray $A[1 .. i - 1]$ contains the $i$ smallest elements from $A$ in sorted order.

**Initialization:** Before the first run, $i = 0$ and the the subarray $A[1 .. i - 1]$
is the empty array which is obviously in sorted order.

**Maintenace:** Given the subarray $A[1 .. i - 1]$ contains the $i - 1$ smallest elements
from $A$ in sorted order and the inner loop in lines $2 - 5$ finds the smallest element
remaining in the subarray $A[i .. length[A]]$, once we swap it with $A[i]$, line 6, we have
that the subarray $A[1 .. i]$ is in sorted order and contain the smallest $i$ elements from $A$.

**Termination:** After running from $i = 1$ to $lenght[A] - 1$, we have that $A[1 .. length[A] - 1]$
is in sorted order and contain the smallest $lenght[A] - 1$ elements of $A$. So the remaining
element $A[lenght[A]]$ is the largest element in $A$ and the array is in sorted order. $\blacksquare$

<br>

Thats why we do not need to check the last element of $A$. Its guaranteed to be the
larget element of the array and will be in its final position.

<br>

#### Performance analysis

Let $t_i$ be the number of times the check at line 3 is run for every $i$. For $i = 1$,
$t_i = (n + 1) - (i + 1) = n - i$, e.g. from $i + 1$ to $n + 1$, since an extra check is made for
each loop header than the body.

<table class="performance">
  <tr>
    <td>
      SELECTION-SORT(A)
    </td>
    <td>
      $cost$
    </td>
    <td>
      $times$
    </td>
  </tr>
  <tr>
    <td>
      1&emsp;<b>for</b> $i \leftarrow 1$ <b>to</b> $length[A] - 1$
    </td>
    <td>
      $c_1$
    </td>
    <td>
      $n - 1$
    </td>
  </tr>
  <tr>
    <td>
      2&emsp;&emsp;<b>do</b> $min \leftarrow i$
    </td>
    <td>
      $c_2$
    </td>
    <td>
      $n - 1$
    </td>
  </tr>
  <tr>
    <td>
      3&emsp;&emsp;&emsp;<b>for</b> $j \leftarrow i + 1$ <b>to</b> $length[A]$
    </td>
    <td>
      $c_3$
    </td>
    <td>
	$\sum_{i = 1}^{n - 1}{t_i}$
    </td>
  </tr>
  <tr>
    <td>
      4&emsp;&emsp;&emsp;&emsp;<b>do if</b> $A[j] < A[min]$
    </td>
    <td>
      $c_4$
    </td>
    <td>
      $\sum_{i = 1}^{n - 1}{t_i - 1}$
    </td>
  </tr>
  <tr>
    <td>
      5&emsp;&emsp;&emsp;&emsp;&emsp;<b>then</b> $min \leftarrow j$
    </td>
    <td>
      $c_5$
    </td>
    <td>
      $t_j$
    </td>
  </tr>
  <tr>
    <td>
      6&emsp;&emsp;&emsp;$\rhd$ swap $A[i]$ and $A[min]$
    </td>
    <td>
      $c_6$
    </td>
    <td>
      $n - 1$
    </td>
  </tr>
</table>

The value for $t_j$ is, in the best case, zero, for when $A[min]$ is the smallest in
$A[i .. length[A]]$. But the worst case is equal to the line 4 above it.

TODO: $T(n)$, but I know it's $\Theta(n^2)$ in the best or worse case.

### 2.2-4

How can we modify almost any algorithm to have a best-case running time?

<hr>

By pre-calculating, or *caching*, the correct output for a given input. This special
input would cost at most a linear time to calculate, time required to match the given
input to that from the precalculated one.

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}
});
</script>

<script type="text/javascript"
  src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
