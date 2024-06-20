---
tags:
  
  
  - programming
---
# Encryption

Let's go! Terminology:

- Ciphertext: result of encryption
- Cipher: Method of encryption or decryption. Mondern ciphers are cryptographic, but there non cryptographic ciphers
  like Caaser
- Plaintext - Data before encryption
- Encryption - converting data into cihpher text
- Encoding - not encryption. Just how data is represented
- Key: Info needed to decrypt the data
- Passphrase: password used to protect the key
- Asymmetric encryption - different encrypt and decrypt keys
- Symmetric encryption - same encrypt and decrypt keys
- Brute Force - just try everything
- Cryptanalyis - find weaknesses in the encryption method
- Alice and Bob - two people who want to talk (used in examples)


## Why Encryption?

This is used to protect information in transit and stored sensitive information. Web connections, bank connections, SSH,
downloads, etc are all encrypted. Encrypted at rest also applies to some data as well. 

Passwords SHOULD NOT be stored encrypted unless it is a system that intends to decrypt them as well (password managers
are one example).


## Crypto Myths

Bigfoot, Mothman, oh.... that's Cryptozoology. 

Oh, this is just about the modulo operator.

## Types of Encryption

Symmetric encryptions uses the same key for encryption and decryption. These tend to be faster and have smaller keys
(128 or 256 bits for AES, DES are 56 bits)

Asymmetric encryption uses two keys - one for encryption and one for decryption. RSA is one. These are referred to as
public and private keys. Publics keys can be shared, but DO NOT share the private key. These tend to be a bit slower and
have larger keys. RSA is 2048-4096 bits long.

## RSA - Rivest Shamir Adleman


