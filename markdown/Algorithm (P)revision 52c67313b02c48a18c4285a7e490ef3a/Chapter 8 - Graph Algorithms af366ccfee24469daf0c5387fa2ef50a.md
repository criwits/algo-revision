# Chapter 8 - Graph Algorithms

Graph is a kind of data structure which contains two sets—vertices set $V$ and edges set $E$. A graph $G(V, E)$ can be imaged as several cities and roads connecting them to each other. Many mathematicians have devoted themselves into the study of graph and therefore there're so many classic and interesting graph problem & algorithms.

## Single Source Shortest Path Problem (SSSP)

Given a weighted directed graph $G$, find the shortest path from a source vertex to a destination vertex. 

Apparently, **the shortest path contains problem has the optimal sub-structure**. Otherwise, we can find another shorter path to replace part of out 'shortest path' to get a shorter path, and that's impossible. 

### Relaxation & Bellman-Ford Algorithm

*Relaxation* is an important method widely used by many SP algorithms. Relaxation means, if the original shortest distance to a vertex $v$ is $\mathrm{d}[v]$, and we found that $\mathrm{d}[v]$ is larger than $\mathrm{d}[u]+\mathrm{distance}_{u\to v}$, then we update $\mathrm{d}[v]$ to $\mathrm{d}[u]+\mathrm{distance}_{u\to v}$. By doing this we *relaxed* the edge $u\to v$. The figure below shows this process.

![Left: Relax successfully; Right: Relax failed for 5+2>6](Chapter%208%20-%20Graph%20Algorithms%20af366ccfee24469daf0c5387fa2ef50a/Untitled.png)

Left: Relax successfully; Right: Relax failed for 5+2>6

The Bellman-Ford algorithm is just based on the relaxation method. 

- First, we create a set (array, in fact) to store the distance between the source vertex and other vertices. Obviously, $\mathrm{d}[v]=0$, and for other vertices $\mathrm{d}[i]=\infty$.
- Do this for $|V|-1$ times: for each edge $u\to v$ in $G$, if $\mathrm{d}[u]+\mathrm{distance}_{u\to v}<\mathrm{d}[v]$, then update $\mathrm{d}[v]$.
- Now check each edge $u\to v$. If there're still edges whose $\mathrm{d}[u]+\mathrm{distance}_{u\to v}<\mathrm{d}[v]$, return 'no solution'. This means there're negative circles in this graph, under which circumstance there's no shortest path.
    
    ![Negative circle](Chapter%208%20-%20Graph%20Algorithms%20af366ccfee24469daf0c5387fa2ef50a/Untitled%201.png)
    
    Negative circle
    

The figure below shows the progress of performing Bellman-Ford algorithm on a graph.

![Untitled](Chapter%208%20-%20Graph%20Algorithms%20af366ccfee24469daf0c5387fa2ef50a/Untitled%202.png)

### Dijkstra's Algorithm

The Bellman-Ford Algorithm has a time complexity of $O(|V||E|)$, which is not satisfying for us. As matter of fact, **if there're no negative edges in a graph**, the Dijkstra's algorithm works faster than Bellman-Ford algorithm. Dijkstra's algorithm takes the advantage of greedy algorithm, using a best-first strategy to choose vertices, and has a better time complexity than Bellman-Ford's.

- First, we create a set (array, in fact) to store the distance between the source vertex and other vertices. Obviously, $\mathrm{d}[v]=0$, and for other vertices $\mathrm{d}[i]=\infty$.
- Prepare a set $S$ to place the vertices we've visited; use a priority queue $Q$ to store the vertices that have not been used. At the beginning, $S=\empty$, $Q=V$.
- While $Q\neq\empty$ do this continuously: dequeue the vertex $u$ in $Q$ with the smallest $\mathrm{d}[u]$, enroll it into $S$, then for each vertex in $Q$ that linked to $u$ relax the edge between it and $u$.
- The $\mathrm{d}[t]$ is our final answer.

The figure below shows the process.

![Untitled](Chapter%208%20-%20Graph%20Algorithms%20af366ccfee24469daf0c5387fa2ef50a/Untitled%203.png)

- a)
    
    ```
    index     s    t    x    y    z
    d         0    inf  inf  inf  inf
    S         EMPTY      
    Q         s    t    x    y    z
    ```
    
- b)
    
    ```
    index     s    t    x    y    z
    d         0    10   inf  5    inf
    S         s
    Q              t    x    y    z
    ```
    
- c)
    
    ```
    index     s    t    x    y    z
    d         0    8    14   5    7
    S         s              y
    Q              t    x         z
    ```
    
- d)
    
    ```
    index     s    t    x    y    z
    d         0    8    13   5    7
    S         s              y    z
    Q              t    x         
    ```
    
- e)
    
    ```
    index     s    t    x    y    z
    d         0    8    9    5    7
    S         s    t         y    z
    Q                   x        
    ```
    
- f)
    
    ```
    index     s    t    x    y    z
    d         0    8    9    5    7
    S         s    t    9    y    z
    Q         EMPTY    
    ```
    

## Multiple Source Shortest Path Problem (MSSP)

