# Chapter 6 - Search

Dive down deep into the 'water' and find the buried 'treasure'.

## DFS & BFS

### Depth-first Search

Single-minded to the point of recklessness.

For DFS, the data structure stack is used. Firstly, we add the root node into the stack. For each turn, we pop the first element in the queue, check if it's the thing we want. If not, push it's children nodes and go for the next turn.

### Breadth-first Search

Live in the moment, you only live once.

For BFS, the data structure queue is used. Firstly, we add the root node into the queue. For each turn, we dequeue the first element in the queue, check if it's the thing we want. If not, enqueue it's children nodes and go for the next turn.

## Hill Climbing

Hill climbing is not a particular algorithm but a optimisation technique. Based on DFS, hill climbing uses a target function to decide which child node should be searched first. The goal of this procedure, is to optimise the target function in each turn of DFS. For instance, in the 8-Puzzle problem, define $f(x)$ as 'number of wrongly placed figure in the puzzle', and in each turn we choose the choose node $x$ with lowest $f(x)$. Then the search tree should be like:

![Untitled](Chapter%206%20-%20Search%200eb4ed3633a4422cbf09a4c391b7e68e/Untitled.png)

To apply the hill climbing DFS, just change the order when you're pushing children nodes into the stack by their $f(x)$ value. Nodes with higher $f(x)$ should be pushed earlier so that they'll be picked later.

## Best-First Search

A good cat is the one who can catch mice, regardless of its colour. 

Despite DFS and BFS, best-first search only cares how important a node is. In each turn of best-first search, we always pick the node with lowest target function value, no matter whether it's root or child. Instead of stack and queue, we use a heap maintained by a target function to store our nodes to be searched.

## Pruning

While we're performing searching, if a solution (may not be the best) has been worked out, then we'll know that the best solution should be no larger than it. Therefore, all branches whose current weight is higher than this solution can be pruned, for it's impossible to find final solution by them. For instance, find the shortest path from $v_0$ to $v_3$:

![Untitled](Chapter%206%20-%20Search%200eb4ed3633a4422cbf09a4c391b7e68e/Untitled%201.png)

Here goes the search tree while using DFS or BFS

![Untitled](Chapter%206%20-%20Search%200eb4ed3633a4422cbf09a4c391b7e68e/Untitled%202.png)

And if we use pruning, the search tree will be like:

![Untitled](Chapter%206%20-%20Search%200eb4ed3633a4422cbf09a4c391b7e68e/Untitled%203.png)

After a possible solution upper bound 5 is found, all branches with weight over 5 are pruned.

## Topological Sorting & Personnel Assignment Problem

### Topological Sorting

Given a partially ordered set (poset) $(S, \leqslant)$, try to find a topological sequence of $S$ s.t. $s_i$ is behind $s_j$ when $s_i\leqslant s_j$.

Take courses in HIT as an example. Given that

1. *Advanced Mathematics* should be taught before *Advanced Physics*.
2. *The C Programming Language* and *Discrete Mathematics* should be taught before *Data Structures*, while *Algorithm Design & Analysis* should be taught after *Data Structures.*

Then a possible topological sorting of those 6 courses is:

> AM → DM → CPL → DS → ADA → AP
> 

The topological sorted sequence can be worked out with a tree. The procedure is:

1. Prepare a root (empty, just a root);
2. Choose ALL elements without pre-elements in the poset as root's children nodes. Remove these elements in poset;
3. For each child node of root, recursively treat them as a root node.

![Untitled](Chapter%206%20-%20Search%200eb4ed3633a4422cbf09a4c391b7e68e/Untitled%204.png)

You will get all possible topological sequences in this tree.

### Personnel Assignment Problem

Assume that all jobs $j_i, i=1,2,\cdots,n$ are in a poset $J$, all people are in a set $P$, $\boldsymbol{C}_{ij}$ means the cost when $j_j$ is assigned to $p_i$, $\boldsymbol{X}_{ij} = 1$ means $j_j$ is assigned to $p_i$. We want to find the $\boldsymbol{X}$ to make $\sum_{i, j}\boldsymbol{C}_{ij}\boldsymbol{X}_{ij}$ minimum.

Our goal is to search the topological sort tree with the weight from matrix $\boldsymbol{C}$. To make it suitable for pruning, we need to pre-process the cost matrix:

![Untitled](Chapter%206%20-%20Search%200eb4ed3633a4422cbf09a4c391b7e68e/Untitled%205.png)

