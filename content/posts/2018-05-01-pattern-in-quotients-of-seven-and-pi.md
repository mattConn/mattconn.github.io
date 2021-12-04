---
layout: post
title: "A Pattern in Quotients of Seven and Rational Pi"
date: 2018-05-01 08:39:58 -0400
comments: true
draft: true
categories: ['mathematics']
---
 $$ \frac{1}{7} \ = \ 0.\overline{142857}  $$  

A quotient of seven will always have this repeating pattern in the mantissa (the fractional portion beyond the decimal point). This pattern also "shifts" a certain predictable amount of digits every division. We could also think of a quotient of seven as a circular queue, with its mantissa "wrapping around", like a speedometer or certain type of combination lock.

<!--more-->

For every seven quotients, we can see the number of digit shifts (or rotations, in terms of the queue/lock comparison) repeats, marked in the table below of 14 quotients of 7, in the column labeled "offset from 1/7" and underlined in the "decimal" column:

$$
\begin{array}{c|l|c}
\text{Quotient} & \text{Decimal} & \text{Offset from} \ \frac{1}{7} \\
\hline
1/7 & 0.14285714285714285 & 0 \\
2/7 & 0.\underline{2857}142857142857 & 4 \\
3/7 & 0.\underline{42857}142857142855 & 5 \\
4/7 & 0.\underline{57}14285714285714 & 2 \\
5/7 & 0.\underline{7}142857142857143 & 1 \\
6/7 & 0.\underline{857}1428571428571 & 3 \\
7/7 & 1.0 & 0 \\
8/7 & 1.1428571428571428 & 0 \\
9/7 & 1.\underline{2857}142857142858 & 4 \\
10/7 & 1.\underline{42857}14285714286 & 5 \\
11/7 & 1.\underline{57}14285714285714 & 2 \\
12/7 & 1.\underline{7}142857142857142 & 1 \\
13/7 & 1.\underline{857}1428571428572 & 3 \\
14/7 & 2.0 & 0 \\
\end{array}
$$

Concatenating these offsets, you get the number:

$$ 0452130 $$

As an integer, this number is evenly divisible by 7, which is interesting.

$$ 452130 \ \div \ 7 \ = \ 64590 $$

Here is the Julia script I wrote to find these offsets:
```
# division by 7
# =============

master = string(1/7)

println("quotient, decimal, and offset from 1/7:")

last = '\0'
for i in 1:100
	current = string(i/7)
	last = current

	point = false # decimal point detection
	cindex = 1 # character index of quotient string current (1-indexed)
	
	# find decimal point to ignore
	while !point
		if current[cindex] == '.'
			point = true
		end

		cindex += 1
	end


	# count offset from master quotient
	masterOffset = 0
	while current[cindex] != master[3] && cindex < length(current)
		masterOffset += 1
		cindex +=1
	end


	println(i,"/7 : ",current, " : ", masterOffset)

end
```

## A Quotient of Seven as a Bad Rational Approximation of Pi

$$ \frac{22}{7} \ = \ 3.142857142857143 $$

This is one of the smaller rational approximations of pi, and it is a bad one; not because it only gets you two digits of pi (or pi rounded to one thousandth), but because it relates more to the nature of division by seven than to the nature of pi.  
<br>
Looking at the table shown earlier, quotients 1/7, then 8/7, both have the same mantissa as 22/7. We can then see that we can pick the offset from 1/7 that we want (from the available offsets 0, 4, 5, 2, 1 and 3), knowing the nature of division by 7, which can be modeled by an expression like the one below, the braces denoting the fractional part.

$$ \{ \frac{(7 \cdot n) \ + k}{7}  \}$$

Here, n denotes some integral value that will give us a multiple of seven, with k denoting an increase in this multiple necessary to give us a mantissa after division by seven.  
Then if we wanted to design a rational number that looked something like pi, we could consider that 1/7 has the digits 1 and 4 at the beginning of its mantissa. All that's left would be the integer 3 in front. Using the expression above, we would wind up with:

$$\{ \frac{(7 \cdot 3) \ + 1}{7} \}$$

This gets us our integer 3 and the mantissa we want through an addition of 1. As seen in the table, the first division by 7 out of any 7 divisions will have a mantissa starting with 1 and 4.

$$\{ \frac{22}{7} \} \ = \ \{\frac{1}{7} \}$$

If we wanted the next offset available (mantissa of 1/7 shifted 4 digits right) but the same integer part (3.0), we would use the same expression above, but add 2 instead of 1, to get the second offset, and so on.  
