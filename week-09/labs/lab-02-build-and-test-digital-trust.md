# Week 9 Lab 02 - Build and Test Digital Trust

**Student Name:** N. Williams

**Date Completed:** September 22, 2026

**Module:** 3 - Practical Cryptography | **Week:** 9  
**Submission Path:** `week-09/labs/lab-02-build-and-test-digital-trust.md`

> ## Vault Exchange Trust and Key Safety Rule
> Use your assigned Ubuntu training computer (VM) as `analyst`. Keep Week 6–8 files and your SSH login keys unchanged. Do not run `sudo` (administrator commands), change network rules, add your pretend badge office to the computer or browser’s accepted list, or make the practice server available to other computers. All new work stays inside a fresh practice folder under `~/cloud-heights/week9-digital-trust/`.
>
> **Evidence safety:** Never display, upload, or commit private-key contents. Publish selected worksheet text and reviewed screenshots only. Do not upload the whole VM workspace or run `git add .` from it. Crop credentials, access URLs, and account information.

---

## Mission

Ivy needs a digital badge for a practice service running on her training computer. You will help her apply for the badge, issue it from a pretend badge office, and check it.

The digital badge is a **certificate**. The issuing office is a **certificate authority**, shortened to **CA**. You will play both roles for this exercise. Your pretend office will not become trusted by public websites or other people’s browsers.

You will also try a wrong name and a wrong office on purpose. A check refusing the wrong information is the result we want. Finally, you will compare your practice service with the real website from Lab 01.

The badge application is called a **certificate signing request (CSR)**. Signing an application shows that its maker used the matching secret key. It does not prove they have permission to get a badge for someone else’s website. A real issuing office must check that permission before issuing a certificate.

## What You Already Know

Think back to these ideas; you do not need to memorize the technical names:

- A **service** is a program waiting to help another program, like a reception desk waiting for visitors.
- A **VM (virtual machine)** is your assigned training computer, accessed through Cloud Heights.
- A **terminal** is a window where you type instructions, called **commands**. **Bash** reads and runs those instructions.
- **OpenSSL** is the tool we will use to make keys and check certificates.
- A **key pair** has a secret private key and a shareable public key. Never share the private key.
- **TLS** is the method programs use to set up a protected connection. It checks identity and arranges keys to protect the messages.

Looking at a badge, checking a badge, and successfully using it at a door are different activities. We will collect evidence for each one.

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Assigned Cloud Heights Ubuntu VM, account `analyst`, Bash shell |
| Tools | OpenSSL 3.x, core utilities, `ss`, and two terminal sessions |
| Working root | `~/cloud-heights/week9-digital-trust/` |
| Separate practice area | Fresh `practice-*` folder; no preloaded certificate fixture assumed |
| Practice connection address | `127.0.0.1:8443`, expected DNS name `localhost` |
| Time | 90-120 minutes; work in Parts A-D with pauses |
| Submission | Selected evidence only; private keys stay on the VM |

- [x] I completed Lab 01 or have a dated public-site observation ready.

- [x] I am on my assigned VM and can open a second terminal session to that same VM.

- [x] My account is `analyst` and I can write to the Week 9 workspace.

- [x] I know that an intentional negative test is successful learning evidence when it rejects for the intended reason.

### Cloud Heights Idle Stop

Respond to the Portal's idle warning while actively working. A stopped/deallocated VM is not deleted; restart it from My Lab Environment. Saved files remain, but the TLS listener must be started again after a restart. Reopen your recorded practice path instead of regenerating keys over existing files.

### Before You Type Commands

1. Work in your assigned VM, using the account named `analyst`. Do not paste these commands into your personal computer’s terminal.
2. Copy one entire **bash** box at a time, paste it into the terminal, and press Enter if the last line has not run. Do not copy the box borders or the word `bash`.
3. Wait for your usual command prompt to return before pasting the next box. A **prompt** is the terminal’s “ready for your next command” line. Step 10 is the exception: the service keeps running there.
4. Long commands may wrap onto another screen line. Copy the whole command. You do not need to memorize the options beginning with `-`.
5. Text in **text** boxes is for your worksheet answers, not the terminal. Replace the blank prompts with your own observations.
6. If the result differs from the expected result, stop at that step and show your instructor the command and error. Do not remove checking options to make an error disappear.

A **path** is a file or folder’s address. `~` means your home folder. `cd` means “move into this folder”; `pwd` prints the folder you are currently in. A value such as `W9_RUN` is a named storage box the command uses to remember a path. Keep the commands exactly as written.

## Predict First

Make a best guess for each test. “Expected name” means the name we want the badge to cover. A “listener” is a running program waiting for a connection. Port `8443` is the numbered door this program uses. Explain each prediction in 2–3 sentences; it is okay to be unsure before trying it.

