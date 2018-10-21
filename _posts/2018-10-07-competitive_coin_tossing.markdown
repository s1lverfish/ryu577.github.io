---
layout: post
comments: true
title:  "Coin toss markov chains"
date:   2018-10-07 19:02:40 -0800
categories: jekyll update
---


## 1. The question
Let's start with a simple question that will motivate the content of this blog. Not only is the answer beautiful, 
but it also helps us develop a framework for answering a whole family of such questions. The question goes like this - 
let's say I have a fair coin (50-50 chance of heads and tails) and so do you. Both of us start tossing our coins. 
What is the probability that you will get three heads in a row before I get two heads in a row? 
It's quite clear that I have a higher chance of winning (but don't worry,
my better odds are balanced by us obsessing over your victory throughout this post). 
Now, how do we go about calculating how much higher?


## 2. Thinking in terms of states
We can think of how close each player is to realizing his goal in terms of states. The first player (I) keeps tossing until he
reaches two consecutive heads. So, we can define his state after n tosses as the number of consecutive heads he has so far.
In the beginning, there are no tosses. So, the number of consecutive heads at that point is obviously zero. 

If he gets a heads on the first toss then the number of consecutive heads after the first toss is one whereas if he gets
a tails, it stays at zero. Hence, anytime a player is in the state "zero consecutive heads", there is a 50% chance they will
go to one consecutive head and 50% chance they will stay at zero consecutive heads after the next toss. The probability that 
the player will jump from zero consecutive heads to two consecutive heads in one toss is zero.

We can collect these three numbers into a vector of probabilities.

$$
v = (.5, .5, 0)
$$

Similarly, if a player is at one consecutive head so far on any toss, the probability that they will be at two consecutive heads
after the next toss is 50% and the probability of dropping back to zero is 50%. There will be a vector for transitions from this
state as well. 

So, each state has a vector of probabilities of going to each of the other states. We can collect these vectors and create a matrix.

This is what it will look like for me:


$$
M_3 = \left( \begin{array}{ccc}
		0.5 & 0.5 & 0  \\
		0.5 & 0 & 0.5 \\
		0 & 0 & 1
		\end{array} \right)
$$

Note that the third row corresponds to the transitions from state 2. Since I plan on stopping once I reach two consecutive heads,
I will not be making transitions to any other states once I reach state 2. That's why state 2 (third row) has a probability 1 of
simply transitioning to itself.

Similarly, you plan on stopping once you reach three consecutive heads. So, your transition matrix will look like this:


$$
M_4 = \left( \begin{array}{ccc}
		0.5 & 0.5 & 0 & 0  \\
		0.5 & 0 & 0.5 & 0\\
		0.5 & 0 & 0 & 0.5\\
		0 & 0 & 0 & 1
		\end{array} \right)
$$


## 3. Probabilities at nth toss
Now, let's think of the probabilities that I am in each of the possible states after a certain number of tosses. As said before,
the probabilities at zero tosses are pretty clear. Both sequences are at zero consecutive heads. For the first sequence for example,
there is a 100% chance I'm in state 0, a 0% chance I'm in state one (one consecutive head) and 0% chance I'm in state two. These three
probabilities can be collected into a vector:

$$
P_0 = (1, 0, 0)
$$


Now, what about after one toss. If I get a heads, I go to state one and stay in state zero if I get a tails. In other words, there
is a 50-50 chance I'll be in state 0 or state 1 after the first toss.

$$
P_1 = (0.5, 0.5, 0)
$$

This argument might give you a feeling of deja-vu since we used the exact same one when constructing the matrix $$M_3$$. In fact, we can get $$P_1$$
by simply multiplying the $$P_0$$ with $$M_3$$.

$$P_0 M_3 = (1,0,0) \left( \begin{array}{ccc}
		0.5 & 0.5 & 0  \\
		0.5 & 0 & 0.5 \\
		0 & 0 & 1
		\end{array} \right) = (.5,.5,0) = P_1$$


Similarly to get $$P_2$$, we multiply $$P_1$$ with $$M_3$$.

$$P_1 M_3 = (.5,.5,0) \left( \begin{array}{ccc}
		0.5 & 0.5 & 0  \\
		0.5 & 0 & 0.5 \\
		0 & 0 & 1
		\end{array} \right) = (.5,.25,.25) = P_2$$

Which means that after 2 tosses, there is a .25 probability that I would reach my goal.

But from the previous equation we can say,

