# Week 6 Lab 04 — Reading the Blueprints

**Student Name:** N. Williams

**Date Completed:** August 25, 2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 6  
**Submission Path:** `week-06/labs/lab-04-reading-the-blueprints.md`

---

> ### 🔒 Cloud Heights Security Rule
> Your Bastion link and Cloud Heights password are **private access credentials**. Never paste either into a worksheet, screenshot, GitHub repository, Circle post, or chat message. When taking screenshots, crop out the browser address bar and all login information.

---

## Overview

**This is a SHORT lab — 15 to 20 minutes.** It is deliberately small. You already have the commands; this lab is about matching a drawing to reality.

The **Cloud Heights Network Blueprint** is displayed at the top of this lab page in the portal. Everything you write about the network's architecture comes from that blueprint or from your own machine — never from a guess.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Cloud Heights — live Ubuntu 22.04 VM, reached through Azure Bastion |
| Source of truth | The Cloud Heights Network Blueprint shown at the top of this lab page |
| Commands used | `ip addr`, `ip route` |
| Known value | Student subnet: **`10.60.6.0/26`** |

---

## Part A — Read the Drawing

### Step 1 — Record the Architecture Values

From the blueprint at the top of this page, record each value **exactly as drawn**. If a value is not shown on the blueprint, write "not shown on blueprint" — do not guess.

| Item | Value from the blueprint |
| --- | --- |
| VNet name | vnet-cf-labs |
| VNet address space | 10.60.6.0/24 |
| Student subnet range | 10.60.6.1 - 10.60.6.62 |

---

## Part B — Verify Against Your Own Machine

### Step 1 — Confirm Your Address Lives in the Subnet

Run `ip addr` and find your private IPv4 address.

Command and output:

```
ip addr

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 7c:ed:8d:57:01:e4 brd ff:ff:ff:ff:ff:ff
    inet 10.60.6.21/26 metric 100 brd 10.60.6.63 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::7eed:8dff:fe57:1e4/64 scope link 
       valid_lft forever preferred_lft forever
3: enP53352s1: <BROADCAST,MULTICAST,SLAVE,UP,LOWER_UP> mtu 1500 qdisc mq master eth0 state UP group default qlen 1000
    link/ether 7c:ed:8d:57:01:e4 brd ff:ff:ff:ff:ff:ff
    altname enP53352p0s2
```

Your private IP:

```
10.60.6.21/26
```

Explain how you know your address falls inside `10.60.6.0/26` — what range does that prefix actually cover:

```
I know that my address falls inside 10.60.6.0/26 because the /26 lets me know that there are 64 addresses in the block. Of the 64 addresses 10.60.6.0 is the network address and 10.60.6.63 is the broadcast address. The network address and broadcast address are not useable so that leaves me with 62 useable addresses, 10.60.6.1 - 10.60.6.62. My address is 10.60.6.21 and it falls in that range.
```

### Step 2 — Confirm Route Behaviour

Run `ip route`.

Command and output:

```
ip route

default via 10.60.6.1 dev eth0 proto dhcp src 10.60.6.21 metric 100 
10.60.6.0/26 dev eth0 proto kernel scope link src 10.60.6.21 metric 100 
10.60.6.1 dev eth0 proto dhcp scope link src 10.60.6.21 metric 100 
168.63.129.16 via 10.60.6.1 dev eth0 proto dhcp src 10.60.6.21 metric 100 
169.254.169.254 via 10.60.6.1 dev eth0 proto dhcp src 10.60.6.21 metric 100 

```

What the default route tells you about traffic that is not destined for your own subnet:

```
The default route tells me where traffic goes when the destination is not part of my own local subnet.
```

### Step 3 — Capture Your Evidence

**Required filename:** `blueprint-verified.png`

This must be **your own `ip addr` and `ip route` output** — not a re-screenshot of the blueprint. Crop out the address bar and any login information.

![Blueprint verified — my address inside the student subnet](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-06/blueprint-verified.png)

---

## Part C — How Traffic Actually Moves

### Step 1 — No Public IP

Your VM has a private address and **no public IP**. Explain what that means for who can reach it directly from the internet:

