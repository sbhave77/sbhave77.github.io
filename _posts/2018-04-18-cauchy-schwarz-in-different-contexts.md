---
layout: post
title: Musings about dot products, cauchy-schwarz inequality and inner product spaces
date: 2018-04-18 00:00:00
description: 
---


I was just recently working on understanding the relationship between $$\beta$$, $$r$$ and $$r^2$$ and this led me to get into the weeds of linear algebra including dot products, inner product spaces and the Cauchy-Schwarz inequality. This post outlines my review and renewed understanding of these topics.
	
First of, what exactly is a dot product both mathematically and intuitively?

It's defined as follows in two different ways:

$$
\begin{align*}
a \cdot b &= \sum_{i=1}^{n} a_i b_i \text{   (algebraic definition)}\\
&= ||a|| ||b|| cos(\theta) \text{   (geometric definition)} \\
\end{align*}
$$

Intuitively, the dot product is essentially a scalar measure of how much two vectors are in the same direction.

![Geometric Dot Product Interpretation](https://upload.wikimedia.org/wikipedia/commons/3/3e/Dot_Product.svg "Geometric Dot Product")

Looking at the figure above, we can see that the projection of a onto b is basically derived as follows:

$$
\begin{align*}
cos(\theta) &= \frac{proj_{ab}}{||a||} \\
proj_{ab} &= ||a|| cos(\theta)
\end{align*}
$$

Given this, the geometric definition just becomes:

$$
||a|| ||b|| cos(\theta) = proj_{ab} ||b||
$$

This means that the dot product is basically multiplying the norm of b and the norm of the component of a in the direction of b.

Also, as a result of cos function being between -1 and 1:

$$ -||a||||b|| \leq a \cdot b \leq ||a|| ||b|| $$

This is a good way to intuitively understand what a dot product is, but what is the exact relationship between the algebraic definition and the geometric one? Why is taking the sum of the elementwise products of the vectors related to projections?

This can be better understood by understanding each of the vectors in terms of a standard basis.

If we think about coordinate systems in linear algebra, we note that the following is true:

$$
a = [a_1, a_2, \dots, a_n] = \sum_{i=1}^n a_i e_i \\
b = [b_1, b_2, \dots, b_n] = \sum_{i=1}^n b_i e_i
$$

where $$e_i$$ represent standard basis vectors.

The connection between the geometric and algebraic definition of the dot product becomes clear when we consider standard basis vectors.

$$ a \cdot e_i = ||a|| ||e_i|| cos(\theta) = ||a|| cos(\theta) = a_i $$

Basically, no matter what high dimensional space the vector a is in, projecting it onto a standard basis vector (which has norm 1) is exactly the same as choosing the $$ith$$ element of that vector.

We can generalize this idea when thinking about $$ a \cdot b $$.

$$ a \cdot b = a \cdot (\sum_{i=1}^n b_i e_i) = \sum_{i=1}^n a \cdot (b_i e_i)  = \sum_{i=1}^n b_i (a \cdot e_i) = \sum_{i=1}^n b_i a_i$$

Above, we simply leverage the distributive property and scalar homogeneity properties of the algebraic dot product which are simply results of basic algebraic properties.

The interpretation is what establishes the connection. Basically, we can think about the dot product of $$a$$ and $$b$$ as projecting $$a$$ onto a standard basis vector scaled by the scalar $$b_i$$ that is in that standard basis direction.

Ok, but what does this have to do with the Cauchy-Schwarz inequality?

Well, given our knowledge from above, the Cauchy-Schwarz inequality becomes clearer. It is as follows:

$$ (abs(u \cdot v))^2 \leq (u \cdot u)(v \cdot v)$$

$$ abs(u \cdot v) \leq ||u|| ||v||$$

$$ -||u|| ||v|| \leq u \cdot v \leq ||u|| ||v|| $$

Basically, all it's really saying is that the absolute value of the dot product is bounded by the product of the norms of the two vectors, which is pretty obvious from the geometric definition as we showed above.

But, Cauchy-Schwarz is actually not specific to a dot product. It holds under any valid inner product space (satisfies three main axioms listed [here](https://en.wikipedia.org/wiki/Inner_product_space)).

In probability theory, a commonly used inner product space is the expectation of the product of two random variables $$X$$ and $$Y$$.

$$\langle X,Y \rangle := E[XY]$$

This follows the three axioms:

1. $$ E[XY] = E[YX] $$
2. $$ \langle X, Y + Z \rangle = E[X(Y + Z)] = E[XY + XZ] = E[XY] + E[XZ]$$
3. $$
\begin{align*}
\langle X, X \rangle &= E[X^2] = Var(X) + (E[X])^2 \geq 0 \\ 
\langle X, X \rangle &= 0 \iff X = 0 
\end{align*}
$$

The iff statement is true because if the inner product of X with itself is 0, this means the variance and the mean are 0 which means by definition the random variable must be a vector of 0.

Given this, what does Cauchy-Shwarz imply in this particular inner product space?


$$
\begin{align*}
abs(\langle X, Y \rangle)^2 &\leq \langle X, X \rangle \langle Y, Y \rangle \\
(abs(E[XY]))^2 &\leq E[X^2] E[Y^2]
\end{align*}
$$

Interesting, this seems to have a close relation to covariance.

$$
\begin{align*}
(cov(X,Y))^2 &= (E[(X - E[X])(Y - E[Y])])^2 \\
&=  \langle X - E[X], Y - E[Y] \rangle ^2 \\
&\leq  \langle X - E[X], X - E[X] \rangle \langle Y - E[Y], Y - E[Y] \rangle \\
&= E[(X - E[X])^2] E[(Y - E[Y])^2] \\
&= Var(X) Var(Y) \\
&= \sigma_x^2 \sigma_y^2
\end{align*}
$$

or in other words..

$$
cov(X,Y) \leq \sigma_x \sigma_y
$$

Thus, the covariance of X and Y is less than or equal to the product of the standard deviations of X and Y. Intuitively this makes some sense because covariance can be thought of as the average of the product of the deviation of X and Y from their respective means. You might expect that the maximum of the covariance to be the straight product of the two standard deviations (a perfect linear relationship).

This has implications when understanding why correlations must be bounded between 1 and -1 which is explored more in this [post](https://sbhave77.github.io/blog/2018/what-does-corr-coeff-mean/).
