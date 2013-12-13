---
layout: post
title: pthreads and PRNGs, oh my!
description: "Professors teaching Concurrency classes are fond of saying that when there is a race condition in your code, anything can happen. But can it?"
category: blog
tags: [programming, concurrency, prng]
image:
  feature: chinatown_thread.jpg
---

We learn in concurrency class that if you have a race condition in your code, the results are unpredictable. Certainly, the results are not usually what you wanted, but does that make them unpredictable?

For those who haven't taken a concurrent programming class, race conditions most often occur when two or more pieces of code (usually called threads) are running at the same time, and the behaviour of the program will change depending on which thread gets to a milestone first.

For example, you might have two threads, `hp`, and `ij`. They're both frantically setting the value of a variable `winner` to be different values (note that this is not real code in any language I know of):

```python
winner = ""

thread hp:
    while winner_is_unannounced:
        winner = "Harry Potter"

threap ij:
    while winner_is_unannounced:
        winner = "Indiana Jones"

...wait a moment (let them fight over it)...
print "The winner is:", winner
```

Depending on which thread most recently set the value of `winner`, a different name is printed out. Either one is possible.

<figure>
    <img src="{{ site.url }}/images/dumbledore_gof.gif">
    <figcaption>“Did you put your name into the Goblet of Fire, Harry?” he asked calmly.</figcaption>
</figure>

You might be thinking that whether Harry or Dr. Jones ends up winning would be random. Well, you wouldn't be alone. Austin Amoruso (who I know from WWU and who is currently a developer at [SnapBi.com](http://www.snapbi.com/)) posted the following on facebook:

<figure>
    <img src="{{ site.url }}/images/pthreads_prng_austin_post.png">
</figure>

Concurrency teacher and textbooks will tell us that programs with race conditions are unpredictable. So if we make a program with a race condition, can we use it as a pseudorandom number generator (PRNG). Before we can answer that, I think we need a better definition for unpredictable.

## What is unpredictable?

On [wikipedia](http://en.wikipedia.org/wiki/Pseudorandom_number_generator), you can read about a whole host of criteria for deciding if a PRNG is 'good enough'. For PRNGs that are used for cryptography, there is a requirement that "an adversary has only negligible advantage in distinguishing the generator's output sequence from a random sequence".

That means that no adversary -- a program that, given the earlier bits of a sequence, tries to predict the next one -- should do better than a random guesser. 

<figure>
    <img src="{{ site.url }}/images/xkcd_1210_im_so_random.png">
    <figcaption>Source: <a href="http://xkcd.com/1210/">http://xkcd.com/1210/</a></figcaption>
</figure>

We'll use a more relaxed criteria today. I want to see if the adversary from [this page](http://www.loper-os.org/bad-at-entropy/manmach.html) can do better than expected. 

That website, [Man vs. Machine, or, why Man is not a Particularly Good Source of Entropy](http://www.loper-os.org/bad-at-entropy/manmach.html), pits your own "random" bit generation against a fairly simple algorithm that tries to guess your next move. 

The website gives the machine the benefit that it can decide to pass, and forgo guessing for a turn without penalty. That doesn't seem fair to me, so I've made the machine instead flip a (metaphorical) coin when it would otherwise pass, effectively forcing it to make a guess.


## Methods

In order to give the PRNG a fighting chance, I added a sleep of 0.01 seconds between subsequent drawings of the bits. That is, the PRNG code examines the contested value, prints it out and then waits for 0.01 seconds before starting again. 

Meanwhile, the adversary code is reading each bit and making a prediction about the next one. After reading the last bit, it reports the proportion that it guessed correctly.

I performed 100 trials of 100 bits each. Each trial consisted of setting up the PRNG to produce 100 bits, allowing the adversary code to make its guesses, and then storing only the proportion correct.

Finally, to determine whether the adversary has "a non-negligible advantage", we use a one-sample t-test against the hypothesis that the distribution of those 100 proportions has a true mean of 0.5.

If I lost you in that last paragraph, the idea is just that we are going to do some stats to decide if the proportions correct are enough higher than 0.5 to say that the adversary has an advantage over a random guesser. 

## Results

As you may have guessed, we have statistical significance! In fact, \\(p \\approx 0.000489\\), which is *way* lower than the usual 0.05 needed to qualify as significant. This \\(p\\) measures the likelihood that, in fact, the adversay has no advantage and it only did better by chance. Since we have such a low \\(p\\)-value, it's very unlikely that the adversary has no advantage.

However, the advantage is pretty small. The mean of the proportions comes out to only \\(0.5213\\), just \\(0.0213\\) better than a random guesser.

This doesn't mean that our PRNG is close to random -- just that maybe we didn't choose the best adversary for the job.

## Challenger Approaching

After looking at the output of the PRNG, I came up with a much simpler adversary. Here's how it goes: always guess whatever bit was most recently seen.

There are a lot of 'runs' in the bit sequences from this PRNG. For example, here is a sequence of 40 bits:

```
0 1 0 1 1 1 0 0 1 1 1 1 1 0 0 0 1 1 0 1 1 1 1 1 0 0 1 1 1 1 0 0 1 1 1 1 1 1 1 1 
``` 

Just from looking, there is more repetition than we would expect. This new adversary takes advantage of this fact, to pretty good results.

Our \\(p\\)-value for this adversary is embarrassingly low at \\( 0.00000000000000000000006208 \\). *Very* significant, then. On average, this new adversary gets 58.77% of its guesses correct, 8.77% better than a random guesser.

The code is available as a [GitHub Gist](https://gist.github.com/bgschiller/7939613). Please give it a run if you'd like. Maybe the PRNG performs better on a computer with lots of cores (I only have two). Maybe it does better if you increase the sleep time. I'm excited to hear what you find out!


