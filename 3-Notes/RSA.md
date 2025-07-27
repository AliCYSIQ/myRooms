[[Crypto]]

Modular exponentiation is an operation that is used extensively in cryptography and is normally written like: 210mod  17210mod17  
  
You can think of this as raising some number to a certain power (210=1024210=1024), and then taking the remainder of the division by some other number (1024mod  17=41024mod17=4). In Python there's a built-in operator for performing this operation: `pow(base, exponent, modulus)`.