$$P_2 = P_1 M_3 = P_0 M_3 M_3 = P_0 M_3^2$$

In general we can say,

$$\begin{equation} P_n = P_0 M_3^n \tag{1}\end{equation}$$

Now, we can of course say this for any transition probability matrix $$M$$ (non-negative entries and rows sum to one). 

For your markov chain (you need three consecutive heads) we can similarly define the probabilities $$Q_n$$, that you will be in 
each of the states 0, 1, 2 and your goal state of 3 consecutive heads. Like before,

$$\begin{equation} Q_n = Q_0 M_4^n \tag{2}\end{equation}$$

$$Q_0$$ is your state at zero tosses which is $$(1,0,0,0)$$ (with a probability of one, you are in state 0 - you haven't tossed,
so have obtained zero consecutive heads so far). 

Let's plot the probabilities that you will be in each of the states as a function of $$n$$.

![Probability sequences]({{site.url}}/Downloads/CompetitiveCoinToss/probs_seq.png)

In the beginning, the starting state (0 consecutive heads) is obviously at 1.0. But it soon drops to zero. The other two non-absorbing states also
make a climb initially since the final absorbing state must go through them. However, they quickly fall to zero as all the probability
mass is sucked up by the absorbing state (in green).

Here is some simple, self-contained python code that shows how to calculate the probabilities of this sequence getting to the absorbing state as a function of the
number of tosses (the green line in the plot).

```python
import numpy as np

start = np.array([1,0,0,0])

# The transition matrix.
m_3 = np.matrix([
	            [.5,.5,0],
	            [.5,0,.5],
	            [0,0,1]
	            ])

## Probabilities of getting to absorbing state after n tosses.
# First raise the matrix to nth power
# then get the index of absorbing state
p_n = np.array([np.dot(start, np.linalg.matrix_power(m_3,n))\
						 [0,3]\
                         for n in range(100)])#Calculate this upto 100 tosses.
```

The plot will look very similar for the elements of the vector $$P_n$$ of my states. However, my absorbing state (two consecutive heads)
will be reached much quicker. In fact, let's compare the probabilities me and you reach our goals (absorbing states: 2 heads and 3 heads respectively) 
by plotting them side-by-side. 

![Probability sequences]({{site.url}}/Downloads/CompetitiveCoinToss/probs_two_chains.png)


## 4. Using the sequences to get the answer

All this is well and good but how is it going to help us get the number we're looking for? What is the probability that you will
reach three consecutive heads before I reach two? Let's call this event $$A$$. We then want to calculate $$P(A)$$ (probability of event $$A$$).

Since we have the probabilities of both sequences being in each state, $$P_n$$ and $$Q_n$$, let's try and use those. We can start with
thinking of the event where we win in the nth toss. Let's call this event $$A_n$$. The only way $$A$$ will happen is that one of the $$A_n$$s happen.

So, we can say that event $$A$$ is the union of the events represented by $$A_n$$.

$$A = \bigcup_{n=1}^{\infty}A_n$$

Also, the $$A_n$$'s are mutually exclusive (meaning you can't win on the third toss **and** the fourth toss). Because of this we can say:

$$P(A) = \sum_{n=1}^{\infty}P(A_n)$$

So, if we get $$A_n$$'s, we can sum them to get the probability we are interested in.

What needs to happen for you to win on the nth toss?

1) The losing sequence (mine), given by $$P_n$$ should not have reached it's absorbing state by the nth toss. Otherwise, you **didn't** reach
three consecutive heads before I reached two. The probability is $$(1-P_n[2])$$. 

2) The winning sequence (yours), given by $$Q_n$$ should have reached a state where it needs just one more heads to win in the toss *one earlier than*
the current toss. In this case, it means I should have reached two consecutive heads by the $$(n-1)$$th toss. The probability is $$Q_{n-1}[2]$$.

3) And finally, you should get a heads in the $$n$$th toss and complete the coup-de-grace. The probability of this is $$\frac{1}{2}$$ since the coins are fair.

Combining those three events, we get - 

$$P(A_n) = (1-P_n[2])(Q_{n-1}[2]) \frac{1}{2}$$

And so we get to the most important equation of this blog, 

$$\begin{equation}P(A) = \sum_{n=0}^{\infty} (1-P_n[2])(Q_{n-1}[2]) \frac{1}{2} \tag{3} \end{equation}$$

