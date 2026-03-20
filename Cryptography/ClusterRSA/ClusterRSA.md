# ClusterRSA
Number of Points: 400

## Description
A message has been encrypted using RSA, but this time something feels... more crowded than usual. Can you decrypt it? Download the message.

## Hints
* RSA usually means two primes... but what if someone got greedy?
* Prime factors decomposition

## Analysis
The first hint suggests that the modulus may be made using multiple prime numbers.

Here is the `message.txt`.
```
n = 8749002899132047699790752490331099938058737706735201354674975134719667510377522805717156720453193651
e = 65537
ct = 5834177939681309704189596065676685903729910303824560560929082759974421613450454803823195850471530486
```

Note that $n$ is large, but not as large as if you were to multiple more than two large prime numbers in practice. This suggests that is actually has rather small prime numbers.

Let us try looking up $n$ on [factordb](https://factordb.com/), a site of collection of prime decompositions as a database. (Note that you could attempt to find decompositions yourself, though if there is an easier way, why not take it!)

According to factordb, the given $n$ has four factors:
* 9671406556917033397931773
* 9671406556917033398314601
* 9671406556917033398439721
* 9671406556917033398454847

To compute the prime modulus, we need totient function evaluation for the $n$, which
can be done by noting that for $n=p_1 p_2 p_3 p_4$ (prime decomposition),
the totient value is $\phi(n) = (p_1 - 1)(p_2 - 1)(p_3 - 1)(p_4 - 1)$.

After find this value, one computes the prime modulus by,
$d = e^{-1} \mod n$.

This can now be used to decrypt the cipher text.

## Solution
For the implementation of this, see [analysis.ipynb](./analysis.ipynb)
