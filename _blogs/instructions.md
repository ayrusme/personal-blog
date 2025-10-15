---
title:  "Instructions vs Loops"
date:   2019-02-07
permalink: instructions-vs-loops
categories: ["performance"]
layout: content
---
# More Instructions per loop vs More Loops per Instruction?

Would you execute `More Instructions in a single Loop` or `Have different loops running for different instructions?`

In order to find out the best way, I chose a simple problem, iterated the data for 500 Million list elements, and plotted a simple graph with the results. 

The problem statement is simple:
> Given an list, find the minimum element of the list and the maximum element of the list. 

Ironically, I know the minimum, and maximum element of the list as I am initializing it for the test case. But let us pass the list through two functions.

I am going to be using `Python` to run this example. I am going to plot the result with the help of `pandas` and `matplotlib`. One function will run a single loop per instruction, herewith referred to as `single`, as this guy is running single instruction in a loop. Let's call the other guy `multiple`, as that guy is going to run multiple instructions per iteration of the loop.

<script src="https://gist.github.com/ayrusme/7a6f2e1b9edcd56313c8b84b7bb6250a.js"></script>

The iterator was multiplied by a factor two with every iteration, and it took an average run time of three minutes with about three tries. The results were the same, and it indicated only one thing.

![results](assets/images/figure.png){:class="img-fluid"}

The lower the time taken, the faster the code is. It is evident that in lower data sets, it does not matter which type of execution style we choose because the time difference is negligible. But, as the time increases, the difference between the execution times is evident. 

I mean, it is logical, right? Let's imagine a real world scenario where I have to distribute pizza slices to all the members living in a street. There are eight houses in the street, with each house having three members. For each row, I try a different delivery approach. For the first row, I deliver to the first member of the each house till the last house, come back to the start, deliver to the second member, go back again to the start, and deliver to the third member.
I am exhausted.
I decide to rethink my strategy for the second row. I decide to deliver to all the members of the house at the same time when I am there. I finish all my instructions in the single iteration. Yay me!

With ample data, I can now confidently vouch for executing more instructions in the same iteration, as opposed to running different iterations with less instructions per iteration. 
