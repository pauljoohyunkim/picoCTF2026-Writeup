# Name of the Challenge
Number of Points: 200

## Description
Everything seems secure; strong numbers, familiar parameters but something small might ruin it all. Can you recover the message? Download the message. And source code

## Hints
* This might be a job for Boneh-Durfee.

## Analysis
Notice that the private exponent is only 256 bits.

One rule of thumb is that public keys can be as short or long as an algorithm permits,
but private key must be from a very large source in general.

The hint already states that this is a job for Boneh-Durfee.

## Solution
There already is an excellent [implementation](https://github.com/maximmasiutin/rsa-boneh-durfee) of Boneh-Durfee attack for this.
