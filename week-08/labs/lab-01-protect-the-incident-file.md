# Week 8 Lab 01 — Protect the Incident File

**Student Name:** N. Williams

**Date Completed:** September 9, 2026

**Module:** 3 — Practical Cryptography  
**Submission Path:** `week-08/labs/lab-01-protect-the-incident-file.md`

> ## STOP — Protect secrets
> Never paste, upload, commit, or screenshot a password, passphrase, or private key. Do not run `cat` on any private-key file. Submit only the worksheet and the named screenshots. Keep all cryptographic files on the VM.

## How to Use This Lab

- **[TERMINAL]** means type or paste the command into the Cloud Heights terminal.
- **[WORKSHEET]** means type your response in this lab worksheet.
- Run commands in the order shown. Do not type the sample output.
- If your result does not match the stated success check, stop at the Troubleshooting box. Do not improvise with `sudo`, package installation, Azure settings, or SSH server configuration.

## Mission

Encrypt a readable incident report, inspect the encrypted file safely, decrypt it, and prove the recovered file is identical to the original.

## What You Already Know

Encryption changes readable **plaintext** into protected **ciphertext**. In this lab, one passphrase protects and recovers the file. Encryption supports confidentiality, but it does not prevent someone from deleting or copying the encrypted file.

## Lab Environment / Pre-Lab Check

| Item | Required value |
|---|---|
| VM | Your assigned `cf-student-XX` VM |
| Linux account | `analyst` |
| Working directory | `~/cloud-heights/week8-cryptography` |
| Starting file | `evidence/incident-report.txt` |
| Estimated time | 25–35 minutes |

**[TERMINAL] Run:**

```bash
whoami
hostname
cf-week8-check
cd ~/cloud-heights/week8-cryptography
pwd
```

**Continue only if:** `whoami` prints `analyst`, the hostname begins with `cf-student-`, every environment check reports `PASS`, and `pwd` ends with `/cloud-heights/week8-cryptography`.

### If the VM Stops

Return to **My Lab Environment** in the Lab Portal and start your assigned VM. A stopped or deallocated VM is not deleted; saved disk files remain.

## Predict First

**[WORKSHEET]** What do you expect to see when encrypted data is inspected as bytes? Why should it not look like the original report?

```text
I expect the encrypted data to look like random bytes that cannot be read and have no resemblance to the original text. The encryption is designed to scramble content so that it cannot be interpreted without the correct key. If the encrypted report still looks like the original report that is an indication of weak or broken encryption.
```

## Guided Steps

### Step 1 — Confirm the Original Is Readable

**[TERMINAL] Run:**

```bash
cat evidence/incident-report.txt
```

**Expected result:** A readable report beginning with `CLOUD HEIGHTS INCIDENT REPORT`.

### Step 2 — Encrypt the Report

Choose one temporary lab passphrase that you can re-enter during this lab. Do not write it in the worksheet.

**[TERMINAL] Run:**

```bash
openssl enc -aes-256-cbc -salt -pbkdf2 -iter 100000   -in evidence/incident-report.txt   -out encrypted/incident-report.enc
```

At `enter AES-256-CBC encryption password:`, type the passphrase. Nothing may appear while you type; this is normal. Press Enter, type the same passphrase again, and press Enter.

**Expected result:** The prompt returns without an error message.

### Step 3 — Inspect the Encrypted File

**[TERMINAL] Run:**

```bash
file encrypted/incident-report.enc
xxd -l 64 encrypted/incident-report.enc
```

**Expected result:** `file` identifies data, and `xxd` displays hexadecimal bytes. The readable incident report does not appear.

**Evidence moment:** Capture the terminal now as `week08-lab01-encrypted-inspection.png`.

### Step 4 — Decrypt the File

**[TERMINAL] Run:**

```bash
openssl enc -d -aes-256-cbc -pbkdf2 -iter 100000   -in encrypted/incident-report.enc   -out encrypted/incident-report-decrypted.txt
```

Enter the same passphrase from Step 2.

