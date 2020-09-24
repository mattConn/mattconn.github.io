---
title: "Deleting Python Array Elements In-Place"
date: 2020-09-23T02:59:12-04:00
draft: false 
---

![](/images/python.png)

_Some notes on learning Python._

I want to delete certain Python elements "in-place"; no copies. To start, I will try to delete every element in the array:
```
# An array of integers 0 through 9.
array = [*range(10)]
```

The approach below will not work:

```
for i in array:
    del i
```

del is like pop, minus the return of the removed element. This will only delete the iterator copied locally to the scope of the loop.

Trying a more old-school loop with indices:
```
for i in range(len(array)):
    del array[i]
```
This throws an `IndexError: list assignment out of range`. Some investigation through printing:
```
for i in range(len(array)):
    print(i,'before del:',array)
    del array[i]
    print(i,'after del:',array,'\n')
``` 
Gives the following output:
```
0 before del: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
0 after del: [1, 2, 3, 4, 5, 6, 7, 8, 9] 

1 before del: [1, 2, 3, 4, 5, 6, 7, 8, 9]
1 after del: [1, 3, 4, 5, 6, 7, 8, 9] 

2 before del: [1, 3, 4, 5, 6, 7, 8, 9]
2 after del: [1, 3, 5, 6, 7, 8, 9] 

3 before del: [1, 3, 5, 6, 7, 8, 9]
3 after del: [1, 3, 5, 7, 8, 9] 

4 before del: [1, 3, 5, 7, 8, 9]
4 after del: [1, 3, 5, 7, 9] 

5 before del: [1, 3, 5, 7, 9]
```
And then finally comes to a halt and throws the same `IndexError`.

It now becomes clear... the last line printed shows what went wrong, and just how exactly we're out of range with index 5 in an array 5 elements long.

# Removing Based On Certain Conditions
Now I want to remove only odd numbers from my array:
```
for i in range(len(array)):
    if array[i] % 2 != 0:
        del array[i]
```
Again, `IndexError`. Maybe some investigative printing would help...
```
for i in range(len(array)):
    print(i,'before del:',array)
    
    if array[i] % 2 != 0:
        del array[i]
        
    print(i,'after del:',array,'\n')
```
Well, this doesn't even get to run. Unlike earlier, we don't even get to see on which iteration of the loop this fails; it throws an error right out of the gate.

This error points to our if statement, but if we replace the del with some other expression, this code will run fine:
```
for i in range(len(array)):    
    if array[i] % 2 != 0:
        print(array[i])
```
The above will print all odd numbers in the array. The issue must then lie in the del operation? It almost feels like its being "hoisted", in javascript parlance....

# Managing The Array Yourself
If you want something done right, you do it yourself! From a C and C++ perspective, this would be the only way to do most things. With a dynamic language like Python, this feels odd. But it works and there is no mystery:
```
length = len(array)
i = 0
while i < length:
    if array[i] % 2 != 0:
        del array[i]
        length -=1 
    i+=1
```
This is a bit more to type, and uses two variables that are useless beyond the scope of this operation (a waste). But when can cut that down to one variable; we don't have to store length:

```
i = 0
while i < len(array):
    if array[i] % 2 != 0:
        del array[i]
    i+=1
```
The length of the array updates as elements are deleted, and `len` returns the new length every time, so we only need to manage where we are in the array.

# The Easiest Way
```
for i in array:
    if i % 2 != 0:
        array.remove(i)
```
The remove method! This will do it. Not pop, but remove.