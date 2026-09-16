# Week 8 Reflection — Practical Cryptography

**Student Name:** N. Williams

**Date:** September 15, 2026

Answer each question in 3–5 sentences.

1. How are encryption and hashing different?

```text
Encryption changes readable content into unreadable ciphertext. It can be reversed with the correct key and it protects confidentiality. Hashing produces a fixed fingerprint of the content, it can't be reversed and is used to verify integrity. Even though both are used to transform data, each one serves a different purpose.
```

2. How are symmetric and asymmetric cryptography different?

```text
Symmetric cryptography uses one shared key for encrypting and decrypting. Asymmetric cryptography uses the public and private key pair so that two parties will never need to share the same secret. Asymmetric cryptography enables things such as digital signatures without the need to transmit a shared secret. 
```

3. Why does a private key need stronger protection than a public key?

```text
A private key needs stronger protection than a public key because it proves identity and grants access, so if anyone obtains they can impersonate the owner. The public key can be shared freely and poses no risk if it is exposed. The risk differences is why private keys need to be protected with a passphrase and handled securely but public keys don't.
```

4. What changed between Week 6 password-based SSH and Week 8 key-based SSH? What stayed the same?

```text
The method of authentication changed, Week 6 used an account password and Week 8 used a private key with password authentication disabled. The goal to verify that the user is authorized before granting access stayed the same. The destination and the account being accessed also stayed the same.
```

5. Which Week 8 task felt most connected to a real cybersecurity job, and why?

```text
The Week 8 task that  felt most connected to a real cybersecurity job was proving that the key based authentication worked without falling back to a password. This reflects the real security practice that reduces risk from weak or compromised passwords. Verifying a control with real evidence is required  in audit and IAM work. 
```

6. What question do you have about certificates, identity, or trust before Week 9?

```text
How does a digital signature's proof of key possession connect to a real person or organization's verified identity since the signature alone does not prove who used the key? I would like to understand who decides that the certificate's claimed identity is trustworthy and what mechanism enforces that trust. Lastly, I am curious to know what stops someone from generating their own certificate, falsely claiming to be someone else.
```
