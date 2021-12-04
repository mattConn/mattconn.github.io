---
title: "Useful Formulas"
---

### Sum of consecutive numbers in base P  
$$
\sum_{i=0}^{n} P^i = \frac{p^{n+1}-1}{P-1}
$$
This counts the number of nodes in a tree of depth n with fanout P.

### Depth and number of leaves in tree, given number of nodes and fanout
$$
d = \log_f(2|v|+1)-1
$$  
$$
|leaves| = f^d
$$

### Sum of consecutive integers
$$
\sum_{n=1}^{k} n = \frac{n(n+1)}{2}
$$