We can get $$P_n$$ and $$Q_n$$ by using equations (1) and (2) and simply multiplying the transition matrices $$n$$ times. Of course, the sum in 
equation (3) goes to infinite terms and we can't multiply the matrices infinite times. However, note that (as we can see from figure 1), $$(1-P_n[2])$$
will tend to zero as $$n$$ becomes large (as the absorbing state - which is 2 - sucks up all the probability mass as $$n$$ increases 
and this means one minus it's probability tends to zero) 
and $$Q_{n-1}[2]$$ will tend
to zero for the same reason. So, the contributions to the sum of terms corresponding to large $$n$$ become smaller and smaller. 
Hence, we can get a very, very good approximation by simply ignoring all terms associated with $$n$$s larger than some reasonable value (like 100 or 50).

Here is some python code that demonstrates [this][3Hbefore2H].

```python
import numpy as np
start1 = np.array([1,0,0])
m_3 = np.matrix([[.5,.5,0], [.5,0,.5], [0,0,1]])

start2 = np.array([1,0,0,0])
m_4 = np.matrix([[.5,.5,0,0], [.5,0,.5,0],[.5,0,0,.5], [0,0,0,1]])

# p_n must always be one toss ahead of q_n_minus_1. So, when p_n is at toss 1, q_n_minus_1 
# should be at 0. When it is at 2, q_n_minus_1 should be at 1 and so on.
# that is why p_n starts with 1 and goes to 100 while q_n_minus_1 starts with 0 and goes to 99.
p_n = np.array([np.dot(start1, np.linalg.matrix_power(m_3,n))[0,2]\
                         for n in range(1,101)])
q_n_minus_1 = np.array([np.dot(start2, np.linalg.matrix_power(m_4,n))[0,2]\
                         for n in range(100)])

print("Prob(3 consec heads b4 2 consec heads):" + str(sum(q_n_minus_1*(1-p_n))/2))
```

Using this, we get that the probability we were after (you'll get 3 consecitive heads before I get 2 consecutive heads) is **21.25%**. 

Similarly by flipping things, we can get the probability that I'll get 2 consecutive heads before you get 3 consecutive heads
```python
# Assumes you have run the previous block
p_n = np.array([np.dot(start2, np.linalg.matrix_power(m_4,n))[0,3] for n in range(1,101)])
q_n_minus_1 = np.array([np.dot(start1, np.linalg.matrix_power(m_3,n))[0,1] for n in range(100)])
print("Prob(2 consec heads b4 3 consec heads):" + str(sum(q_n_minus_1*(1-p_n))/2))
```
This shows that the probability that I will win is **73.98%**.


Of course, the two numbers above don't sum to one since there is also the third possibility of a draw (both reach their goals on the same toss).


We have now answered the question we posed at the starting of this blog. But often when one climbs to the top of a mountain, one 
simply sees more mountains on the other side. In that spirit, we will see what other questions we can answer with the tools developed
in seeking the answer to this one in the next section (5). 

Also, note that the approach of taking a large sequence formed by repeated matrix multiplication is somewhat
ugly and inefficient. In section 6, we will improve on this and solve equation (3) in a cleaner, more efficient manner.   



## 5. Other mountains

### 5.1. Even the odds
In the question we started the blog with, it was clear that the stakes were in my favor.
Let's consider now a contest where that isn't obvious. I still need to get to two consecutive heads and you still need three.
But to even the odds for you, your heads no longer need to be consecutive. As soon as you see three heads in total, you win.

This turns out to be a pretty close contest. Who should a gambler bet their money on?

We can solve this problem in a pretty similar manner to the previous one. Just construct the two Markov chains, use them to calculate 
the sequences of being in their various states after $$n$$ tosses and plug the sequences into equation (3).  

My transition matrix will still be the $$M_3$$ defined in section 2. Your transition matrix however, will not be the $$M_4$$ defined there.

Now since you need three total heads, when you are in state $$i$$, you don't go back to zero if you get a tail on that toss. Instead, you simply stay 
at the state you were. In this way, I'm penalized more severely for a tails (I have to start over and you don't).

Your new transition matrix, $$O_4$$ will look like this:

$$
O_4 = \left( \begin{array}{ccc}
		0.5 & 0.5 & 0 & 0  \\
		0 & 0.5 & 0.5 & 0\\
		0 & 0 & 0.5 & 0.5\\
		0 & 0 & 0 & 1
		\end{array} \right)
$$

And just like in section 4, we can solve this with the exact same code, just replacing your $$M_4$$ matrix with $$O_4$$ defined above.

```python
import numpy as np
start1 = np.array([1,0,0])
m_3 = np.matrix([[.5,.5,0], [.5,0,.5], [0,0,1]])

start2 = np.array([1,0,0,0])
o_4 = np.matrix([[.5,.5,0,0], [0,.5,.5,0],[0,0,.5,.5], [0,0,0,1]])

# See previous code block in section 4 for detailed comments.
p_n = np.array([np.dot(start1, np.linalg.matrix_power(m_3,n))[0,2] for n in range(1,101)])
q_n_minus_1 = np.array([np.dot(start2, np.linalg.matrix_power(o_4,n))[0,2] for n in range(100)])

print("Prob(3 running heads b4 2 consec heads):" + str(sum(q_n_minus_1*(1-p_n))/2))
```
We see that the probability of you getting 3 running heads before my 2 consecutive is **36.74%**. Your chances of winning have improved
a little (from ~24% to ~37%), but there is more good news. When we flip things we get the probability that I will win:


```python
# Assumes you have run the previous block
p_n = np.array([np.dot(start2, np.linalg.matrix_power(o_4,n))[0,3] for n in range(1,101)])
q_n_minus_1 = np.array([np.dot(start1, np.linalg.matrix_power(m_3,n))[0,1] for n in range(100)])
print("Prob(2 consec heads b4 3 running heads):" + str(sum(q_n_minus_1*(1-p_n))/2))
```

And this turns out to be **54.77%**. So, my chances of winning have gone down from ~74% to ~55%. It seems there is a much higher chance 
the game will end in a draw (1-.55-.37 = 8%, up from 5% earlier).

So, your odds against me have gone from roughly 2:7 in the previous game to 2:3 in this one. That evens things out quite a bit, you're probably still
not happy, so let's see what else we can do.


### 5.2. Unfair coins
In this section, we won't settle for anything less than a completely even game. One way to do that is to go back to the old criterion (you need
three consecutive heads while I need 2). However, we'll let you cheat and use a biased coin (one that has a higher probability of heads than mine).

