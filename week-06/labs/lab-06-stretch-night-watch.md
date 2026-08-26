# Week 6 Stretch Lab — Night Watch (Optional)

**Student Name:** N. Williams

**Date Completed:** August 26, 2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 6  
**Submission Path:** `week-06/labs/lab-06-stretch-night-watch.md`

---

## Overview

**This lab is optional.** Skipping it costs you nothing. Your Week 6 submission is complete and full-credit with Labs 01–05 alone, and choosing not to do this does not make your week's work lesser in any way. It is here for students who want to connect Cloud Heights to the network they actually live on. Expect about 30 minutes if you take it on.

**Built-in tools only.** Use commands that already ship with your own computer. **Do not install any software** for this lab.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Your own personal computer — not Cloud Heights |
| Tools | Built-in only: `ipconfig` / `Get-NetIPConfiguration` (Windows) or `ifconfig` / `ip addr` / `netstat -rn` (macOS/Linux), plus `ping` |
| Installs | None. If it needs installing, it is out of scope. |
| Screenshot | Required **only if you submit**: `stretch-real-network.png`, redacted |

> ### 🔒 Redaction Rule
> If you submit this lab, you must redact before uploading. Never publish your home network identity.

---

## Part A — Your Own Address

### Step 1 — Read Your Machine's Network Configuration

Use the built-in command for your operating system to show your address, subnet mask/prefix, and default gateway.

Command you ran:

```
ipconfig
```

Your private IP and prefix/mask (this is a private address — safe to record):

```
Private IP: 192.168.1.156
Prefix: 255.255.255.0
Mask: /24
```

Your default gateway (private address — safe to record):

```
192.168.1.1
```

**Do not record your public IP address anywhere in this file.**

### Step 2 — Compare to Cloud Heights

Compare your home addressing to your Cloud Heights addressing — address range, prefix size, and how many machines each network could hold:

```
My home network uses a private address in the 192.168.0.0 - 192.168.255.255 range with a /24 prefix (giving it 254 usable addresses). My Cloud Heights VM used a smaller /26 prefix (giving it 62 usable addresses) because it's a lab environment with a smaller, separate subnet allocated to each student instead of putting everyone on one large shared subnet. Both networks use private IP addresses that are not routable on the public internet.
```

---

## Part B — Two Gateways, Two Behaviours

### Step 1 — Ping Your Home Gateway

Ping your own default gateway.

Output:

```
Pinging 192.168.1.1 with 32 bytes of data:
Reply from 192.168.1.1: bytes=32 time=4ms TTL=64
Reply from 192.168.1.1: bytes=32 time=14ms TTL=64
Reply from 192.168.1.1: bytes=32 time=4ms TTL=64
Reply from 192.168.1.1: bytes=32 time=3ms TTL=64

Ping statistics for 192.168.1.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 3ms, Maximum = 14ms, Average = 6ms
```

### Step 2 — Compare to Azure's Silent Gateway

Your home router almost certainly answered. The Azure gateway in Lab 03 did not.

Explain why both behaviours are normal, and what that teaches you about assuming a non-answer means failure:

```
Both behaviors are normal because the ping/ICMP response or lack of, is a deliberate configuration on each machine. MY home router is configured to respond to ICMP by default and for security reasons, the Azure Gateway is deliberately configured not to respond to ICM, even though traffic is still being routed. This teaches me that no response to a ping does not automatically mean failure, it only tells me about the ICMP configuration for the devices and not if the network is working. This is why other evidence, such as successful connections to known destinations is needed before coming to the conclusion that anything is broken.
```

---

## Part C — Redaction (Required Only If You Submit)

**Required filename:** `stretch-real-network.png`

Redact **before** uploading. Redaction targets:

- your **public IP address**
- your computer's **hostname**
- your **shell username**
- any **ISP-identifying names** (router model strings, provider names, SSIDs)

**Method:** crop the image, or cover the text with **solid opaque boxes**. **Do not use blur or pixelation** — both can be reversed.

![Home network configuration — redacted](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-06/stretch-real-network.png)

List what you redacted and the method you used:

```
I redacted my shell username and IPv6 address. I redacted this information with solid opaque boxes. 
```

---

## Analysis Questions

**Analysis Question 1.** What is genuinely different between the network you sit on at home and the one Cloud Heights sits on, and what is essentially the same? *(Minimum 3 sentences.)*

```
THe scale and purpose is genuinely different. My home network is a small personal network with a /24 subnet (254 addresses) and Cloud Heights sits on a smaller, isolated /26 subnet (62 addresses). MY home network connects through my own router and ISP. Cloud Heights connects through Azure's gateway, NAT and Bastion and I don't control these. The underlying logic is essentially the same, both use private IP addresses that are not reachable from the public internet, both relay on a default gateway in order to reach anything beyond their own local network and the core concepts - subnet masks, routing and address identically apply either way.
```

**Analysis Question 2.** Why is publishing your public IP, hostname, and username together riskier than publishing any one of them alone? *(Minimum 3 sentences.)*

```
Publishing my public IP, hostname and username together riskier than publishing any one of them alone because when combined they build a more complete profile that can identify me and target me or my machine. A public IP address alone shows where my connection is but without a username or hostname, it does not clearly tie back to me or my device. However, if they are together, my IP address can be correlated to my network, my hostname to my machine and my username to the active account.
```

---

## Submission Checklist (Only If You Choose to Submit)

- [x] Home address, mask, and gateway recorded — **public IP not recorded**

- [x] Home network compared to Cloud Heights (Part A, Step 2)

- [x] Home gateway pinged and compared to Azure's silent gateway (Part B)

- [x] `stretch-real-network.png` redacted with crop or solid boxes (no blur), uploaded to `assets/screenshots/week-06/`

- [x] Redaction list recorded (Part C)

- [x] Both Analysis Questions answered

- [x] This file is committed to your portfolio repo at `week-06/labs/lab-06-stretch-night-watch.md`

---

## GitHub Commit Subsection

1. Open **Week 6 → Stretch Lab: Night Watch** in the Lab Portal.
2. Fill in the worksheet fields and upload your redacted screenshot to `assets/screenshots/week-06/`.
3. Click **Submit to GitHub** — the Portal commits to `week-06/labs/lab-06-stretch-night-watch.md`.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
