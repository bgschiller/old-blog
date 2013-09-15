---
layout: post
title: Putnam Boxes and Balls
description: "Every December, undergrads across the country take the Putnam Exam, regarded as one of the toughest math competitions around. This is a walkthrough of a problem from the 2010 test."
category: blog
tags: [math, putnam, teaching]
image:
  feature: texture-feature-02.jpg
  thumb: all-the-balls2.jpg
putnam_boxes_head: true
---

School starts soon at my alma mater, [WWU](http://www.wwu.edu/). For me, Fall quarter always meant I would be taking the Putnam Exam, regarded as one of the most difficult (or _the_ most difficult) math competitions for undergraduates. It consists of two three-hour sessions with six problems each. Most years, the median score on the exam is zero out of 120 points.

The first year I sat the exam, in 2010, this was one of the problems:

> There are 2010 boxes labeled \\(B\_{1}, B\_{2}, \ldots, B\_{2010}\\) and \\(2010n\\) balls have been distributed among them, for some positive integer \\(n\\). You may redistribute the balls by a sequence of moves, each of which consists of choosing an \\(i\\) and moving _exactly_ \\(i\\) balls from box \\(B\_i\\) into any one other box. For which values of \\(n\\) is it possible to reach the distribution with exactly \\(n\\) balls in each box, regardless of the initial distribution of balls?


### Break it down
Whew, what a mouthful! Let's break that into pieces.

> There are 2010 boxes labeled \\(B\_{1}, B\_{2}, \ldots, B\_{2010}\\)

So far, so good.

> ... and \\(2010n\\) balls have been distributed among them, for some positive integer \\(n\\).

Okay, so if \\(n\\) is 2, for example, there are 4020 boxes. And all the balls are in some box or other.

<figure>
    <img src="{{ site.url }}/images/all-the-balls2.png">
    <figcaption>"My game's got a basketball, a football, a kickball, and a tetherball: it's got <em>all</em> the balls. So I call it 'All The Balls'. You get it?" -Lawson</figcaption>
</figure>

> You may redistribute the balls by a sequence of moves,

That sounds good to me. What are the moves?

> each of which consists of choosing an \\(i\\) and moving _exactly_ \\(i\\) balls from box \\(B\_i\\) into any one other box.

So I could take 3 balls out of box \\(B\_3\\) and put them in box \\(B\_5\\). But if I wanted to take balls out of box \\(B\_5\\), I'd have to take them out 5 at a time. And I can do this as many times as I want.

> For which values of \\(n\\) is it possible to reach the distribution with exactly \\(n\\) balls in each box,

We're looking to divide the \\(2010n\\) balls evenly among all the boxes, but maybe it doesn't work for every \\(n\\)?

> regardless of the initial distribution of balls?

Okay, so my worst enemy could be stacking the deck against me, setting things up so in the worst way they know how, and I still have to be able to evenly distribute the balls.

### A smaller version of the same problem

It's a little hard to visualize 2010 boxes, so lets go with something smaller. How about four? 

<figure>
    <img src="{{ site.url }}/images/boxes/fourboxes.jpg">
    <figcaption>Four is _definitely_ smaller than 2010</figcaption>
</figcaption>

So now we're trying to distribute \\(4n\\) balls evenly among the boxes.

<div id="four-boxes-game">
    <label for="ball_multiplier">
      Ball Multiplier:
      <input type="text" id="ball_multiplier" size="2" value="2"/>
    </label>
    &nbsp;
    &nbsp;
    <label for="num_boxes">
      Number of boxes:
      <input type="text" id="num_boxes" size="2" value="4"/>
    </label>
    &nbsp;
    &nbsp;
    <button onclick="init();">New Game</button>
    <div class="wrapper" id="boxes-container"></div>
    <div id="successMessage">
        <h2>You did it!</h2>
        <button onclick="init()">Play Again</button>
    </div>
</div>