So, while my head still has a 50% chance of getting heads, yours will be higher. If your coin has a 100% chance of getting heads, note that your win
is still not guaranteed. I could get heads on my first two tosses and that would mean you lose. The probability I'll win is therefore 25% ($$.5 \times .5$$ 
for the two heads I need).

The only way a draw can happen is if I get a tails on the first toss and then two consecutive heads. The probability of this is therefore 12.5% ($$.5\times
.5 \times .5$$).

So, the probability that you'll win is 1-.125-.25 = 62.5%.

But that tips things too much in your favor. We want the game to be even. Also note that if your probability of winning is 50%, you're still going to
be at an advantage because of the possibility of a draw, which will make my probability of winning less than 50%. We want to find the probability of
heads $$p$$ your coin should have for none of us to have an advantage.

For this, we just need to wrap the code from the blocks above in a function that takes the probability of heads as input (instead of hard coding to 0.5)
and then we can a simple numeric method like bisection to find the $$p$$ at which the probabilities of both of us winning are the same.

I simply plotted these probabilities with $$p$$ in the code [here][gist_3hb42hwithp]. The result of which is:

![Probability sequences]({{site.url}}/Downloads/CompetitiveCoinToss/probs_with_p.png)

You can see from the plot that at $$p \sim 0.77$$ at which point, both of us will have a $$\sim 45\%$$ chance of winning.

### 5.3. Longer sequences

Now that we've evened the odds in the previous sub-section (just make your coin have a 77% instead of 50% chance of getting heads), 
we can try and test the power of these new wings we've developed. What if we go back to both of us having fair coins
and wanted to get the probability that I reach 4 consecutive heads before you reached 3? 
What if it were me reaching 5 before you reaching 4? In other words, you still require one more consecutive head than me, but we're 
gradually increasing the number of heads I need. In general, what is the chance I'll reach $$n$$ heads before you reach $$(n+1)$$ heads?


You can imagine now that the markov matrices become larger and larger as we increase $$n$$ (there are $$n^2$$ terms in the smaller one)
and the chance that the game is resolved by the hundredth toss will become smaller and smaller (two or three consecutive heads would have
appeared by the 100th toss, but ten consecutive heads is far less likely). So, we need to extend the sequences to a much larger number
of tosses. 

This makes the method we described earlier very slow. But, we can still use the faster method referenced earlier (will be described in section 6)
to get the exact solution for $$n$$ going all the way up to the late teens. 

