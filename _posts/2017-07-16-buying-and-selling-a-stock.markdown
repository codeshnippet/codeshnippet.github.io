---
layout: post
title:  "Buying and selling a stock."
date:   2017-07-16 21:54:35 +0200
categories: interview algorithm
---

This post could be useful to both sides: the interviewer and the interviewee. But it mostly targets the former.
What makes a good technical interview question? In my opinion, most important are the following qualities:
- Simple to explain and understand;
- Problem has multiple solutions;
- Problem complexity can be easily increased or decreased;
<br>

Lets take a look at an example of a **good** technical interview question:  
> Design an algorithm that determines maximum profit that could have been made by buying and selling a single share over a given day range.
<br>

Usually, the question is followed by clarifications. An important one is:
> What would be the input parameters?

Lets say the input is an array of nonnegative floating point values, representing the starting price for each day.
Since stock price changes during the day, for simplicity, we'll assume, that purchase and sale always occurs in the beginning of the day.

An example of an input parameters could be:  
{% highlight javascript %}
[62, 63, 55, 59, 52, 54, 58, 46, 51, 50]
{% endhighlight %}
<br>

> What would be the output parameters?

We expect the algorithm to return the maximum profit value. For the input above that would be:  
{% highlight javascript %}
6
{% endhighlight %}  
(If we buy at 52 and sell at 58)

interviewee may suggest finding minimum and maximum values of the array and returning the difference.
But this approach is not going to work for cases when maximum price appears in the array before minimum price.
As you can see in the example above, neither 52 is a minimum, nor 58 is a maximum.

### Quadratic-time solution
At this point a naїve / brute-force algorithms would be a good starting point.

In order to find the maximum possible profit we could compare all possible day pairs, by looping through the array in two nested loops.

<script src="https://gist.github.com/codeshnippet/196052f4b83b83ed75e1e2d5e68c4481.js"></script>

Click on [NestedLoopCalculator.java]{:target="_blank"} to view full class code.

This approach has n^2 time complexity and will not work for big inputs.

### Linearithmic-time solution

A significantly quicker but more complicated solution would be applying Divide and Conquer algorithm design pattern.
- Lets split the input array into two subarrays [0:n/2) and [n/2:n)
- Compute the best result for both subarrays
- And combine these results

However we need to consider a situation when best buy and sell take place in separate subarrays.
When this is the case, buy must be in the minimum of the first subarray and sell has to be the maximum from the second subarray.

<script src="https://gist.github.com/codeshnippet/1f67d4e36271d1e4fc5f662cde9a67e3.js"></script>

Click on [RecursiveCalculator.java]{:target="_blank"} to view full class code.

I've read somewhere, that a good developer should be able to write that solution in 20-30 minutes.

### Linear-time solution

By looking into the combine step, you may have noticed, that the maximum profit that can be made on a specific day is determined by the minimum of the stock prices over the previous days.

Lets iterate through input array while keeping track of the minimum element seen so far.
If the difference between current element and minimum seen so far is greater than the maximum profit recorded so far, update the maximum profit.

<script src="https://gist.github.com/codeshnippet/e34c31c021dc3d4c8e5cd0113032f576.js"></script>

Click on [LinearCalculator.java]{:target="_blank"} to view full class code.

All of the solutions above use O(1) additional space.

### Candidate evaluation

A good candidate should be able to come up with the brute-force solution on his own, recursive and linear solution with minor hints. He should also be able to implement the recursive solution and explain the testing strategy.

[NestedLoopCalculator.java]: https://github.com/codeshnippet/BuySellStock/blob/master/src/main/java/com/codeshnippet/buysellstock/calculators/NestedLoopCalculator.java

[RecursiveCalculator.java]: https://github.com/codeshnippet/BuySellStock/blob/master/src/main/java/com/codeshnippet/buysellstock/calculators/RecursiveCalculator.java

[LinearCalculator.java]: https://github.com/codeshnippet/BuySellStock/blob/master/src/main/java/com/codeshnippet/buysellstock/calculators/LinearCalculator.java
