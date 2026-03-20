# Secure Dot Product
Number of Points: 300

## Description
Our intern thought it was a great idea to vibe code a secure dot product server using our AES key. Having taken a class in linear algebra, they're confident the server can't ever leak our key, but I'm not so sure... Download the source code remote.py

Additional details will be available after launching your challenge instance.

## Hints
* A system is only as secure as what it trusts!
* You should use pwntools to automate the solution, it might not always be solvable.

## Analysis
Let us look at the initialization process.
```python
    flag = read_flag()
    key = secrets.token_bytes(KEY_SIZE)
    iv, ciphertext = encrypt_flag(flag, key)

    print("==================== Encrypted Flag ====================")
    print(f"IV: {iv.hex()}")
    print(f"Ciphertext: {ciphertext.hex()}")

    service = SecureDotProductService(key)
    service.run()
```
After flag is read, an encryption key of 32 bytes is generated.
This is a cryptographically secure pseudorandom number generation, so no issue here.

IV for AES-CBC and ciphertext of the flag are given.

```python
    def __init__(self, key):
        self.key_vector = [byte for byte in key]
        self.salt = secrets.token_bytes(SALT_SIZE)
        self.trusted_vectors = self.generate_trusted_vectors()
        
    def hash_vector(self, vector):
        vector_encoding = vector[1:-1].encode('latin-1')
        return hashlib.sha512(self.salt + vector_encoding).digest().hex()

    def generate_trusted_vectors(self):
        trusted_vectors = []

        for _ in range(5):
            length = random.randint(1, 32)
            vector = [random.randint(-2**8, 2**8) for _ in range(length)]
            trusted_vectors.append((vector, self.hash_vector(str(vector))))

        return trusted_vectors

```
Salt is generated securely.
Five trusted vectors are generated where each of the vectors is of random length between 1 to 32.

It turns out that `hash_vector` function takes in `vector: str`, that is it takes Python's conversion to string into the hash function.
The actual hash function SHA512 is applied after prepending the salt.

**But this is not how you are supposed to do this!** There is a thing called HMAC for a reason!
This means this server might be vulnerable to **length extension attack**.

To actually make this a vulnerability, they even stripped away the square brakets, which would in fact make this vulnerable.

```python
    def parse_vector(self, vector):
        sanitized = "".join(c if c in '0123456789,[]' else '' for c in vector)
        try:
            parsed = ast.literal_eval(sanitized)
        except (ValueError, SyntaxError, TypeError):
            return None
        
        if isinstance(parsed, list):
            return parsed
        return None
        
    def dot_product(self, vector):
        return sum(vector_entry * key_entry for vector_entry, key_entry in zip(vector, self.key_vector))
```
`dot_product` has no issues.

`parse_vector` seems like it is doing proper sanitization, but I realized that '-' symbol also gets taken away, meaning when you enter even one of the "trusted vectors", there is a good chance that you will not get the actual dot product of what you entered as a trusted vector.

Instead, you will get dot product of the key with vector formed using the absolute value of each entry of the trusted vector.

### (Failed Attempts?)

## Solution
Since we identified length extension attack, we need a way to actually do the length extension attack.

We used [hashpump](https://github.com/mheistermann/HashPump-partialhash) for this.
Setting this up is as simple as cloning and `make`-ing it.

In my implementation of the solution [analysis.ipynb](./analysis.ipynb), I call this binary and process the output to read it back into Python.
