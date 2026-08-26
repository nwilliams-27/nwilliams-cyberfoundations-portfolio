# Week 6 Lab 05 — Layer Detective

**Student Name:** N. Williams

**Date Completed:** August 25, 2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 6  
**Submission Path:** `week-06/labs/lab-05-layer-detective.md`

---

## Overview

**This is a SHORT lab — 20 to 30 minutes — and it needs no VM.** No Cloud Heights session, no simulator, no screenshot. This is a thinking lab: you take the evidence you have already collected in Weeks 5 and 6 and sort it into layers.

This is an **independent** lab.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | This worksheet only — nothing to start, nothing to connect to |
| Prerequisite | Week 5 labs and Week 6 Labs 01–04 |
| Screenshot | None required |

---

## Part A — The Seven-Row Table

Fill in every row. For the last column, name one **real thing you personally saw** in Weeks 5–6 that belongs at that layer.

| # | Layer name | One-line job | Real thing from Weeks 5–6 |
| --- | --- | --- | --- |
| 7 | Application | The software or service that I interact with, such as Google. | HTTP request in the Packet Inspector |
| 6 | Presentation | Formats, encrypts and traslates data between systems | TLS encrytion in packet 10 of the packet inspector that retuend the "not readable in this capture" result |
| 5 | Session | Starts, stops and maintains connections between two machines | Connecting to Bastion, staying authenticated and running exit to end it |
| 4 | Transport | Manages delivery between two machines, usus ports to direct traffic to the right service | The TCP handshake from the packet inspector |
| 3 | Network | IP addressing and routing, how data gets from one network to another | IPv4 address, subnet mask and default gateway ib Bastion |
| 2 | Data Link | Communication between devices on the same local network: | MAC address in the ip addr output |
| 1 | Physical | Electrical signals, cables and hardware that transfers raw bits | CPU, RAM and disk in the Azure data center |

---

## Part B — Case Files

For each case, name the layer where the problem lives, and name the evidence proving the layers **below** it were already working.

### Case File 1 — The Name That Went Nowhere

A hostname lookup fails, but pinging the machine's IP address directly succeeds.

Layer:

```
Application - Layer 7
DNS
```

Evidence that the layers below were working:

```
A successful ping to the IP address proves that layers 1-3: the physical connection, addressing and routing were functioning properly because the packets reached their destination and returned successfully. This isolated the failure to name resolution, which happens at the application layer.
```

### Case File 2 — Permission Denied

`ssh` to a host returns `Permission denied` after a password prompt.

Layer:

```
Application - Layer 7

```

Evidence that the layers below were working:

```
In order to receive a prompt for a password, the network, port 22 and the SSH handshake protocol have to be successful. If a lower level had failed I would have gotten a connection error. This confirms that Layers 1 through 6 were working and isolates the failure to rejected credentials at the application layer.
```

### Case File 3 — The Cable Story

A machine reports no link on its interface and has no address at all.

Layer:

```
Physical - Layer 1
```

Evidence and reasoning:

```
Layer 1 is the lowest layer so there is no evidence to support if any layers below it were working. A "no link" status means that the physical connection has failed and without a physical link none of the higher layers will work.
```

### Case File 4 — Ping Works, The Page Does Not

`ping` to a server succeeds, but `curl http://<that server>` returns nothing useful.

Layer:

```
Application - Layer 7
```

Evidence that the layers below were working:

```
The successful ping confirmed that the physical connection, addressing and routing were working because the server responded to the network level request. This isolates the problem to the web service or application not working properly because reachability has been confirmed.
```

### Case File 5 — Wrong Neighbourhood

A machine has an address, but its default route points somewhere that cannot forward its traffic.

Layer:

```
Network - Layer 3
```

Evidence and reasoning:

```
A machine with valid address confirms that Layers 1 and 2 are working because an address was obtained successfully and there is a working physical/data link connection. The problem is at Layer 3 because the routing configuration is broken. The default route directed traffic to a destination that was unable to forward it so the machine's ability to reach anything beyond its own local segment failed even though the local connection works.
```

---

## Part C — The Silent Gateway Case

In Lab 03 the Azure default gateway did not answer your ping. However, your VM had a valid default route configured, and your local communication with the Grid Beacon — the ping replies, the HTTP banner, and `TRACE ID: CF-NET-0604` — succeeded.

A failed gateway ping is one piece of evidence — not automatically proof of a gateway or network failure. But the evidence you weigh against it has to be the right kind of evidence.

The Grid Beacon at `10.60.6.4` sits on the same local subnet as your VM (`10.60.6.0/26`). Reaching it proves **local-subnet connectivity** — that traffic never crosses the default gateway, so beacon success alone cannot prove the gateway forwarded anything. Your `ip route` output proves a **default route is configured** — your VM knows where it intends to send non-local traffic — but it does not prove the gateway forwarded that traffic. The evidence that demonstrates the **default path is functioning** is successful communication with a destination outside `10.60.6.0/26`, such as the outbound internet access through NAT that you examined in Lab 04.

### Step 1 — Rule on the Case

Is the failed gateway ping enough evidence to declare a network-layer failure? Explain your answer using the other evidence you collected. In your response, distinguish between:

