---
layout: post
title:  Difference between slope, r and r^2 in computing Pearson and Spearman correlation
date:   2018-04-18 00:00:00
description: Simple explainer piece on differences between statistical quantities.
---

Recently, I was working on an exploratory analysis to identify environmental and socioeconomic variables that are most correlated with disease prevalences on a population scale across different US cities. Before blindly computing these values, I wanted to get a deeper understanding of the assumptions behind computing a [Pearson correlation](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient) and also remind myself of the relationship between $$\beta$$, $$r$$ and $$r^2$$ in simple linear regression. Furthermore, I wanted to explore exactly what this means in the context of Spearman correlates as well.

First of all, what is a Pearson correlation ($$r$$)?

In simple terms, it is a metric between -1 and 1 to measure the **linear** correlation.

In mathematical terms, it's basically just the covariance of two random variables normalized by their standard deviations.

In particular, it is the covariance of the normalization of two random variables. A simple derivation is as follows:

$$
\begin{align*}
cov(X,Y) &= E[(X - \mu_x)(Y - \mu_y)] \\
cov(\frac{X - \mu_x}{\sigma_x}, \frac{Y - \mu_y}{\sigma_y}) &= E[(\frac{X - \mu_x}{\sigma_x} - E[\frac{X - \mu_x}{\sigma_x}]) (\frac{Y - \mu_y}{\sigma_y} - E[\frac{Y - \mu_y}{\sigma_y}])] \\
&= E[\frac{1}{\sigma_x} (X - \mu_x - E[X-\mu_x]) \frac{1}{\sigma_y} (Y - \mu_y - E[Y - \mu_y])] \\
&= \frac{1}{\sigma_x \sigma_y} E[(X - E[X])(Y - E[Y])] \\
&= \frac{1}{\sigma_x \sigma_y} E[(X - \mu_x)(Y - \mu_y)] \\
&= \frac{1}{\sigma_x \sigma_y} cov(X,Y) \\
&= cor(X,Y)
\end{align*}
$$

Thus, we see that correlation is a scaled version of covariance. Using the Cauchy-Schwarz inequality, we can easily prove that correlation must, in fact, be between -1 and 1 as follows. The basic idea is that we can upper bound the covariance of two random variables by the product of their standard deviations. For a closer look at this, see this [post](https://sbhave77.github.io/blog/2018/cauchy-schwarz-in-different-contexts/) I wrote!

Ok, this seems to make sense. Basically, the idea is to scale the variables so if they are measured in different units, or their ranges are wildly different we can account for that and then take the resulting covariance.

Another way of thinking about this is to simply transform X and Y.

$$
X' = \frac{X - \mu_x}{\sigma_x}
$$

$$
Y' = \frac{Y - \mu_y}{\sigma_y} \\
$$

$$
\begin{align*}
cov(X',Y') &= E[(X' - \mu_{x'})(Y' - \mu_{y'})] \\
&= E[X'Y'] - E[X']E[Y'] \\
&= E[X'Y']
\end{align*}
$$

Thus, taking the correlation is the same as standardizing the random variables and in the context of a sampling of n points $$ {(x_i,y_i)}^n $$ and graphing them on a plot and then subsequently taking the sample correlation by finding the average of the product of the samples as follows:

$$
r = \frac{1}{n-1} \sum_{i=1}^n (\frac{x_i - \bar{x}}{s_x})(\frac{y_i - \bar{y}}{s_y})
$$

I find this a useful way to think about this visually.
