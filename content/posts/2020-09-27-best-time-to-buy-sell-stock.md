---
title: "Best Time To Buy And Sell Stock"
date: 2020-09-27T02:59:12-04:00
draft: true 
---

![](/images/stock.png)

This leetcode problem poses the question: 
Given an array of prices for a single stock on different days, design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

(Try it here: https://leetcode.com/explore/interview/card/top-interview-questions-easy/92/array/564/)

Profit is defined as the difference of the price the stock was purchased for and the price at which it was sold. e.g.:
```
prices = [a,b,c,d,e]
profit = a-c # profit after buying on day 1 and selling on day 3
```
We want to maximize this value. It doesn't have to be between only 2 days; we can perform as many transactions as we like.

# Observations
This is like a backtracking problem I was given in my algorithms course: find if a sum can be made from elements in a set. Start summing the elements, and once the current sum exceeds the target, prune the current list of numbers and try the next element over.

I would like to use some sort of pseudo-mathematical notation to represent this sum of two elements (a & b) in some set (S).
```
sum(a,b in S)
```

Now we're looking for the difference:
```
sum(a, -b in S)
```
And the max sum:
```
max(sum(a, -b in S))
```
The above notation could be understood to mean:
```
S = [a,b,c,d,...]
sums = {sum(a,-b) | a =/= b in S}
target = max(A, B, C,... in sums)
```
An important note, this would account for buying and selling only once. I've narrowed the scope to make solving the problem easier. I'll extend the solution from a pair to a tuple later (we can make as many transactions as possible).