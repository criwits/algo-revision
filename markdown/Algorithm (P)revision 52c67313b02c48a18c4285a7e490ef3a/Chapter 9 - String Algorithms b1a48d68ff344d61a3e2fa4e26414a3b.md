# Chapter 9 - String Algorithms

In this chapter we'll talk about string-matching problem.

We formalise the string-matching problem as follows: Assume that the *text* is an string array $T[1,2,\cdots,n]$ of length $n$ and that the *pattern* is an string array $P[1,2,\cdots,m]$ of length $m$. We further assume that the elements of $P$ and $T$ are characters drawn from a finite alphabet $\Sigma$. Our goal is to find all valid shifts with which $P$ occurs in $T$.

![Untitled](Chapter%209%20-%20String%20Algorithms%20b1a48d68ff344d61a3e2fa4e26414a3b/Untitled.png)

## Naïve String-Matching Algorithm

Use two for-loops to find out the valid shift(s). Like:

![Untitled](Chapter%209%20-%20String%20Algorithms%20b1a48d68ff344d61a3e2fa4e26414a3b/Untitled%201.png)

The worst-case running time of this naïve string-matching algorithm is $\Theta((n-m+1)m)$. Thus, the time complexity of this algorithm is $O(mn)$, which is not a good algorithm, especially when the text is longer than we expected. 

## Knuth-Morris-Pratt Algorithm

The Knuth-Morris-Pratt algorithm, or KMP algorithm, is a high-efficiency string-matching algorithm. Compared to the naïve string-matching algorithm, the KMP algorithm takes advantage of the information that a failed matching gives us. On the contrary, In the naïve string-matching algorithm, when failure occurs, we just let pattern move forwards and ignore all the information this turn. 

### Finite State Machine in String-Matching

The essence of KMP algorithm is finite state machine, or FSM. With a given pattern, such as `ababaca`, we can generate a FSM to perform string-machine with:

![Untitled](Chapter%209%20-%20String%20Algorithms%20b1a48d68ff344d61a3e2fa4e26414a3b/Untitled%202.png)

There are 8 states of this FSM. S0 is the initial state, S7 is the final state, and the state transfers when a new character is scanned. A letter that is not presented in $P$ will let the FSM transfer to S0. 

By visiting [https://zhuanlan.zhihu.com/p/83334559](https://zhuanlan.zhihu.com/p/83334559) you may understand this better with a GIF animation to simulate this process.

### Prefix Table

The KMP algorithm uses a 'prefix table' to determine how many characters should be skipped when failed matching occurs. Usually the prefix table is an array whose length is the same as our pattern, and it's name is often `next`.

Given a pattern `ababaca`, then we can calculate the `next` array:

| a | b | a | b | a | c | a |
| --- | --- | --- | --- | --- | --- | --- |
| 0 | 0 | 1 | 2 | 3 | 0 | 1 |

This is how this table calculated:

- For `next[0]`, we focus on $P[0]$, which is `a`. The prefix and suffix of `a` are all $\empty$ (prefix: substrings without at least the last character; suffix: ...first...), so 0 for no common prefix & suffix.
- For `next[1]`, focus on $P[0..1]$, which is `ab`. The prefix is `a` while suffix is `b`. Apparently there're no common strings. So it's 0.
- For `next[2]`, focus on $P[0..2]$, which is `aba`. The prefixes are `a` and `ab` while suffixes are `a` and `ba`. `a` is the common item and its length is 1, so `next[2]` is 1.
- For `next[3]`, focus on $P[0..3]$, which is `abab`. The (longest) common string in both prefixes and suffixes is `ab`, so `next[3]` is 2.
- ...

### KMP Algorithm

For each turn of KMP matching, assume we have $m$ characters match successfully, which means the $(m+1)$th character failed to match correctly. In naïve string-matching algorithm, we will move our pattern 1 character forwards to try for the next turn. In KMP, we will move too, but not 1 character, but $m-\mathrm{next}[m]$ characters.

```
text:      ababcabdababacad
pattern:   ababaca
               ^ missed (m = 4), move 4-next[4]=2

text:      ababcabdababacad
pattern:     ababaca
               ^ missed (m = 2), move 2-next[2]=2

text:      ababcabdababacad
pattern:       ababaca
               ^ missed (m = 0), move 1

text:      ababcabdababacad
pattern:        ababaca
                  ^ missed (m = 2), move 2-next[2]=2

text:      ababcabdababacad
pattern:          ababaca
                  ^ missed (m = 0), move 1

text:      ababcabdababacad
pattern:           ababaca
													matched
```

## Boyer-Moore-Horspool Algorithm

Unlike naïve algorithm and KMP algorithm, BMH algorithm scans the pattern from tail to head. Instead of checking characters in text and pattern from left to right, BMH checks reversely. 

### Shift Table

BMH algorithm also uses kind of pre-processed table, called 'shift table' `shift`. The table can be worked out with the following formula. 

$$\mathrm{shift}[w]=\left\{\begin{align*}m-\max\{i<m|P[i]=w\}&,w\in P[1..(m-1)]\\m&,others\end{align*}\right.$$

For example, for pattern `kettle`, $m=6$:

- `shift[k]=5`, for the last time `k` appears in $P[1..(m-1)]$ is when $i=1$.
- `shift[e]=4`, for the last time `e` appears is when $i=2$. Note that the last `e` will not be considered.
- `shift[l]=1`.

### BMH Algorithm

Given text `abdacbacdbcacabcac` as well as pattern `abcac`. Now let's firstly work out the shift table.

| a | b | c | other letters |
| --- | --- | --- | --- |
| 1 | 3 | 2 | 5 |

And the following code block shows the whole BMH algorithm progress.

```
text:       abdacbacdbcacabcac
pattern:    abcac
              ^-^ missed, check `c`'s shift amount, move 2

text:       abdacbacdbcacabcac
pattern:      abcac
                  ^ missed, check `a`'s shift amount, move 1

text:       abdacbacdbcacabcac
pattern:       abcac
                 ^-^ missed, check `c`'s shift amount, move 2

text:       abdacbacdbcacabcac
pattern:         abcac
                     ^ missed, check `b`'s shift amount, move 3

text:       abdacbacdbcacabcac
pattern:            abcac
                    ^---^ missed, check `c`'s shift amount, move 2

text:       abdacbacdbcacabcac
pattern:              abcac
                          ^ missed, check `b`'s shift amount, move 3

text:       abdacbacdbcacabcac
pattern:                 abcac
                              matched
```