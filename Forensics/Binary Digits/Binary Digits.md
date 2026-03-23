# Binary Digits
Number of Points: 100

## Description
This file doesn't look like much... just a bunch of 1s and 0s. But maybe it's not just random noise. Can you recover anything meaningful from this? Download the file here.

## Hints
N/A

## Analysis
Since the file is 0s and 1s, I am first wanting to check if the length of the file is a multiple of 8.
```bash
# wc -c digits.bin
71192 digits.bin
```
71192 is indeed a multiple of 8.

The plan is to read the file, and assemble in chunks of 8 to form a byte, and see where that leads to.

## Solution
See [analysis.ipynb](./analysis.ipynb)
