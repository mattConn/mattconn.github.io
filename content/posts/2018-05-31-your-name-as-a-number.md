---
layout: post
title: "Your Name as a Number in Base-36 (Some Notes On Number Systems)"
date: 2018-05-31 08:39:58 -0400
comments: true
description: Your name (or any word) can be converted to a decimal number for fun and obfuscation.
image: name-as-number.png
categories: ['mathematics']
---

Your name (or any word) can be converted to a decimal number.  

Take my name. This could be read as a base-36 number: $ \  \mathrm{matthew}_{36} $   

While base-36 may sound unusual, it is just a number system that follows the same rules as any other; for any base $b$, the largest digit that can be represented before having to carry is $b-1$.  

For example, in decimal (base-10), we can count up to 9, but once we reach 10 (the base), we reset the current digit back to 0 and carry the 1. A base-36 number system would obey the same rules; once we count up to 35 ($b-1$), counting any further would require a digit carry until we reach 35 again.

<!--more-->

We would use base-36 because we need enough room in the number system for digits 0-9 and the digits for the letters of the alphabet (A-Z, which will represent decimal values 10-26); the sum of these bases (10+26) gives us our new base: 36.

As can be seen in hexadecimal (base-16), a number system that extends beyond base-10 will use letters to represent numeric values; the number 10 is A, 11 is B, and so on up to F for 15. Our base-36 number system will also have this property, only extending to 35, which will be represented by Z.

$$
\begin{array}{c|c}
\text{Base-10} & \text{Base-36} \\
\hline
0 & 0 \\
\vdots & \vdots \\ 
9 & 9 \\
10 & \mathrm{A} \\
11 & \mathrm{B} \\
12 & \mathrm{C} \\
\vdots & \vdots \\ 
33 & \mathrm{X} \\
34 & \mathrm{Y} \\
35 & \mathrm{Z} \\
\end{array}
$$

We're treating these letters as though they were digits (which they are now). Above is a table of base-10 numbers and their base-36 representation; a double-digit base-10 number can fit in a single base-36 digit.

If you want to convert your name or a word from base-36 to base-10 yourself, you can try the small REPL below that simply uses javaScript's `parseInt(string, base)` function, which takes a string and converts it to an integer in a given base.
<p data-height="233" data-theme-id="0" data-slug-hash="NzqBOO" data-default-tab="result" data-user="mattConn" data-embed-version="2" data-pen-title="Name to Base-10" class="codepen">See the Pen <a href="https://codepen.io/mattConn/pen/NzqBOO/">Name to Base-10</a> by Matthew Connelly (<a href="https://codepen.io/mattConn">@mattConn</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>
<br>
Converting the letters in my name from base-36 to base-10 yields:

$$[\mathrm{m,a,t,t,h,e,w}]_{36} \ = \ [22,10,29,29,17,14,32]_{10} $$

Then, to concatenate these base-10 digits, take the following sum:

$$ (22\cdot36^6) + (10\cdot36^5) + (29\cdot36^4) + (29\cdot36^3)+(17\cdot36^2)+(14\cdot36^1)+(32\cdot36^0)$$

$$ \implies \mathrm{matthew}_{36} \ = \ 48543957608_{10} $$

(Aside from being neat, this could make for some simple obfuscation).

We take this sum because a number in any base $b$ incorporating some digit(s) $n$ is represented as the following:

$$ \underbrace{\dots (n\cdot b^3)+(n\cdot b^2)+(n\cdot b^1)+(n\cdot b^0)}_{integer}\underbrace{.}_{point}\underbrace{(n\cdot b^{-1})+ (n \cdot b^{-2}) + (n \cdot b^{-3}) + (n \cdot b^{-4}) \dots}_{rational}$$

When we multiply a digit by a base to some power, this power sets the position of the digit. We can concatenate any digits we want using this property of number systems, as well as extract them and chop off trailing digits (truncation).

For example, for some arbitrary base-10 number, 215.25 (the price of a textbook maybe):

$$ 215.25 \ = \ (2\cdot10^2) + (1 \cdot 10^1) + (5\cdot10^0) + (2\cdot10^{-1})+(5\cdot10^{-2}) $$

Or for some base-36 number that looks like the word "hello":

$$ \mathrm{hello} \ = \ (\mathrm{h} \cdot36^4)+(\mathrm{e}\cdot36^3)+(\mathrm{l}\cdot36^2)+(\mathrm{l}\cdot36^1)+(\mathrm{o}\cdot36^0) $$