| Test | Your prediction and reason |
| --- | --- |
| Intended lesson CA + expected name `localhost` | Intended lesson CA + expected name `localhost` should pass verification. The correct CA issued the certificate and the name check is covered. Therefore, the name match and the trust chain should succeed. |
| Same certificate and CA + expected name `wrong.test` | Same certificate and CA + expected name `wrong.test` should fail. Although the CA is correct and it's trusted, the certificate's SAN list will not include wrong.test. Therefore, the name check should reject the connection just as a badge with a printed name would be refused. |
| Same certificate and expected name + unrelated CA | Same certificate and expected name + unrelated CA should fail. Even with a name match, running a check against an unrelated CA means that the signature will not be traced back to a trusted issuer. Therefore, the trust chain breaks. |
| Same intended inputs, but no listener on port 8443 | Same intended inputs, but no listener on port 8443 should fail. When there is no listener running on the port nothing is there to attempt a TLS connection with. Therefore, the failure would happen at the connection/reachability and not at the certificate validation level. |

## Guided Steps

### Part A - Prepare the Key, Request, and Issuer

#### Step 1 - Check the Environment

This first box checks your account, the installed tools, the clock, and whether our numbered door is already in use. It also makes the Week 9 folder if it does not exist. **It does not start the practice service.**

```bash
whoami
openssl version
command -v ss
date -u
mkdir -p ~/cloud-heights/week9-digital-trust
test -w ~/cloud-heights/week9-digital-trust && printf 'Workspace writable\n'
ss -ltn 'sport = :8443'
```
**What you should see, in order**:

1. `analyst` — your account name.
2. An OpenSSL version beginning with `3.` — the tool is installed.
3. A file path ending in `ss` — the tool for listing waiting services is available.
4. Today’s date and time in **UTC**, a shared time standard. It can differ from your local clock by your time-zone offset.
5. `Workspace writable` — you can save files in the Week 9 folder.

```text
Account name shown: analyst
OpenSSL version shown: OpenSSL 3.0.2 15 Mar 2022 (Library: OpenSSL 3.0.2 15 Mar 2022)
Path to the ss tool: /usr/bin/ss
UTC date and time shown: Wed Sep 23 01:00:19 UTC 2026
Workspace writable result: Workspace writable
Port 8443 listener result (blank output means nothing is listening): blank output
```

6. A heading with no service row below it — port 8443 is free.

**Stop and ask for help** if an item is missing, the date is wrong, or a service row appears. Do not close an unknown program or change the clock yourself.

```text
Account: analyst
OpenSSL version: OpenSSL 3.0.2 15 Mar 2022 (Library: OpenSSL 3.0.2 15 Mar 2022)
UTC clock: Wed Sep 23 01:00:19 UTC 2026
Port 8443 free? Evidence: Yes port 8443 is free because a heading with no service row listed underneath was returned when the  ss -ltn `sport = :8443` command was ran, confirming that the is no process currently listening on port 8443.
```

#### Step 2 - Create a Fresh Practice Folder

This box makes a new folder for this attempt. **Directory** means folder. `mktemp` chooses a fresh name so you do not overwrite earlier work. `umask` restricts who can read newly created files. The four inner folders will hold the badge office files, applications, finished badges, and check results.

```bash
umask 077
W9_RUN=$(mktemp -d "$HOME/cloud-heights/week9-digital-trust/practice-XXXXXX")
cd "$W9_RUN"
mkdir ca requests certificates verification
printf '%s\n' "$PWD"
```

```text
My exact practice path: cloud-heights/week9-digital-trust/practice-Ee6YVu/ca
```

**What you should see:** a path ending in `practice-` plus a random ending. Copy that exact path into your answer above. It will be different for each attempt.

**To resume later:** type `cd`, then a space, then paste your recorded path and press Enter. Type `pwd` and press Enter to confirm you are in that folder. Do not run Step 2 again just to resume; that would create a different attempt.

#### Step 3 - Create the Service Key and CSR

First make a new key pair for Ivy’s practice service. **RSA** is the kind of key we are making; **2048** is its size in bits. These values are provided for you. This box saves the private key and writes a separate file containing only the public key. Do not reuse a key you use to log in.

```bash
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048 -out requests/service.key.pem
chmod 600 requests/service.key.pem
openssl pkey -in requests/service.key.pem -pubout -out requests/service.public.pem
```
**What you should see:** progress symbols may appear, then the prompt returns. Some lines finish without printing anything. The private-key file is restricted to your account by `chmod 600`. It has no password protection for this temporary practice exercise; real systems need a planned way to protect their keys.

**Do not open or print `service.key.pem`.** `cat` displays a file’s contents, so use it only on the exact public-information files listed in these instructions.

Next make the badge application: the **CSR**. It includes the service’s public key, its requested name, and a signature made with its private key. `localhost` is the practice name for this computer. **SAN** means the list of names the certificate should cover. This request asks for `localhost`. The box also checks the request’s signature and displays the request, which contains public information.

```bash
openssl req -new -sha256 -key requests/service.key.pem -out requests/service.csr.pem -subj "/O=CyberFoundations Lab/CN=localhost" -addext "subjectAltName=DNS:localhost"
openssl req -in requests/service.csr.pem -noout -verify
openssl req -in requests/service.csr.pem -noout -text > verification/csr-inspection.txt
cat verification/csr-inspection.txt
```
Expected: a request-signature verification message such as `Certificate request self-signature verify OK`, plus requested `DNS:localhost` in the inspection. A CSR contains public information and a signature, not the private key. Record the actual output, not this example.

