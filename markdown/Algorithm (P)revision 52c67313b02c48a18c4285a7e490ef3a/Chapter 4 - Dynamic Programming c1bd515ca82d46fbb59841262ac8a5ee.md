# Chapter 4 - Dynamic Programming

Remember the path you've walked on, don't hesitate going back, and you can consider the overall situation.

## Applicability

Dynamic programming actually does these things:

- Divide the original problem into several sub-problems;
- Store the results of sub-problems into a table, and use them directly when necessary instead of calculating again;
- Calculate from bottom to top.

On contrary to Divide-and-Conquer, dynamic programming uses a table to store answers of sub-problems instead of recurrence again and again. In this way, dynamic programming is suitable for problems which have:

- Optimal sub-structure: The optimized answer of a problem contains the optimized answers of it's sub-problems.
- Overlapping sub-problem: Many answers of sub-problems will be used over once during the solution process of original problem.

Some classical problems solved with DP will be introduced in this chapter.

## Multiplication of Matrix Chain

If $\boldsymbol{A}$ is a $p\times q$ matrix and $\boldsymbol{B}$ is a $q\times r$ matrix, then the complexity of $\boldsymbol{A}\times\boldsymbol{B}$ is $pqr$.

Assume that $\boldsymbol{A}_1$ is $10\times100$, $\boldsymbol{A}_2$ is $100\times5$ and $\boldsymbol{A}_3$ is $5\times50$, then

- $T(\boldsymbol{A}_1\times(\boldsymbol{A}_2\times\boldsymbol{A}_3))=75000$, while
- $T((\boldsymbol{A}_1\times\boldsymbol{A}_2)\times\boldsymbol{A}_3)=7500$.

Given $n$ matrixes $\boldsymbol{A}_1, \boldsymbol{A}_2, \cdots, \boldsymbol{A}_n$, find the quickest way to calculate their multiplication.

### Optimal sub-structure

This problem can be divided into several sub-problem which follows the 'optimal sub-structure' rule. If the optimal answer of $\boldsymbol{A}_{1\to n}$ breaks down on $\boldsymbol{A}_k$, which means $\boldsymbol{A}_{1\to n}=\boldsymbol{A}_{1\to k}\times\boldsymbol{A}_{(k+1)\to n}$, then the answers two sub-problem $\boldsymbol{A}_{1\to k}$ and $\boldsymbol{A}_{(k+1)\to n}$ has to be optimal answers too, otherwise the $\boldsymbol{A}_{1\to n}$ won't be the best answer of a better sub-problem answer exists.

### Overlapping sub-problem

This problem also follows the rule 'overlapping sub-problem', which means the answers of sub-problems may be used over once. For example:

![Untitled](Chapter%204%20-%20Dynamic%20Programming%20c1bd515ca82d46fbb59841262ac8a5ee/Untitled.png)

### DP Matrix and Transfer Equation

Given that the problem has characteristics of DP-friendly problem, we can use DP to solve it. We use a 2-dimension array (actually matrix; 1-indexed) `dp` to solve some our useful numbers, where `dp[i][j]` means the lowest cost of multiplication $\boldsymbol{A}_{i\to j}$. Then `dp[1][n]` is what we want.

It's not hard to discover that:

- `dp[i][i]` should always be 0;
- `dp[i][j]` should be `dp[i][k] + dp[k+1][j] + p[i-1]*p[k]*p[j]`, where
    - `k` is a unknown number in $[i, j]$. We should find the `k` makes `dp[i][j]` minimum.
    - $\boldsymbol{A}_i$ is a `p[i-1]` $\times$ `p[i]` matrix.

So we can write a transfer equation of our `dp` matrix:

