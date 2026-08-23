# Week 6 Lab 02 — Knocking on Door 22

**Student Name:** N. Williams

**Date Completed:** August 22, 2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 6  
**Submission Path:** `week-06/labs/lab-02-knocking-on-door-22.md`

---

> ### 🔒 Cloud Heights Security Rule
> Your Bastion link and Cloud Heights password are **private access credentials**. Never paste either into a worksheet, screenshot, GitHub repository, Circle post, or chat message. When taking screenshots, crop out the browser address bar and all login information.

---

## Overview

Week 5 told you SSH is how administrators reach a machine over the network, and that it knocks on **port 22**. This week you knock yourself. You are already inside Cloud Heights through Bastion — now you will open a second, nested SSH session from your machine *to itself* and watch every step of what SSH does before it lets you in.

Starts **guided**, finishes **independent**. Expect 30–40 minutes.

**This lab uses password authentication only.** SSH keys are Week 8. Do not go looking for them yet.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Cloud Heights — live Ubuntu 22.04 VM, reached through Azure Bastion |
| Username | `analyst` |
| Password | Provided separately. Never typed into this worksheet. |
| Commands used | `ssh`, `whoami`, `hostname`, `pwd`, `exit` |
| Prerequisite | Week 6 Lab 01 completed |

**Before you start:** open **My Lab Environment**, start your VM if needed, wait for **Running**, then open Cloud Heights.

---

## Part A — Two Ways Into the Same Room

### Step 1 — Name the Path You Already Used

You reached Cloud Heights through a browser session. Something else handled the network hop for you.

Describe, in your own words, what the Bastion/browser path did on your behalf:

```
Bastion acted as the client on my behalf. It was the side that started the connection.
```

### Step 2 — Predict the Manual Path

You are about to type an SSH command by hand. Before you run it, write what you expect to happen and what you expect to be asked for:

```
I am expecting the machine to verify the host and I expect to be asked for a password.
```

---

## Part B — Knocking

### Step 1 — Run the SSH Command

In your Cloud Heights terminal, run:
```
ssh analyst@localhost
```

**Stop before typing anything else.**

### Step 2 — Read the First-Connection Prompt

The first time SSH connects to a host it has never seen, it shows you the host's **fingerprint** and asks whether you want to continue connecting. This is not an error. It is SSH telling you: *I have no record of this machine yet — do you recognise it?*

Paste the prompt you saw (fingerprint line included — a fingerprint is not a credential):

```
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ED25519 key fingerprint is SHA256:G1fOlmXJg55eP+m1UNopr0wQPS4f3O8J7MCxy9B7cx4.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? ssh 
```

Explain why you were willing to answer `yes` here — what makes this an expected first connection rather than a suspicious one:

```
I would answer yes because localhost is referring to my own machine and I'm connecting to myself not an unfamiliar remote network somewhere else on a server. This is an expected first connection and not suspicious because the localhost can't be established warning appears because I've never made an SSH connection to this host before. 
```

### Step 3 — Enter Your Password

Type `yes`, then enter your password when prompted.

**Linux does not echo password input.** No characters, no dots, no asterisks appear as you type. The terminal looks frozen. It is not — type the password and press Enter.

What did the screen show while you typed:

```
The screen was still as I typed, nothing was happening.
```

### Step 4 — Prove You Are in the Nested Session

Inside the new session run each of these and record the output:
```
whoami
```

```
analyst
```

```
hostname
```

```
cf-student-02
```

```
pwd
```

```
/home/analyst
```

### Step 5 — Notice the Prompt

Compare the prompt now to the prompt before you ran `ssh`. Describe anything that changed and anything that looks identical, and explain why it looks that way given where you connected to:

```
The command prompt changed to yellow.
```

### Step 6 — Capture Your Evidence

Screenshot the terminal showing the first-connection prompt and the successful session.

**Required filename:** `ssh-first-connection.png`

**Crop rules.** No Bastion URL, no address bar, no password field, no login screen. The fingerprint text is fine.

![SSH first connection and nested session](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-06/ssh-first-connection.png)

### Step 7 — Leave

Run:
```
exit
```
What did the prompt look like after exiting, and how do you know you are back in the original session:

