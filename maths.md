---
title: Maths
layout: page
---

Table of Contents
=================
  * [Geometric series](#geometric-series)
  * [Taylor series](#taylor-series)
  * [Gradient](#gradient)
  * [Scalar product](#scalar-product)
  * [Vector Norm](#vector-norm)
  * [Cauchy\-Schwarz inequality](#cauchy-schwarz-inequality)
  * [Law of Cosine](#law-of-cosine)
  * [Orthogonal vectors](#orthogonal-vectors)
  * [Projection of a vector on a line](#projection-of-a-vector-on-a-line)
  * [LU Decomposition (A = LU)](#lu-decomposition-a--lu)

### Geometric series
Geometric series is a special series where there is a constant ratio between two successive terms. The geometric series can be written in the form,

$$
S = \sum_{n=0}^{\infty} ar^{n} = a + ar + ar^2 + ar^3 + ... + ar^n
$$

where $a$ is a constant. Note that, $S$ is convergent i.i.f $|r| < 1$

__Example 1:__

Given $a = \frac{1}{2}$, $r = \frac{1}{2}$, we have,

$$
S_1 = \sum_{n=0}^{\infty} \frac{1}{2} (\frac{1}{2})^n = \frac{1}{2} + \frac{1}{4} + \frac{1}{8} + \frac{1}{16} + ...
$$

The ratio between two successive terms in $S_1$ is $\frac{1}{2}$.

__Example 2:__

Given $a = 1$, $r = r$, we have,

$$
S_2 = \sum_{n=0}^{\infty} r^n = 1 + r + r^2 + r^3 + ... + r^{n-1} + r^n
$$

Multiply both side by $r$,

$$
rS_2 = r + r^2 + r^3 + ... + r^n + r^{n+1}
$$

Then,

$$
S_2 - rS_2 = 1
$$

Thus, $S_2$ converges at

$$
S_2 = \frac{1}{1-r}
$$

__References :__
  1. [http://www.sosmath.com/calculus/geoser/geoser01.html](http://www.sosmath.com/calculus/geoser/geoser01.html)
  2. [https://en.wikipedia.org/wiki/Geometric_series](https://en.wikipedia.org/wiki/Geometric_series)
### Power series
The power series is a series in form of,

$$
P = \sum_{n=0}^{\infty} a_n (x - c)^n = a_0 + a_1(x - c) + a_2(x - c)^2 + ... + a_{n-1}(x-c)^{n-1} + a_n(x-c)^{n}
$$

where $a_n$ is a coefficient of $x_n$, $c$ is a constant

__References :__
  1. [http://tutorial.math.lamar.edu/Classes/CalcII/PowerSeriesandFunctions.aspx](http://tutorial.math.lamar.edu/Classes/CalcII/PowerSeriesandFunctions.aspx)
  2. [https://en.wikipedia.org/wiki/Power_series](https://en.wikipedia.org/wiki/Power_series)

### Taylor series
Taylor series is a series expansion of a function about a point, i.e., a function $f(x)$ can be approximated about a point $x = a$ by using Taylor series.

If $f(x)$ is differential in $(-\infty, +\infty)$, the Taylor expansion of $f(x)$ about a point $x = a$ has a form,

$$
f(x) = \sum_{n=0}^{\infty} \frac{f^{(n)}(a)}{n!} (x - a)^n
$$

where $f^{(n)}(x)$ is n-th derivative of $f(x)$, e.g., $f^{(1)}(x) = f'(x)$, $f^{(2)}(x) = f''(x)$.

__Example 1:__

Find the Taylor series of $f(x) = e^{-x}$ about $x=0$.

We have,

$$
\begin{align}
f(x) = e^{-x} & \rightarrow f(x = 0) = 1 \\
f'(x) = e^{-x} & \rightarrow f'(x = 0) = -1 \\
f''(x) = e^{-x} & \rightarrow f''(x = 0) = 1 \\
f^{(3)}(x) = e^{-x} & \rightarrow f^{(3)}(x = 0) = -1 \\
& \dots \\
f^{(n)}(x) = (-1)^{n} e^{-x} & \rightarrow f^{(n)}(x = 0) = (-1)^{n}
\end{align}
$$

__References:__
  1. [https://en.wikipedia.org/wiki/Taylor_series](https://en.wikipedia.org/wiki/Taylor_series)
  2. [http://tutorial.math.lamar.edu/Classes/DE/TaylorSeries.aspx](http://tutorial.math.lamar.edu/Classes/DE/TaylorSeries.aspx)
  3. [http://davidlowryduda.com/an-intuitive-overview-of-taylor-series/](http://davidlowryduda.com/an-intuitive-overview-of-taylor-series/)

### Gradient
The gradient vector $\nabla f$ of a function $f(x_1, x_2, ..., x_n)$ is denoted as,

$$
\nabla f =
\begin{bmatrix} \frac{\partial f}{\partial x_1} & \frac{\partial f}{\partial x_2} & \frac{\partial f}{\partial x_3} \dots \frac{\partial f}{\partial x_n} \end{bmatrix}
$$

Simply speaking, $\nabla f$ is simply a vector of _partial derivatives_ of $f$.

__Example 1:__

The $\nabla f$ of the function $f(x, y) = 2x^2y^2 + xy^2$ is

$$
\nabla f = \begin{bmatrix} 4xy^2 + y^2 & 4x^2y + 2xy\end{bmatrix}
$$

__References:__
  1. [http://mathworld.wolfram.com/Gradient.html](http://mathworld.wolfram.com/Gradient.html)
  2. [https://goo.gl/C5jBBs](https://goo.gl/C5jBBs)

### Scalar product
The scalar product (also known as _dot product_, _inner product) of two vectors $x = [x_1, x_2, ..., x_n]$ and $y = [y_1, y_2, ..., y_n]$ is defined as

$$
x \cdot y = x^Ty = \sum_{i=1}^{n} x_iy_i = x_1y_1 + x_2y_2 + ... + x_ny_n
$$

Another way to calculate the scalar product of two vectors $x,y$ is

$$
x \cdot y = \lVert x \rVert_2 . \lVert y \rVert_2 . cos \theta
$$

where $\theta$ is the angle between $x,y$.

Note that scalar product is different to _scalar multiplication_.

__Example:__

Suppose $x = [1, 2, 3]$, $y = [4, 5, 6]$. We have,

$$
x \cdot y = x^Ty = \sum_{i=1}^{3} x_iy_i = 1*4 + 2*5 + 3*6 = 32
$$

Alternatively, using R we can have,
```
> x
[1] 1 2 3
> y
[1] 4 5 6
> sum(x*y)
[1] 32
```

__References:__
  1. [http://livebooklabs.com/keeppies/c5a5868ce26b8125/73a4ae787085d554](http://livebooklabs.com/keeppies/c5a5868ce26b8125/73a4ae787085d554)
  2. [http://www.purplemath.com/modules/mtrxmult.htm](http://www.purplemath.com/modules/mtrxmult.htm)

### Vector Norm
Intuitively speaking, norm of a vector $x = [x_1, x_2, ..., x_n]$ is the _length_ of the vector in the space. Norm of the vector $x$ is denoted as $\lVert x \rVert$.

In middle school, the definition of $\lVert x \rVert$ is constraint within the formula $\lVert x \rVert = \sqrt{x_1^2 + x_2^2 + ... + x_n^2}$. However, indeed there are numerous ways to calculate the norm and that formula is just one of them.

The norm is universally called as $l_p-norm$. The $l_p-norm$ of $x$ is defined as,

$$
\lVert x \rVert_p = \sqrt[p]{\sum_{i=1}^{n} |x_i|^p}
$$

  - Given $p = 2$, we will have $l_2-norm$, which is also known as __Euclidean norm__. Specifically, we have,

$$
\begin{align}
\lVert x \rVert_2 = \sqrt{\sum_{i=1}^{n} |x_i|^2} & = \sqrt{x_1^2 + x_2^2 + ... + x_n^2} \\
& = \sqrt{x^Tx}
\end{align}
$$

  - Given $p=3$, we will have $l_3-norm$, which is,
$$
\lVert x \rVert_3 = \sqrt[3]{\sum_{i=1}^{n} |x_i|^3} = \sqrt[3]{x_1^3 + x_2^3 + ... + x_n^3}
$$

__Example:__

Suppose we have the vector $x = [3, 4, 5]$, the $l_2-norm$ (or Euclidean norm) of $x$ is

$$
\lVert x \rVert_2 = \sqrt{\sum_{i=1}^{n} |x_i|^2} = \sqrt{3^2 + 4^2 + 5^2} = 7.071
$$

Alternatively, using R, we can have,

```
> x <- as.matrix(c(3,4,5))
> x
     [,1]
[1,]    3
[2,]    4
[3,]    5
> norm(x, "F") #"F" for Euclidean form
[1] 7.071068
```
__References:__
  1. [https://rorasa.wordpress.com/2012/05/13/l0-norm-l1-norm-l2-norm-l-infinity-norm/](https://rorasa.wordpress.com/2012/05/13/l0-norm-l1-norm-l2-norm-l-infinity-norm/)
  2. [https://en.wikipedia.org/wiki/Norm_(mathematics)](https://en.wikipedia.org/wiki/Norm_(mathematics))

### Cauchy-Schwarz inequality
For any two vectors $x, y \in \mathbb{R}^n$, we have,

$$
x^Ty \leq \lVert x \rVert_2 . \lVert y \rVert_2
$$

__References:__
  1. [http://livebooklabs.com/keeppies/c5a5868ce26b8125/73a4ae787085d554](http://livebooklabs.com/keeppies/c5a5868ce26b8125/73a4ae787085d554)
  2. [https://en.wikipedia.org/wiki/Cauchy%E2%80%93Schwarz_inequality](https://en.wikipedia.org/wiki/Cauchy%E2%80%93Schwarz_inequality)

### Law of Cosine
The law of cosine (or cosine law) is defined as follows.

Given three vectors $x, y, z \in \mathbb{R}^n$, we have

$$
z^2 = x^2 + y^2 - 2 . x. y.cos\theta
$$

where $\theta$ is the angle between vectors $x, y$.

__References:__
  1. [https://www.mathsisfun.com/algebra/trig-cosine-law.html](https://www.mathsisfun.com/algebra/trig-cosine-law.html)

### Orthogonal vectors
Given two vectors $x, y \in \mathbb{R}^n$, $x,y$ are orthogonal i.i.f the __scalar product__ of $x,y$ equals 0. Mathematically, we have,

$$
x \perp y \leftrightarrow x \cdot y = x^Ty = 0
$$


### Projection of a vector on a line
The explanation is given in this [video](https://www.khanacademy.org/math/linear-algebra/matrix-transformations/lin-trans-examples/v/expressing-a-projection-on-to-a-line-as-a-matrix-vector-prod).

__References:__
  1. [https://www.khanacademy.org/math/linear-algebra/matrix-transformations/lin-trans-examples/v/expressing-a-projection-on-to-a-line-as-a-matrix-vector-prod](https://www.khanacademy.org/math/linear-algebra/matrix-transformations/lin-trans-examples/v/expressing-a-projection-on-to-a-line-as-a-matrix-vector-prod)

### LU Decomposition (A = LU)
LU decomposition simply refers to the transformation of the square matrix $A$ into the product of two different matrices __$L$ (lower triangular matrix)__ and __$U$ (upper triangular matrix)__. Mathematically, we have $A = LU$.

__Example:__

$$
\begin{align}
A = \begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \\ 7 & 8 & 9 \end{bmatrix} & = LU \\
& = \begin{bmatrix} 1 & 0 & 0 \\ 4 & 1 & 1 \\ 7 & 2 & 2 \end{bmatrix}\begin{bmatrix} 1 & 2 & 3 \\ 0 & -3 & -6  \\ 0 & 0 & 0 \end{bmatrix}
\end{align}
$$

By using R, we can also have,
```
> library(pracma)
> x <- matrix(c(1, 2, 3, 4, 5, 6, 7, 8, 9), nrow = 3, byrow = TRUE)
> x
     [,1] [,2] [,3]
[1,]    1    2    3
[2,]    4    5    6
[3,]    7    8    9
> lu(x)
$L
     [,1] [,2] [,3]
[1,]    1    0    0
[2,]    4    1    0
[3,]    7    2    1

$U
     [,1] [,2] [,3]
[1,]    1    2    3
[2,]    0   -3   -6
[3,]    0    0    0
```

__References:__
  1. [https://autarkaw.org/2008/06/04/lu-decomposition-takes-more-computational-time-than-gaussian-elimination-what-gives/](https://autarkaw.org/2008/06/04/lu-decomposition-takes-more-computational-time-than-gaussian-elimination-what-gives/)
  2. [http://math.stackexchange.com/questions/266355/necessity-advantage-of-lu-decomposition-over-gaussian-elimination](http://math.stackexchange.com/questions/266355/necessity-advantage-of-lu-decomposition-over-gaussian-elimination)
