---
layout: post
title:  "Sum of array elements in a range"
date:   2017-12-25 13:25:35 +0200
categories: interview algorithm
---

* TOC
{:toc}

### Why this task?
I like the following interview question, because:
- It is easy to explain and understand
- It has a simple straightforward solutions
- It triggers a conversation about time and space complexity
- It involves some math
- It requires familiarity with different data structures (array, HashMap, Binary Tree)
- You can make it harder or easier if needed

This question will give you some data to make a good hire / no-hire decision.

### The task
> Calculate sum of all array elements in a given range.

Usually, the question is followed by clarifications. For example:
> What would be the input parameters?

An array of integers and two more integers representing the [a, b) range.

> Can array elements be negative?

Yes.

Example input and output:

[4, 7, 2, -3, 1, 6], 2, 4 => -1

### Linear-time solution
A trivial and straightforward solution is to iterate through elements in the range summing them up on the way:

<script src="https://gist.github.com/codeshnippet/89c0fe8770e32b75ffa121b2273d0890.js"></script>

One of Java8 ways of doing this would be:

<script src="https://gist.github.com/codeshnippet/e6985de4656fad75cce4cf4efbeb542d.js"></script>

Too easy, right? Well it's time to make things harder.
Imagine that input array changes very rarely and we need to support super quick sum of elements range calculations.
Ask the candidate if he can do better then Liner time.

### Constant-time solution
We could cache the sums in a map.
For that we could use range as a key and sum as a value.

One of existing Range classes could be used: one from [Apache Commons Lang]{:target="_blank"}  or one from [Google Guava]{:target="_blank"}

<script src="https://gist.github.com/codeshnippet/84c68334cb6ed5dd507662139ee7f24d.js"></script>

You could ask candidate to implementing Range class himself.
Make sure that class is immutable and has correct hashCode() and equals() methods.
An example of Range class implementation can be found [here]{:target="_blank"}.

Now, assuming a good hash code function implementation, getting the sum value should be \\( O(1) \\).
But how much additional space will the cache take?
We have to find out how many ways we can pick two different elements out of n.
Since this is a combination, number of way can be calculated using following formula:
<br>

$$C(n,k) = \frac{n!}{(n-k)!k!}$$

Take a look at this [post]{:target="_blank"} if you want to find out where that formula comes from.

Replacing k with 2, we are getting:

$$C(n,k) = \frac{n!}{(n-2)!2!} = \frac{n!}{(n-2)!2} = \frac{n (n-1) (n-2)!}{(n-2)!2} = \frac{n(n-1)}{2}$$

That means our cache will take \\( O(n^2) \\) additional space.

If you still have time left, continue with the following task:
> Let's assume we cannot afford \\( O(n^2) \\) additional space and linear time complexity. Is there a solution in between?

###  Logarithmic time solution
We could build a Range Tree to store range sums. Here's a nice [video]{:target="_blank"} explaining the concept.
Warning: Our implementation differs form the one in the video in a way we treat ranges. Our [Range]{:target="_blank"} class has inclusive start and exclusive end.

[TreeNode]{:target="_blank"} class contains following fields:
- pointer to left sub-node
- pointer to right sub-node
- range of array elements
- sum of elements in the range

LogarithmicCalculator class contains a recursive method to build a Range Tree and a recursive method to find a sum of elements in a range:

<script src="https://gist.github.com/codeshnippet/a88000e6cea16d56ceb7f3065e579d0b.js"></script>

I would not expect the candidate to know the Range Tree data structure, but he should be able to grasp the concept and build the searching functionality with minor hints.

###  Test scenarios
If you still have some time left, discuss testing scenarios. You could use following for a reference:
- Calculate sum of elements when range starts in the beginning of the array
- Calculate sum of elements when range end in the end of the array
- Calculate sum of elements when range is in the middle of the array
- Calculate sum of elements when array contains a single element
- Verify an exception is thrown if null array is given as an input
- Verify an exception is thrown if empty array given as an input
- Verify an exception is thrown if range start is smaller than zero
- Verify an exception is thrown if range start is bigger than range end

[TreeNode]: https://github.com/codeshnippet/SumOfRange/blob/master/src/main/java/com/codeshnippet/sumofrange/models/TreeNode.java

[video]: https://www.youtube.com/watch?v=0l3xN3BpxHg

[here]: https://github.com/codeshnippet/SumOfRange/blob/master/src/main/java/com/codeshnippet/sumofrange/models/Range.java

[Range]: https://github.com/codeshnippet/SumOfRange/blob/master/src/main/java/com/codeshnippet/sumofrange/models/Range.java

[post]: https://betterexplained.com/articles/easy-permutations-and-combinations/

[Apache Commons Lang]: https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/Range.html

[Google Guava]: https://github.com/google/guava/wiki/RangesExplained
