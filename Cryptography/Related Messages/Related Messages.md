# Related Messages
Number of Points: 200

## Description

## Hints
* How are the two messages related?
* Franklin Reiter _______ _______ attack.

## Analysis
Franklin Reiter related message attack is possible when you are given two plaintexts and two ciphertexts encrypted with same RSA paraeters,
and the difference between plaintexts is known (or guessable).

Suppose given $(m_1, c_1), (m_2, c_2)$, with the information that $\Delta = m_2 - m_1$.

Then the following holds.
```math
\begin{align}
c_1 &\equiv m_1^e \mod n \\
c_2 &\equiv m_2^e = \left(m_1 + \Delta \right)^e \mod n
\end{align}
```

Consider the following two polynomials.
```math
\begin{align}
p_1(x) &= x^e - c_1 \\
p_2(x) &= \left(x + \Delta \right)^e - c_2
\end{align}
```
Then it is trivial that $p_1(m_1) = p_2(m_1) = 0 \mod n$

By factor theorem,
$p_1(x)$ and $p_2(x)$ both have $x-m_1$ as a factor modulo $n$.

By finding GCD of $p_1$ and $p_2$ symbolically (using Euclidean algorithm).

## Solution
In practice, this is easy to compute, but a bit tricky to implement for those who are not familiar with polynomial rings,
as many GCD algorithms and polynomial division are not done modulo $n$ which is a composite number.

In my original solution, I've used sympy,
but in the newly polished [analysis.ipynb](./analysis.ipynb), I provide SageMath version.
