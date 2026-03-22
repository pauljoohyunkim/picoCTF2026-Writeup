# shift registers
Number of Points: 200

## Description
I learned about lfsr today in school so i decided to implement it in my program. It must be safe right? chall.py output.txt

## Hints
No hints

## Analysis
Note that due to `encrypt_lfsr` starting off with
```python
lfsr = key & 0xFF
```
the key being 126 bytes does not matter at all.
In fact I could simply brute force the key from 0 to 255.

Note also that this is a simply stream cipher and by knowing the sequence of pseudorandom number to be generated, it is easy to decrypt the message using the same encryption function and take the one that starts with the string "picoCTF".

## Solution
I have modified [chall.py](./chall.py) so that it can be better integrated with analysis.
See [analysis.ipynb](./analysis.ipynb)