In any case, we plot the probabilities of me winning, you winning and a draw in the figure below. If you want to use the more efficient method 
described in section 6, you can do so with the aid of an open source library that goes along with this blog.


```python
# To install the library, pip install stochproc from command line.
# hosted at - https://github.com/ryu577/stochproc
from stochproc.competitivecointoss.smallmarkov import *

ns = np.arange(2,15)
win_probs = []
for n in ns:
    # The losing markov sequence of coin tosses that needs (n-1) heads.
    lose_seq = MarkovSequence(get_consecutive_heads_mat(n))
    # The winning markov sequence of coin tosses that needs n heads.
    win_seq = MarkovSequence(get_consecutive_heads_mat(n+1))
    # If you multiply the two sequence objects, you get the probability
    # that the first one beats the second one.
    win_prob = win_seq*lose_seq
    win_probs.append(win_prob)

plt.plot(ns, win_probs)
```


![Probability sequences]({{site.url}}/Downloads/CompetitiveCoinToss/probs_with_n.png)

Looking at the figure, something surprising pops out.

As we increase the number of tosses I need, the probability of you winning starts approaching 33.33% or one third.
Similarly, the probability of me winning starts approaching 66.67% or two thirds. It's a little surprising that such a simple number pops out.

There is a tantalizing possibility that there exists some elegant reason for this, but I haven't been able to devise the thought experiment. 


## 6. Closed forming the markov sequences

In the previous section, the approximation of cutting off at 100 tosses worked pretty well. 
This is because there is a very high chance the game would have ended well before 100 tosses (either one of the players reaching their goal). 

What if however, we wanted the probability the first player gets
50 consecutive heads before the second one got 51. The probability that this would get resolved before 100 tosses is quite low, so cutting off
at 100 would not give us a good approximation. And let's say we needed to go to a million tosses to get a good approximation. Multiplying these 
large matrices millions of times would be a very computationally expensive affair to the point where we might not even be able to get the answer.

We can make this method more scalable to questions about large numbers of tosses and not dependent on multiplying matrices many times. 
But we'll need some linear algebra - in particular, the concept of eigen decomposition - to do so.

To follow this section therefore, it is best you're somewhat familiar with eigen value decomposition. It is basically a way to really x-ray matrices
and see what they're all about. The idea is that for the matrices $$M_3$$ and $$M_4$$, you can find three and four dimensional vectors respectively
that when multiplied with them, don't change. And then there are other vectors that are also special because they change, but are only scaled, not rotated.