### Step 5 — Prove the Files Match

**[TERMINAL] Run:**

```bash
cmp -s evidence/incident-report.txt encrypted/incident-report-decrypted.txt   && echo "MATCH: decrypted file equals original"   || echo "MISMATCH: stop and troubleshoot"
```

**Required result:** `MATCH: decrypted file equals original`

**Evidence moment:** Capture the terminal now as `week08-lab01-decryption-match.png`.

## Stop & Check

Do not submit unless both required evidence moments succeeded.

### Troubleshooting

- `bad decrypt` or `bad password read`: rerun Step 4 and enter the exact Step 2 passphrase.
- `No such file or directory`: run `cd ~/cloud-heights/week8-cryptography`, then rerun the failed step.
- `MISMATCH`: delete only the decrypted copy with `rm -f encrypted/incident-report-decrypted.txt`, then repeat Steps 4–5.

## Explain

**[WORKSHEET]** In 3–4 sentences, explain how this lab demonstrates confidentiality and why encryption does not prevent deletion.

```text
This lab demonstrates confidentiality by showing that once a file has been encrypted, the contents become unreadable to anyone without the correct pass phrase, even if the file is intercepted. By inspecting the encrypted data as bytes and seeing no resemblance to the original data it was confirmed that the encryption successfully hid the meaning of the content. The encryption process does not prevent deletion because it only protects what is inside of the file. Encryption does nothing to stop a person with access to the file from deleting or destroying the file in its entirety because reading the data is not required in order to delete it. 

Analysis Questions

Why can an encrypted file still be copied or deleted?

An encrypted file can still be copied or deleted because encryption only protects the content that is inside of the file. Encryption does nothing to control basic file system operations such as copying or deleting because reading or understanding the contents of the file is not required in order to perform those operations. The confidentiality of the file and the accessibility to the file are separate concerns that are handled by different mechanisms. Encryption addresses the confidentiality and file permissions and access control the accessibility. This is why someone with access to the file can copy or delete an encrypted file just as easy as they can copy or delete and unencrypted file without ever being able to read its contents.

What job did the passphrase perform?

The passphrase served as the secret key that was needed to encrypt and later decrypt the file, which controlled access to the readable content of the file. Without the correct passphrase data will remain unreadable because encryption is used to scramble and reverse the scrambling of the original data. The job of the passphrase was to enforce confidentiality which ensures the only someone who knows the pass phrase can recover the original, readable version of the file.

Why does the match test prove correct recovery but not prove who handled the file?

The match test proved correct recovery because it confirmed that the encrypted file was identical to the original file, which proves the encryption and decryption process worked properly without corrupting or altering the data. However, the test only verified the contents of the file itself, it said nothing about ho performed the encryption, decryption or who handled the file at any point during the process. Proving identity or or accounting for who touched the file requires separate evidence such as access logs or audit records because a successful content match alone does not identify the person that's responsible for any step.
```

## Analysis Questions

1. Why can an encrypted file still be copied or deleted?
2. What job did the passphrase perform?
3. Why does the match test prove correct recovery but not prove who handled the file?

## Required Evidence

Save exactly these files in `assets/screenshots/week-08/`:

- `week08-lab01-encrypted-inspection.png`
- `week08-lab01-decryption-match.png`

## Submission Checklist

- [x] Both required results appeared.

- [x] Both screenshots use the exact filenames above.

- [x] No passphrase or other secret appears.

- [x] Every worksheet response is complete.

- [x] The worksheet is saved at the stated submission path.

## GitHub / Lab Portal Submission

1. In the Lab Portal, open the matching Week 8 lab.
2. Complete every **[WORKSHEET]** response.
3. Upload only the required screenshots to `assets/screenshots/week-08/`.
4. Select **Submit to GitHub**.
5. Open the committed worksheet and screenshots on GitHub. Confirm they are readable and contain no secrets.

**Never submit:** `.pem` files, files from `~/.ssh/`, passwords, passphrases, private-key contents, or a Bastion shareable URL.
