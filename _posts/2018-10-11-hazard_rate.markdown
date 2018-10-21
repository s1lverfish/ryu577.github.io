---
layout: post
comments: true
title:  "Hazard rate"
date:   2018-10-11 21:31:00 -0800
categories: jekyll update
---


## What is it good for?
Hazard rate is a very useful concept when modelling the time to an event. It has the "hazard" in it's name because the people who named it were 
modelling negative events like deaths or failures. But it's just as useful when modelling positive events like recovery from a failure.

## What is it?
The mathematical definition of hazard rate is the probability we will see the event we are interested in in the next unit of time conditional 
on it not having occurred till now. That might or might not have been too useful.


To understand it intuitively, we'll need to think about my favorite Batman villain, Two-Face.

For those unfamiliar, Two-Face is basically a guy with a coin he likes to toss. When he gets heads, bad things happen.

![Probability sequences]({{site.url}}/Downloads/HazRate/twoface.jpg)

Now imagine Two-Face standing by the side of a freeway, watching cars zip by. He tosses his coin every milli-second. 
If he gets heads in any of these milli seconds, an accident immediately ensues. Luckily, the probability of heads is quite low at one in a billion. 
But on the other hand, the guy tosses his coin roughly 100 million times a day. So, we should expect about one accident every 10 days.

Now, the probability that we will see a heads in the next toss doesn't depend on how many times he tossed the coin before.

The coin doesn't care what it did in previous tosses; it has no memory. If you think it does, you suffer from a condition known as the gamblers
fallacy (side effects include losing all of ones pocket money on unsound hypotheses).

## A changing coin?

If Two-Face keeps tossing the same coin (with the same probability of heads), the hazard rate for the next accident doesn't change with time.
Again, this is because the coin doesn't care that it didn't show you a heads for the past $$t$$ tosses.

This might be a good assumption in certain scenarios (and modelling freeway accidents might well be one of them), but not in most. For example, 
when modelling the recovery of a machine, it is logical to assume that if a machine didn't auto-recover in a certain time, it becomes less likely to
do so in the next second. This is equivalent to Two-Face changing his coin to one with a lower probabilities of heads the more he tosses.

Alternately, if we're modelling the lifetime of a person or machine before catastrophic failure, it is logical to assume that the longer they have lived the higher the chance they  are going
to die/fail in the next year or so. In this case, Two-Face is gradually increasing the probability of heads as the number of tosses increase (perhaps because he's
getting impatient).

We can see that most positive events have a decreasing hazard rate while negative ones have an increasing hazard rate. 

What conclusion you draw from that is up to you.