```text
Requested subject and SAN: Subject: CN=localhost
SAN: DNS:localhost
CSR signature-check result: Certificate request self-signature verify OK
What the successful signature check tells me about the request: The successful signature check tells me that the CSR was created using the matching private key and that the request has not been tampered with.
Why signing an application does not prove permission to use someone else’s website name: Signing an application does not prove permission to use someone else’s website name because it only proves possession of a private key and not ownership of the requested domain name. Anyone can generate a key pair and request a name that they do not own and the signature would sill check out which is domain ownership must be verified by the CA before a certificate is issued.
```

#### Step 4 - Model the Issuing Office

Now pretend you work at the badge office. The office needs its **own** key pair, separate from the service’s key. This box makes that pair and the office’s certificate. The office signs its own certificate; that is called **self-signed**. Printing and signing your own badge does not make other people accept it. Later, we will tell our checking tool to accept this particular office for a particular test.

```bash
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048 -out ca/lesson-ca.key.pem
openssl req -new -x509 -sha256 -days 365 -key ca/lesson-ca.key.pem -out ca/lesson-ca.cert.pem -subj "/O=CyberFoundations Lab/CN=Week 9 Lesson CA" -addext "basicConstraints=critical,CA:TRUE,pathlen:0" -addext "keyUsage=critical,keyCertSign,cRLSign"
chmod 600 ca/lesson-ca.key.pem
openssl x509 -in ca/lesson-ca.cert.pem -noout -subject -issuer -dates
```
**What you should see:** Subject and Issuer both name `Week 9 Lesson CA`, followed by dates. That is expected for this self-signed office certificate. Here the office will sign the service badge directly; there is no middle office (intermediate CA). Your browser and computer’s accepted-office lists have not changed.

```text
Lesson CA subject shown: O = CyberFoundations Lab, CN = Week 9 Lesson CA
Lesson CA issuer shown: O = CyberFoundations Lab, CN = Week 9 Lesson CA
Lesson CA validity dates: notBefore=Sep 23 16:31:23 2026 GMT
notAfter=Sep 23 16:31:23 2027 GMT
Why a self-signed office certificate is not automatically accepted: A self-signed office certificate is not automatically accepted because anyone can create and sign their own certificate and claim to be any office that they want. Trust can only come from a browser or the system deliberately choosing to add that specific CA to its trusted list. The certificate would carry no real authority on its own without the explicit trust decision from the browser or the system.
```

**Pause point:** save your worksheet and practice path before Part B. Your keys and request are saved on the VM.

### Part B - Issue, Inspect, and Verify

#### Step 5 - Set the Badge Rules and Issue the Certificate

This box writes the office’s rules into a small settings file. The rules say: “This badge is for a service called `localhost`, for use as a web server. It cannot issue other badges.”

Copy the entire box, including the last `EOF` on its own line. `EOF` tells the terminal “the file text ends here.” If you see a `>` prompt while entering the block, the terminal is waiting for the remaining lines. Do not paste the next command until the complete block has finished.

You do not need to memorize these settings. `CA:FALSE` means “not an issuing office”; `serverAuth` means “may identify a server”; `DNS:localhost` is the allowed name. The office chooses the final rules instead of accepting everything an applicant might ask for.

```bash
cat > ca/server-ext.cnf <<'EOF'
[server_cert]
basicConstraints=critical,CA:FALSE
keyUsage=critical,digitalSignature,keyEncipherment
extendedKeyUsage=serverAuth
subjectAltName=DNS:localhost
subjectKeyIdentifier=hash
authorityKeyIdentifier=keyid,issuer
EOF
```
**What you should see:** the usual prompt returns; this settings-file step normally prints nothing.

Now issue the badge. The next box uses the **office’s private key** to sign the certificate, gives it a 30-day date period, saves it, and displays it. Its serial number is a randomly chosen tracking number; yours need not match another student’s.

```bash
openssl x509 -req -in requests/service.csr.pem -CA ca/lesson-ca.cert.pem -CAkey ca/lesson-ca.key.pem -set_serial "0x$(openssl rand -hex 16)" -days 30 -sha256 -extfile ca/server-ext.cnf -extensions server_cert -out certificates/service.cert.pem
openssl x509 -in certificates/service.cert.pem -noout -text > verification/certificate-inspection.txt
cat verification/certificate-inspection.txt
```
**What you should see:** Subject includes `localhost`; Issuer includes `Week 9 Lesson CA`; SAN includes `DNS:localhost`; Basic Constraints says `CA:FALSE`; and Extended Key Usage lists TLS Web Server Authentication. If a required value differs, stop and ask for help before the next step.

```text
SAN shown in the certificate inspection: DNS:localhost
Basic Constraints shown in the certificate inspection: critical
CA:FALSE
Extended Key Usage shown in the certificate inspection: TLS Web Server Authentication
Any difference from the expected result (write "none" if there is none): none
```

Use these reminders to fill the table: **Subject** describes the badge holder; **Issuer** names the signing office; **SAN** lists allowed names; **Not Before/Not After** give the start/end dates; **algorithm** means calculation method; **Basic Constraints** says whether this is an issuing office; **Key Usage/EKU** describe allowed jobs. The website’s key and the office’s signature have different jobs.

Your dates, tracking number, and key details will differ from classmates’ files. Copy your finished certificate’s values, not just the application’s values.