Both Bellman-Ford algorithm and Dijkstra's algorithm can only solve the shortest from one source vertex to other vertices at once. If we want to work out the shortest paths from all vertices to all other vertices, we have some more algorithms, for example, Floyd-Warshall algorithm.

### Floyd-Warshall Algorithm

Floyd-Warshall algorithm uses a matrix (2-dimension array) to store shortest paths' distances. That is, $\mathrm{d}[i][j]$ will finally stores the length of shortest path between vertices $i$ and $j$. With the thought of relaxing again and again, Floyd-Warshall algorithm uses 3 for-loops to update this distance matrix.

This is the pseudo code for Floyd-Warshall algorithm (only uses one matrix):

```
for k = 1 to n do:
	for i = 1 to n do:
		for j = 1 to n do:
			if (d[i][j] > d[i][k] + d[k][j]) then:
				d[i][j] = d[i][k] + d[k][j];
```

Note that we can only get the *length* of the shortest paths, and the fact is that we want to know what those paths are. To store the shortest paths themselves, we use another matrix `p` to place the vertices.

```
p = { 0 };
for k = 1 to n do:
	for i = 1 to n do:
		for j = 1 to n do:
			if (d[i][j] > d[i][k] + d[k][j]) then:
				d[i][j] = d[i][k] + d[k][j];
				p[i][j] = k;
```

Then use this recursive function to print path from $i$ to $j$:

```
path(i, j):
	if (p[i][j] != 0) then:
		path(i, p[i][j]);
		println(p[i][j]);
		path(p[i][j], j);
		return;
	else return;
```

## Network Flows Problem

Network is a kind of directed non-negatively weighted graph. In a network there'll be a source vertex $s$ whose in-degree is zero, as well as a target vertex $t$ whose out-degree is zero.

Flow, however, is a 'function' in network $G(V, E)$ from $E$ to $\mathbb{R}$. For each $e\in E$, $0\leqslant f(e)\leqslant\ c_{e}$, where $c_{e}$ is the weight of edge $e$. For any vertex $v$ besides $s$ (source) and $t$ (target), $\sum_{e\to v}f(e)=\sum_{v\to e}f(e)$. And for $s$ and $t$, $\sum_{e\to t}f(e)=\sum_{s\to e}f(e)$.

![Left: Network; Right: Flow.](Chapter%208%20-%20Graph%20Algorithms%20af366ccfee24469daf0c5387fa2ef50a/Untitled%204.png)

Left: Network; Right: Flow.

Let $f^{in}(t)$ be the summary of in-edges of vertex $t$, $f^{out}(t)$ be the summary of out-edges of vertex $t$. Given a network $G(V, E)$, find a solution to calculate the maximum $f^{in}(t)$. Namely, our goal is to find a maximum summary of flows in the given network.

Imagine that the network is a system made up with several PVC tubes. For each tube it has a maximum capacity. Pour water at a speed into one side of this tube system, and undoubtfully water will come out from the other side, with a specific speed for the limitations of tubes. Our goal is to find the limited speed.

### Ford-Fulkerson Algorithm

The Ford-Fulkerson algorithm is aimed at find the maximum flows of a network.

- Use DFS or other methods to find a flow (*augmenting path*) from $s$ to $t$. Intuitively, the capacity of this flow $f$ is limited by the edge with smallest weight.
- Now, we need to get the *residual graph* of this flow. For each edge $e(u\to v)$ on this flow:
    - Change its weight to $c_{e}-f$. If $c_{e}=f$, then this edge is removed.
    - Change weight of its reversed edge $e'(v\to u)$ to $f$.
- Perform DFS (or other thing you'd like) on the new graph again and again, until you cannot even find one more new flow.

An example:

![(a) Left: The network, and an augmenting path; Right: The residual graph. Sic passim.](Chapter%208%20-%20Graph%20Algorithms%20af366ccfee24469daf0c5387fa2ef50a/Untitled%205.png)

(a) Left: The network, and an augmenting path; Right: The residual graph. Sic passim.

![(b)](Chapter%208%20-%20Graph%20Algorithms%20af366ccfee24469daf0c5387fa2ef50a/Untitled%206.png)

(b)

![(c)](Chapter%208%20-%20Graph%20Algorithms%20af366ccfee24469daf0c5387fa2ef50a/Untitled%207.png)

(c)

![(d)](Chapter%208%20-%20Graph%20Algorithms%20af366ccfee24469daf0c5387fa2ef50a/Untitled%208.png)

(d)

![(e)](Chapter%208%20-%20Graph%20Algorithms%20af366ccfee24469daf0c5387fa2ef50a/Untitled%209.png)

(e)

![(f) Finally there's no more augmenting path.](Chapter%208%20-%20Graph%20Algorithms%20af366ccfee24469daf0c5387fa2ef50a/Untitled%2010.png)

(f) Finally there's no more augmenting path.

## Matching Problem

Reference: [https://blog.csdn.net/dark_scope/article/details/8880547](https://blog.csdn.net/dark_scope/article/details/8880547) as well as [https://www.cnblogs.com/cruelty_angel/p/10808729.html](https://www.cnblogs.com/cruelty_angel/p/10808729.html).

These two articles have explained this problem and Hungary algorithm clearly, so... omitted here.