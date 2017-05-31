---
layout: post
title:  Thoughts on efficiency and context within programming
date:   2017-05-30 20:04:43 -0400
---


The idea of efficiency has been often misunderstood, and outside of the context of available hardware, one may be led to think that all concepts pertaining to efficiency are equal.  With the ideas of Big-O notation permeating our minds, one may be led to believe that efficiency is and only is Big-O.  For those that need a refresher on Big-O, [have a look here](https://rob-bell.net/2009/06/a-beginners-guide-to-big-o-notation/). Big-O, while a very important and key concept, doesn't tell the whole story and is one of the conceptual tools available to programmers.

I wanted to touch on some of the concepts that I feel are important for less-than-casual programmers to be thinking about when working with data sets and certain hardware limitations.


**Efficiency and memory**.  

While solving one of the curriculum labs that tests for Prime numbers, I embarked on a much different approach than used by the good folks at [Learn.co](http://www.learn.co).  The solution used the concept, the "[Seive of Aristophenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)". Without a deep dive into the process, essentially the "seive" is formed by generating a list of integers from 2 to the number in question.  I chose to use a different, but related method which essentially starts with a guess for the square root and then begins iterating through all of the integers up to and including the square root of the number of interest. (And since we couldn't use the math library, I had to program a square root method as well).

When I finished the lab, I then looked into refining my method as the challenge suggested, and duplicated the seive method and began testing.  What immediately stood out was that testing for Prime numbers of signficant size the time to build the seive began growing seemingly exponentially with the test number input. In fact, the size of 1x10^9 hit the memory limit and on 8 GB of RAM (while also utilizing swap memory), the problem could not be solved in any reasonable amount of time.  I let it run for several hours before quitting.  

With the method I had originally chosen, since no values were stored in memory other than that being tested, it proved to work so long as enough time to process the integers was available.  I tried one number, 105,557,999,999,978,742,919, which tied up my MacBook Pro for almost a day before I hit Control-C.  This number was later solved in 17 steps by altering the solution's approach (see below).

So, for something to think about with problems that utilize a lot of memory, efficiency begins to blur because processor and even Big-O become overtaken by memory issues. In fact, using the seive method, the computer spent most of its time in actually generating the test array! It's important to understand this when dealing with large sets of resident data stored in memory.

Here are a few recorded examples from the two methods:
```
Using the "seive" method:

#Measured memory usage: 938.0 MB
#Solved for 105557999 in 1261 steps.
#19.460000   0.720000  20.180000 ( 20.445541)

```
```
 Using the trial division method: 
 
#Measured memory usage: 5.0 MB
# The number 105557999 is NOT Prime in 1769 steps!
#   0.000000   0.000000   0.000000 (  0.000339)
```

The point here being, that while the first method used 508 more steps, the memory and processing time were several orders of magnitude improved. 

When looking at where the method fails, we can see:

```
Using the seive method:

#puts Benchmark.measure{prime?(1055579999)}
#Measured memory usage: 7.91 GB, max on my machine. Compressed: ~5 GB 99% CPU
#Cannot store the seive! The program does not proceed past this point, merely incrementally as swap is being utilized.
```

```
Using the trial division method:

#Measured memory usage: 5.0 MB
#The number 1055579999 is NOT Prime!
#  0.070000   0.070000   0.140000 (  0.170812)
```

In this example, avaliable memory and algorithm approach can be clearly shown to have a significant impact on solving a given problem! 


**Efficiency and method**.  

After digging around in the method itself, I began to notice a trend.  The divisor that would trigger a numbert to **not** be Prime always seemed to be hanging out closer to the lower end rather than the higher end of the square root estimate. Although I fully don't understand why just yet, I will say that by observation this seemed consistent. 

Why does this matter?  Simple. The goal of testing a Prime in either case, if the number is truly Prime, it will need to be tested for *every single integer value for remainderless division*, **however** to be **not** Prime, we only need prove one single case.  This becomes of clear advantage when searching for Primes by more quickly eliminating candidates.  To show this:

```
Prior to using bottom up integer divisor

#The number 10555799999997874291 is NOT Prime in 3248961857 steps!
#500.560000   2.350000 502.910000 (510.178326)

After using bottom up integer divisor

#The number 10555799999997874291 is NOT Prime in 7211 steps!
#0.010000   0.000000   0.010000 (  0.003260)
```

The difference is extremely clear!

And to be thorough, I went back to a comparison with the seive method:

```
Using the seive method:

#Solved for 101013 in 66 steps.
#  0.030000   0.000000   0.030000 (  0.025474)
#

Using the trial division method:

#The number 101013 is NOT Prime in 3 steps!
#  0.000000   0.000000   0.000000 (  0.000067)
```

The difference in method has shown signficant gains in both iterations *and* computational efficiency!



**Efficiency and correctness**

Of course, what good would a broken algorithm do for anyone? While ideally, all methods should be tested with all know combinations for completeless, I used the highest known Primes as test candidates.  Sure enough the method works:

```
#The number 67280421310721 is Prime in 8606701 steps!
#  0.300000   0.010000   0.310000 (  0.308551)
#
```

Thanks for reading, and if you want to have a look at my approach and comparsion code, you can find this on my Github page: https://github.com/zoebisch/prime-ruby-v-000