If the matrix is $$n \times n$$, you can find $$n$$ such directions (including that one that wasn't changed or was scaled by 1).

This translates to being able to write:

$$\begin{equation}M_3 E =  E \left( \begin{array}{ccc}
		1 & 0 & 0  \\
		0 & \lambda_1 & 0 \\
		0 & 0 & \lambda_2
		\end{array} \right) \tag{4}\end{equation}$$

where the matrix $$E$$ contains the special vectors that are scaled and not rotated as it's columns.



Now if we look at equations (1) and (2), we basically need to raise the markov matrices to the power of $$n$$, meaning multiply them with themselves over and
over. 

If we assume the matrix $$E$$ from equation (4) is invertible, we can write (pre-multiplying both sides by $$E^{-1}$$).

$$\begin{equation}M_3 = E \Lambda E^{-1}\tag{5}\end{equation}$$

Where

$$\Lambda = \left( \begin{array}{ccc}
		1 & 0 & 0  \\
		0 & \lambda_1 & 0 \\
		0 & 0 & \lambda_2
		\end{array} \right)$$

Now, if we want to square $$M_3$$, we can use equation (5)

$$M_3^2 = E \Lambda E^{-1} E \Lambda E^{-1} = E \Lambda \Lambda E^{-1} = E \Lambda^2 E^{-1}$$

We can repeat this $$n$$ times to conclude:

$$M_3^n = E\Lambda^n E^{-1}$$

Since $$\Lambda$$ is diagonal, we can write - 

$$\Lambda^n = \left( \begin{array}{ccc}
		1 & 0 & 0  \\
		0 & \lambda_1^n & 0 \\
		0 & 0 & \lambda_2^n
		\end{array} \right)$$

Now from equation (1), 

$$P_n = P_0 M_3^n = (1,0,0) E \Lambda^n E^{-1}$$

If you multiply these out, this is equivalent to saying

$$\begin{equation}P_n = (1,\lambda_1^n, \lambda_2^n) C \tag{6}\end{equation}$$

Where $$C$$ is a constant matrix of coefficients given by:

$$C = \left( \begin{array}{ccc}
		E_{1,1} & 0 & 0  \\
		0 & E_{1,2} & 0 \\
		0 & 0 & E_{1,3}
		\end{array} \right) E^{-1}$$

and $$E_{1,1}$$, $$E_{1,2}$$ and $$E_{1,3}$$ are the elements of the first row of $$E$$.

In equation (3), we only need $$P_n[2]$$ which will be given by:

$$P_n[2] = C_{1,3}+C_{2,3}\lambda_1^n + C_{3,3} \lambda_2^n$$

You can see [here][eigvalstoch] a proof that a stochastic matrix will always have eigen values less than one in magnitude.
This means that the second and third terms of the expression above will tend to zero as $$n \to \infty$$. 

Since the sequence must eventually end up in the absorbing state for large enough $$n$$ we must have,

$$\lim_{n \to \infty} P_n[2] = C_{1,3} = 1$$

Giving us,

$$\begin{equation}P_n[2] = 1 + C_{2,3}\lambda_1^n + C_{3,3}\lambda_2^n\tag{7}\end{equation}$$

Similarly, we will have a corresponding matrix $$D$$ for your $$Q_n$$ sequence and we can write:

$$\begin{equation}Q_n[2] = 1 + D_{2,3}\mu_1^n+D_{3,3}\mu_2^n+D_{4,3}\mu_3^n\tag{8}\end{equation}$$

Plugging (7) and (8) into equation (3) we get,

$$P(A) = \frac{1}{2}\sum_{n=1}^{\infty} (C_{2,3}\lambda_1^n + C_{3,3}\lambda_2^n)(1 + D_{2,3}\mu_1^{n-1}+D_{3,3}\mu_2^{n-1}+D_{4,3}\mu_3^{n-1})$$

$$=\frac{1}{2}\sum_{n=1}^{\infty} (C_{2,3}\lambda_1^n + C_{3,3}\lambda_2^n)(1 + \frac{D_{2,3}}{\mu_1}\mu_1^{n}+\frac{D_{3,3}}{\mu_2}\mu_2^{n}+\frac{D_{4,3}}{\mu_3}\mu_3^{n})$$

$$ = \frac{1}{2}\left(C_{2,3}\left(\frac{\lambda_1}{1-\lambda_1}\right) + C_{2,3}\frac{D_{2,3}}{\mu_1}\left(\frac{\lambda_1\mu_1}{1-\lambda_1 \mu_1}\right)
+\dots\right)$$

As long as we can construct the transition matrices for the two sequences of coin tosses and get their eigen values and eigen matrices, we can
get the probability that one of them will beat the other using the approach above.




## 7. A "different" equation
**Note: This section is under construction**

There is a way to tame these processes coming from a completely different direction. By 
constructing a difference equation (see what I did there? Anyone? Anyone?). 


## 8. I'll give you the answer on one condition
**Note: This section is under construction**

We started this blog with a simple to understand problem. The solution provided involved Markov chains and thinking in terms of states.
That solution was like a silver bullet for a whole family of problems of this nature. In this section, we will pursue a simpler solution
that focuses on just the original problem and involves just conditional probabilities.

## 9. One big chain
**Note: This section is under construction**

The original problem stated in this blog (you get three consecutive heads before I get two consecutive heads) was solved using Markov chains.

However, there is more than one Markov chain based solution. 


## 10. Without a computer
**Note: This section is under construction**

Let's say this question was posed to you while you we're on vacation, sitting somewhere on a beach. Ghastly as that prospect is,
would we have some means of solving it with an advanced stick to draw on the sand with? Or is reaching for some kind of modern computer
your only option?

Unfortunately, I don't know a way to get the exact answer without a computer. If you do, please leave a comment. But, I know how to
do the next best thing - get upper and lower bounds on the probabilitiy.

Since the premise of this section is that you don't have access to a computer, all the calculations shown in this section can be carried
out on a pen and paper (or stick and sand).




[eigvalstoch]: https://yutsumura.com/eigenvalues-of-a-stochastic-matrix-is-always-less-than-or-equal-to-1/
[3Hbefore2H]:https://gist.github.com/ryu577/89e4ef0b0b7fcba33e08a549f0793c86
[gist_3hb42hwithp]:https://gist.github.com/ryu577/9464d39e1b40ffdd37773d44ffb81eab
