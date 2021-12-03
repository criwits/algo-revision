# Chapter 5 - Greedy Algorithm

Always choose the best choice every single step. Never look back, the past is in the past.

## Applicability

The greedy algorithm may be used when the problem has the two characteristics:

- Optimal sub-structure, exactly the same thing in the previous chapter;
- Greedy choice property, we can make whatever choice seems best at the moment and then solve the subproblems that arise later.

If a problem doesn't have one of these characteristics, then using greedy algorithm will usually not get the best answer, for greedy algorithm often gets an answer too early.

## Activity Selection Problem

Now given several activities all need to use the same playground, and their scheduled start & end time are also known. Make an arrangement to get most activities arranged.

![Untitled](Chapter%205%20-%20Greedy%20Algorithm%208cdce6e028e54c35aecba39fc7c634fe/Untitled.png)

Use $a_i(i=1,2,\cdots,n)$ to present activities, $s_i$ is the start time of $a_i$ and $f_i$ is the end time of it. $S_{ij}$ means activities between time points $i$ and $j$. This problem can be solved using greedy algorithm.

### Optimal Sub-structure

Assume $a_k$ is in the maximum compatible activities subset of $S_{ij}$—$A_{ij}$, then $S_{ik}$ and $S_{kj}$ will also have their maximum compatible subsets $A_{ik}$ and $A_{kj}$. Otherwise, there'll be a better choice $A'$ in $S_{ik}$ ($S_{kj}$) which can make a better choice $A_{ij}'$. It's impossible.

### Greedy Choice Property

We follow this rule: always pick the activities with the earliest end time. For $S_{ij}$, we pick $f_m=\min\{f_k|a_k\in S_{ij}\}$. Then:

1. $S_{im}=\empty$, then the only non-empty sub-problem of $S_{ij}$ is $S_{mj}$. It's obvious.
2. $a_m$ will be included in one of $S_{ij}$'s maximum compatible activities subset. Proof:
    
    Assume $A_{ij}$ is a maximum compatible subset of $S_{ij}$. Now we sort the activities in it by their end time. Pick the earliest one $a_k$. Then:
    
    1. If $a_k$ is $a_m$, then $a_m$ is in $A_{ij}$. QED.
    2. Otherwise, use $a_m$ to replace $a_k$ in $A_{ij}$, and we get $A_{ij}'$. Because $a_m$ is the first activity to end in $S_{ij}$, which means $a_k$ will end no earlier than $a_m$, namely $A_{ij}'$ will also be a maximum compatible subset, and $a_m$ is in it. QED.

To sum up, by always choosing the first activity that ends first, we will get a maximum compatible subset of $S_{ij}$.

## Huffman Coding

The process of using Huffman coding is introduced in both *Discrete Mathematics* and *Data Structure*, so omitted here...

![Untitled](Chapter%205%20-%20Greedy%20Algorithm%208cdce6e028e54c35aecba39fc7c634fe/Untitled%201.png)

## Minimal Spanning Tree

A **minimum spanning tree** (**MST**) or **minimum weight spanning tree** is a subset of the edges of a [connected](https://en.wikipedia.org/wiki/Connected_graph), edge-weighted undirected graph that connects all the [vertices](https://en.wikipedia.org/wiki/Vertex_(graph_theory)) together, without any [cycles](https://en.wikipedia.org/wiki/Cycle_(graph_theory)) and with the minimum possible total edge weight. That is, it is a [spanning tree](https://en.wikipedia.org/wiki/Spanning_tree) whose sum of edge weights is as small as possible. (Quoted from Wikipedia)

![Untitled](Chapter%205%20-%20Greedy%20Algorithm%208cdce6e028e54c35aecba39fc7c634fe/Untitled%202.png)

Two major algorithms of finding MST of a undirected graph—Kruskal's algorithm and Prim's algorithm are all kinds of greedy algorithms.

### Kruskal's Algorithm

Always choose the edge with lowest weight which will not spawn a cycle.

- Greedy Choice Property: Assume that $uv$ is the edge with lowest weight in $G$, then there'll always be an MST containing $uv$. Otherwise, we can add $uv$ to any MST of $G$ and a cycle containing $uv$ will be spawned. Remove an edge whose weight is lower than $uv$, we'll get another Tree, whose weight is lower than the original MST. It's impossible.
- Optimal Sub-structure: Define such operation 'shrinking': replace edge $uv$ with a vertex $C_{uv}$ in $G$, connect those edges used to connected to $u$ and $v$ to $C_{uv}$. Assume that $T$ is an MST containing $uv$ in $G$, then $T-uv+C_{uv}$ will be an MST of $G-uv+C_{uv}$. That's obvious.

Then the Kruskal's algorithm works for it follows the rules above.

### Prim's Algorithm

Choose a vertex as root, then add the vertex who has the nearest distance to the root into the tree, and choose vertices who is closest to the tree one by one.

![Untitled](Chapter%205%20-%20Greedy%20Algorithm%208cdce6e028e54c35aecba39fc7c634fe/Untitled%203.png)

Proof of greedy choice property & optimal sub-structure is similar to the Kruskal's algorithm.