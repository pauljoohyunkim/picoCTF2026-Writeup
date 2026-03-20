# Black Cobra Pepper
Number of Points: 200

## Description
i like peppers. (change!) chall.py output.txt

## Hints
* Does this remind you of any other popular encryption?
* What purpose does the s-box serve?

## Analysis
This is an implementation of AES without S-Box.

Because the security of AES basically comes from S-Box being a "bent function" (or "maximally nonlinear function"),
eliminating S-Box means this implementation is a linear cipher.

In fact, this is a fully linear cipher, so one could write the encryptor function as an affine transformation:

```math
f(\mathbf{b}) =
\underbrace{
\begin{pmatrix}
a_{00} & a_{01} & \cdots & a_{0,255} \\
a_{10} & a_{11} & \cdots & a_{1,255} \\
\vdots & \vdots & \ddots & \vdots \\
a_{255, 0} & a_{255, 1} & \cdots & a_{255, 255}
\end{pmatrix}
}_{A}
\underbrace{
\begin{pmatrix}
b_0 \\
b_1 \\
\vdots \\
b_{255}
\end{pmatrix}
}_{\mathbf{b}}
+
\underbrace{
\begin{pmatrix}
k_0 \\
k_1 \\
\vdots \\
k_{255}
\end{pmatrix}}_{\mathbf{k}}
```
where each of the entries are in Galois field $GF(2)$ and multiplications and addition of numbers are done on $GF(2)$ as well.

Note also that $A$ is independent of the key used for this "weak AES",
as the key is merely being "added" in each round, which is a constant addition operation (hence only $\mathbf{k}$ depends on the key)

For analysis, first set key to be `00 00 00 ... 00`,
so that we can analyze the entries of $A$.

To find entries, a track is to encrypt 256 trivial messages,
that are:
* $\mathbf{e}_0^T = (1, 0, 0, \cdots, 0)$
* $\mathbf{e}_1^T = (0, 1, 0, \cdots, 0)$
* ...
* $\mathbf{e}_{255}^T = (0, 0, 0, \cdots, 1)$

Because of how matrices capture linear transformations, the output can be written as the columns, that is

```math
A = [\mathbf{e}_0 | \mathbf{e}_1 | \cdots | \mathbf{e}_{255}]
```

Then since a single plaintext-ciphertext pair is given, we can use that to find $\mathbf{k}$.

After determining $\mathbf{k}$, since we have now fully described the encryptor, one could write decryptor as $f^-1(\mathbf{c}) = A^{-1}(\mathbf{c} + \mathbf{k})$. (Note that addition is equal to subtraction in $GF(2)$)

## Solution
For implementation see [analysis.ipynb](./analysis.ipynb)
