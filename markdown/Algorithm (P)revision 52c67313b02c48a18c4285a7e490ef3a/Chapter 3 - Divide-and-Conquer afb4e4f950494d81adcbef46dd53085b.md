# Chapter 3 - Divide-and-Conquer

Divide, conquer and combine.

## Divide-and-Conquer

There're 3 stages when performing any Divide-and-Conquer algorithms:

- **Divide** the main problem into several sub-problem which is similar to each other;
- **Conquer** those sub-problem (maybe recursive);
- **Combine** the answers of sub-problems, and the answer of the original problem will be worked out.

Some interesting problem solved with this thought:

### Multiplication of Large Integers

Given two binary large integers and calculate their multiplication. We can try to divide the given numbers into parts to reduce the complexity of calculation.

![Untitled](Chapter%203%20-%20Divide-and-Conquer%20afb4e4f950494d81adcbef46dd53085b/Untitled.png)

In this way $T(n)=4T(n/2)+\Theta(n)$, then $T(n)=O(n^2)$. And if we optimize it by using $(A-B)(D-C)+AC+BD$ to replace $AD+BC$, then $T(n)=3T(n/2)+\Theta(n)=O(n^{1.6})$, for we only need to calculate 3 multiplications instead of 4.

### Checkerboard Cover Problem

Use L-shaped cards to cover a whole $2^k\times2^k$ checkerboard without overlapping.

![Untitled](Chapter%203%20-%20Divide-and-Conquer%20afb4e4f950494d81adcbef46dd53085b/Untitled%201.png)

This problem can be solved recursively with a Divide-and-Conquer method:

![Untitled](Chapter%203%20-%20Divide-and-Conquer%20afb4e4f950494d81adcbef46dd53085b/Untitled%202.png)

And the complexity:

![Untitled](Chapter%203%20-%20Divide-and-Conquer%20afb4e4f950494d81adcbef46dd53085b/Untitled%203.png)

## Sorting Algorithms

Sorting means rearranging an array containing several numbers to a ordered sequence. There're mainly 4 types of sorting algorithms:

- **Insert**: Insert elements into an ordered sequence one by one to sort the whole array.
- **Select**: Select the minimum (or maximum) element one by one to sort the whole array.
- **Swap**: Swap some elements in original array to rearrange it.
- **Merge**: Merge two or more ordered sub-sequence to get the whole ordered sequence.

Sorting algorithms based on comparing have the lower bound of complexity $O(n\log n)$.