$$\mathrm{dp}[i][j]=\left\{\begin{aligned}0&,i=j\\\min_{k\in[i, j]}\{\mathrm{dp}[i][k]+\mathrm{dp}[k+1][j]+\mathrm{p}_{i-1}\mathrm{p}_k\mathrm{p}_j\}&,i<j\end{aligned}\right.$$

By filling the `dp` matrix from bottom to top we can work out the final answer.

![Untitled](Chapter%204%20-%20Dynamic%20Programming%20c1bd515ca82d46fbb59841262ac8a5ee/Untitled%201.png)

This figure shows how should we fill the `dp` matrix.

## Longest Common Subsequence

Longest common subsequence, or the LCS of two sequences, can be solved out using DP elegantly. 

### Optimal sub-structure

If $(z_1,z_2,\cdots,z_k)$ is the LCS of $(x_1, x_2, \cdots, x_m)$ and $(y_1, y_2, \cdots, y_n)$, then

- If $x_m= y_n$, then $z_k=x_m=y_n$, $(z_1,z_2,\cdots,z_{k-1})$ will be the LCS of  $(x_1, x_2, \cdots, x_{m-1})$ and $(y_1, y_2, \cdots, y_{n-1})$. Otherwise, there'll be a sequence longer than $(k-1)$ which is the LCS of $X_{1\to(m-1)}$ and $Y_{1\to(n-1)}$. Add $x_m$ to the tail of it, we'll get a sequence longer than $k$ which also is a LCS of $X_{1\to m}$ and $Y_{1\to n}$. It's impossible.
- If $x_m\neq y_n$, then
    - If $z_k\neq y_n$, then $Z$ will be the LCS of  $X_{1\to m}$ and $Y_{1\to(n-1)}$; or
    - If $z_k\neq x_m$, then $Z$ will be the LCS of  $X_{1\to (m-1)}$ and $Y_{1\to n}$.
    
    Otherwise, there'll be a sequence longer than $k$ which is LCS of $X$ and $Y$. It's impossible.
    

### Overlapping sub-problem

Apparent.

![Untitled](Chapter%204%20-%20Dynamic%20Programming%20c1bd515ca82d46fbb59841262ac8a5ee/Untitled%202.png)

### DP Matrix and Transfer equation

`dp[i][j]` means the length of LCS of $X_{1\to i}$ and $Y_{1\to j}$.

$$\mathrm{dp}[i][j]=\left\{\begin{aligned}0 &, ij=0\\\mathrm{dp}[i-1][j-1]+1 &, ij>0, x_i=y_j\\\max\{\mathrm{dp}[i][j-1], \mathrm{dp}[i-1][j]\} &,  ij>0, x_i\neq y_j\end{aligned}\right.$$

By filling `dp` we get our answer.

![Untitled](Chapter%204%20-%20Dynamic%20Programming%20c1bd515ca82d46fbb59841262ac8a5ee/Untitled%203.png)

This figure shows the DP matrix when finding the LCS of `amputation` and `spanking`, which is `pain`.

## 0-1 Knapsack Problem

The 0-1 knapsack problem can be explained as this mathematical form:

$$\left\{\begin{align*}opt.& \sum_{i=1}^nv_ix_i,\\s.t.&\sum_{i=1}^nw_ix_i\leqslant c, \\&x_i\in\{0, 1\}, 1\leqslant i\leqslant n\end{align*}\right.$$

### Optimal sub-structure

Assume that $(y_1, y_2, \cdots, y_n)$ is the optimal answer of a 0-1 knapsack problem, then $(y_2, y_3, \cdots, y_n)$ will be the optimal answer of the 0-1 knapsack problem without the first item. Otherwise, there'll be a better choice $(y_1, z_2, z_3, \cdots, z_n)$ instead of $(y_1, y_2, \cdots, y_n)$ as original problem's solution.

### Overlapping sub-problem

Obvious. ~~The fact is that I don't know how to describe such an obvious thing~~

### DP Matrix and Transfer Equation

We use `dp[i][j]` to indicate the sub-problem where only first $i$ items can be used and the maximum total value is $j$.

It's easy for us to get the transfer equation:

$$\mathrm{dp}[i][j]=\left\{\begin{align*}0&,j=0\\\mathrm{dp}[i-1][j]&,w_i>j\\\max\{\mathrm{dp}[i-1][j],\mathrm{dp}[i-1][j-w_i]+v_i\}&,w_i\leqslant j\end{align*}\right.$$

In this equation, $\max\{\mathrm{dp}[i-1][j],\mathrm{dp}[i-1][j-w_i]+v_i\}$ means to determine whether the item indexed $i$ should be taken or not.

By filling the `dp` matrix we get our answer at `dp[n][c]`(all indexes start at 1).

## Optimal BST

> BST = Binary Search Tree
> 

Optimal BST is the BST with lowest expectation when finding elements. 

![Untitled](Chapter%204%20-%20Dynamic%20Programming%20c1bd515ca82d46fbb59841262ac8a5ee/Untitled%204.png)

Given this BST, as well as some information:

- Nodes: $\{k_1, k_2, \cdots, k_n\}$;
- Leaves: $\{d_0, d_1, \cdots, d_n\}$;
- Probability of finding $k_i$: $p_i$;
- Probability of finding $d_i$: $q_i$.

Then the expectation:

$$E(T)=\sum_{i=1}^n(DEP(k_i)+1)p_i+\sum_{j=0}^n(DEP(d_i)+1)q_i$$

Given a series of keys, our goal is to find the optimal BST of them. Now a method to get optimal BST using DP is introduced. First, assume that $\mathrm{w}[i][j]$ means the summary of probabilities in the sub-tree containing $k_i, k_{i+1},\cdots,k_j$, namely the $\mathrm{w}[i][j]=\sum_{l=i}^jp_l+\sum_{l=i-1}^jq_l$. Then:

$$\mathrm{w}[i][j]=\left\{\begin{align*}q_{i-1}&,j=i-1\\\mathrm{w}[i][j-1]+p_j+q_j&,others\end{align*}\right.$$

Then we let matrix $\mathrm{E}[i][j]$ store the expectations. That is, $\mathrm{E}[i][j]$ means the optimal expectation of sub-tree containing $k_i,k_{i+1},\cdots,k_j$. Accordingly, the $\mathrm{E}[1][n]$ is the answer we want. And the recursive expression of $\mathrm{E}$ is:

$$\mathrm{E}[i][j]=\left\{\begin{align*}q_{i-1}&,j=i-1\\\min_{i\leqslant r\leqslant j}\{\mathrm{E}[i][r-1]+\mathrm{E}[r+1][j]+\mathrm{w}[i][j]\}&, j\geqslant i\end{align*}\right.$$

And we will use another matrix to store the $r$ in the formula above for $r$ means the root of sub-tree containing $k_i,k_{i+1},\cdots,k_{r-1}$ and sub-tree $k_{r+1},k_{r+2},\cdots,k_j$.

![Untitled](Chapter%204%20-%20Dynamic%20Programming%20c1bd515ca82d46fbb59841262ac8a5ee/Untitled%205.png)

For this optimal BST above, we have the three matrixes below:

![Untitled](Chapter%204%20-%20Dynamic%20Programming%20c1bd515ca82d46fbb59841262ac8a5ee/Untitled%206.png)

## Something Maybe Useful

The two dimensions of DP matrix are usually (1) The limitation of used 'elements' (such as coins, items, etc.), and (2) The limitation of kind of value (such as weight, total value, etc.).