| Field | Issued value | Why it matters |
| --- | --- | --- |
| Subject | CN = localhost | The badge holder, identifies who or what the certificate describes |
| Issuer | O = CyberFoundations Lab, CN = Week 9 Lesson CA | Names the office that signed and couches for the certificate |
| SAN | DNS:localhost | Lists the exact names the certificate is allowed to cover, used to check whether or not a visited name is covered |
| Not Before / Not After | Sep 24 01:04:27 2026 GMT / Oct 24 01:04:27 2026 GMT | Defines the valid time window of the certificate, if the dates are outside of this range the certificate should be rejected |
| Subject key algorithm and size | rsaEncryption (2048 bit) | Describes the type and the strength of the service's own public key |
| Certificate signature algorithm | sha256WithRSAEncryption | Describes what method the issuing CA used to sign the certificate, this is a separate job from the website's own key |
| Basic Constraints | critical CA:FALSE | Confirms that this certificate can't act as the issuing office, it can only represent one end entity (the service) |
| Key Usage / Extended Key Usage | TLS Web Server Authentication | Confirms the specific and limited job that the certificate is authorized for, identifying the web server |

#### Step 6 - Check That the Same Public Key Was Carried Through

Did the public key from Ivy’s key pair make it into the application and then the finished badge? This box copies out the public information and calculates a **SHA-256 fingerprint** for each public-key file. A fingerprint is a calculated label we can compare. This box does not display private keys.

```bash
openssl req -in requests/service.csr.pem -pubkey -noout > requests/csr.public.pem
openssl x509 -in certificates/service.cert.pem -pubkey -noout > certificates/service.public.pem
sha256sum requests/service.public.pem requests/csr.public.pem certificates/service.public.pem
```
**What you should see:** three lines beginning with the same long string of letters and numbers. Those strings are the fingerprints, also called **digests**. Matching strings show that the same public key appears in the original public-key file, the CSR, and the certificate. They do not tell us whether the name, dates, or issuing office should be accepted. If the strings differ, stop and ask for help before continuing.

```text
Fingerprint for requests/service.public.pem: 24b57cbea284d1481fa896c9cc58e7a23014c5e9bb0674dc75d3dc6176423068  requests/service.public.pem
Fingerprint for requests/csr.public.pem: 24b57cbea284d1481fa896c9cc58e7a23014c5e9bb0674dc75d3dc6176423068  requests/csr.public.pem
Fingerprint for certificates/service.public.pem: 24b57cbea284d1481fa896c9cc58e7a23014c5e9bb0674dc75d3dc6176423068  certificates/service.public.pem
Do all three match, and what does that show: Yes, all three fingerprints match. This shows that the same public key was consistently carried from the original key pair, into the CSR and into the issued certificate, confirming the the identity of the service was not swapped or changed at any point. It does not confirm whether or not the certificate's name, dates or issuing office should be trusted.
```

#### Step 7 - Run the Passing Verification

Now check the saved certificate without connecting to a running service. This is an **offline check**: it reads files on your VM.

Read the main choices in the command before running it:

| Command part | What we are asking |
|---|---|
| `-CAfile ca/lesson-ca.cert.pem` | Accept this badge office for this check. This chosen starting point is called a trust anchor. |
| `-no-CApath -no-CAstore` | Do not also use the computer’s default certificate folders or store. |
| `-purpose sslserver` | Check that the badge is suitable for a server. |
| `-verify_hostname localhost` | Check that it covers the name `localhost`. |

OpenSSL also uses the computer’s clock to check dates. The last three lines save the result number, show the message, and print that number. An **exit status** of `0` means the command succeeded; another number means it did not. Read the message to learn why. `> ... 2>&1` saves the command’s messages in a text file; it is included for you.

```bash
openssl verify -CAfile ca/lesson-ca.cert.pem -no-CApath -no-CAstore -purpose sslserver -verify_hostname localhost certificates/service.cert.pem > verification/pass.txt 2>&1
W9_STATUS=$?
cat verification/pass.txt
printf 'Exit status: %s\n' "$W9_STATUS"
```
**What you should see:** `certificates/service.cert.pem: OK` and exit status `0`. This is our working starting test, also called the **baseline**. If it fails, stop and ask for help. The later wrong-input tests are meaningful only after this starting test works.

```text
Command message shown by verification/pass.txt: certificates/service.cert.pem: OK
Exit status printed: 0
What this baseline result proves: This baseline result proves that the certificate correctly verifies when checked against the trusted CA that I explicitly specified (Week 9 Lesson CA) for its intended purpose (SSL server) and the correct hostname (localhost). It confirms that the name, dates, purpose and issuing office genuinely match what is expected and the verification process succeeds cleanly, which establishes a known good baseline to compare against when the later tests deliberately introduce a wrong name or wrong CA.
```

#### Step 8 - Ask for the Wrong Name on Purpose

Keep the same badge and office, but ask whether the badge covers `wrong.test`. It was issued for `localhost`, so these names should not match. Copy this complete box; the change is already made for you.