- evidence that proves **local-subnet connectivity**
- evidence that proves a **default route is configured**
- evidence that supports **successful off-subnet connectivity**

```
A failed gateway ping is not enough evidence to declare a network-layer failure. The output received from ip addr shows a valid address and subnet configuration on my interface and proves local subnet connectivity. The output received from ip route shows a gateway showing a specific gateway address set for traffic leaving the local subnet proves that the default route is configured. The successful ping to a known good target that is beyond my local network proves that the off subnet connectivity works, which means the traffic passed through the gateway and returned successfully. All of this evidence combined proves that the network layer is functioning properly. The failed ping to the gateway only shows the ICMP configuration and not a network failure.
```

### Step 2 — Name the Correct Conclusion

For each of these four results, state what it actually proves: the Grid Beacon at `10.60.6.4` answering, the default route shown by `ip route`, a successful connection to a destination outside your local subnet, and the gateway's failed ping. Then state the rule you would give a junior colleague about the difference between an observation ("the gateway did not answer my ICMP probe") and a diagnosis ("the gateway is broken"):

```
The Grid Beacon at 10.60.6.4 answering proves that the destination can be reached on the local network. The default route from ip route proves that a path beyond the subnet has been configured on my machine, it does not prove that the path works. A successful connection outside of my local subnet proves that the route is functioning because the traffic passed through and returned. The failed ping to the gateway proves that the device did not respond to ICMP, it does not prove that anything is broken.

A rule I would give a junior colleague: interpretation is not attached to an observation, the observation is only what the tool reported and the diagnosis is conclusion that has been formed from evidence. "The gateway didn't answer my ICMP probe" is true; "the gateway is broken" is a conclusion that needs more evidence to support it in order for it to be justified.
```

---

## Part D — Two Models, One Job

The OSI model has seven layers. The practical TCP/IP model most engineers speak day to day has four or five.

### Step 1 — Map Them

Briefly show how the seven OSI layers collapse into the practical model:

```
Application -> Layer 5, Layer 6 and Layer 7 -> Session, Presentation, Application
Transport -> Layer 4 -> Transport
Internet -> Layer 3 -> Network
Network Access -> Layer 1 and Layer 2 -> Physical, Data Link
```

### Step 2 — When Each Is Useful

Explain when the seven-layer vocabulary helps and when the practical model is the better tool:

```
The seven-layer vocabulary helps when trying to identify exactly where the problem lived because it separates the addressing, sessions and application behavior into layers which helps with troubleshooting and explaining findings clearly. The practical TCP/IP model is a better tool in real world, day to day trouble shooting because most tools and protocols are mapped directly to it.
```

---

## Analysis Questions

**Analysis Question 1.** Explain the Ladder Rule using layer language. What does "test the near thing first" mean when the rungs are layers? *(Minimum 3 sentences.)*

```
Testing the near thing first means to start with the layers that are closest to my machine; physical connection, my address and my local subnet before assuming that a problem exists further away. In the OSI model each layer depends on the one below it so I would need to start with the lower layers first. When the rungs are layers this means testing Layers 1-3 (physical, data, network) before testing Layers 4-7 (transport, session, presentation, application) instead of starting with the furthest, most complex layer.
```

**Analysis Question 2.** Why is "which layer is this?" a faster question than "what is broken?" when you are under pressure? *(Minimum 3 sentences.)*

```
When under pressure, "which layer is this?" is a faster question than "what is broken" because it immediately narrows the problem down to a specific area instead of leaving the area wide open. "What is broken?" is an invitation to guess what the problem is which wastes valuable time on possibilities that don't match the symptoms. By identifying the layer first, evidence proves that everything below it is working, therefore troubleshooting can be directed to the likely cause at that specific layer.
```

**Analysis Question 3.** Pick one case file from Part B and describe the very next command you would run to confirm your ruling, and what result would change your mind. *(Minimum 2 sentences.)*

```
When the ping to the server succeeded but curl://http<that-server> returned nothing useful, the next command I would run is curl -i http://<that-server> so that I could see the response headers and status code because that confirms if the web interface responded at all or if it timed out completely. If curl -i http://<that-server> returned an HTTP status code and headers instead of nothing I would change my mind a suggest that the problem is more specific, such as a wrong URL path or an application error instead of the web service being completely unresponsive.
```

---

## Submission Checklist

- [x] All seven rows of the OSI table completed with a real Week 5–6 anchor each (Part A)

- [x] All five case files given a layer and supporting evidence (Part B)

- [x] Silent gateway case ruled on correctly (Part C)

- [x] OSI vs. practical TCP/IP model compared (Part D)

- [x] All three Analysis Questions answered (minimum sentence counts met)

- [x] No screenshot required for this lab

- [x] This file is committed to your portfolio repo at `week-06/labs/lab-05-layer-detective.md`

---

## GitHub Commit Subsection

1. Open **Week 6 → Lab 05: Layer Detective** in the Lab Portal.
2. Fill in the worksheet fields.
3. Click **Submit to GitHub** — the Portal commits to `week-06/labs/lab-05-layer-detective.md`.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
