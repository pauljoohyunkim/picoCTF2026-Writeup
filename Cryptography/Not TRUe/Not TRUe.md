# Not TRUe
Number of Points: 400

## Description
no. no. that's Not TRUe. that's impossible! Download encryption script and public info.

## Hints
* This is a lattice based cryptosystem. Is there a lattice attack that allows you to compute the private key given the public information?

## Analysis
This challenge is based on NTRU, a public key cryptosystem designed for post-quantum cryptography (PQC).

Take a look at `generate_keys`,
```python
def generate_keys():
    while True:
        # Random ternary polynomials f and g
        f = gen_poly()
        g = gen_poly()
        
        # Check if f is invertible modulo p and q
        try:
            f_p_inv = R_modp(f)**-1
            f_q_inv = R_modq(f)**-1
            break
        except:
            continue

    h = R_modq(p*(f_q_inv*g))
    
    private_key = (f, g, f_p_inv, f_q_inv)
    public_key = h
    return public_key, private_key
```

where

```python
N = 48
p = 3
q = 509
R_modq = PolynomialRing(Integers(q), 'x').quotient(x**N - 1, 'xbar')
R_modp = PolynomialRing(Integers(p), 'x').quotient(x**N - 1, 'xbar')
```

I am not "well-versed" in the dark arts of ring theory,
but it seems like `f` and `g` being used to form $h$ is analogous to how $p$ and $q$ form $n$ in RSA.

Note that in $\mathbb{Z}[x]/\left(x^N-1\right)$
```math
h \equiv p f^{-1} g 
```
so rearranging,
```math
fh \equiv pg
```
Reemmbering that $p=3$ and that $f, g \in \mathbb{Z}[x]/\left(x^N-1\right)$ where this quotient ring allows "cyclic convolution of $X^N \equiv 1$
one could write a linear system of equations

```math
\begin{align}
f &= f_0 + f_1 x + \cdots + f_{N-1} x^{N-1} \\
g &= g_0 + g_1 x + \cdots + g_{N-1} x^{N-1} \\
\begin{pmatrix}
p g_0 & p g_1 & \cdots & p g_{N-1}
\end{pmatrix}
&=
\begin{pmatrix}
f_0 & f_1 & \cdots & f_{N-1}
\end{pmatrix}
\begin{pmatrix}
h_0 & h_1 & \cdots & h_{N-1} \\
h_{N-1} & h_0 & \cdots & h_{N-2} \\
h_{N-2} & h_{N-1} & \cdots & h_{N-3} \\
\vdots & \vdots & \ddots & \vdots\\
h_1 & h_2 & \cdots & h_0
\end{pmatrix}
\end{align}
```
However this is done in modulo $q$ 
so taking this into account, the last equation becomes
```math
\begin{pmatrix}
f_0 & f_1 & \cdots & f_{N-1} & g_0 & g_1 & \cdots & g_{N-1} & k_0 & k_1 \cdots & k_{N-1}
\end{pmatrix}
\begin{pmatrix}
h_0 & h_1 & \cdots & h_{N-1} \\
h_{N-1} & h_0 & \cdots & h_{N-2} \\
h_{N-2} & h_{N-1} & \cdots & h_{N-3} \\
\vdots & \vdots & \ddots & \vdots\\
h_1 & h_2 & \cdots & h_0 \\
-p & 0 & \cdots & 0 \\
0 & -p & \cdots & 0 \\
\vdots & \vdots & \ddots & 0 \\
0 & 0 & \cdots & p \\
q & 0 & \cdots & 0 \\
0 & q & \cdots & 0 \\
\vdots & \vdots & \ddots & 0 \\
0 & 0 & \cdots & q
\end{pmatrix}
= 0
```
Just as in [MSS_ADVANCE Revenge](../MSS_ADVANCE%20Revenge/MSS_ADVANCE%20Revenge.md),
we could write this into a lattice basis representation, but one thing to note is that once $f$ is known, $g$ is determined automatically as it was assumed that $f$ was invertible in polynomial modulo $q$ hence we optimize by omitting $g$ logic.

Then after finding $f$ and $g$ we could reverse the process to find the plaintext.

**Disclaimer**: For the last part of decryption, I was running out of time, so I used Google Gemini to generate me the script, though it is probably not difficult to research and write it yourself.

## Solution
See [analysis.ipynb](./analysis.ipynb)