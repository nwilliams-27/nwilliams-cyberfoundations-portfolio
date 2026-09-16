# Week 8 Notes — Practical Cryptography

**Student Name:** N. Williams

**Date:** September 15, 2026

## Vocabulary in My Own Words

```text
Plaintext: Original, readable content before encryption
Ciphertext: Scrambled, unreadable content after plaintext encryption
Encryption: Changing readable content to unreadable ciphertext, can only be read with the right ley
Hash / digest: Fixed length fingerprint of the content's in a file, any changes to the file changes the hash, hash cannot be reversed back to the original content
Public key: Used to verify signatures or encrypt data meant for the user, can be shared
Private key: Used to sign data or decrypt what was encrypted with the matching public key, must be kept private
Digital signature: Cryptographic data that's created by using a private key to sign the content's of a file, proof that the file has not changed and came from a specific key holder, does not hide the contents of the file
`authorized_keys`: A file on the server that lists public keys that are allowed to log in, can be authenticated by a matching private key, no password required
```

## Command-to-Purpose Map

| Command | What it demonstrated |
| --- | --- |
| `openssl enc` | File encryption and decryption, proved confidentiality |
| `sha256sum` | File hashing to detect if the contents changed |
| `ssh-keygen` | Generated the public/private key pair |
| `openssl dgst` | Signed and verified the file, proved it was not altered |
| `ssh ... -o PasswordAuthentication=no` | Proved the login worked only using the key, no password was used |

## Safety Rules I Must Remember

```text
Rule 1: Never paste, screenshot, upload, or commit a passphrase or private key, they must never appear anywhere in a submission, only on the VM itself.
Rule 2: Only the public key belongs in authorized_keys, the private key should never be copied to a server or shared at any time
Rule 3: Do not delete or edit `~/.ssh/known_hosts` just to recreate a prompt I missed the first time.
```

## Question for the Instructor

```text
My question for the instructor: For this lab, the passphrase and the account password were separate on purpose, but in a real world environment do organizations enforce both or is one used more often due to convenience?
```
