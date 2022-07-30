---
title: "P-Hacking Example"
layout: post
date: 2022-07-29 21:09:37
---
One of the most interesting lessons from the pandemic is the harm that can be caused by [p-hacking](https://en.wikipedia.org/wiki/Data_dredging).  A paper with errors related to p-hacking that hasn't been peer-reviewed is promoted by one or more people with millions of followers on social media and then some of those followers suffer horrible outcomes because they had a false sense of security.  Maybe the authors of the paper did not even realize the problem but for whatever reason, the social media rock stars felt the need to spread the misinformation.  And another very interesting lesson is that the social media rock stars seem to almost never issue a correction after the paper is reviewed and rejected.

To illustrate p-hacking with a non-serious example, I am using real public data with my experience attending drop-in hockey.

I wanted to know if goalies tended to show up more or less frequently on any particular day of the week because it is more fun to play when at least one goalie shows up.  I collected 85 independent samples.
  
For all days, there were 27 days with 1 goalie and 27 days with 2 goalies and 31 days with 0 goalies.

Our test procedure will define the test statistic X = the number of days that at least one goalie registered.  

I am not smart so instead of committing to a hypothesis to test prior to looking at the data, I cheat and look at the data first and notice that the numbers for Tuesday look especially low.  So, I focus on goalie registrations on Tuesdays.  Using the data above for all days, the null hypothesis $$ H_0 $$ is that the probability that at least one goalie registered on a Tuesday is 0.635.

For perspective, taking 19 samples for Tuesday would give an expected value of 12 samples where at least 1 goalie registered.

Suppose we wanted to propose an alternative hypothesis that p < 0.635 for Tuesday.  What is the rejection region of values that would refute the null hypothesis (p=0.635)?

Let's aim for α = 0.05 as the level of significance.  This means that (pretending that I had not egregiously cherry-picked data beforehand) we want there to be less than a 5% chance that the experimental result would occur inside the rejection region if the null hypothesis was true (Type I error).

For a binomial random variable X, the pmf b(x; n, p) is

$$ {n \choose x} p^x (1-p)^{n-x} $$

```
def factorial(n)
  (1..n).inject(:*) || 1
end

def combination(n,k)
  factorial(n) / (factorial(k)*factorial(n-k))
end

def pmf(x,n,p)
  combination(n,x) * (p ** x) * ((1 - p) ** (n-x))
end
```
The cdf B(x; n, p) = P(X $$\le$$ x) is

$$\sum_{i=0}^x b(i; n, p) $$

```
def cdf(x,n,p)
  (0..x).map {|i| pmf(i,n,p)}.sum
end
```

For n=19 samples, if x $$\le$$ 9 was chosen as the rejection region, then α = P(X $$\le$$ 9 when X ~ Bin(19, 0.635)) = 0.112

```
2.4.10 :001 > load 'stats.rb'
 => true 
2.4.10 :002 > cdf(9,19,0.635)
 => 0.1121416295262306 
```
This choice is not good enough because even if the null hypothesis is true, there is a large 11% chance (again, pretending I had not cherry-picked the data) that the test statistic falls in the rejection region.

So, if we narrow the rejection region to x $$\le$$ 8, then α = P(X $$\le$$ 8 when X ~ Bin(19, 0.635)) = 0.047

```
2.4.10 :003 > cdf(8,19,0.635)
 => 0.04705965393607316
```

This rejection region satisfies the requirement of a 0.05 significance level.

The n=19 samples for Tuesday are [0, 0, 0, 1, 0, 0, 0, 0, 1, 2, 0, 1, 1, 0, 1, 2, 2, 0, 0].

Since x=8 falls within the rejection region, the null hypothesis is (supposedly) rejected for Tuesday samples.  So I announce to my hockey friends on social media "Beware! Compared to all days of the week, it is less likely that at least one goalie will register on a Tuesday!"

![Nope, not how it works](https://coffeebucks.s3.amazonaws.com/nope_not_how_it_works.jpg)

Before addressing the p-hacking, let's first address another issue.  The experimental result was x = 8 which gave us a 0.047 probability of obtaining 8 or less days in a sample of 19 assuming that the null hypothesis (p=0.635) is true.  This result just barely makes the 0.05 cutoff by the skin of its teeth.  So, just saying that the null hypothesis was refuted with α = 0.05 does not reveal that it was barely refuted.  Therefore, it is much more informative to say the [p-value](https://en.wikipedia.org/wiki/P-value) was 0.047 and also does not impose a particular α on readers who want to draw their own conclusions.

Now let's discuss the p-hacking problem.  I gave myself the impression that there was only a 5% chance that I would see a significant result even if the null hypothesis (p=0.635) were true.  However, since there is data for 5 days (Monday, Tuesday, Wednesday, Thursday, Friday), I could have performed 5 different tests.  If I chose that same p < 0.635 alternative hypothesis for each, then there would similarly be a 5% chance of a significant result for each test.  The probability that all 5 tests would not be significant would be 0.95 * 0.95 * 0.95 * 0.95 * 0.95 = 0.77.  Therefore, the probability that at least one test would be significant is 1 - 0.77 = 0.23 (the [Family-wise error rate](https://en.wikipedia.org/wiki/Family-wise_error_rate)) which is much higher than 0.05.  That's like flipping a coin twice and getting two heads which is not surprising at all.  We should expect such a result even if the null hypothesis is true.  Therefore, there is not a significant result for Tuesday.

I was inspired to write this blog post after watching [Dr. Susan Oliver](https://twitter.com/DrSusanOliver1)'s [Antivaxxers fooled by P-Hacking and apples to oranges comparisons](https://www.youtube.com/watch?v=drSAsfuMkuw).  The video references the paper [The Extent and Consequences of P-Hacking in Science](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.1002106) (2015).

