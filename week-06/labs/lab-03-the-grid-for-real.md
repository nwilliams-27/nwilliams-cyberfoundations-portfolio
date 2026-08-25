# Week 6 Lab 03 — The Grid, For Real

**Student Name:** N. Williams

**Date Completed:** August 23, 2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 6  
**Submission Path:** `week-06/labs/lab-03-the-grid-for-real.md`

---

> ### 🔒 Cloud Heights Security Rule
> Your Bastion link and Cloud Heights password are **private access credentials**. Never paste either into a worksheet, screenshot, GitHub repository, Circle post, or chat message. When taking screenshots, crop out the browser address bar and all login information.

---

## Overview

In Week 5 you ran `ip addr`, `ip route`, `ping`, and `traceroute` in a simulator that always behaved. Today you run the same toolkit against real cloud infrastructure that does **not** always behave the way the textbook implies — and you learn to tell "broken" apart from "normal."

This is an **independent** lab. It tells you what to accomplish; you choose the commands. Expect about 40 minutes.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Cloud Heights — live Ubuntu 22.04 VM, reached through Azure Bastion |
| Commands used | `ip addr`, `ip route`, `ping`, `traceroute`, `curl` |
| Known-good target | **Grid Beacon — `10.60.6.4`** |
| Prerequisite | Week 6 Labs 01–02 |

---

## Part A — Where You Actually Are

### Step 1 — Read Your Own Address

Run the command that lists your interfaces and addresses.

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
3: enP59490s1: <BROADCAST,MULTICAST,SLAVE,UP,LOWER_UP> mtu 1500 qdisc mq master eth0 state UP group default qlen 1000
    link/ether 7c:ed:8d:57:01:e4 brd ff:ff:ff:ff:ff:ff
    altname enP59490p0s2

```

Your private IPv4 address and prefix length:

```
inet 127.0.0.1/8 scope host lo
```

### Step 2 — Read Your Route

Run the command that shows the routing table.

Command and output:

```
ip route

default via 10pi.60.6.1 dev eth0 proto dhcp src 10.60.6.21 metric 100 
10.60.6.0/26 dev eth0 proto kernel scope link src 10.60.6.21 metric 100 
10.60.6.1 dev eth0 proto dhcp scope link src 10.60.6.21 metric 100 
168.63.129.16 via 10.60.6.1 dev eth0 proto dhcp src 10.60.6.21 metric 100 
169.254.169.254 via 10.60.6.1 dev eth0 proto dhcp src 10.60.6.21 metric 100 
```

Your default gateway:

```
default via 10.60.6.1 dev eth0 proto dhcp src 10.60.6.21 metric 100 
```

### Step 3 — Compare to Week 5

Compare this live Ubuntu output to what the CLI Simulator produced in Week 5. What looks the same, what looks different, and what surprised you:

```
The basic command structure and the eth0 interface name was the same in Ubuntu and the CLI simulator. There differences were Ubuntu showed an altname, an IPV6 address and the ip route command returned more detailed results. I was surprised to see more detailed information in Ubuntu.
```

---

## Part B — The Gateway That Does Not Answer

### Step 1 — Ping the Gateway

Ping the default gateway address you recorded. Let it run a few seconds, then stop it.

Command and output:

```
ping 10.60.6.1

--- 10.60.6.1 ping statistics ---
253 packets transmitted, 0 received, 100% packet loss, time 258075ms
```

### Step 2 — Interpret It Correctly

You almost certainly got **no replies**. In Azure, the platform gateway commonly does not answer ICMP. This is **expected platform behaviour** and by itself proves nothing about whether your machine or network is broken.

Explain why "the gateway did not answer ping" is weak evidence:

```
The gateway not answering ping is weak evidence because no reply only tells me that the gateway is not responding to ICMP requests, not that anything is wrong with the network or connection. Not answering to ping traffic is a security feature and is not related to the network functioning correctly. I would need to check other evidence, such as the DNS
```

---

## Part C — The Known-Good Target

The **Grid Beacon** at `10.60.6.4` is a machine that is known to be up and known to answer. When your first probe fails, you test against something known-good before you conclude anything.

### Step 1 — Ping the Beacon

```
ping 10.60.6.4
```
Output:

```
PING 10.60.6.4 (10.60.6.4) 56(84) bytes of data.
6PING 10.60.6.4 (10.60.6.4) 56(84) bytes of data.
64 bytes from 10.60.6.4: icmp_seq=1 ttl=64 time=1.38 ms
64 bytes from 10.60.6.4: icmp_seq=2 ttl=64 time=1.04 ms
64 bytes from 10.60.6.4: icmp_seq=3 ttl=64 time=1.52 ms
64 bytes from 10.60.6.4: icmp_seq=4 ttl=64 time=1.04 ms
^C
--- 10.60.6.4 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
rtt min/avg/max/mdev = 1.037/1.245/1.524/0.212 ms
```

### Step 2 — Trace the Path

```
traceroute 10.60.6.4
```
Output:

```
traceroute to 10.60.6.4 (10.60.6.4), 30 hops max, 60 byte packets
 1  grid-beacon.internal.cloudapp.net (10.60.6.4)  1.256 ms * *
