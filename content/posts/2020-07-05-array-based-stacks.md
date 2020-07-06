---
title: "Comparing Array-Based Stacks in C and Go"
date: 2020-07-05T20:36:18-04:00
---

I'll be comparing stack-based arrays in C and Go. There are many ways to implement a stack, but the aim here is for simplicity.

# Stacks in C
---
## Non-Functionial Approach
Filling a stack with integers 1 through 10.
```
#include <stdio.h>

int main()
{
        int stack[10];
        int top = -1; 

        int i = 1;
        while(top+1 < 10) 
        {   
                stack[++top] = i;
                i++;
        }   

	// print 
        for(int i=0; i < 10; i++) printf("STACK[%d]=%d\n",i,stack[i]); 


        return 0;
}
```
In need of a stack, this will do. The big deal here is `stack[++top]=i` which is our push functionality. Pop can be achieved similarly, and peek is `stack[top]`.

## Functional Approach
Things get more interesting with functions involved. The code is more resuable, unlike the code above which is more of a one-off.
```
#include <stdbool.h>
#include <stdio.h>

bool push(int arr[], int size, int *top, int element);

int main(){
        int stack[10];
        int top = -1; 

        int i = 0;
        while(push(stack, 10, &top, ++i)){}

	// print 
        for(int i=0; i < 10; i++) printf("STACK[%d]=%d\n",i,stack[i]); 

        return 0;
}

bool push(int arr[], int size, int *top, int element)
{
        if((*top)+1 == size) return false;
        arr[++(*top)] = element;

        return true;
}
```
Now the function will all the work in a while loop like `while(push(stack, 10, &top, ++i)){}`.

# Stacks in Go
---
## Without Functions
Nearly identical to C:
```
package main

import "fmt"

func main(){
        var stack [10]int                         
        top := -1 

        i := 1
        for top+1 < 10 {
                top++
                stack[top] = i
                i++
        }                     

	// print
        for i := range stack {fmt.Printf("STACK[%d]=%d\n",i,stack[i])}
}
```
There are some syntax differences such as Go's lack of a prefix increment operator which would make a terse statement like `stack[++top] = i;` possible in C.

It's also worth noting that the array declared as `var stack [10]int` is automatically zeroed-out.

## With Functions

```
package main

import "fmt"

func push(arr []int, top *int, element int) bool {
        if *top + 1 == len(arr) {return false}
        *top++
        arr[*top] = element

        return true
}

func main(){
        var stack [10]int
        top := -1

        i := 1
        for push(stack[:],&top,i) != false {i++}

        // print
        for i := range stack {fmt.Printf("STACK[%d]=%d\n",i,stack[i])}

}
```
We can put what is essentially a while loop to work again but this time, in Go syntax as `for push(stack[:],&top,i) != false {i++}`. We cannot increment our `i` variable here in our function call like we can in C unfortunately, so the body of our loop gets a single line.

There is a greater difference here, however. When passing the array to the `push` function, it must be passed as a slice (a dynamic subarray pointing to the actual array). The expression `stack[:]` creates this slice with the endpoints omitted.

To not pass in a slice, the function signature would look like `func push(arr [10]int, top *int, element int) bool`, where the size of the array must be explicitly stated and therefore only size 10 arrays would be allowed in this function.

# Go's Built-In Dynamic Arrays
Go will let you append to a slice with a statement like `stack = append(stack, i)`, which would manage the top of the stack for you, the catch being that this can only be done with slices and not statically sized arrays.
