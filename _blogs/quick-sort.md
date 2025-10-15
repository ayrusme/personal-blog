---
title:  "Quick Sort - My Learnings"
date:   2021-02-01
permalink: quick_sort
layout: content
categories: ["algorithms", "sorting"]
---

# Quick Sort

Quick Sort is a divide and conquer algorithm. We pick a pivot, we maintain two pointers, one on each side of the pivot, and we keep moving the pointers and swapping elements if they're supposed to be on the other side of the pivot. Basically, we're trying to maintain elements greater than the pivot on one side, and lesser than the pivot on the other side. This is the first implementation I've seen. Other implementations include choosing the last element of the array as the pivot. Let me learn this in-depth and document my learnings in this file. Is this too much? Don't worry, I've placed a line by line execution of quick sort below.

From a nice article in [GeeksForGeeks](https://www.geeksforgeeks.org/quick-sort/), I can see that there are four ways to pick the pivot. This means, I need to see the differences between these algorithms and weigh the pros and cons. Even without going in-depth into the topic, it is clear that the core of the algorithm is the partition function which sets the tone for the quicksort. Repeatedly keep partitioning the array until you hit 1-2 items and it becomes sorted. Time complexity is O(n log n) and the sorting happens in-place without consuming extra memory. Pretty sweet. I found one more good resource which explains this perfectly [QuickSort KC Ang](https://www.youtube.com/watch?v=MZaf_9IZCrc). Now, let's get to the problem statement. 

### Basic Working

If this is the `array [2, 5, 7, 3, 9, 1, 10, 6]`, then let's choose the `last element as the pivot element`. The index to denote the pointer of where the pivot should be will be marked as -1 ( one less than the first element of the array ) and the end of the loop will be marked as one less than the pivot's position ( len(arr) - 1 ). Let's call the first one's position as low and the pivot's position as pp and the length of the arr as high.

We're going to go from `j = low till high-1`. Our pivotPointer is set to arr[high] now. If the element at index j is less than or equal to the pivot, we will increment the low pointer and then swap the element at index low and element at index j.
Let's begin.

### Pseudocode ( Kind of )

```bash
> [2, 5, 7, 3, 9, 1, 10, 6]

low = 0, high = pp = 7, arr[pp] = 6

loop till j = high - 1 (6)

i = -1, j = 0
arr[j] = arr[0] = 2 <= arr[pp] => true. So we will increment the i index and then swap arr[i] and arr[j]
i = -1 + 1 = 0, j = 0
swap arr[i] and arr[j]
latest arr is the same since i = j = 0 so the swap does not happen
> [2, 5, 7, 3, 9, 1, 10, 6]

next iteration
now i = 0 and j = 1
arr[j] = 5 <= arr[pp] => true
increment i => i = 1
swap arr[i] and arr[j]
since i and j are same, swap does not happen
> latest arr [2, 5, 7, 3, 9, 1, 10, 6]

next iteration
now i = 1 and j = 2
arr[i] = 5
arr[j] = 7 <= pivot => false. continue.

next iteration
now i = 1 and j = 3
arr[i] = 5
arr[j] = 3 <= pivot => true.
increment i
i = 2
swap arr[i] and arr[j] => swap (7,3)
>latest arr [2, 5, 3, 7, 9, 1, 10, 6]

next iteration
now i = 2 and j = 3
arr[i] = 3
arr[j] = 7 <= pivot => false. continue.

next iteration
now i = 2 and j = 4
arr[i] = 3
arr[j] = 9 <= pivot => false. continue.

next iteration
now i = 2 and j = 5
arr[i] = 3
arr[j] = 1 <= pivot => true.
increment i
i = 3
arr[i] = 7
swap arr[i] and arr[j] => swap (7,1)
>latest arr [2, 5, 3, 1, 9, 7, 10, 6]

next iteration ( and the last iteration since j is now equal to high - 1 = 6 )
now i = 3 and j = 6
arr[i] = 1
arr[j] = 10 <= pivot => false. continue.

loop ends.

>latest arr [2, 5, 3, 1, 9, 7, 10, 6]

Now let us swap the elements arr[i+1] and pivot (arr[high]) to place the pivot in the right place.
i = 3
i + 1 = 4
arr[i+1] = 9
arr[high] = pivot = 6
swap(arr[i+1], arr[high])
>latest arr [2, 5, 3, 1, 6, 7, 10, 9]
```

Now if we notice, all the elements on the left side of the pivot are smaller than the pivot and all elements on the right side of the pivot are larger than the pivot. We can now call this array as partitioned. How to sort the rest of the array? Quite simply, call the same method again with the parameters slightly changed.

To sort the left side of the pivot
`quickSort(arr[], low, i-1)` 
Here param one is the array, param 2 is the starting index, param 3 is the ending index

To sort the right side of the pivot
`quickSort(arr[], i+1, high)` 
Here, param one is the array, param 2 is the starting index, param 3 is the ending index


Here is a working solution in javascript

<script src="https://gist.github.com/ayrusme/77adbc579df630291eebc02215763d5d.js"></script>

### Time Complexity 

This will keep sorting the array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm), making it optimised in terms of memory as well. And in each iteration, we are reducing the iteration size by half, so the time complexity will be `O(n) -> for the first partition * O(log N) -> for the subsequent sorting for smaller arrays`. Together, it is `O(N log N)`, an excellent time complexity for an algorithm.
