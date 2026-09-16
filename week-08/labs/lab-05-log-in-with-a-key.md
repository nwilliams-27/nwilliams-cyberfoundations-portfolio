# Week 8 Lab 05 — Log In with a Key

**Student Name:** N. Williams

**Date Completed:** September 14, 2026

**Module:** 3 — Practical Cryptography  
**Submission Path:** `week-08/labs/lab-05-log-in-with-a-key.md`

> ## STOP — Protect secrets
> Never paste, upload, commit, or screenshot a password, passphrase, or private key. Do not run `cat` on any private-key file. Submit only the worksheet and the named screenshots. Keep all cryptographic files on the VM.

## How to Use This Lab

- **[TERMINAL]** means type or paste the command into the Cloud Heights terminal.
- **[WORKSHEET]** means type your response in this lab worksheet.
- Run commands in the order shown. Do not type the sample output.
- If your result does not match the stated success check, stop at the Troubleshooting box. Do not improvise with `sudo`, package installation, Azure settings, or SSH server configuration.

## Mission

Authorize the public key created in Lab 03 and prove that `analyst@localhost` authenticates with that key without falling back to the account password.

## What You Already Know

In Week 6, SSH used the `analyst` account password. Here, the server stores the public key in `authorized_keys`, while the private key stays in the client account. The same user, protocol, and destination remain; only the authentication method changes.

## Lab Environment / Pre-Lab Check

**Required prerequisite:** Complete Lab 03 first.

**[TERMINAL] Run:**

```bash
whoami
cf-week8-check
test -f ~/.ssh/week8_analyst_ed25519 &&   test -f ~/.ssh/week8_analyst_ed25519.pub &&   echo "READY: Lab 03 key pair exists" ||   echo "STOP: complete Lab 03 before continuing"
```

**Continue only if:** `whoami` prints `analyst`, all checks report `PASS`, and the final line begins with `READY`.

### If the VM Stops

Return to **My Lab Environment** in the Lab Portal and start your assigned VM. A stopped or deallocated VM is not deleted; saved disk files remain.

## Predict First

**[WORKSHEET]** Does the private key need to be copied into `authorized_keys`? Explain.

```text
No, the the private key need to be copied into authorized_keys and should never be copied there because only the public key belongs there. The authorized_keys file is designed to hold public keys because its purpose is to let the server verify that the incoming connection is in possession of the matching private key without the server ever having to store or have access to the private key. Copying the private key into the file would be security mistake and defeats the purpose of asymmetric key authentication. 
```

## Guided Steps

### Step 1 — Prepare `authorized_keys`

**[TERMINAL] Run:**

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

### Step 2 — Add the Week 8 Public Key Once

**[TERMINAL] Run:**

```bash
grep -qxF "$(cat ~/.ssh/week8_analyst_ed25519.pub)" ~/.ssh/authorized_keys   || cat ~/.ssh/week8_analyst_ed25519.pub >> ~/.ssh/authorized_keys
```

This command adds the key only if an identical line is not already present.

### Step 3 — Confirm the Entry and Safe Permissions

**[TERMINAL] Run:**

```bash
grep -c 'week8-analyst-key$' ~/.ssh/authorized_keys
stat -c '%a %n' ~/.ssh ~/.ssh/authorized_keys
```

**Required result:** The count is `1`; permissions show `700` for `.ssh` and `600` for `authorized_keys`.

**Evidence moment:** Capture this result as `week08-lab05-authorized-key-permissions.png`.

### Step 4 — Run the Public-Key-Only Proof

**[TERMINAL] Run:**

```bash
ssh   -o PreferredAuthentications=publickey   -o PasswordAuthentication=no   -i ~/.ssh/week8_analyst_ed25519   analyst@localhost   'echo "AUTH_TEST=PUBLICKEY_SUCCESS"; whoami; hostname'
```

If SSH asks whether to trust the host fingerprint, type `yes` and press Enter. If no fingerprint question appears, continue; `localhost` was already known. Do not delete `known_hosts` to force the question.

At `Enter passphrase for key`, enter the **Lab 03 key passphrase**. Do not enter the `analyst` account password.

**Required result:**

```text
AUTH_TEST=PUBLICKEY_SUCCESS
analyst
cf-student-XX
```

The final hostname will contain your assigned VM number instead of `XX`.

**Evidence moment:** Capture the three result lines as `week08-lab05-publickey-auth-success.png`.

## Stop & Check

