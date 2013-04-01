---
layout: post
title: "Mesa e Cadeira with Rafael Rozendaal"
description: "So you think you can do it?"
category: 
tags: []
---
{% include JB/setup %}

<aside class="pull-quote">
  <blockquote>
    <p>To create - individually or collectively - browser compositions that explore the arrangement of elements in time and space.</p>
  </blockquote>
</aside>

That's what [Rafael Rozendaal](http://www.newrafael.com), leader of the 8th edition of [Mesa & Cadeira](http://www.mesaecadeira.org/en/) workshop, challenged us to do.


What the fuck! I haven't felt that way since I first read [Macunaíma](http://en.wikipedia.org/wiki/Macuna%C3%ADma_%28novel%29).

But wait! That's the kind of thing that puts your brain to work.

Let's sketch!

My first one was like this.

<figure>
  <img width="75%" src="/images/snake.png" alt="Snake game revamped sketch">
  <figcaption>
    A snake game with many simultaneous canvases.
  </figcaption>
</figure>

 So the "same" snake would look different depending on the frame you put it in. 

 That may look like a shit idea. But as I've learned with the guys from [It's Nice That](http://www.itsnicethat.com), Alex Bec and Will Hudson, when it comes to brainstorming,

<aside class="pull-quote">
  <blockquote>
    <p>Don't shit in the pool</p>
  </blockquote>
</aside>

Because if you do so, everybody else is gonna have to get out of it. It's really important to make people feel safe, so the ideas keep flowing.

Next day two winning ideas were chosen by voting. That's when the [iwannabealone.com](http://www.iwannabealone.com) insanity started.

<aside class="pull-quote">
  <blockquote>
    <p>A website for people craving to stay out of the crowd.</p>
  </blockquote>
</aside>

### The inception

Exploring the idea of composition for different screen sizes, we decided that we would build a website where each user arrival would change website's appearance to reflect the number of watchers.

<figure>
  <img src="/images/idea.png" alt="First idea sketch">
  <figcaption>
    First idea sketch. Starting on the top left with one user.
  </figcaption>
</figure>

That's very raw. From that we jumped to something more visually interesting, like this

<figure>
  <img src="/images/trying_to_visualize_the_idea.png" alt="Visualizing the idea on a canvas.">
  <figcaption>
    Figuring out how it would looks like at the end.
  </figcaption>
</figure>

But it is still just one way of splitting the screen into N parts. It looked **ok** by the moment. Soon we would discover it was **not**.

### Adding interaction

Then we decided to add some hot sauce. Following Rafael's [Finger Battle](http://www.fingerbattle.us) game, we thought that it would be nice if people fought to be alone, having to kick each others asses out.

That required the game to have some sort of click/tap interaction, what leads to a whole new number of problems to be solved.

### Problems (or we thought they were)

* how to split the screen into N, from 1 to 20? How does it looks like when we have 2? And 3? And 4, 5, 6… How does the transition looks like?

We were thinking that our last sketch was enough, but to split a screen into 5 pieces with equal area does not have a single solution. Some sketches later we arrived at the following stage

<figure>
  <img src="/images/tiling.png" alt="Tilling strategies">
  <figcaption>
    Some of the possible strategies to split the screen.
  </figcaption>
</figure>

The first is plain boring. Through it away!
Can you explain the second algorithmically?
The third one is just perfect. Like slicing a squared pizza! (actually it required some thought. You can see the [code](https://github.com/rcillo/iwannabealone). It's actually simple: base times height divided by two...

Rafael showed up in the middle of the discussion and throught this [Voronoi](http://en.wikipedia.org/wiki/Voronoi_diagram) stuff on us. Are you kiding?

* What's the maximum number of concurrent players?

It doesn't metter! We lost some precious time discussing it. Should we create rooms when more than 20 users enter at the same time? Should there be a waiting line? I had never seem more than 10 users logged in at the same time.

* What's the minimum area comfortable to tap on mobile devices?

Like the last question, this problem really exists. But it's a latent problem. It's going to be there sleeping like a beast until awaken by the noise of the crowd, which may **never** happen.

* Is it technologically viable?

Yes. 

* Is network going to be a problem?

Probably. But after we built it, I've already played it on the aweful brazilian 3G. Looking back, discussing networking limitations was ridiculous.

### Fake problems

Some problems are just distractions. They seem very plausible. Few people would disagree they exist.

But should we **care** about then?

These kind os problems drain energy and make people loose their time and mind over useless minutiae.

Simplicity. It's the hardest achieving ideal.

### A metaphor

I really cannot see the future. 

Yeah… you may be thinking it's obvious, right?

So you are also thinking it's impossible to visualize the final product of an idea, obviously.

That's the point. An idea is like a mirage. You are in the desert. You are really thirst. You look at the horizon and under the hottest sun on earth your mind makes you see a oasis. Water is just over there. Then you start to get close and realize it's farther than you thought. So you walk desperately to get to it. When you get there. You find a fallen aircraft. That might not be water but it may still be useful, you know? So you can keep yourself alive.

Being humble about what to expect is vital when it comes to software products. There will probably be no oasis.

### The real problems

- What is this colored pizza?
- How does someone discovers it's interactive? (touch devices have no click cursor)
- Why it's moving? (recently arrived user trying to figure out what's going on)
- Let's click on the different slice written YOU! (killing himself)
- I've tapped till the other colors disappeared. So what? (winners cannot see that the opponent has being kicked out with a nice game over message).
- Why sometimes I start the game with less than 50% percent of the screen? (confusing game scoring logic)

Original Finger Battle game had two fingers illustrated on the splash screen. That says more than it looks like. The call for action button "play" too. Being able to see the other person tapping the blue and see the immediate feedback of loosing area also helps to understand the game dinamics. 

Borrowing ideas is harder than it seems. 

Context is really important for comprehension.

### Bugs

Should every bug be fixed? I really think the answer is **no**. Here's the list of fixed and [deliberately] unfixed ones.

- _fixed:_ My color changes when a new user comes to the game.
- _ignored:_ The 'YOU' word hides partially sometimes.
- _ignored:_ Sometimes 2 users with the same color share the screen making it looks like there is just one (very rare).
- _unfixed:_ After winning, the word 'You' does not change to 'You are alone.'. Would be nice to fix, but... read [this](http://en.wikipedia.org/wiki/Wabi-sabi).
- _easter egg:_ If I kill myself I do not loose the game and can stay there forever like if I had incarnated into another color.

### The final product

<figure>
  <img src="/images/you_are_alone.png" alt="You are alone">
  <figcaption>
    You are alone screen.
  </figcaption>
</figure>

<figure>
  <img src="/images/example.png" alt="Game dynamics">
  <figcaption>
    Game dynamics example with 4 players.
  </figcaption>
</figure>

<figure>
  <img src="/images/game_over.png" alt="Game over">
  <figcaption>
    That's it.
  </figcaption>
</figure>

### Conclusion

Understanding the real problems is hard. Simplicity is the ability to answer to them first.

Being there already doesn't solve the problem, only make you more aware of it.