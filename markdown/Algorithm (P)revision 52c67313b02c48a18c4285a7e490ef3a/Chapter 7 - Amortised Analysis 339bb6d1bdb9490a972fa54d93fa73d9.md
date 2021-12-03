# Chapter 7 - Amortised Analysis

Amortised analysis is a method of analysing the costs associated with a data structure that averages the worst operations out over time.

There's a detailed handout about this from Cornell University: [http://www.cs.cornell.edu/courses/cs3110/2011sp/Lectures/lec20-amortized/amortized.htm](http://www.cs.cornell.edu/courses/cs3110/2011sp/Lectures/lec20-amortized/amortized.htm)

## An Example

The linear running time of a resizing operation is not as much of a problem as it might sound (although it can be an issue for some real-time computing systems). If the table is doubled in size every time it is needed, then the resizing operation occurs with exponentially decreasing frequency.

As a consequence, the insertion of $n$ elements into an empty array takes only $O(n)$ time in all, including the cost of resizing. We say that the insertion operation has $O(1)$ amortised run time because the time required to insert an element is $O(1)$ *on average*, even though some elements trigger a lengthy rehashing of all the elements of the hash table.

## Aggregate Method

Let $t_i$ be the time cost of $i$th operation. Then we have a total cost of time $T(n)=\sum_{i=1}^nt_i$. By calculating $T(n)/n$ we get the amortised run time, or time complexity.

Considering the data structure 'dynamic table': The initial length of our table (that is, an array) is 1. When the table is fulfilled, then we *expand* this table to twice as it's original length. Let $c_i$ be the cost of $i$th insertion, then

$$c_i=\left\{\begin{align*}i&,i=2^m,m\in\mathbb{N}\\1&,others\end{align*}\right.$$

Then the total cost of $n$ insert operations will be

$$\begin{align*}\sum_{i=1}^nc_i&=\sum_{i=1}^{\lceil \log_2n\rceil}2^i+\sum_{i\leqslant n,i\text{ is not a power of 2}}1\\&\leqslant n+\sum_{i=1}^{\lceil\log_2n\rceil}2^i\\&=2^{1+\lceil\log_2n\rceil}+n-1\\&\leqslant 3n=O(n)\end{align*}$$

By dividing this total cost by $n$ we get the amortised cost per insert is $O(1)$.

## Accounting / Banker's Method

The aggregate method directly seeks a bound on the overall running time of a sequence of operations. In contrast, the accounting method seeks to find a *payment* of a number of extra time units charged to each individual operation such that the sum of the payments is an upper bound on the total actual cost. 

Intuitively, one can think of maintaining a bank account. Low-cost operations are charged a little bit more than their true cost, and the surplus is deposited into the bank account for later use. High-cost operations can then be charged less than their true cost, and the deficit is paid for by the savings in the bank account. In that way we spread the cost of high-cost operations over the entire sequence. The charges to each operation must be set large enough that the balance in the bank account always remains positive, but small enough that no one operation is charged significantly more than its actual cost.

Take the dynamic table as an example. It costs 1 to *insert* an element and costs 1 another to *move* an element when the table is about to be expanded. Apparently a charge of 1 per operation won't work. When the charge is 2 then:

```
index      1  2  3  4  5  6  7  8  9  10 11 12
size       1  2  4  4  8  8  8  8  16 16 16 16
real_cost  1  2  3  1  5  1  1  1  9  1  1  1
cost       2  2  2  2  2  2  2  2  2  2  2  2
bank_acnt  1  1  0  1  -2 <-- Negative!
```

So a charge of 2 per operation won't work. What about 3?

```
index      1  2  3  4  5  6  7  8  9  10 11 12
size       1  2  4  4  8  8  8  8  16 16 16 16
real_cost  1  2  3  1  5  1  1  1  9  1  1  1
cost       3  3  3  3  3  3  3  3  3  3  3  3
bank_acnt  2  3  3  5  3  5  7  9  3  5  7  9
```

So a charge of 3 should just work. Then the amortised cost of insert operation is $O(1)$. The fact is that, the 3 units charged to $i$th item are spent as follows:

- 1 is used to insert the element itself immediately;
- 1 is used to move this element, when the table expands next time;
- 1 is donated to the first $2^k$ elements, where $2^k$ is the largest power of 2 not exceeding $i$. This charge is used to move those elements when the table expands next time. These elements don't have any deposit, for they have used them to pay for the expanding cost last time the table was expanded.

## Potential / Physicist's Method

Suppose we can define a potential function $\Phi$ on states of a data structure with the following properties:

- $\Phi(h_0)=0$, where $h_0$ is the initial state of the data structure.
- $\Phi(h_t)\geqslant 0$ for all states $h_t$ of the data structure occurring during the course of the computation.

Intuitively, the potential function will keep track of the pre-charged time at any point in the computation, which is analogous to the bank balance in the banker's method. But interestingly it depends only on the current state of our data structure.

The amortised time for a sequence of operations overestimates of the actual time by $\Phi(h_n)$, which by assumption is always positive. The total amortized time after $n$ operations is:

$$\sum_{i=0}^{n-1}c_i+\Phi(h_n)$$

Back in the dynamic table problem. We want a potential function with the following properties:

- $\Phi(h_i)=0$, when the table is totally new (just expanded);
- $\Phi(h_i)=n$, when the table is full (length is $n$).

We define $\Phi(h_i)=2n-m$, where $n$ is the number of elements, and $m$ is the length of the table. Now we can analyse the insert operation.

- Case 1: if $n < m$, then the actual cost is 1, $n$ increases by 1, $m$ doesn't change. In this way, the amortized cost is $1+2=3$.
- Case 2: if $n = m$, then actual time is $(n+1)$ for the whole table is doubled. Potential function, however, drops from $n$ to $2$, so the amortised cost is also $n+1+2-n=3$.

So the amortised cost of insert operation in dynamic table is 3, or $O(1)$.