```bash
openssl verify -CAfile ca/lesson-ca.cert.pem -no-CApath -no-CAstore -purpose sslserver -verify_hostname wrong.test certificates/service.cert.pem > verification/wrong-name.txt 2>&1
W9_STATUS=$?
cat verification/wrong-name.txt
printf 'Exit status: %s\n' "$W9_STATUS"
```
Expected: hostname mismatch and a nonzero exit status. The leaf and CA did not change. `wrong.test` is just an expected-name input; this offline command makes no connection to that name.

```text
Command message shown by verification/wrong-name.txt: error 62 at 0 depth lookup: hostname mismatch
error certificates/service.cert.pem: verification failed
Exit status printed: 2
Why the check refused this name: The check refused the name because this certificate's SAN only lists localhost and not wrong test so when verification was told to expect wrong.test, it found no matching entry and reported the hostname mismatch. This confirmed that the name checking part of the verification worked correctly by proving that even a valid, properly signed certificate will still fail verification if the name being checked does not match what the certificate actually covers.
```

#### Step 9 - Ask a Different Office to Vouch for the Same Badge

First make a second pretend office with its own key. It did not issue our service’s badge. This is a **negative test**: we intentionally give wrong information to see whether the check refuses it. The first box makes the second office; the next box checks the unchanged service certificate using only that second office.

```bash
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048 -out ca/unrelated-ca.key.pem
openssl req -new -x509 -sha256 -days 365 -key ca/unrelated-ca.key.pem -out ca/unrelated-ca.cert.pem -subj "/O=CyberFoundations Lab/CN=Unrelated Lesson CA" -addext "basicConstraints=critical,CA:TRUE,pathlen:0" -addext "keyUsage=critical,keyCertSign,cRLSign"
chmod 600 ca/unrelated-ca.key.pem
```

```bash
openssl verify -CAfile ca/unrelated-ca.cert.pem -no-CApath -no-CAstore -purpose sslserver -verify_hostname localhost certificates/service.cert.pem > verification/wrong-ca.txt 2>&1
W9_STATUS=$?
cat verification/wrong-ca.txt
printf 'Exit status: %s\n' "$W9_STATUS"
```
**What you should see:** a message such as `unable to get local issuer certificate`, and an exit status other than `0`. In plain language: “The office you gave me cannot vouch for this badge.” That refusal is expected. A missing-file error is a different problem; ask for help if you see one. Do not install these offices into your browser or computer’s accepted list.

```text
Command message shown by verification/wrong-ca.txt: unable to get local issuer certificate
Exit status printed: 2
Why the unrelated office could not vouch for this badge: The unrelated office couldn't vouch for this badge because it never signed the certificate. The certificate was signed by Week 9 Lesson CA and not this second, unrelated CA. Due to the second CA having no cryptographic relationship to the certificate at all, the verification correctly failed to trace a valid signature chain back to a trusted issuer, refusing to accept a badge it never authorized.
```

**Pause point:** record all three results below before Part C. Keep the same service certificate throughout these tests.

| Test | Leaf file | CA file | Expected name | Actual result / exit status | Explanation |
| --- | --- | --- | --- | --- | --- |
| Passing baseline | service.cert.pem | lesson-ca.cert.pem | localhost | OK / exit  | Name, dates and issuing CA all matched correctly |
| Wrong name | service.cert.pem | lesson-ca.cert.pem | wrong.test | Hostname mismatch / exit 2 | SAN only listed localhost not wrong.test |
| Wrong CA | service.cert.pem | unrelated-ca.cert.pem | localhost | Unable to get local issuer certificate / exit 2 | No valid trust chain exists because the second CA never signed this certificate. |

### Part C - Try a Protected Connection on This VM

So far we checked saved files. Now we will connect two programs on the same VM.

- The **server** waits for a visitor. The **client** makes the visit and checks the badge.
- `127.0.0.1` is the address meaning “this same computer.” Connecting back to yourself is called **loopback**.
- `8443` is the numbered door, or **port**, where our server waits.
- `localhost` is the name we expect on its certificate. An address tells the client where to go; the expected name tells it which identity to check.
- **TLS 1.3** is the protected-connection version we will use. Its handshake is the initial exchange where the programs check identity and arrange message-protection keys.

Keep two terminal windows open: **A runs the server; B runs the checks.** Both must connect to your assigned VM.

#### Step 10 - Start the Listener in Terminal A

Use your existing terminal as **Terminal A**. Make sure you are in the practice folder from Step 2. This box starts your waiting service:

```bash
openssl s_server -accept 127.0.0.1:8443 -cert certificates/service.cert.pem -key requests/service.key.pem -www -tls1_3
```
**What you should see:** `ACCEPT`, and the usual prompt does not return. This is normal: the server is waiting. No client has checked its identity yet. Leave this window alone and continue in Terminal B. Keep the address exactly `127.0.0.1` so the service is available only on this VM. Do not change it to `0.0.0.0` or change network rules.

```text
Message shown in Terminal A after starting the listener: ACCEPT
Time you started the listener: 5:00 p.m.
```

#### Step 11 - Check the Connection in Terminal B

Open a second terminal to the **same VM**. Use `cd` with the exact practice path you recorded in Step 2. Confirm it with `pwd`; the `ca/` and `certificates/` directories must belong to that run. Then execute:

```bash
ss -ltn 'sport = :8443'
```
Expected: a listener at `127.0.0.1:8443`. Save that evidence before the connection test.

