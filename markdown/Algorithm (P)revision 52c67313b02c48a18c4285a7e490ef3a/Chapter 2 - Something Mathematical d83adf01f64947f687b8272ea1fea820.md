# Chapter 2 - Something Mathematical

Chapter 2 introduces the mathematical backgrounds of algorithm analysis.

## Order of a Function

The 'Order' of a function describes how fast the function grows. Usually we use three notations to describe it.

- $\Theta$ notation describes the exact order of a function. For example:
    - $f(x)=3x^2=\Theta(x^2)$;
    - $f(x)=3x^2+4x-3=\Theta(x^2)$ , where lower orders will be omitted when they come with items with higher order.
    
    ![Untitled](Chapter%202%20-%20Something%20Mathematical%20d83adf01f64947f687b8272ea1fea820/Untitled.png)
    
    This figure shows the asymptotic relationship between $f(n)$ and $g(n)$ when $f(n)=\Theta(g(n))$
    
- $O$ notation describes the asymptotic upper bound of a function. For example:
    - $f(x)=4x=O(x)=O(x^2)=\cdots$, in fact, all $x^k$ where $k\geqslant1$ are asymptotic upper bounds of $f(x)$.
    - And it's obvious that $f(x)=3x^2+4x-2=O(x^2)=\Theta(x^2)$. The $\Theta$ notation is stronger than $O$ notation.
    
    ![Untitled](Chapter%202%20-%20Something%20Mathematical%20d83adf01f64947f687b8272ea1fea820/Untitled%201.png)
    
    This figure shows the asymptotic relationship between $f(n)$ and $g(n)$ when $f(n)=O(g(n))$.
    
- $\Omega$ notation describes the asymptotic lower bound of a function, just something similar to the $O$ notation.

Besides $\Theta$, $O$ and $\Omega$, there're two another types of notations: $o$ and $\omega$. They are more strict than the upper-case notations, for instance:

- $f(x)=x^2=O(x^2)=O(x^3)$, but $f(x)=o(x^3)\neq o(x^2)$.
- $f(x)=x^4=\Omega(x^4)=\Omega(x^2)$, but $f(x)=\omega(x^2)\neq\omega(x^4)$.

There're the orders of some common functions:

$$\Theta(1)<\Theta(\lg{n})<\Theta(\sqrt n)<\Theta(n)<\Theta(n\lg n)<\Theta(n^k)<\Theta(k^n)<\Theta(n!)$$

where $k$ is a constant no smaller than 2.

**NOTICE**: Not all functions have asymptotic bounds.

### How to determine the orders' priorities of functions?

- By definition. For instance, $\Theta(g(n))=\{f(n)|\exist c_1, c_2>0, \exist n_0, \forall n>n_0,c_1g(n)\leqslant f(n)\leqslant c_2g(n)\}$. Use this definition to proof.
- L'Hospital's rule.

## Order of a Recursive Expression

Recursive expression like $T(n)=3T(\frac{n}{2})+4\Theta(1)$ can have their order calculated by following methods.

### Master Theorem (recommended)

When the expression looks like

$$T(n)=aT(\dfrac{n}{b})+f(n)$$

then the order of $T(n)$ can be directly calculated:

- Case 1: If $f(n)=O(n^{\log_b{a}-\varepsilon})$, where $\varepsilon$ is a constant larger than 0, then $T(n)=\Theta(n^{\log_ba})$. For example:
    - $T(n)=3T(\frac{n}{5})+\lg^2n$, since $\lg^2n=O(n^{\log_53})$, then $T(n)=\Theta(n^{\log_53})$.
    - $T(n)=10T(\frac{n}{3})+17n^{1.2}$, since $\log_310>\log_39=2>1.2$, then $T(n)=\Theta(n^{\log_310})$.
- Case 2: If $f(n)=\Theta(n^{\log_ba})$, then $T(n)=\Theta(n^{\log_ba}\lg n)$. For example:
    - $T(n)=T(\frac{2n}{3})+1$, since $\log_ba=\log_\frac{2}{3}1=0$, then $T(n)=\Theta(f(n)\lg n)=\Theta(\lg n)$.
- Case 3: If $f(n)=\Omega(n^{\log_ba+\varepsilon})$, then $T(n)=\Theta(f(n))$.

This figure indicates the relationship between $f(n)$ and $T(n)$. Note that when $f(n)$ falls in the red part then Master theorem will not work.

![Untitled](Chapter%202%20-%20Something%20Mathematical%20d83adf01f64947f687b8272ea1fea820/Untitled%202.png)

### Recursion Tree

A recursion tree is useful for visualizing what happens when a recurrence is iterated. Now let's draw the recursion tree of expression $T(n)=3T(\frac{n}4)+cn^2$ where $c$ is a constant.

![Untitled](Chapter%202%20-%20Something%20Mathematical%20d83adf01f64947f687b8272ea1fea820/Untitled%203.png)

$$\begin{align*}T(n)&=cn^2+\dfrac{3}{16}cn^2+(\dfrac{3}{16})cn^2+\cdots+(\dfrac{3}{16})^{\log_4n-1}cn^2+\Theta(n^{\log_43})\\&=\sum_{i=0}^{\log_4n-1}(\dfrac{3}{16})^icn^2+\Theta(n^{\log_43})\\&<\sum_{i=0}^{\infty}(\dfrac{3}{16})^2cn^2+\Theta(n^{\log_34})\\&=O(n^2)\end{align*}$$

(BTW: For this problem Master theorem works, though)