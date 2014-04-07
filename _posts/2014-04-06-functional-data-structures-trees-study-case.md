---
layout: post
title: "Functional data structures: trees study case"
description: ""
category:
tags: []
---
{% include JB/setup %}

Carefully crafted data structures are at the heart of most software we rely on: databases, web frameworks, operational systems, games and so forth. Thats because they are a great way to encapsulate the complexity of how to store and retrieve data, providing developers with tools to write simple yet fast and memory efficient software.

Among a myriad of data structures (hashes, heaps, queues, stacks and graphs, to name a few), I guess we can agree that trees are ubiquitous. They have been the focus of research for decades, which resulted in a well understood field, with tons of papers and blasting fast implementations in many languages.

Providing yet another in depth comparison between a reasonable number of tree implementations is beyond the scope of this article. Instead of that, we are going to highlight some special properties that arise from immutability and lack of constant time random access, core features of functional languages.

For those who would like to dive deeper in the subject, a substantial list of articles is present in the bibliography section.

## Data structures in the functional land

Since imperative languages had been the status quo for the last decades, a huge effort has gone into providing algorithms and data structure that performs efficiently when implemented by those languages.

Back in college time, most students learned algorithms and data structures in languages like C or Java. Not everyone was comfortable with all those pointers. Me included. My personal experience is that concept was far easier to grasp than implementations. But the history has changed after I started reading Chris Okazaki [link] book on purely functional data structures. Funcional languages like Haskell and Elixir allow for clearer implementations. In fact, I’m so excited about it, that I wrote this essay.

The old ideas are still the same, but the language features are different, which imposes a new challenge. A delightful one.

Some may say that “C is faster” and I agree with that. But wasn’t compiling a waste of computing time? All that we need is binary instructions, isn’t it? I share Bret Victor opinion [link] http://vimeo.com/71278954 regarding this subject.

## Persistence and sharing: immutability’s twins

Suppose we need to insert a value at middle of an array, we just go straight ahead and `array[i] = value`, update the array at the index position (usually `i`, `j`, `k` or all of the alphabet letters together).

We don’t have that in functional languages. Nor the random access nor the “destructive” updates. We have lists and `cons`. This is how an equivalent implementation, a dummy one, would looks like in Elixir:

$code$

This function copies every element until the position where we add the new one and point to the old list. The fact that the old list is untouched means that at every update we get a new list and preserve the old one. This property is common to every functional data structure and receives the name of *persistence*.

Since we never update a value in a list, we always build a new one, we can safely point to parts of the old list, sharing its elements. This is called *sharing* and is another nice property.

$diagram$

But it doesn’t looks like a cleaver idea to copy lots of elements just to update a single one. This may potentially require a full copy of the list in the worst case. How do we solve that?

## Indexed arrays using binary trees

As you were expecting, trees are one of the answers to this problem. Let fill tree nodes with indexes and we achieve logarithmic time insertions, searches, removals and updates.

They do not require a full copy to just update an element from a list. You can see a very simple binary tree implementation in $code$

Insertions on a binary tree copies only the elements in the path to the new node.

$diagram$

But its a know fact that non balanced binary trees tends to degrade to linear time after many insertions, specially if the order of insertion is always ascending, as we can see bellow:

$diagram$

Many solutions to this problem had been presented. I remember at least three from college time. Let’s remember then.

## Balanced trees

Most balancing techniques use sophisticated criteria to decide when to rearrange nodes in order to get to keep logarithmic tree height. These balancing strategies come in many flavors	and colors, literally.

So, in a friday afternoon, after office time, @carlosgaldino, @fabioyamate and I implemented a 2-3-tree ($code$). It took us 4 hours to remember how that works and code it. No way we would achieve that with C code. Patterns matching provides a clear way to define scenarios. It helps focusing on the task at hand and break a big problem into smaller and simpler ones.

This is how a 2-3-tree insertion works.

$diagram$

## Performance comparison

Serious performance analysis are hard to do. Some articles claim that synthetic datasets do not lead to realistic tests. But, after so much work, we would like to take a look at how our implementation compares to a dummy one and a professional one (link gb_trees).

$insertions$

$searches$

## Conclusion

Relearning things from college in a new setup is a joyful experience. Even the shameful defeat for Erlang trees. I learned something: being paranoid about balancing is a bad choice. Generally balanced trees are more relaxed and only run balancing operations where and when needed. That’s not a matter of hard work. Thats about doing the right work in the right time. Thats a good analogy to life.

## Next steps

I would like to implement AVL-trees, B-trees and a special one called RRB-tree which provides logarithmic merges between trees. You may find then by watching this repo. Let’s plant some trees.