```
When I ran the exit prompt I was disconnected. Once I clicked reconnect in the dialog box I was back in the original session.
```

---

## Part C — The Deliberate Failure (Independent)

### Step 1 — Knock With the Wrong Name

Run an SSH command to `localhost` using a username that does not exist on this machine — for example `ssh notauser@localhost`. Enter anything at the password prompt.

Command you ran:

```
ssh cyberuser@localhost

```

Output:

```
cyberuser@localhost's password: 
Permission denied, please try again.
cyberuser@localhost's password: 
```

### Step 2 — Read the Failure Correctly

`Permission denied` is a **failure of authentication**, not a failure of the network.

Explain what the network and SSH already had to do successfully in order for you to be told "permission denied" at all:

```
In order for me to be told "permission denied" my machine had to successfully reach the remote host over the network. The SSH service on the remote machine had to be listening and accepting connections, in other words Port 22 had to be open and reachable. The SSH handshake had to be successful and lastly, the host verification had to resolve.
```

---

## Analysis Questions

**Analysis Question 1.** Distinguish *reach* from *authentication*. Which one had already succeeded when you saw a password prompt, and how do you know? *(Minimum 3 sentences.)*

```
Reach refers to my machine connecting to the remote server over the network. Authentication refers to the acceptance of the credentials that I provided and prove if I'm allowed to log in. Seeing the password prompt let me know that reach had succeeded because the network connection, port 22 and the SSH handshake were successful. If reach had failed I would have gotten a connection error or timeout, not a password request.
```

**Analysis Question 2.** You accepted a host fingerprint today because you knew you had just connected to your own machine. Describe a situation where accepting a fingerprint without thinking would be a real problem. *(Minimum 3 sentences.)*

```
Accepting a fingerprint without thinking would be a real problem on an untrusted network where attackers have the ability to intercept connections. When attackers intercept connections they will present a fake server while pretending to be a real server. If I were to blindly type yes without knowing the source is trusted, I would connect to the attackers server and the attacker would have access to everything that I typed after being connected.
```

**Analysis Question 3.** What changed and what stayed the same when you moved from the outer session into the nested SSH session, and why? *(Minimum 2 sentences.)*

```
When I moved from the outer session into the nested SSH session, the color of the terminal remained black but the color of the command prompt changed. When the color of the command prompt changed that let me know that I was in a remote session.
```

**Analysis Question 4.** A colleague says "SSH is broken, I got permission denied." Using only what you learned in this lab, what would you tell them is already working, and what would you check next? *(Minimum 3 sentences.)*

```
If a colleague says "SSH is broken, I got permission denied," I would tell them that the SSH isn't broken because "permission denied" means that the network connection, port 22 and the SSH handshake were all successful. If there was a connection issue a different error such as "timeout" would have occurred. What did fail was the authentication, which means that the server received their credentials but they were rejected as invalid. I would check if they are using the correct username, password or SSH key and if they are trying to connect with the correct account for the server.
```

---

## Submission Checklist

- [x] Bastion path vs. manual SSH path described (Part A)

- [x] `ssh analyst@localhost` run and the first-connection prompt recorded (Part B, Steps 1–2)

- [x] Password entered; non-echoing input observed and described (Part B, Step 3)

- [x] `whoami`, `hostname`, `pwd` run inside the nested session (Part B, Step 4)

- [x] Prompt change described (Part B, Step 5)

- [x] `ssh-first-connection.png` captured, cropped, uploaded to `assets/screenshots/week-06/` (Part B, Step 6)

- [x] Session exited cleanly (Part B, Step 7)

- [x] Bad-username test run and `Permission denied` output recorded (Part C)

- [x] All four Analysis Questions answered (minimum sentence counts met)

- [x] This file is committed to your portfolio repo at `week-06/labs/lab-02-knocking-on-door-22.md`

---

## GitHub Commit Subsection

1. Open **Week 6 → Lab 02: Knocking on Door 22** in the Lab Portal.
2. Fill in the worksheet fields and upload `ssh-first-connection.png` to `assets/screenshots/week-06/`.
3. Click **Submit to GitHub** — the Portal commits to `week-06/labs/lab-02-knocking-on-door-22.md`.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
