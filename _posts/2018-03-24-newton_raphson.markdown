---
layout: post
comments: true
title:  "Visualizing the Newton Raphson method"
date:   2018-03-24 21:10:40 -0800
categories: jekyll update
---

## Newton and Raphson
From the name of this method, you might picture Newton and Raphson working together as a team, coming up with this method. But in reality, they discovered it independently. 
This seems to be a common theme with Newton. Remember how he also discovered calculus with Leibnitz? How come this guy was having so many ideas at the exact time other people were
having them too? It's actually quite possible since for multiple people at the cutting edge of research in a field, the next steps are often unambiguous. It's the same reason
that multiple independent teams discovered the security vulnerabilities in Intel chips (spectere and meltdown).

## What is x?

The essence of algebra is solving equations for unknown quantities. For example, find $$x$$ where

$$2x+5=7$$

It's easy to see that the solution to the equation above is $$x=1$$. Another way to look at this is to take everything to one side and calling the expression $$y=2x-2$$. 
Then, we can see try and find the $$x$$ for which $$y=0$$.

But what happens when the expression of $$y$$ in terms of $$x$$ becomes more and more complex? Can we find all (or at least one of) the values of $$x$$ that satisfy $$y=0$$?

![VariousFunction]({{ site.url }}/Downloads/NewtonRaphson/functions.gif)

If linear functions are the simplest, then perhaps the next in rank are quadratics (involve $$x^2$$). Now, if we have a quadratic equation, how would we go about solving it?
Well, we could simply use the quadratic formula. But let's for a second suppose that we don't know this formula. We only know how to solve linear systems like equation (1).
Can we use this knowledge of solving a linear equation for solving a non-linear (in this case, quadratic) equation? 

$$y = x^2$$

Well we can, but only if we're very persistent. Let's start with any random point, $$x$$. Now, calculate the value of our quadratic function at this $$x$$ and call it $$y$$. 
Now, our tool is a linear equation solver but we have a quadratic equation instead. So, let's convert the quadratic equation into a linear equation. To do this, just
approximate the quadratic equation with a linear equation. Now, when you solve this linear equation, you will get an "answer" for $x$. But we can't expect this answer to be
"right" since we "cheated". Since the definition of insanity is doing the same thing and expecting a different result, we keep repeating this process. The only difference being 
that this time we use the previous solution to the linear equation as our starting point. And eventually, this process leads us to the solution for the quadratic equation as 
you can see below.


![VariousFunction]({{ site.url }}/Downloads/NewtonRaphson/nr_iterations.gif)

## Cranking up the dimensions
Now, it is possible that you might have seen something very similar to the visualization above before. But, how does this extend to multiple dimensions? For example, instead
the one variable $$x$$, let's say we now have two variables, $$x$$ and $$y$$. The most natural way to extend equation (2) to two dimensions is:

$$z=x^2+y^2$$

And here is what that plot looks like:

![VariousFunction]({{ site.url }}/Downloads/NewtonRaphson/imParabola.png)

Like before, we want to solve for $$z=0$$. This happens at the green circle in the figure above is where our equation intersects the x-y plane. 

![VariousFunction]({{ site.url }}/Downloads/NewtonRaphson/circle_intersection.gif)

But unlike the previous case, this are an infinite number of solutions. And this is quite natural since we increased the number of variables by one, keeping the number
of equations the same. In order to get a finite number of solutions, we need to have the same number of equations as variables, which is two.

To keep things simple, let's just replicate our existing equation and move it by a finite amount. And just like that, we have two equations now.

![VariousFunction]({{ site.url }}/Downloads/NewtonRaphson/TwoEquations.gif)

Also, the second equation intersects the x-y plane in a circle as well and the two circles intersect at two distinct points which are the solutions to the 
system of equations. These are the yellow points in the figure below.

![VariousFunction]({{ site.url }}/Downloads/NewtonRaphson/imTwoEquations.png)

Now, how do we use our method from before to get to one of these solutions?

We'll start with any random point on the x-y plane (the pink point in the figure below). This can be projected to the green point on the green parabola and the
yellow point on the yellow parabola. We can then draw the best linear approximations of the green and yellow parabolas at the green and yellow points. Since linear
equations are planes, this gives us the light green and light yellow planes. These planes will intersect in the purple line which will intersect the x-y plane in
the white point. This white point is the solution of the system of two linear equations (approximations of the two parabolas). 


![VariousFunction]({{ site.url }}/Downloads/NewtonRaphson/imApproximateLinear.png)

Now, we can simply repeat this process with the white point instead of the original pink point and keep going. Ultimately, we get to one of the two solutions. This process
is shown below.

![VariousFunction]({{ site.url }}/Downloads/NewtonRaphson/iterations.gif)