```text
Listener row shown by ss: LISTEN            0                 4096                               127.0.0.1:8443                              0.0.0.0:*
```

```bash
openssl s_client -connect 127.0.0.1:8443 -servername localhost -CAfile ca/lesson-ca.cert.pem -no-CApath -no-CAstore -verify_hostname localhost -verify_return_error -tls1_3 -brief < /dev/null > verification/tls-pass.txt 2>&1
W9_STATUS=$?
cat verification/tls-pass.txt
printf 'Exit status: %s\n' "$W9_STATUS"
```
**What you should see:** TLS 1.3, `Verification: OK`, and exit status `0`. If not, confirm Terminal A is still waiting and both terminals use the same practice folder. Ask for help if it still differs.

Two command options have different jobs. `-servername localhost` tells the server which service name we want; this label is called **SNI**. `-verify_hostname localhost` checks whether the certificate actually covers the name. Requesting a name is not the same as checking it. `-verify_return_error` tells the client to stop if the certificate check fails. The short output also shows a **cipher**: the chosen method for protecting messages.

Now change only the expected name, keeping SNI and all other inputs the same:

```bash
openssl s_client -connect 127.0.0.1:8443 -servername localhost -CAfile ca/lesson-ca.cert.pem -no-CApath -no-CAstore -verify_hostname wrong.test -verify_return_error -tls1_3 -brief < /dev/null > verification/tls-wrong-name.txt 2>&1
W9_STATUS=$?
cat verification/tls-wrong-name.txt
printf 'Exit status: %s\n' "$W9_STATUS"
```
**What you should see:** a certificate/name mismatch message and a status other than `0`. Reaching the server’s door is not enough; the badge must also pass the name check. If the message says “connection refused,” the server is not waiting, so that is not evidence of the intended name rejection.

```text
Listener address and port: 127.0.0.1:8443
Connected destination: 127.0.0.1:8443
Expected identity for the passing test: localhost
SNI value: localhost
CA file used: ca/lesson-ca.cert.pem
Negotiated TLS version and cipher from the passing output: TLS_AES_256_GCM_SHA384
Passing verification message and exit status: Verification: OK / 0
Failing verification message and exit status: verify error:num=62:hostname mismatch
What changed between the two tests: What changed between the two sets was the hostname being verified: localhost for the passing test vs. wrong.test for the failing one. Everything else, including the SNI, CA file and the certificate that was presented stayed the same.
Explain each job: the certificate links a name to a public key; the handshake checks use of the matching private key; newly arranged traffic keys protect messages. How are these different? The certificate links a name to a public key and acts as an identity claim to be checked. The handshake proves that the server possess the matching private key and not just a copied certificate. The newly arranged traffic keys are generated during the handshake and afterwards encrypt the actual messages that were exchanged. Each job is independent: the certificate proves identity, the handshakes proves possession and the traffic keys protect the ongoing conversation.
```

#### Step 12 - Stop Only Your Listener

In Terminal A, press **Ctrl+C**. In Terminal B, rerun `ss -ltn 'sport = :8443'`; no listener row should remain. Do not use a broad process-kill command.

In Terminal B, use the original passing inputs but save this stopped-listener result in its own file:

```bash
openssl s_client -connect 127.0.0.1:8443 -servername localhost -CAfile ca/lesson-ca.cert.pem -no-CApath -no-CAstore -verify_hostname localhost -verify_return_error -tls1_3 -brief < /dev/null > verification/no-listener.txt 2>&1
W9_STATUS=$?
cat verification/no-listener.txt
printf 'Exit status: %s\n' "$W9_STATUS"
```

**What you should see:** “connection refused” and a status other than `0` when no service is waiting. There is no one at the door, so the certificate check never gets that far. Keep this result separate from the earlier wrong-badge result.

```text
ss output after you pressed Ctrl+C: State    Recv-Q    Send-Q    Local Address:Port    Peer Address:Port    Process
Message shown by verification/no-listener.txt: 40575DE7C17F0000:error:8000006F:system library:BIO_connect:Connection refused:../crypto/bio/bio_sock2.c:125:calling connect()
Exit status printed: 1
Why this failure is different from a badge failing its check: This failure is different because it happened before any certificate is examined. There is no service listening on port 443 so the TLS handshake never began. A failed badge check means that the connection reached the server successfully and received a certificate but the certificate failed to satisfy the name and trust requirements. This failure meant that there was no one there to check the badge.
```

**Pause point:** the server is now stopped. Save your worksheet and screenshots before Part D.

### Part D - Explain Problems and Collect Your Work

#### Step 13 - Interpret Four Conditions

For each row, explain what went wrong, what message or field you would look at, what you would correct, and which check you would repeat. **Retest** means try the check again after a correction. Use your earlier wrong-name, wrong-office, and stopped-server results to reason about rows 1, 3, and 4. Row 2 is a pretend situation to explain in writing; do not change the VM clock or certificate dates.

