[[Crypto]]

Modular exponentiation is an operation that is used extensively in cryptography and is normally written like: 210mod  17210mod17  
  
You can think of this as raising some number to a certain power (210=1024210=1024), and then taking the remainder of the division by some other number (1024mod  17=41024mod17=4). In Python there's a built-in operator for performing this operation: `pow(base, exponent, modulus)`.

RSA encryption is modular exponentiation of a message with an exponent ee and a modulus NN which is normally a product of two primes: N=p⋅qN=p⋅q.  
  
Together, the exponent and modulus form an RSA "public key" (N,e)(N,e). The most common value for ee is `0x10001` or 6553765537.