```

### Step 3 — Ask the Application

```
curl http://10.60.6.4
```
Output:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GRID BEACON | CVI CyberFoundations</title>
    <style>
        body {
            background: #071426;
            color: #d9f7ef;
            font-family: monospace;
            max-width: 850px;
            margin: 80px auto;
            padding: 30px;
        }
        .beacon {
            border: 1px solid #31d6a6;
            padding: 35px;
        }
        h1 { color: #31d6a6; }
        .label { color: #8ca8ff; }
        .status { color: #31d6a6; }
        .classified {
            margin-top: 30px;
            border-top: 1px solid #31445e;
            padding-top: 20px;
        }
    </style>
</head>
<body>
<div class="beacon">

    <h1>GRID BEACON</h1>

    <p><span class="label">NODE:</span> grid-beacon</p>
     <p><span class="label">NETWORK:</span> CVI Training Grid</p>
    <p><span class="label">STATUS:</span>
       <span class="status">ONLINE</span></p>

    <p>
        Network beacon established.<br>
        If you reached this node, your route is operational.
    </p>

    <div class="classified">
        <p>INVESTIGATION CHECKPOINT</p>

        <p>
            Observe the path that brought you here.
            The destination is only part of the story.
        </p>

        <p>TRACE ID: CF-NET-0604</p>
    </div>
</div>
</body>
</html> 
    
```

> ### ⚠️ Grid Beacon not responding?
> The Grid Beacon is shared course infrastructure and should normally be available. First, confirm your Cloud Heights VM shows **Running** and that you completed the preceding network checks. Then retry the command once after a minute or two.
>
> If the Grid Beacon still does not respond, **stop this part of the lab and contact your instructor.** Record that the shared service was unavailable; do not treat the result as evidence that your VM or your work is incorrect.
>
> Do not change networking, NSGs, firewall rules, routes, DNS, or any Azure settings to try to reach the beacon.
>
> *Instructor note: a confirmed Grid Beacon outage is an environment issue, not a student error. Affected students may complete this portion of Lab 03 after the service is restored, with no penalty.*

### Step 4 — Record the Application Evidence

The beacon returns a banner and a trace ID. Record exactly what you received:

```
   <h1>GRID BEACON</h1>

    <p><span class="label">NODE:</span> grid-beacon</p>
    <p><span class="label">NETWORK:</span> CVI Training Grid</p>
    <p><span class="label">STATUS:</span>
       <span class="status">ONLINE</span></p>

    <p>
        Network beacon established.<br>
        If you reached this node, your route is operational.
    </p>

    <div class="classified">
        <p>INVESTIGATION CHECKPOINT</p>

        <p>
            Observe the path that brought you here.
            The destination is only part of the story.
        </p>

        <p>TRACE ID: CF-NET-0604</p>
    
```

Explain the difference between what the `ping` proved and what the `curl` proved:

```
Running the ping command only proved reachability; whether or not the destination responded to a network level request, confirming that the machine does exist and it did not confirm the services that were running or if they were working correctly. The results of the curl command were more specific, showing that the grid beacon was running, accepting requests and responding with the banner and trace id, which proved a working exchange. Ping tested if the machine was there at all and curl tested if a specific service on that machine was functioning and produced a real response.
```

### Step 5 — Capture Your Evidence

Two screenshots, both cropped to the terminal only:

**Required filename:** `vm-toolkit-live.png` — your `ip addr` and `ip route` output

![Live VM toolkit — ip addr and ip route](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-06/vm-toolkit-live.png)

**Required filename:** `beacon-reply.png` — your beacon ping/traceroute/curl evidence

![Grid Beacon reply](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-06/beacon-reply.png)

---

## Part D — Rewrite the Ladder Rule

