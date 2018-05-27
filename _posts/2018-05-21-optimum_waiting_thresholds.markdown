---
layout: post
comments: true
title:  "Optimum waiting thresholds"
date:   2018-05-21 22:49:40 -0800
categories: jekyll update
---

## 1. Optimum waiting threshold problem
Say it takes ten minutes for you to walk to work. However, there is a bus that also takes you right from your house to work. 
As an added bonus, the bus has internet, so you can start working while on it. The catch is that you don't know how long it will
take for the bus to arrive. 

![Missing a bus]({{ site.url }}/Downloads/opt_thresholds/miss_bus.jpg)

Now, being the productive person you are, you want to minimize the time you spend being in a state where you can't work (walking to work or waiting for the bus).

If we knew exactly how long the bus was going to take on any day, this would be quite easy. If the bus is more than ten minutes away, simply start walking.

However, we know that real life is rarely so predictable. We probably have some distribution in mind for how much longer it will take for the bus to arrive, 
but not the exact time. Assume we have this distribution and it doesn't change from one day to the next. Our strategy is simple - wait a certain amount of time
for the bus (say 5 minutes) and if the bus doesn't arrive by then, give up waiting and simply walk to work.

Now, assume that in parallel universes, you and your clones pick different wait-times. In one version, your clone always waits one minute, in another one, two minutes and so on.

How can you ensure that the total time you'll spend waiting for the bus over a long period (say, a year) will be less than all the clones. In other word, what wait
time should you pick so that the total time you spend either walking or waiting is minimized?

This clearly depends on the distribution for how long the bus is going to take. If we know this distribution, what can we do next? 

### 1.1 Parametric approach
Let's say the time it's going to take for the bus to arrive is $$T$$, which is a random variable. Also, if we give up on the bus and decide to walk, 
the time it's going to take is $$y$$.

$$E[DT] =  P(T \leq \tau). E [T|T<\tau] +   P(T>\tau). (\tau + y)$$

Where : 

* $$T$$ is the organic distribution for the bus to arrive.
* $$\tau$$ is the threshold for the time we will wait before starting to walk to the office.
* $$y$$ is the time it will take for you to get to the office once we start walking.
* $$DT$$ is the time we spend not working given waiting threshold, $$\tau$$. It is a random variable that depends on $$T$$ and $$\tau$$.


as we can see, the equation above can be described as a simple expectation of two possibilities. Either the the bus will arrive before the threshold, $\tau$. 

Or, it won't and we will walk to work losing a total time of $$\tau + y$$.

Now,
$$E[T|T < \tau] = \frac{\int_0^\tau tf_T(t)dt}{P(T < \tau)}$$

\begin{equation}\Rightarrow E[T] = \int_0^\tau x f_X(x) dx + [1-P(X\leq\tau)] \times [\tau + Y] \end{equation}


To find the value of $\tau$ that minimizes the expression in equation (1), we take the derivative with respect to it and set to zero.

$$ \frac{dE[T]}{d\tau} = \tau f_X(\tau) - f_X(\tau) \times [\tau + Y] + [1-P(X\leq\tau)] = 0 $$

$$\Rightarrow 1 - P(X\leq \tau) = Y f_X(\tau)$$

\begin{equation} \Rightarrow \frac{f_X(\tau)}{P(X\geq \tau)} =  \frac{1}{Y}\end{equation}

The expression in equation (2) has a very intuitive meaning. The expression on the L.H.S is called the "Hazard rate" of distributions describing the 
arrival times of certain events. 
The events being modelled are generally negative in nature (like defaults) hence the "hazard" in the name. 

In our case though, the event we are anticipating is positive (the bus arriving - not so hazardous). It is the rate at which evens that 
the distribution is describing arrive. 

Here, the rate is described as the inverse of the average time until the next event arrives as seen from the current state, instantaneously. 
Note that for most distributions, this rate itself will change with time so the average time until the next event won't actually be the inverse of the 
instantaneous rate just as the instantaneous velocity of an accelerating object at a certain time can't on its own predict the time the object will reach 
a certain point. The R.H.S is the inverse of the (deterministic) time it'll take for you to get to work once we start walking. It too is then a kind of rate. 
It is when the rates corresponding to the two competing processes align that the optimal $$\tau$$ is acheived.

In a nutshell, this points to the strategy - "at any point in time, go with the strategy that gives you a higher hazard rate".

Now, how do we get the distribution, $$T$$? We can just observe how long the bus takes each and every day and then use the data to fit a distribution. 
This can be done using maximum likelihood estimation.
In this context though, there is a bit of a wrinkle. And this is discussed next.

#### 1.1.1 Dealing with censored data
So, we record the time it takes each day for the bus to arrive and fit a distribution to these observations. Simple, right? Well, not quite. 
The problem is, you probably can't wait beyond a certain point for the bus. So, what about the times you waited say seven minutes, gave up and just 
walked to work? You know for sure that the bus took more than seven minutes. You just don't know how much longer than seven minutes it took. What do you do with 
these observations? Should we simply throw them away? 


### 1.2 Markov chain based approach


### 1.3 Non-parametric approach

### 1.4 Hybrid approaches


## 2. Experiments

In this section, we conduct a series of experiments.

### 2.1 Non-parametric approaches when we know the distribution

If we exactly know the distribution generating the data, we have shown that we actually know the correct answer for the optimal waiting time. 

So, we can see which of our approaches comes the closest to this right answer.

#### 2.1.1 No censoring

#### 2.1.2 Complete censoring

#### 2.1.3 Random censoring



### 2.2 Non-standard processes

What if our data is generated by weird processes 




