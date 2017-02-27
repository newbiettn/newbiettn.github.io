---
title: "Find $n$-th Fibonacci number using the power matrix"
author: "Ngoc Tran Trung"
date: "25 February 2017"
comments: yes
layout: post
---

#### I. Introduction

Calculating nth Fibonacci number is a classical problem that everyone knows. The most naive solution to that problem is recursive-based, which has the _exponential complexity_ $\mathcal{O}(2^n)$. A better solution, which uses iteration, theoretically has the _linear complexity_ $\mathcal{O}(n)$. When reviewing parts of linear algebra, I've realised that we can cut down the complexity to $\mathcal{O}(log n)$ by exploiting fundamental concepts of linear algebra, such as eigenvalues, eigenvectors, diagonal form of a matrix, and the power matrix.

#### II. Mathematical review

I often keep my maths notes apart in order to be able to review it quickly whenever I need. However, to provide a comprehensible and smooth reading, I want to add to this post a quick review of linear algebra concepts that are essential to the algorithm.

##### 1. $n$-th power of a square matrix

The power $A^n$ is defined as the matrix product of $n$ times of $A$.

$$
\begin{align}
A^n = A ... A \tag{$n$ times}
\end{align}
$$

##### 2. Eigenvalues and eigenvectors

Suppose we have a matrix $A$ and a vector $x$. If we multiply them together we will have a new output vector $Ax$. In this case, we can intuitively understand that $A$ is a function, $x$ is an input of the function, and $Ax$ is an output. There would be a lot of output vectors $Ax$, but we are particularly keen on output vectors that _has the same direction as the input vectors_. Our desire can be expressed by

$$
\begin{align}
Ax = \lambda x
\end{align}
$$

where $\lambda$ is a scalar and known as __the eigenvalue__, and $x$ is called as __the eigenvector__.

We would not specifically discuss on how to derive eigenvalues and eigenvectors here. Any keen reader can find it somewhere else.

Moreover, the eigenvector and eigenvalue of the power matrix $A^n$ is defined as

$$
\begin{align}
A^n x = \lambda^n x
\end{align}
$$

Hence, we can see that the eigenvector of $A^n$ is exactly the same as $A$, whereas the eigenvalues of $A^n$ are $\lambda_1^n, ..., \lambda_k^n$.

##### 3. The diagonal form of a matrix

Given the __eigenvector matrix__ $S$, which has the form $S = \begin{bmatrix} x_1 & x_2 & \cdots & x_n \end{bmatrix}$, where $x_1, ..., x_n$ are the eigenvectors of any _symmetric_ matrix $A$ with _distinct eigenvalues_ $\lambda_1, ..., \lambda_n$, we can use $S$ to _diagonalise_ A mathematically by

$$
\begin{align}
AS = S \Lambda
\end{align}
$$

where $\Lambda$ is __the eigenvalue matrix__, which has the form $$\Lambda =  \begin{bmatrix} \lambda_1 &  &  &  \\ & \lambda_2 &  &  & \\ \ &  &  \ddots & \\ & & & \lambda_n \end{bmatrix}$$

From that we can also have

$$
\begin{align}
AS = S \Lambda \quad \text{or} \quad A = S \Lambda S^{-1}
\end{align}
$$

Similar to $A$, the power matrix $A^n$ can also be diagonlised by $S$

$$
\begin{align}
A^n S = S \Lambda^n \quad \text{or} \quad A^n = S \Lambda^n S^{-1}
\label{abc}
\end{align}
$$

#### III. Finding $n$-th number of the Fibonacci sequence

In this section, we will discuss in detail how the power matrix is useful in helping deriving a faster calculation algorithm for the Fibonacci sequence.

An example of a first few elements of the Fibonacci sequence is

$$
\text{0, 1, 1, 2, 3, 5, 8, ...}
$$

The sequence rule is

$$
F_{k+2} = F_{k+1} + F_k
$$

Thus, if $$u_k = \begin{bmatrix} F_{k+1} \\ F_k \end{bmatrix}$$ then $F_{k+2} = F_{k+1} + F_k$ and $F_{k+1} = F_{k+1}$ becomes $$u_k = \begin{bmatrix} F_{k+2} \\ F_{k+1} \end{bmatrix} = \begin{bmatrix} F_{k+1} + F_k \\ F_{k+1} \end{bmatrix} = \begin{bmatrix} 1 & 1 \\ 1 & 0 \end{bmatrix} u_k$$

Thus, we can rewrite $$u_{k+1} = A u_k$$.

By following the rule, we can have

$$
u_1 = A u_0, u_2 = A^2 u_0, ..., u_{n} = A^n u_0
$$

From (\ref{abc}), we can have