Week 5 taught the Ladder Rule: test the near thing before the far thing. Real infrastructure adds a wrinkle — a silent rung is not automatically a broken rung.

Rewrite the Ladder Rule in your own words so that it survives real cloud infrastructure. Your version must include both **route/path evidence** and **a known-good target**:

```
Check my own address and gateway first to rule out any problems with my setup before assuming the problem is further out. In real cloud infrastructure, a silent rung such as the gateway not answering a ping, is not proof that something is broken because platforms like Azure often configure devices to ignore ICMP by design. To confirm the path is working check check ip route for a valid output, a correctly assigned gate and test a known good target such as the Grid Beacon at 10.60.6.4 or a well known public site such as bbc.co.uk to confirm that the network path is properly functioning, even though one specific one remained silent.
```

---

## Analysis Questions

**Analysis Question 1.** Your ping to the gateway failed and your ping to the beacon succeeded. What does that pair of results, taken together, prove about your machine's networking? *(Minimum 3 sentences.)*

```
When taken together, these results prove that my machine's networking is functioning properly. Being that the beacon responded, this meant that the traffic is leaving my machine successfully, passing through the gateway and reaching a destination that is beyond my local network. Even though the ping to the gateway failed, the successful ping to the beacon confirmed that the path through the gateway works, which isolated the failed gateway response to ping specifically and not a broken network.
```

**Analysis Question 2.** Why is `traceroute` useful even when `ping` already answered? What extra thing does it show you? *(Minimum 2 sentences.)*

```
Traceroute is still useful when ping has already answered because ping only confirms that the machine can be reached and does not show how the traffic got there. Traceroute shows the path detailing every hop along the way between my machine and the destination, traceroute also shows the response time at each hop. In doing so, visibility is given into where traffic passes through and where delays occurs, which is information ping does not provide.
```

**Analysis Question 3.** A service is unreachable and ping to it succeeds. Where would you look next, and why is "the network is fine" an incomplete answer? *(Minimum 3 sentences.)*

```
If a service is unreachable but a ping to it succeeds, tis only proves that the machine responded to the ICMP at the network layer, this does not confirm that the service is running or if it is accepting connections. I would check to see if the services port is open and accepting connections by using tools like curl or checking the TCP handshake since it is possible for a service to fail independently of basic network reachability. The network is fine is an incomplete answer due to ping only testing one layer of the connection when the actual application or service running on the machine could be down, misconfigured or blocking port access, which ping does not test at all.
```

**Analysis Question 4.** Something already controls what is allowed to reach your machine in Cloud Heights. If you could decide those rules, what would you want to allow, what would you want to block, and who in an organization should get to make that decision? *(Minimum 3 sentences.)*

```
A firewall or network security group controls what traffic is allowed to reach my machine in Cloud Heights. This filters the connection based on rules such as allowed ports, source addresses or protocols. If I could decide those rules, I would want to use least privilege and only allow the specific ports and services that are needed for the machine's purpose and block everything else by default instead of leaving unnecessary ports open. In an organization, this  decision should be made by the network admins, security admins or a team dedicated to security because it requires full understanding of what's needed across the organization and the security risks of leaving unnecessary access open.
```

---

## Submission Checklist

- [x] `ip addr` output recorded and own private IP/prefix identified (Part A)

- [x] `ip route` output recorded and default gateway identified (Part A)

- [x] Live output compared to the Week 5 simulator (Part A, Step 3)

- [x] Gateway pinged and the silent result interpreted correctly (Part B)

- [x] Beacon `ping`, `traceroute`, and `curl` all run and recorded (Part C)

- [x] Beacon banner and TRACE ID recorded (Part C, Step 4)

- [x] `vm-toolkit-live.png` and `beacon-reply.png` captured, cropped, uploaded to `assets/screenshots/week-06/` (Part C, Step 5)

- [x] Ladder Rule rewritten with route evidence + known-good target (Part D)

- [x] All four Analysis Questions answered (minimum sentence counts met)

- [x] This file is committed to your portfolio repo at `week-06/labs/lab-03-the-grid-for-real.md`

---

## GitHub Commit Subsection

1. Open **Week 6 → Lab 03: The Grid, For Real** in the Lab Portal.
2. Fill in the worksheet fields and upload both screenshots to `assets/screenshots/week-06/`.
3. Click **Submit to GitHub** — the Portal commits to `week-06/labs/lab-03-the-grid-for-real.md`.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
