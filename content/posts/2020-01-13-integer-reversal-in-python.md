---
title: "Integer Reversal in Python"
date: 2020-06-28T20:39:40-04:00
draft: false 
categories: ['mathematics','python']
---

I’m doing Project Euler right now and problem 4 asks you to find a palindromic number. A palindrome is a word (or a number) or phrase that when reversed, remains the same.
So some integer n is a palindromic number when n = reverse(n). I wrote a palindrome checking function relying on stringification and lists:

```
# palindrome checker
def isPalindrome(n):
	# reverse n
	r = list(str(n)) # list from string
	r.reverse()
	r = r''.join(r) # cast back to string
	
	if str(n) == r: # compare strings
		return true

	return false
```

Feels silly but it gets the job done. It’s not lost on me that this is a big waste of storage and that string comparison when integers are already available is unnecessary.

# A Nicer Numerical Way To Reverse An Integer

A little bit of arithmetic will work and should be more efficient.

```
# reverse integer
def reverseInt(n):
	magnitude = int(math.log(n.10))

	result = 0

	while magnitude >= 0:
		result += (n % 10) * 10**magnitude
		magnitude -= 1
		n //= 10

	return result
```

The gory details below:

```
	magnitude = int(math.log(n.10))
```

Log base 10 will give you the length (minus 1) of the integer, or the magnitude. I truncate this float by casting to an integer.

```
		result += (n % 10) * 10**magnitude
		magnitude -= 1
```

I get the last digit of the integer by mod 10. For example, 123 / 10 = 12.3. The remainder is the last digit. Multiply this digit by the magnitude and then decrement the magnitude for the next digit. The result will look like, using 123 as an example: 300, then 300 + 20, then 300 + 20 + 1 (or 321).

```
n //= 10
```

Lastly, integer division (which is not the default in python 3) by 10 will shift the integer right and truncate.

# The New Palindrome Checker

```
# palindrome checker
def isPalindrome(n):
	if reverseInt(n) == n:
		return True
	
	return False
```

Looks better!
