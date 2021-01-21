---
title: "Counting All Subgraphs in a Complete Graph with Adjacency Matrices"
date: 2021-01-20T20:36:18-04:00
---

![](/images/k6-graph.png)
_Complete graph \\(k_6\\)_


A complete graph \\(k_n\\) can be represented by a \\(n \times n\\) symmetric adjaceny matrix.

\begin{equation*}
k_n = 
\begin{pmatrix}
0 & e_1 & e_2 & ... & e_n \\\\
e_1 & 0 &  &  &  \\\\
e_2 &  & 0 &  &  \\\\
\vdots &  &  & \ddots &  \\\\
e_n &  &  & & 0 
\end{pmatrix}
\end{equation*}


The upper (or lower) triangle will have \\(\frac{{n(n-1)}}{2}\\) components. Flatten this triangle into a bit string, and then there are \\(2^{ \frac{{n(n-1)}}{2} }\\) spanning subgraphs. (\\(n\\) 0's to \\(n\\) 1's, which represent a triangle of all 0's to a triangle of all 1's.)

Then, apply this to all \\(k_n\\) from \\(n\\) down to 1.

$$
\sum_{k=1}^{n} 2^{ \frac{{k(k-1)}}{2} }
$$

This sum represents the number of subgraphs in \\(k_n\\).

---

## An example with \\(k_4\\):
![](/images/k4-graph.png)

\begin{equation*}
k_4 = 
\begin{pmatrix}
0 & 1 & 1 & 1 \\\\
1 & 0 & 1 & 1 \\\\
1 & 1 & 0 & 1 \\\\
1 & 1 & 1 & 0
\end{pmatrix}
\end{equation*}

Because the matrix is symmetrical, we only need to consider the upper triangular matrix \\(U_4\\).

\begin{equation*}
U_4 = 
\begin{pmatrix}
0 & 1 & 1 & 1 \\\\
0 & 0 & 1 & 1 \\\\
0 & 0 & 0 & 1 \\\\
0 & 0 & 0 & 0
\end{pmatrix}
\end{equation*}

Flatten this into a bit string of 6 bits:

$$
b(U_4) = 111111
$$

Where \\(| b(U_4) | = \frac{{n(n-1)}}{2} = \frac{{4(3)}}{2} = 6\\).
 
Then you can represent all spanning subgraphs as 000000, 000001, 000010, ... 111111.

6 bits can represent \\(2^{| b(U_4) |} = 2^{6}\\) graphs this way.

Then repeat these steps for \\(k_3, k_2, k_1\\). Then sum the following:

$$
\sum_{k=1}^{4} 2^{ | b(U_k) | } = \sum_{k=1}^{4} 2^{ \frac{{k(k-1)}}{2} } = 2^0 + 2^1 + 2^3 + 2^6 = 75
$$

There are 75 subgraphs in \\(k_4\\).