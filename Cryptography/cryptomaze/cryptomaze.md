# cryptomaze
Number of Points: 200

## Description
In this challenge, you are tasked with recovering a hidden flag that has been encrypted using a combination of Linear Feedback Shift Register (LFSR) and AES encryption. The LFSR is used to derive a key for AES encryption, making it crucial to understand its workings to decrypt the message.The flag has been stored in a file and encrypted. Your goal is to derive the key used for encryption from the LFSR state and taps provided in the output, and then decrypt the flag to retrieve it.Download the encrypted flag from here(**output.txt**). which contains the following information:

- The initial state of the LFSR
- The taps used for the LFSR
- The encrypted flag in hexadecimal format

**output.txt:**
```
LFSR Initial State:
[0, 0, 1, 0, 0, 1, 0, 1, 1, 1, 1, 0, 1, 1, 0, 0, 1, 0, 0, 1, 0, 1, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1, 0, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 0, 1, 0, 1, 1, 1, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 1, 0, 1, 1]
LFSR Taps:
[63, 61, 60, 58]
Encrypted Flag:
8f0e6d0f5b0dc1db201948b9e0cebd8f184b92a3e4793abf8dac3d9e135f39a638338e7e04fbddef0c6260a4eb758417
```

## Hints
- Use the LFSR’s initial state and taps to generate a 128-bit sequence.
- Convert this 128-bit sequence into a 16-byte AES key by:
	- Grouping the bits into 8-bit chunks (16 chunks in total).
	- Converting each chunk from binary to a byte to form the AES key.
- Decrypt the flag using AES in ECB mode.
- Convert the encrypted flag from hex to bytes before decrypting.

## Analysis
Since the initial state and taps used for the LFSR were given in ouput.txt, we can simply reproduce the process of key generation by implementing LFSR, and then decrypt the encrypted flag using the recovered AES key.

LSFR is a sequential logit curcuit commonly used to generate pseudo-random bit sequences. The output is pseudo-random because the operation is deterministic and determined completely by its previous state. At each clock cycle, it updates its state by shifting all bits in the register by one position, and the new input bit is computed as XOR of the selected tap positions. This new input bit is then appended to the output sequence.

## Solution
I wrote a python script to emulate LSFR.
```python
state =[0,0,1,0,0,1,0,1,1,1,1,0,1,1,0,0,1,0,0,1,0,1,1,0,1,0,0,1,0,1,0,1,0,1,0,0,1,1,0,1,1,0,0,0,1,0,1,1,1,1,0,0,0,1,0,0,0,1,0,1,1,0,1,1]
taps = [63,61,60,58]

def lfsr(state, taps, n):
	s = state[:]
	out = []
	
	for _ in range(n):
		out_bit = s[0] # output bit
		out.append(out_bit)
		feedback = 0
		
		for t in taps:
			feedback ^= s[t]
		
	s = s[1:] + [feedback] # shift left
	
	return out

# 128-bit sequence
bits = lfsr(state, taps, 128)

# ----- bits → AES key -----
key = bytes(
	int("".join(map(str, bits[i:i+8])), 2)
	for i in range(0, 128, 8)
)
```
In this case, LSFR is run for 128 iterations to generate 128-bit sequence. The resulting sequence is then grouped into bytes and used as a AES key to decrypt the flag.

```python
# ----- hex → bytes -----
cipher_hex = "8f0e6d0f5b0dc1db201948b9e0cebd8f184b92a3e4793abf8dac3d9e135f39a638338e7e04fbddef0c6260a4eb758417"
ciphertext = bytes.fromhex(cipher_hex)

# ----- AES ECB decrypt -----
from Crypto.Cipher import AES

cipher = AES.new(key, AES.MODE_ECB)
plaintext = cipher.decrypt(ciphertext)

print("Decrypted:", plaintext)
```