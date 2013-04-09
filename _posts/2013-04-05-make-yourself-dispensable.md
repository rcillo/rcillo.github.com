---
layout: post
title: "Make yourself dispensable"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Looking back a year ago I remember to have no idea that thing would come to this point. Who could imagine that I would get to feel the taste of managing just to dissolve my place after noticing that things would be better if I just let things happen. In certain ways, I just made myself dispensable and I feel great about that!

### Evolving a development culture

By january 2012 I was hired, as a developer, by [Airu](http://www.airu.com.br/), a marketplace startup, based in São Paulo.

What a chaos I found there! I started working on a noisy room, near a bathroom door, assembled my own chair and setup my programming environment on a temporary pc. Messy but functional. That's what I was looking for.

But like everything else, the code looked just the same.

You may know where we get from there, don't you? High error rates, dev time drained by bug fixes, fear of breaking things, big bang deploys which sums up to compromised team speed and mood.

But the worst part: it worked! How are you supposed to argue with non technical executives about the importance of good practices if it isn't hurting them yet?

Argumentation, for sure. I've chosen mainly authority args extracted from books <sup><a href="#fn-item1" id="fn-return1">1</a></sup>. But it would had been much easier if there were some serious work on the comparison of team speed, with just a simple plot like the one bellow.

<figure>
  <img width="75%" src="/images/productivity_vs_time.png" alt="Productivity Vs Time">
  <figcaption>
    Blue line represents a team adopting <a href="http://www.extremeprogramming.org/">XP</a> with some good practices. The red line a team running <a href="http://helio.loureiro.eng.br/index.php/pessoal/39-blog/240-extreme-go-horse-xgh">XGH</a>.
  </figcaption>
</figure>

But I'm sure it would be hard to tell when the crossing threshould would happen. Startups aren't sure about their near future. They keep thinking that maybe they should run as fast as they can just to test and hypothesis and that this would happen before the threashold. But honestly, if your project is going to last for more than a month, **don't screw up**.

So we started to add some basic XP practices, like unit testing, and kept applying more complicated ones <sup><a href="#fn-item2" id="fn-return2">2</a></sup> untill we gained more control over the process. But we quite didn't knew how to write tests. That's when we started doing [coding dojos](http://codingdojo.org/cgi-bin/wiki.pl?WhatIsCodingDojo).

But the turning point was a talk we had about being **brave**. No one can tell how to do your job. You are totally accountable for it. For good and bad things. You have to take the responsability and do what you beleive is right. That's when things start to happen.

### Broadening sight

Delivering features with quality and speed is a limited way of improving the business. Improving development metrics doesn't imply in overall business metrics improvement. Influenced by an [article](http://xrds.acm.org/article.cfm?aid=1961679) from the XRDS magazine, about how Google managed to build efficient data centers, I started to get interested in the business as a whole.

<figure>
  <img width="75%" src="/images/hot_spot.png" alt="The hot spot">
  <figcaption>
    Awesome things happen when business, product and technology come together.
  </figcaption>
</figure>

Business knowledge went as far as the product management layer. The motivation for the features wasn't very clear for the devs. This created a problem: tasks had to be extensively detailed because developers couldn't help to solve the problem because they didn't know what the problem was. They were seeing the interaction with the dev team as a client-server structure. The product manager was responsible for telling then what to do and they delivered. That created a bottleneck.

That started to change when we asked for more transparency. So the CEO started to report business updates monthly so the dev team started to figure out where we were going and what we could do to help.

### Becoming the PM

Our product manager left the company and the responsibility was offered to me, for my previous declared interest on the business and product areas.

The same night that I accepted it, I didn't slept well. Now I felt the weight of the responsibility for making the right choices and not let people waste their time developing things that would not help the company reach its objective of becoming a sustainable growing startup.

There were no clear job description, differently from being a developer. With only a vague idea of what I should do, I suddenly became anxious.

### The analytical framework

I had no previous experience doing this kind of management job. Founding my opinions on data would make it more objective and less susceptible to subjective judgements.

That reasoning led me through a path of an analytical product design opposed to a more psychological-behavioral guessing strategy.

I organized my job around the following workflow:

1. define key performance indicators
2. identify the features that contribute to the indicators
3. pick the most underperforming contributor
4. develop an hypothesis about why it's underperforming
5. test the hypothesis using A/B testing
6. iterate over hypothesis until the performance has improved
7. back to the step 1

This approach helped us to **double** the conversion rate. There were lots of things to optimize.

### A linear programming analogy

Like a [linear programming problem](http://en.wikipedia.org/wiki/Linear_programming) there are some constraints when choosing the optimization path.

<figure>
  <img width="75%" src="/images/pl.png" alt="Linear programming analogy">
  <figcaption>
    Green path leads to the optimal point F on 5 steps. Red path on 5 also. But green path gets closer to F more slower at the beginning and red more slower at the end. Points are analogous to features or bug fixes. The yellow path represents a shortcut by not having to fix bugs. 
  </figcaption>
</figure>

Relaxing the constraints allowed us to improve performance faster. The most important constraint that had been ignored is the "fix all bugs". I started to deliberately not fixing banal bugs. What a relief!

Following the most increasing step may lead to a path with a higher number of iterations to reach the optimal solution.

But like the simplex algorithm, decisions are half informed. You can only choose the next step (or two or three, rarely) on this framework.

The hardest part is deciding what parts are underperforming (lack of benchmarks) and how much can it be improved.

Changing the business strategy, changes the constraint and the optimal value. That was not my main concern, but I always asked myself if couldn't we tweak the business side to make the problem more interesting.

### Dissolving the PM rule

Defining a framework to drive product decisions automatized part of my job. Made it more feasible to anyone in the project with google analytics access (all the developers, at least).

With a backlog full of prioritized problems, we had lots to be done.

During the planning meetings I started to notice the development team had lots of ideas on how to solve the problems. Of course, they were being informed about them! That led to another part of my job being deprecated: specifications.

By the end, I delegated and automated the rule of the PM. Almost completely. My main task now was to analyze and inform people. Eventually simplifying developers suggestions (sometimes they think of the best solution, which sometimes are not required, but only because they take decisions on a sprint wide timespan, not because they tend to be wrong).

### A career perspective

Being a developer for the last 7 years made me a reasonable good developer. Becoming a manager started a 6 month experience career branch. I would compete with guys that had 7 years of experience with management if I decided to change my job. I'm not a manager. I only wanted to make everybody efforts more valuable working smarter. I hate political relations on the workplace. The closest to the business layer you get, more political it gets.

I really think that the plain old developer rule is outdated and that companies only loose making their employees alienated.

But I did not wanted to change my carreer.

My plan is to become awesome, inpired by [Jiro](http://www.imdb.com/title/tt1772925/), on what I'm already quite skilled at. So I'm goint to work with the guys from [Plataformatec](http://plataformatec.com.br/).

### Conclusion

Building a smart company is hard. You need to be brave and make yourself accountable. Trusting people is always the better way to go. Working only with awesome people is the key. Making yourself dispensable is a sign that you've done your job well. Not because you were useless, but because you made things smarter by getting out of the way.

### Things I would like to see

I really hope my friends are going to tackle down the technical debt and post an awesome article on how they did it.

I would like to see a 100% customer satisfaction rate.

Would like to see the product development receiving 10x more money them the marketing.

### Acknowledgments

I would like to thank everyone that worked with me this last year. You are among the greatest people I've got to know in my life. It was a pleasure, besides being very [fun](http://grooveshark.com/s/Jobi+Joba/3mjOW9?src=5).

Really would like to work with you again.

<footer>
  <ol class="foot-notes">
    <li id="fn-item1">The main reference: Extreme Programming Explained: Embrace Change, 2nd Edition, by Kent Beck.<a href="#fn-return1">↩</a></li>
    <li id="fn-item2">Automatic deploys, live deployment, refactoring, one week sprints, better communication.<a href="#fn-return2">↩</a></li>
  </ol>
</footer>