If the command asks for the `analyst` account password, reports `Permission denied`, or does not print `AUTH_TEST=PUBLICKEY_SUCCESS`, do not submit. The options in the proof command disable account-password fallback for this one test only; they do not change the VM server.

### Troubleshooting

1. Run `grep -c 'week8-analyst-key$' ~/.ssh/authorized_keys`; required value is `1`.
2. Run `stat -c '%a %n' ~/.ssh ~/.ssh/authorized_keys`; required values are `700` and `600`.
3. Run `ssh-keygen -lf ~/.ssh/week8_analyst_ed25519.pub`; it must show the Lab 03 key.
4. If any check fails, stop and contact the instructor. Do not edit `/etc/ssh/sshd_config`, use `sudo`, disable password authentication, or delete `known_hosts`.

## Explain

**[WORKSHEET]** In 4–5 sentences, compare Week 6 password authentication with this key-based test. State what changed and what stayed the same.

```text
In Week 6 authenticating over SSH was reliant on entering the account password which meant that the server verified identity based upon something that the user knew and typed at the time of logging in. For this Week 8 test authentication relied on having possession of the correct private key matching against the public key that is already placed in authorized_keys with no password to enter or fall back to. What changed was the method of proving identity. It changed from a shared secret (password) to cryptographic key key possession. What stayed the same is the overall goal to confirm that analyst@localhost is who they claim to be before access is granted to the system. The shift from password to key based authentication is considered to be stronger because it removes the risk of a guessable or stolen password being used to gain access to the system.
```

## Analysis Questions

1. What changed between the Week 6 and Week 8 SSH logins?

```text
The method of authentication changed from entering an account password to prove possession of a private key that matches a public key that is already authorized on the server. In Week 6 the server checked whether or not the password matched the stored account credential. In Week 8, the server performed a cryptographic challenge that only the person holding the correct private key could answer successfully, a password was not involved at all.
```

2. What stayed the same?

```text
The overall purpose of the login which was to verify that the person or system attempting to gain access is authorized before the access is granted stayed the same. The goal of both methods is to result in a successful authenticated SSH session for the analyst@localhost account. The destination and the account being accessed did not change, the only thing that changed was the underlying method used to prove the right access to it.

```

3. Why does `authorized_keys` contain the public key rather than the private key?

```text
The `authorized_keys` file only needs the public key because the job of the server is to verify the connecting client, not prove it's identity. Verification can be done only using the public key and never using the private key. Starting the private key on the server is unnecessary and dangerous because anyone who gains access to the file is able to impersonate the legitimate key holder. Exclusively keeping the private key on the client's machine while placing the public key on the server is how the foundation of the asymmetric authentication works securely.

```

4. Why is the forced proof stronger evidence than a normal successful SSH login?

```text
It is possible for a normal successful SSH login to potentially rely on a fallback method such as a password, even if key based authentication was configured and available. A forced proof specifically demonstrates that authentication using only the key with no password fallback available at all. This rules out the possibility that the password granted access. This makes forced proof stronger evidence because it isolates and confirms the key based method is genuinely functioning on its own instead of just working alongside another, untested method.

```

5. How is the key passphrase different from the `analyst` account password?

```text
The account password authenticates the user directly to the operating system when normally logging in, controlling access at the account level. The key passphrase only protects the private key file itself, locally unlocking it so that it can be used for the cryptographic authentication process. These are two independent separate secrets protecting twp. things, someone could know one without having any access to the other because each secures a distinct part of the overall system.

```

## Required Evidence

- `assets/screenshots/week-08/week08-lab05-authorized-key-permissions.png`
- `assets/screenshots/week-08/week08-lab05-publickey-auth-success.png`

## Submission Checklist

- [x] Lab 03 was completed first.

- [x] The authorized-key count is exactly `1`.

- [x] Permissions are `700` and `600`.

- [x] The public-key-only proof printed all three required lines.

- [x] Both screenshots use the exact filenames.

- [x] No password, passphrase, private key, or Bastion URL appears.

- [x] Every worksheet response is complete.

## GitHub / Lab Portal Submission

1. In the Lab Portal, open the matching Week 8 lab.
2. Complete every **[WORKSHEET]** response.
3. Upload only the required screenshots to `assets/screenshots/week-08/`.
4. Select **Submit to GitHub**.
5. Open the committed worksheet and screenshots on GitHub. Confirm they are readable and contain no secrets.

**Never submit:** `.pem` files, files from `~/.ssh/`, passwords, passphrases, private-key contents, or a Bastion shareable URL.