For each row of the matrix, minus a number to make sure there's at least one zero in that row; and after this operation, if there's a column which doesn't contain any zero, then minus a number to make sure this column has at least one zero. Now that we know the lower bound of our solution cost should be $3+12+26+3+10=54$.

The reason why we need do this is that, this operation flattens the costs' start point, but increases the differences between choices. Using this processed cost matrix to replace original cost matrix, pruning will be available for more cases, which incredibly improves the efficiency.

![Untitled](Chapter%206%20-%20Search%200eb4ed3633a4422cbf09a4c391b7e68e/Untitled%206.png)

## TSP Problem

Given a graph $G=(V, E)$. No vertex in $G$ has an edge towards itself. For each pair of vertices there's a non-negative weighted edge. Find a path starting from a vertex, visiting all vertices (at least) once, and going back to the initial vertex with the lowest summary of weight.

### Building of Solution Tree

We use a binary tree to store all possible solutions. Let the root node of this tree to be the set of all possible solutions, and our goal would be 'divide this binary tree recursively to find out the best answer'. 

Specifically, the divide procedure can be like this:

- For each node, an edge $(i, j)$ is selected;
- The left child tree of this node means the set of solutions containing edge $(i, j)$;
- The right child tree of this node means the set of solutions without edge $(i, j)$;
- The choose of $(i, j)$ is aimed at bringing the highest right child tree's weight lower bound.

Now we use an example to explain this.

Given this weight matrix:

![Untitled](Chapter%206%20-%20Search%200eb4ed3633a4422cbf09a4c391b7e68e/Untitled%207.png)

Minus some values to make at least one zero per row / column, we got:

![Untitled](Chapter%206%20-%20Search%200eb4ed3633a4422cbf09a4c391b7e68e/Untitled%208.png)

Now for each zero in this matrix, we pick one edge who brings the largest lower bound of right child tree's weight. For example, we check $(3,5)$: 

- The lowest value in row 3 is 1 ($(3,5)$ can't be considered);
- The lowest value in column 5 is 17.

Then the lower bound of $(3,5)$ is 18.

Check all zeros and it's easy for us to find $(4,6)$, who brings the lower bound of 32. Namely, our solution tree will be like this:

![Untitled](Chapter%206%20-%20Search%200eb4ed3633a4422cbf09a4c391b7e68e/Untitled%209.png)

Now it's high time to operate on both left child tree & right child tree recursively.

### Recursively Perform on Left & Right Child Tree

**For left child tree**: All edges starts from vertex 4 and ends at vertex 6 don't need to be considered again. So the new matrix is:

![Untitled](Chapter%206%20-%20Search%200eb4ed3633a4422cbf09a4c391b7e68e/Untitled%2010.png)

**For right child tree**: Edge $(4,6)$ is not included, so use $\infty$ to replace it:

![Untitled](Chapter%206%20-%20Search%200eb4ed3633a4422cbf09a4c391b7e68e/Untitled%2011.png)

Recurse again and again, using pruning, we will get this answer tree:

![Untitled](Chapter%206%20-%20Search%200eb4ed3633a4422cbf09a4c391b7e68e/Untitled%2012.png)

## A* Algorithm

### The Power of Estimation

Instead of pruning using a possible answer, A* algorithm uses a function to *estimate* the cost. In a search tree, we define that:

- $g(n)$ means the length of path from root to $n$.
- $h^*(n)$ means the cost of the optimal path from $n$ to our destination.
- $f^*(n)=g(n)+h^*(n)$, means the optimal cost when the path passes $n$.

Obviously, when we have just visited $n$, $h^*(n)$ is unknown for we don't know what's the 'optimal path'. However, we can find a function $h(n)$ to estimate $h^*(n)$. If $h(n)\leqslant h^*(n)$, then we have $f(n)=g(n)+h(n)\leqslant g(n)+h^*(n)$. We use this $f(n)$ to perform best-first algorithm, and we can also find our last best solution.

### Pathfinding Problem

Given this graph. Our goal is to find the shortest path from $S$ to $T$.

![Untitled](Chapter%206%20-%20Search%200eb4ed3633a4422cbf09a4c391b7e68e/Untitled%2013.png)

(Obviously you *can* use Dijkstra's algorithm)

We define our $g(n)$ and $h(n)$ as follows:

- $g(n)$ is just the length from $S$ to $n$;
- $h(n)$ means the lowest weighted edge starting from $n$. Apparently it follows our rule.

Now we perform the best-first algorithm and will get this search tree:

![Untitled](Chapter%206%20-%20Search%200eb4ed3633a4422cbf09a4c391b7e68e/Untitled%2014.png)