```
Due to my VM only having a private IP address and no public IP address, it cannot be reached directly from the internet. Private addresses cannot be routed on the public internet. An outside device cannot send traffic directly to my VM because my private address only has meaning within my own local network. If anyone attempts to reach my VM they would have to go through a controlled entry point because my VM does not have a public facing address for external traffic to target.
```

### Step 2 — Outbound vs. Inbound

Outbound internet traffic from your VM leaves through address **translation (NAT)**. Inbound access for you arrives through **Azure Bastion**, not through a public address on the VM.

Explain both directions in your own words:

```
The outbound traffic from my VM leaves through NST so when my VM sends traffic out to the internet Azure will translate my private address into a public address, send the traffic and translate the response back to my VM without my VM needing its own public IP address. Inbound access is different because the VM has no public IP for anything to directly connect to so Azure Bastion provides its own public IP and connects to my IP using that private IP address. Therefore, my VM does not need a public IP in order for me to access it.
```

### Step 3 — The Guard Post You Do Not Touch Yet

Each student machine sits behind its own **network security group** — a per-student guard post that decides what traffic is allowed in.

**In Week 6 you do not configure it.** Week 7 is when you take control of those rules.

Write one sentence naming what the guard post does and one sentence stating what you are *not* doing with it this week:

```
The network security group determines what traffic is allowed to go in or out of the machine based on defined rules for ports, protocols and source addresses. This week, I am not creating or modifying any of the rules myself.
```

---

## Analysis Questions

**Analysis Question 1.** Why would an organization put every student machine in one small subnet instead of giving each machine a public address? *(Minimum 3 sentences.)*

```
An organization would put every student machine in one small subnet instead of going each machine a public address because one small subnet keeps the machines isolated from the public internet, which would reduce the attack surface and prevent outside access to the machine. Grouping into one small subnet would also make it easier  to apply consistent security policies and monitoring across the entire group at once instead of managing separate public facing security for each machine individually. Lastly, public IP address are limited and are not cost effective so using private addresses in a shared subnet is more efficient for coursework because it doesn't need to be exposed to the public.
```

**Analysis Question 2.** Segmentation means separating a network into parts that cannot freely reach each other. Give one concrete benefit of segmentation during a security incident. *(Minimum 3 sentences.)*

```
A concrete benefit of segmentation during a security incident would be containment. If an attacker compromises one machine that sits on a segmented submit, their access would not automatically extend to machines that sit in other segments, which limits how far the incident can spread. It would be a lot easier to isolate the affected segment by cutting it off from the rest of the network while the incident is being investigated without having to shut down the entire system. Segmentation also narrows the scope of what needs to be checked while responding to the incident so that responders can focus on the specific segment that contains the compromised machine.
```

**Analysis Question 3.** A diagram and a live machine disagree about an address range. Which do you trust, what do you do next, and why? *(Minimum 2 sentences.)*

```
If a diagram and a live machine disagree about an address range, I would trust the output from the live machine over the diagram because the diagram only represents what was documented at some point and the live machine reflects the current configuration. The next step I would take would be to further investigate in order to find out why the live machine and the diagram are disagreeing by checking if the diagram is outdated or if the live configuration was incorrectly changed since the discrepancy could be an indication of a documentation gap or an unauthorized change that needs to be flagged. 
```

---

## Submission Checklist

- [x] VNet name, address space, and subnet range recorded from the blueprint (Part A)

- [x] `ip addr` run and own private IP confirmed inside `10.60.6.0/26` (Part B, Step 1)

- [x] `ip route` run and default route behaviour explained (Part B, Step 2)

- [x] `blueprint-verified.png` captured from your own terminal, cropped, uploaded to `assets/screenshots/week-06/` (Part B, Step 3)

- [x] Private address / NAT / Bastion explained (Part C, Steps 1–2)

- [x] Per-student guard post identified — and explicitly not configured this week (Part C, Step 3)

- [x] All three Analysis Questions answered (minimum sentence counts met)

- [x] This file is committed to your portfolio repo at `week-06/labs/lab-04-reading-the-blueprints.md`

---

## GitHub Commit Subsection

1. Open **Week 6 → Lab 04: Reading the Blueprints** in the Lab Portal.
2. Fill in the worksheet fields and upload `blueprint-verified.png` to `assets/screenshots/week-06/`.
3. Click **Submit to GitHub** — the Portal commits to `week-06/labs/lab-04-reading-the-blueprints.md`.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
