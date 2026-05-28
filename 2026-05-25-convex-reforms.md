
  <script type="text/javascript" defer
    src="https://cdn.jsdelivr.net/npm/mathjax@4/tex-chtml.js">
  </script>


## Introduction

There are many optimization problems whose straightforward formulation of the objective function leads to a non-concave function that cannot be solved with a convex solver. Some of these problem classes can be reformulated in a way leading to convex problems resp. convex objective functions. Here we will describe some of these problems and how they can be transformed into convex problems in the R programming language and be solved with solvers such as CVXR.


## Ratio of two linear functions

The problem to minimize a ratio of two linear functions pops up everywhere in finance (e.g., minimizing the Sharpe ratio, which is return divided by variance/risk), operations research, and machine learning (e.g., precision-recall tradeoffs).

#### 1. The "obvious" non-convex formulation

Suppose you want to minimize the ratio of two linear functions subject to some linear constraints. The obvious formulation is:

$$
\begin{aligned}
\min_{x \in \mathbb{R}^n} \quad & f(x) = \frac{c^T x + d}{e^T x + f} \\
\text{s.t.} \quad & Ax \leq b \\
& e^T x + f > 0
\end{aligned}
$$

**Why is this not convex?**. Even though the numerator and denominator are both linear (and therefore convex), their *ratio* is not. If you calculate the Hessian (the matrix of second derivatives) of $$f(x)$$, you will find that it is indefinite—meaning the function curves upwards in some directions and downwards in others. Its level sets are hyperbolas, not convex ellipsoids. A standard convex solver like CVX will reject this.
