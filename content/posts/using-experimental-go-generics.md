+++
title = "Using Experimental Go Generics"
date = 2018-07-04T13:19:13-04:00
draft = false
tags = ["generics"]
categories = []
+++

## Overview

There are at least two experimental implementations of Go that add generics (see note 1). I experimented with
[albrow/fo](https://github.com/albrow/fo). I don't know enough to classify the approached used by `fo`, 
but in simple terms, both type declarations and functions/methods are extended to take optional type parameters

I performed the experiment by porting some of the algorithms in 
`floyernick/Data-Structures-and-Algorithms`. These data structures and algorithms are all written
using integers and the Go developer
is expected to copy/paste/modify in order to use them for something other than integers.

Here is an example of reversing the elements of a slice in the generic form:
```go
func reverseArray[T](a []T) {
	i := 0
	u := len(a) - 1
	for i < u {
		a[i], a[u] = a[u], a[i]
		i, u = i+1, u-1
	}
}
```

Notice that type parameters are listed between square brackets and the
placeholder `T`, is used in the function.


Then to use it, you supply the actual type at the calling location:
```go
ia := []int{1,2,3} 
reverseArray[int](ia)
```

Again, notice the `[int]` after the function name, which specifies that T will be
an integer in this case. To use a string slice instead:
```go
reverseArray[string](ia)
```

This package has a number of limitations at present, but it is active and provides
enough to experiment. The syntax used is, to borrow a math term, topologically 
equivalent to many other approaches. By this I mean, they all share many of the same
pros and cons.

My experiments are in repo [mandolyte/fo-experiments](https://github.com/mandolyte/fo-experiments).

## Summary

On the positive side, the syntax was easy to understand and use. I was able to work-around
pretty easily all the issues I found. Since the package translates the `fo` code to `go` code,
I did not get a feel for how compile times might be affected by this approach. The translated Go 
code is saved and you can take a look if you wish.

The two issues I found are common to other approaches and have been discussed in detail elsewhere.
I found it valuable to encounter them in concrete ways. 

*The first issue is error handling.* The common Go idiom is to return two values with the second
value holding a boolean or an error object (or nil if no error). Thus, for boolean cases:

```go
return 0, false // integers
return "", false // for strings
return 0.0, false // for floats
return nil, false // for structs
```

Since there is no Go keyword that represents the zero value for all types, the generic code
cannot sensibly use this two-value return method. 
One solution might be to expand `nil` or introduce a new keyword to represent a generic zero-value.
Then, if a type was numeric, it might replace nil with zero. Of if a
string, with "". Of course, if the type was a struct, then `nil` works fine.

*The second issue is operator overloading.* Without overloading, the generic code cannot
use equality or inequalify operators. However, since Go has anonymous functions, the user
can write functions that perform the comparison and supply them to the generic code to use.
This approach works rather well and simplifies expressing how to compare two (x,y) pairs
(a Point type). Given my experience doing it this way, I think Go generics can easily 
live without operator overloading.

See my repo for other examples which include: 

- a circular buffer (i.e., a ring)
- a function to revers the elements in a slice
- a double linked list
- serveral slice search methods (linear, binary, and ternary)

Finally, here is a complete working example of a Bubble Sort, where the generic function
is used for integers, floating point numbers, and strings.

```go
package main

import "fmt"

func bubbleSort[T](gt func(T,T) bool, array []T) {
	swapCount := 1
	for swapCount > 0 {
		swapCount = 0
		for itemIndex := 0; itemIndex < len(array)-1; itemIndex++ {
			//if array[itemIndex] > array[itemIndex+1] {
			if gt(array[itemIndex], array[itemIndex+1]) { 
				array[itemIndex], array[itemIndex+1] = array[itemIndex+1], array[itemIndex]
				swapCount += 1
			}
		}
	}
}

func main() {
	ia := []int{4,2,3} 
	gti := func(x,y int) bool { return x > y }
	bubbleSort[int](gti, ia)
	fmt.Println(ia)
	
	sa := []string{"z", "b", "c"}
	gts := func(x,y string) bool { return x > y }
	bubbleSort[string](gts, sa)
	fmt.Println(sa)

	ra := []float64{4.4, 2.2, 3.3}
	gtf := func(x,y float64) bool { return x > y }
	bubbleSort[float64](gtf, ra)
	fmt.Println(ra)
	
}
```
(1) The other is [cosmo72/gomacro](https://github.com/cosmos72/gomacro)