| Condition | Which check cannot pass? | What would you look at? | What would you fix, and why? | Which check would you repeat? |
| --- | --- | --- | --- | --- |
| `localhost` is absent from a service's SAN | hostname / name verification | The SAN field to confirm the names the certificate covers | I would reissue the certificate with localhost included in the SAN because the name checkcompares the expected name against the SAN list, not just the subject. | Offline verification test (verify-pass and the live TLS connection test) |
| Not After is in the past relative to the correct clock | Date / validity check | The Not Before and Not After fields on the certifcate, compared against the current time | I would reissue the certificate with a new date range that is valid because an expired certificate fails regardless of how correct everything else is. | Offline verification test and the live TLS connection test |
| An unrelated CA file is supplied | Trust / issuer check | The certificate chain to confirm with CA signed it vs. which CA file was provided for verification | I would supply the correct CA file, the one that signed the certificate because verification only succeeds when checked against the true issuer. | Offline verification test using the correct CA file |
| No listener exists at `127.0.0.1:8443` | The connection itself before any certificate check happens | Whether or not the service is listening by running ss -ltn 'sport = :8443' | I would start the listener because without an active service there is nothing to connect to or check. | THe ss check to confirm if the listener is active followed by the live TLS connection test |

A badge may need replacing when its dates run out. Getting a new certificate is called **renewal**. The service must also be set up to use the new file; that is **deployment**. An office can cancel a certificate before its end date; that is **revocation**. Our file checks did not contact an online cancellation service.

In your own words, explain why getting a replacement file is not enough until the server uses it. Then explain why our passing file check does not prove that someone checked an up-to-date cancellation list.

```text
Getting a replacement certificate isn't enough by itself because the server still has to be reconfigured in order to use it. If the old certificate is still what's being presented, a valid file sitting on a disk does not change what is in practice.

Our passing file check does not prove that someone checked an up-to-date cancellation list because the verification only compared local files and not a live outside service. A certificate could pass our check and still be revoked in the real world.
```

#### Step 14 - Compare the Real Website with Your Practice Service

Use your dated Lab 01 observation and your actual local certificate.

| Comparison | Public website from Lab 01 | Isolated lesson service |
| --- | --- | --- |
| Intended hostname / SAN | example.com, *.example.com | localhost |
| Issuer and chain roles | Leaf signed by Cloudflare TLS Issuing ECC CA 3, which was signed by SSL.com TLS ECC Transit ECC CA R2, which was signed by root SSL.com TLS ECC Root CA 2022 | Leaf (localhost) signed by Week 9 Lesson CA, a single self-signed root with no intermediate |
| Validity interval | July 29, 2026 - October 27, 2026 | September 23, 2026 - September 23, 2027 |
| Allowed purpose | TLS WWW Server Authentication | TLS Web Server Authentication |
| Who accepts the top issuing office, and how is that choice made? | My real browser, because SSL.com's root CA is already included in its built-in trusted store | Only my VM accepts it because I manually told the offline/TLS check to trust it directly using -CAfile; no browser or other system trusts it |
| Where does the service run, and how did you connect? | A real, public server somewhere on the internet, reached over the public internet by my normal browser | A practice server running on my VM only reached by loopback (127.0.0.1:8443) on the same machine |
| Evidence actually observed | Browser's certificate viewer and DevTools showing the real chain and validity | The terminal's output from openssl commands showing my self-created certificate, verification results and TLS handshake results |
| What does this evidence NOT tell you? | If the site's actual content or downloads are safe or if every signature in the chain was independently verified by me | If this setup would work the same way in a real, public-facing deployment or if a real browser or other system would ever trust this practice CA |

## Stop & Check

- Can you explain the separate jobs of the service’s secret key, its application (CSR), the office’s secret key, and the finished badge (certificate)?
- Does the issued SAN contain the intended name?
- Do all three public-key fingerprints match, with no private key displayed?
- Did the correct-input check pass before you tried the wrong inputs?
- Did each failure occur for the predicted reason rather than because of a missing file?
- Did the service wait only at `127.0.0.1:8443`, and did you stop it afterward?

## Test

Your evidence must show: three matching public-key fingerprints; a passing file check; refusal of the wrong name; refusal of the wrong office; a passing live TLS connection; refusal of the wrong name during TLS; and a refused connection after stopping the service. For each check, include the command you ran, the message, and the exit-status number. A number other than `0` is not enough by itself: explain the message. Do not copy the terminal’s prompt into a command.

## Capture Evidence

Take screenshots of the public CSR/certificate inspection and each required test outcome. Capture command inputs and result together when possible. Do not screenshot private keys or unfiltered verbose TLS session dumps. Copy exact relevant text into the worksheet as well, so another learner can follow your reasoning.

## Explain

Write 5–7 sentences telling the story of what you did. Start with “First I made…”, then describe the badge application, the issuing office’s signature, your inspection, your file checks, and the live connection. Say which office you told the client to accept. Include one wrong-input test, why it failed, and what you kept the same.

```text
First, I made a private key and CSR for a practice service named localhost. Then I created my own pretend issuing office, Week 9 Lesson CA and used it to sign that CSR into an actual issued certificate. Next, I inspected the certificate's fields and confirmed that the same public key carried through the key, CSR and certificate using matching fingerprints. Afterwards, I ran offline checks telling the client to accept only my Week 9 Lesson CA, that certificate passed against the correct name and issuer. Followed by a wrong-input test, where I checked the same certificate against an unrelated CA that never signed it and it correctly failed with an 'unable to get local issuer certificate' error. Lastly, I kept the service certificate unchanged throughout, only varying the CA or expected name and finally confirmed the same certificate also passed a live TLS connection test using localhost.

```