$$
\begin{align}
u_n = A^n u_0 = S \Lambda S^{-1} u_0
\end{align}
$$

Our goal now is to compute $A^n$. When we have $A^n$, we can easily derive $F_n$.


__1. Find the eigenvectors and eigenvalues of $A$__

The eigenvalues of the matrix $$A = \begin{bmatrix} 1 & 1 \\ 1 & 0 \end{bmatrix}$$ are $\lambda_1 = \frac{1+\sqrt{5}}{2}$, $\lambda_2 = \frac{1 - \sqrt{5}}{2}$.

For the eigenvalues $\lambda_1$ and $\lambda_2$, we have the eigenvectors $$x_1 = \begin{bmatrix} \lambda_1 \\ 1 \end{bmatrix}$$, $$x_2 = \begin{bmatrix} \lambda_2 \\ 1 \end{bmatrix}$$.

__2. Find the eigenvector matrix $S$ and the eigenvalue matrix $\Lambda$__

The eigenvector matrix $$S = \begin{bmatrix} x_1 & x_2 \end{bmatrix} = \begin{bmatrix} \lambda_1 & \lambda_2 \\ 1 & 1 \end{bmatrix}$$

The eigenvalue matrix $$\Lambda = \begin{bmatrix} \lambda_1 & 0 \\ 0 & \lambda_2 \end{bmatrix}$$

__3. Find $S^{-1}$__

Given $$S = \begin{bmatrix} \lambda_1 & \lambda_2 \\ 1 & 1 \end{bmatrix}$$, we have $$S^{-1} = \frac{1}{\lambda_1 - \lambda_2} \begin{bmatrix} 1 & -\lambda_2 \\ -1 & \lambda_1 \end{bmatrix}$$

__4. Find $u_n$__

$$
\begin{align}
u_n &= A^n u_0 \\

&= S \Lambda^n S^{-1} u_0 \tag{$A^n = S \Lambda^n S^{-1}$}\\

&= \frac{1}{\lambda_1 - \lambda_2} \begin{bmatrix} \lambda_1 & \lambda_2 \\ 1 & 1 \end{bmatrix} \begin{bmatrix} \lambda_1 & 0 \\ 0 & \lambda_2 \end{bmatrix} \begin{bmatrix} 1 & -\lambda_2 \\ -1 & \lambda_1 \end{bmatrix} \begin{bmatrix} 1 \\ 0 \end{bmatrix} \tag{$u_0 = \begin{bmatrix} F_1 \\ F_0 \end{bmatrix} = \begin{bmatrix} 1 \\ 0 \end{bmatrix}$}\\

&= \frac{1}{\lambda_1 - \lambda_2} \begin{bmatrix} \lambda_1^{n+1} - \lambda_2^{n+1} \\ \lambda_1^{n} - \lambda_2^{n} \end{bmatrix}

\end{align}
$$

Because $$u_n = \begin{bmatrix} F_{n+1} \\ F_n \end{bmatrix}$$, we can have

$$
\begin{align}
\begin{bmatrix} F_{n+1} \\ F_n \end{bmatrix} = \frac{1}{\lambda_1 - \lambda_2} \begin{bmatrix} \lambda_1^{n+1} - \lambda_2^{n+1} \\ \lambda_1^{n} - \lambda_2^{n} \end{bmatrix}
\end{align}
$$

$$
\begin{align}
\Rightarrow F_n = \frac{\lambda_1^{n} - \lambda_2^{n}}{\lambda_1 - \lambda_2} = \frac{(1 + \sqrt{5})^n - (1 - \sqrt{5})^n}{2^n \sqrt{5}}
\label{fn}
\end{align}
$$

By using the formula (\ref{fn}), we can easily find the exact value of the $n$-th number of the Fibonacci sequence.

#### IV. Implementation
##### 1. Matrix-based implementation

The Github code snippet is an implementation of our algorithm in R. In this implementation, we fully exploit the matrix computation of R, hence, we do not explicitly use the formula given in (\ref{fn}) to derive it.

The sample `n` is set to 20.

<script src="https://gist.github.com/newbiettn/94a1d6a8ec4c17715e354ad26367f885.js"></script>

Run the code above and get the output:

```R
> f_n
[1] 6765
```

##### 2. Formula-based implementation
This implementation sole depends on the derived formula (\ref{fn}) to find the $n$-th Fibonacci number. `n` is also set to 20.

```R
> fibonacci_cal <- function(n){
+     lambda1 <- (1 + sqrt(5))/2
+     lambda2 <- (1 - sqrt(5))/2
+     return ((lambda1^n - lambda2^n)/(lambda1 - lambda2))
+ }
> fibonacci_cal(n = 20) #20-th Fibonnaci number
[1] 6765
```