## Required Evidence

Save screenshots under `assets/screenshots/week-09/`:

- `week09-lab02-csr-check.png`
- `week09-lab02-issued-certificate.png`
- `week09-lab02-key-correspondence.png`
- `week09-lab02-verify-pass.png`
- `week09-lab02-verify-wrong-name.png`
- `week09-lab02-verify-wrong-ca.png`
- `week09-lab02-loopback-listener.png`
- `week09-lab02-tls-pass.png`
- `week09-lab02-tls-wrong-name.png`
- `week09-lab02-no-listener.png`

Add numbered image suffixes if necessary for readability. Keep `verification/*.txt` on the VM for your own reference; publishing those logs is optional only after a content review. Do not upload any `.key.pem` file, even though it is a classroom key.

### Evidence Uploads (required)

![week09-lab02-csr-check.png](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab02-csr-check.png)

**Caption - CSR inspection:**

![week09-lab02-issued-certificate.png](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab02-issued-certificate.png)

**Caption - issued certificate:**

![week09-lab02-key-correspondence.png](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab02-key-correspondence.png)

**Caption - public-key fingerprints:**

![week09-lab02-verify-pass.png](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab02-verify-pass.png)

**Caption - passing offline check:**

**Caption - wrong-name refusal:**

![week09-lab02-verify-wrong-ca.png](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab02-verify-wrong-ca.png)

**Caption - wrong-office refusal:**

![week09-lab02-loopback-listener.png](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab02-loopback-listener.png)

**Caption - loopback listener:**

![week09-lab02-tls-pass.png](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab02-tls-pass.png)

**Caption - passing TLS connection:**

![week09-lab02-tls-wrong-name.png](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab02-tls-wrong-name.png)

**Caption - TLS wrong-name refusal:**

![week09-lab02-no-listener.png](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab02-no-listener.png)

**Caption - stopped listener:**

### Optional Extra Evidence (leave blank if you do not need it)

**Caption - extra image 1:**

**Caption - extra image 2:**

## Analysis Questions

**Analysis Question 1.** Who signs the badge application: the service or the office? Who signs the finished badge? Why does signing an application not prove you have permission to use someone else’s website name? Write at least 3 sentences.

```text
The service signs the badge application (the CSR) with its own private key. The issuing office signs the finished badge (the certificate) with its own key. Signing an application only proves possession of a key, not ownership of the domain name, which is why a real CA must independently verify domain ownership before issuing.

```

**Analysis Question 2.** You kept the same certificate but changed the name or office used for the check. Why did the answer change? Use the messages from both wrong-input tests. Write at least 3 sentences.

```text
The answer changed because each test purposely mismatched a different condition. Changing the expected name to wrong.test caused a hostname mismatch because the SAN only lists localhost. The wrong CA test failed because that CA did not sign the certificate. This proves that the right name and the right issuer are needed in order for verification to pass.
```

**Analysis Question 3.** Explain the difference between a server waiting at a door, a badge passing its checks, and messages traveling through a protected connection. If you see “connection refused,” what would you check first, and why? Write at least 3 sentences.

```text
A server waiting at a door means that the service is running and reachable on its port, independent of any certificate. A badge passing its checks means the certificate's name and issuer are correct, this only matters once a connection is reached. Messages traveling through a protected connection is the encryption that follows when both are satisfied. If I saw 'connection refused' I would check whether or not the listener was running because that is a reachability problem and not a trust problem.

```

## Submission Checklist

- [x] Fresh practice path recorded; no previous files or SSH configuration changed

- [x] Dedicated service key and CSR created; issuer role explained

- [x] Issued certificate fields and matching public components recorded

- [x] Offline success, wrong-name failure, and wrong-CA failure explained

- [x] Loopback listener address and passing/failing TLS evidence captured

- [x] Listener stopped; connection failure distinguished from certificate failure

- [x] Four-condition diagnosis and public/local comparison completed

- [x] Analysis questions and walkthrough written in my own words

- [x] Lab 01, notes, and reflection included for Portfolio Deliverable 3

- [x] No private keys, credentials, or access URLs in the submission

- [x] Worksheet committed to `week-09/labs/lab-02-build-and-test-digital-trust.md`

## GitHub / Lab Portal Submission

Your **portfolio repository** is your course-work folder on GitHub. A **commit** saves a set of changes there. Use the editing or upload method you practiced in Week 7; ask for a demonstration if you need one. Upload only the listed documents and reviewed images, never the whole VM folder.

1. Fill in this worksheet on this page and press **Save Progress** as you work. Your answers are stored in the portal and reload when you return.
2. Press **Submit to GitHub**. The portal commits the finished worksheet to the submission path shown at the top of your connected portfolio repository.
3. Add only the reviewed evidence images under `assets/screenshots/week-09/`, or paste their image addresses in the evidence fields above.
4. Complete the Week 9 Notes and Reflection worksheets in the portal the same way. Use the submissions guide to assemble Portfolio Deliverable 3.
5. Open the committed Markdown and every image on GitHub to confirm formatting and privacy.

*CyberVisionaries Institute · CyberFoundations · Tier I*
