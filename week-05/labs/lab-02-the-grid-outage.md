# Week 5 Lab 02 — The Grid Outage (CLI Simulator)

**Student Name:** Na'Ketta Williams

**Date Completed:** August 17, 2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 5  
**Submission Path:** `week-05/labs/lab-02-the-grid-outage.md`

---

## The Ticket

> **TICKET #GRID-0412 — PRIORITY: HIGH**
> **Reported by:** Foundry District operations staff
> **Summary:** Foundry staff report they cannot reach the relay station. Multiple people, multiple machines, since this morning. No changes reported on their end.
> **Assigned to:** you.
>
> **You are on call.** Find out what is actually broken and write it up.

---

## Overview

Lab 01 handed you the tools. This lab hands you a problem and gets out of your way.

Somewhere on The Grid, something is down. Your job is not to guess which thing — your job is to **find out**, one rung at a time, and then write down what the evidence says. This is the first time in this course you're being asked to do real diagnostic work rather than follow a recipe, and it is a genuine skill. You already have everything you need.

**The method is the Ladder Rule from Lesson 4:** Work outward, one rung at a time and let the evidence pick the culprit.

1. **Check yourself** — do you have a valid address?
2. **Check your gateway** — can you get out of your own neighbourhood?
3. **Check the target by NAME.**
4. **Check the target by IP.**
5. **Trace the path.**

*Work outward, one rung at a time, and let the evidence pick the culprit.*

The reason the ladder exists is that instinct is a terrible investigator. Instinct wants to name a villain in the first thirty seconds — "it's DNS," "it's the firewall," "it's the router" — and then spend an hour proving itself right. Evidence is slower and it is correct. **Do not decide what is broken until Part E.** Run the rungs in order, record what each one actually said, and let the finding assemble itself.

You will also be told, partway through, what somebody else thinks the problem is. Treat that the way you'd treat any other claim: test it.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | CyberFoundations CLI Simulator (browser-based, inside the Lab Portal) |
| Shell | **bash** throughout this lab |
| Prerequisite | Week 5, Lessons 1–4 completed; Lab 01 completed first (this lab assumes those tools) |
| Commands used | `ip addr`, `ip route`, `ping`, `dig`, `traceroute` |

**Before you start:** log into the Lab Portal, open **Week 5 → CLI Simulator**, and load the **"The Grid — Outage Response (Bash)"** scenario. It boots you into the same workstation you used in Lab 01, on the same segment, with the same gateway. Only one thing about The Grid has changed since yesterday. Find it.

---

## Part A — Establish Your Baseline

Rungs 1 and 2 of the ladder. It is tempting to skip straight to the thing users complained about — resist that. If you don't establish that *your own machine and your own way out are healthy*, then every failure you find afterwards is ambiguous. You won't know whether the target is broken or whether you are.

### Step 1 — Check Yourself

Confirm your workstation still holds a valid address on The Grid, and that you know where its traffic leaves from.

Run `ip addr` for your address, then `ip route` for your gateway — the same two commands you used in Lab 01. (Remember: addresses and routes live in different places, so it takes both.)

The full `ip addr` output:

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 02:1a:7c:44:0b:5e brd ff:ff:ff:ff:ff:ff
    inet 10.20.5.42/24 brd 10.20.5.255 scope global eth0
       valid_lft forever preferred_lft forever
```

The full `ip route` output:

```
default via 10.20.5.1 dev eth0
10.20.5.0/24 dev eth0 proto kernel scope link src 10.20.5.42
```

Your IPv4 address and default gateway, pulled out of those two results:

```
IPv4 address: 10.20.5.42/24

Default Gateway: 10.20.5.1 
```

### Step 2 — Check Your Gateway

Ping your default gateway. This is the door out of your neighbourhood — if this fails, nothing past it means anything.

Command you ran:

```
ping 10.20.5.1
```

Output (the full ping result, including the summary line):

```
PING 10.20.5.1 (10.20.5.1) 56(84) bytes of data.
64 bytes from grid-gateway (10.20.5.1): icmp_seq=1 ttl=64 time=1.000 ms
64 bytes from grid-gateway (10.20.5.1): icmp_seq=2 ttl=64 time=1.200 ms
64 bytes from grid-gateway (10.20.5.1): icmp_seq=3 ttl=64 time=1.100 ms
64 bytes from grid-gateway (10.20.5.1): icmp_seq=4 ttl=64 time=1.300 ms

--- 10.20.5.1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3005ms
rtt min/avg/max/mdev = 1.000/1.150/1.300/0.100 ms
```

### Step 3 — State What the Baseline Proves

Two healthy results is not "nothing happened." It's a finding, and it eliminates an entire category of causes before you've touched the target once.

What your baseline rules out, and why that matters before you go further:

```
My baseline, the healthy ping to my gateway with 0% packet loss, rules out that my machine, my local network connection and my default gateway as being the source of the problem. This matters because it narrows down where to look next. I know that the problem is not at the start of the path so instead of blindly guessing, I can focus my trouble shooting efforts on what's beyond the gateway instead of re-checking things that I have confirmed to be working.
```

---

## Part B — Test the Target

Rungs 3 and 4. This is the fork in the road, and the two tests must be run **in this order** and read **as a pair**. Either one alone tells you almost nothing. Together they tell you almost everything.

### Step 1 — Test the Target by Name

Ping `relay-station.grid.local` — the host the Foundry staff say they can't reach.

Command you ran:

```
ping relay-station.grid.local
```

Output (the full ping result, including the summary line):

```
PING relay-station.grid.local (10.20.5.30) 56(84) bytes of data.

--- relay-station.grid.local ping statistics ---
4 packets transmitted, 0 received, 100% packet loss, time 3005ms
```

### Step 2 — Read That Output Carefully Before You Move On

There are two separate things happening in a ping, and this output separates them for you. Look at the **first line**, then look at the **summary line**, and notice that they do not agree about whether things are going well.

Did the name turn into an IP address — and if so, which one? Then: what did the packet loss say?

```
Yes, relay-station.grid.local, turned into a IP address. It resolved to 10.20.5.30. The packet loss says 100%, which means the machine at the IP address did not respond. 
```

### Step 3 — Ask DNS Directly

Ping *told* you the name resolved, but it told you in passing, on one line, while it was busy doing something else. Ask the question on its own so the answer is unambiguous — and so you have it in writing when you escalate.

Run `dig relay-station.grid.local`.

Command you ran:

```
dig relay-station.grid.local
```

Output (the full dig result):

```
; <<>> DiG 9.18.24 <<>> relay-station.grid.local
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 41207
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;relay-station.grid.local.			IN	A

;; ANSWER SECTION:
relay-station.grid.local.	3600	IN	A	10.20.5.30

;; Query time: 1 msec
;; SERVER: 10.20.5.10#53(10.20.5.10)
```

Look at the `status:` field in the header and the `ANSWER SECTION`. Does DNS know this host, and if so, what address does it hand back?

What the status field said, and the A record it returned:

```
Yes, DNS knows the host and the address it hands back is 10.20.5.30.

status field: NOERROR

A record: relay-station.grid.local.	3600	IN	A	10.20.5.30
```

This is the single most quotable piece of evidence in the whole investigation, because it settles a question your colleague is about to raise.

### Step 4 — Test the Target by IP

Now take the IP address out of that first line and ping it directly: `10.20.5.30`. You are deliberately removing name lookup from the equation.

Command you ran:

```
ping 10.20.5.30
```

Output (the full ping result, including the summary line):

```
PING 10.20.5.30 (10.20.5.30) 56(84) bytes of data.

--- 10.20.5.30 ping statistics ---
4 packets transmitted, 0 received, 100% packet loss, time 3005ms
```

### Step 5 — State What the Combination Proves

Now put the two results side by side. The by-name test and the by-IP test each had a job:

- The by-name test asks: *can this name be turned into a number, and does that number answer?*
- The by-IP test asks: *forget names entirely — does that number answer?*

Work through it. If the name failed to become a number, that would point one direction. If the name became a number just fine but the number won't answer, that points somewhere else entirely. And if you removed the name from the equation altogether and the failure **didn't change**, then whatever is broken was never about the name.

What the two tests together prove — and, just as importantly, what they rule out:

```
Together, the by name and the by IP address prove that the DNS is working properly. The name resolved to 10.20.5.30 but the machine at that address did not respond, when trying to reach it by name or by number. This ruled out DNS being the problem because the same failure happened even when the DNS was bypassed. The machine or network path to 10.20.5.30 is what's broken, not the name resolution. The two tests isolate the problem to being reachability, not naming.
```

---

## Part C — Trace the Path

Rung 5. Ping tells you *whether* something answers. Traceroute tells you *how far your traffic got before it stopped* — it lists each hop along the way, and a row of `* * *` means the path died at that point.

One traceroute on its own is hard to read if you've never seen a healthy one. So you'll run two, and compare.

### Step 1 — Trace to the Broken Target

Run `traceroute relay-station.grid.local`.

Command you ran:

```
traceroute relay-station.grid.local
```

Output (the full trace, including the timeout rows):

```
traceroute to relay-station.grid.local (10.20.5.30), 8 hops max, 60 byte packets
  1  grid-gateway (10.20.5.1)  1.000 ms  1.200 ms  1.100 ms
  2  * * *
  3  * * *
  4  * * *
  5  * * *
  6  * * *
  7  * * *
  8  * * *
```

The last hop that answered, and what happened after it:

```
hop 1

 1  grid-gateway (10.20.5.1)

The subsequent hops 2-8 show ***: no response at all, which means the trace never reached relay-station.grid.local or any other point beyond my gateway.
```

### Step 2 — Trace to a Known-Good Target for Comparison

Now trace to a host you have no reason to suspect — `cloud-heights.grid.local`, out on the far edge of The Grid. (You'll be spending a lot of time in Cloud Heights next week. Consider this an introduction.)

Command you ran:

```
traceroute cloud-heights.grid.local
```

Output (the full trace):

```
traceroute to cloud-heights.grid.local (10.20.7.80), 8 hops max, 60 byte packets
  1  grid-gateway (10.20.5.1)  1.000 ms  1.200 ms  1.100 ms
  2  grid-core (10.20.0.1)  4.000 ms  4.200 ms  4.100 ms
  3  cloud-heights.grid.local (10.20.7.80)  12.000 ms  12.200 ms  12.100 ms
```

The complete list of hops, in order:

```
 1  grid-gateway (10.20.5.1)  1.000 ms  1.200 ms  1.100 ms
 2  grid-core (10.20.0.1)  4.000 ms  4.200 ms  4.100 ms
 3  cloud-heights.grid.local (10.20.7.80)  12.000 ms  12.200 ms  12.100 ms
```

### Step 3 — Compare the Two Traces

The comparison is the whole point of running two. One trace completed and one didn't — and where each one stopped is the evidence.

Pay attention to whether the **first hop** was the same in both. If traffic to the broken target and traffic to the healthy target both got through the same first hop successfully, that tells you something specific about which part of the path is fine.

What the comparison tells you about where the problem is — and where it is not:

```
The comparison shows that hop 1 grid-gateway was successful in both traces which means that my machine, my local network and my default gateway all all working no matter the destination and this rules out my own end of the connection being the cause of the failure. The problem is not in the first hop or anything before it. The problem is somewhere in the path beyond the gateway that leads to relay-station.grid.local since a completely different destination past the same gateway, cloud-heights.grid.local is successful all the way through. This isolates the failure to something being broken further along the route, not something on my side of the network.
```

---

## Part D — The Colleague's Theory

A more experienced colleague on the on-call channel reads your notes and jumps in:

> "Oh, I've seen this one. It's the relay station's DNS entry — it's `relay-station-old.grid.local` that everything actually points at, and that name is busted. This is a DNS problem, not a host problem. Go check it."

They sound confident. They may even be senior to you. **Test the claim anyway.** In security work, "somebody confident said so" is not evidence, and the fastest way to be respectfully wrong-footed is to accept a theory you could have checked in ten seconds.

### Step 1 — Test the Colleague's Host

Ping `relay-station-old.grid.local`.

Command you ran:

```
ping relay-station-old.grid.local
```

Output (the exact error or result — copy it precisely):

```
ping: relay-station-old.grid.local: Name or service not known
```

### Step 2 — Explain Why This Is a Different Kind of Failure

This result does **not** look like Part B. Compare them closely:

- In Part B, the name became a number, and then the packets went nowhere.
- Here, you never got a number at all.

Those are two genuinely different failures, at two different stages, and confusing them is one of the most common mistakes in network troubleshooting. In Lesson 2 you met the pair: **a name that doesn't resolve at all** versus **a name that resolves perfectly to a host that's dead**. One is the directory not having an entry. The other is the directory being completely correct about an address where nobody answers the door.

For context: `relay-station-old.grid.local` was decommissioned. It was retired on purpose, its directory entry was removed on purpose, and it has nothing to do with today's ticket. A retired name failing to resolve is *expected behaviour*, not an outage.

Explain, in your own words, why this failure is a different kind from the one in Part B — and why it is not the cause of today's outage:

```
This failure is different from the failure in Part B because it happened at an earlier stage. In Part B relay-station.grid.local the DNS resolved to 10.20.5.30 but the machine did not respond which means there's a failure in reachability. Today's outage was not caused by a broken machine or a broken path. The outage was caused by a name that was intentionally retired, so the DNS is correct in having no entry for it. 

In today's outage, relay-station-old.grid.local failed as: Name or service not known, which means the DNS could not resolve it at all. Name or service not known is expected behavior when a name has been removed intentionally and would not a reason for the problem that's actually being investigated. 

```

### Step 3 — Say How You'd Reply to Your Colleague

You now know they're wrong. Being right is easy; being useful about it is a professional skill. Write the reply you'd actually send — polite, specific, and grounded in what you ran rather than in who's more senior. Point at the evidence, not at the person.

Your reply to your colleague:

```
Hi,

I checked and relay-station-old.grid.local has been decommissioned and the DNS entry was removed on purpose so failing to resolve is expected behavior and not a bug. The name is question relay-station.grid.local resolves fine with dig showing as status: NOERROR and returning the correct A record 10.20.5.30. The problem is not the DNS. The problem is the machine at that address is not responding. I confirmed that by directly pinging the IP and the traceroute that died right after the gateway. This appears to be an issue with reachability on the network path and not an issue with the name.
```

### Step 4 — One Control Test Before You Write It Up

You are about to tell people something is broken. Before you do, prove that *not everything* is broken — otherwise the first question you'll be asked is whether the problem is you.

Ping a host you already know is healthy: `foundry-archive.grid.local`.

Command you ran:

```
ping foundry-archive.grid.local
```

Output (the full ping result):

```
PING foundry-archive.grid.local (10.20.5.20) 56(84) bytes of data.
64 bytes from foundry-archive.grid.local (10.20.5.20): icmp_seq=1 ttl=64 time=2.000 ms
64 bytes from foundry-archive.grid.local (10.20.5.20): icmp_seq=2 ttl=64 time=2.200 ms
64 bytes from foundry-archive.grid.local (10.20.5.20): icmp_seq=3 ttl=64 time=2.100 ms
64 bytes from foundry-archive.grid.local (10.20.5.20): icmp_seq=4 ttl=64 time=2.300 ms

--- foundry-archive.grid.local ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3005ms
rtt min/avg/max/mdev = 2.000/2.150/2.300/0.100 ms
```

Why this matters: a report that says "the relay station is unreachable" is weaker than one that says "the relay station is unreachable **while every other host I tested answers normally**." The second version has already ruled out the boring explanations, and it's the difference between a ticket that gets acted on and one that gets handed back to you.

### Step 5 — Capture Your Evidence (REQUIRED screenshot)

Take a screenshot of your simulator session showing your evidence trail — at minimum the failed ping by name, the successful `dig`, the failed ping by IP, and the failed traceroute. **This screenshot is required.** Name it `cli-grid-outage.png`. You'll upload it and record its filename via the GitHub Commit section below.

An incident note with no attached evidence is just an opinion. This is the attachment.

---

## Part E — Write the Finding

This is the part that makes you a security professional rather than someone who can run commands.

Everything you've done so far is raw evidence. Nobody upstream of you — not your team lead, not the Foundry staff who filed the ticket, not the person who picks this up on the next shift — is going to read six terminal outputs and work out the conclusion themselves. Your job is to hand them the conclusion **and** the reason to believe it.

This is your first incident-report rep. You will do many more; Week 12's capstone asks you to write these for real. So we're giving you the structure now, while the stakes are low. Fill in each field — this is not a blank page, it's a form, and every professional incident note answers these same four questions.

**Keep it short and specific.** A good finding is a paragraph, not an essay. Write what you can defend.

### Step 1 — What Is Broken

One or two sentences. Name the specific thing that is not working. Be precise about *what* is down — not "the network," not "the relay station area," but the actual component your evidence points at.

Your statement of what is broken:

```
The host relay-station.grid.local (10.20.5.30) cannot be reached. The DNS resolution is working properly but the machine is not responding to any traffic and the path beyond the local gateway towards that specific destination is failing at every hop.
```

### Step 2 — What Evidence Proves It

List the specific tests you ran and what each one returned. This is where your Parts B and C outputs earn their keep. Someone reading this should be able to re-run your tests and reach the same conclusion without asking you a single question.

Your evidence, test by test:

```
Baseline connectivity/pinged my own gateway: ping 10.20.5.1
Results: 4 packets transmitted, 4 received, 0% packet loss, ~1-1.3 ms response times. This confirmed that the DNS resolved the local machine, network and gateway are functioning properly.

Ping by name/target host: ping relay-station.grid.local
Results: the name resolved to 10.20.5.30, 4 packets transmitted, 0 received, 100% packet loss. This confirmed that the DNS resolved the name but the host did not respond.

DNS lookup/independently confirmed the resolution: dig relay-station.grid.local
Results: status: NOERROR, ANSWER SECTION returned relay-station.grid.local. 3600 IN A 10.20.5.30. This confirmed that the DNS is functioning properly snd consistenly returns the right IP address.

traceroute/target host: traceroute relay-station.grid.local
Results: hop 1 (grid-gateway, 10.20.5.1) responded successfully; hops 2 through 8 returned as * * * (no response). This confirmed that traffic successfully left the local network but it failed immediately after the gateway on the path towards the specific destination.

traceroute/comparison target: traceroute cloud-heights.grid.local
Results: all three hops responded successfully, reaching the destination (10.20.7.80) at hop 3. This confirms that the gateway and the next hop are working properly for other destinations and isolates the failure to the specific path towards relay-station.grid.local.

Ruled out the unrelated name/decomissioned host
ping relay-station-old.grid.local
Results: Name or service not known. This confirmed that this is an intentionally retired host name with no DNS entry and it's unrelated to today's outage.

```

### Step 3 — What You Ruled Out, and How

Just as important as what you found. Say explicitly which possible causes you eliminated and which specific test eliminated each one. At minimum, address: your own workstation, your local network path out, name resolution, and the colleague's theory.

Ruling out possibilities is what separates a finding from a guess. A reader who sees only your conclusion has to trust you. A reader who sees what you eliminated can *check* you.

What you ruled out and the evidence that ruled it out:

```
Ruled out the local machine, network connection and the gateway as being the cause of the outage. Evidence: Pinging the default gateway, 10.20.5.1 returned 4/4 packets received and 0% packet loss confirming that the local machine and the first hop out of the network are working properly.

Ruled out DNS/naming as being the cause of the outage. 
Evidence: dig relay-station.grid.local returned status: NOERROR with a correct A record 10.20.5.30 and directly pinging the IP address produced the same 100% packet loss as pinging by name and proved the failure independently exists of the DNS and is not related to the name resolution.

Ruled out a broader network/backbone wide outage (traceroute)
Evidence: the traceroute to a different destination, cloud-heights.grid.local, through teh same gateway was completed successfully, all three hops responded and confirmed that the gateway and core network path are working properly for other traffic, which isolates the failure to the path toward relay-station.grid.local.

Ruled out relay-sation-old.grid.local as being related to today's outage.
Evidence: relay-sation-old.grid.local was intentionally decommissioned so it returned "Name or service not known", this result was expected, it's not an indicator of the current outage and it's unrelated to this ticket.

```

### Step 4 — What Should Happen Next

One or two sentences. Based on your finding, who or what needs to be looked at, and by whom? You don't have the access to fix this yourself — and that's normal. Knowing exactly what to escalate, and to whom, is the deliverable.

Your recommended next step:

```
Being that the failure occurred at the network path that's beyond my local gateway (10.20.5.30), the ticket should be escalated to network/infrastructure operations to investigate the routing or hardware between the grid-core and the segment of the relay station. My access is limited and I'm unable to check firewall, switch or router configurations at that level and since the host never responds even after the DNS resolves it correctly, whoever manages the physical machine at 10.20.5.30 should verify that it is powered on and connected to the network because it may be offline.
```

---

## Analysis Questions

**Analysis Question 1.** In Part B you pinged the target by name and then by IP. Explain why running *both* is necessary — describe what a person would wrongly conclude if they had run only the by-name test and stopped there. *(Minimum 3 sentences.)*

```
Pings by name tests two things at once: if the DNS resolve by name and if resulting IP address responds. A failure does not tell you which of those two are broken. Pinging by IP address completely removes DNS for the equation, which is why it's important to run both tests in order to isolate exactly where the failure is happening. If someone only pings by name they may wrongly conclude that the problem is a DNS issue. In this case the DNS was working properly and the problem was the host not responding. Jumping to the conclusion that the problem is with the DNS would result in chasing a DNS fix that would not resolve the outage.
```

**Analysis Question 2.** A name that returns "Name or service not known" and a name that resolves fine but never replies to a ping are two different failures with two different causes. Describe each one in plain English — what is actually going wrong in each case — and explain why treating them as the same problem would send a troubleshooter down the wrong path. *(Minimum 3 sentences.)*

```
Name or service not known means that the is no DNS record at all for that name. A name that resolved but never replies to the ping is different because the DNS finds a real address but there is no answer when you try to reach it and this means that the machine cannot be reached. If these are treated the same the troubleshooter would go down the wrong path because Name or service not known is related to a DNS problem and a name that resolved but never replied to the ping is related to a broken host or network and chasing a DNS fix for a name that resolved but never replied to the ping would waste valuable time on a system that is already working.
```

**Analysis Question 3.** Part A asked you to confirm your own address and your gateway before touching the reported problem at all — even though the ticket had nothing to do with your workstation. Defend that step to a colleague who thinks it's a waste of time on an urgent ticket. *(Minimum 3 sentences.)*

```
Checking my own address and gateway first was not wasted time because doing so makes every test after it trustworthy. If I skip checking my own address and gateway and it turns out that my machine is not the problem, it would be easy for me to misread later results as being about the target when they are really about me. Initially ruling out my own setup saves times because it means that every finding afterwards can be immediately trusted and not questioned later. 
```

**Analysis Question 4.** Your colleague was confident, possibly senior, and wrong. Describe how you decided their theory was wrong, and what you think that says about how much weight confidence should carry compared to evidence in security work. Where relevant, consider what would have happened if you had simply accepted their theory and reported it upward. *(Minimum 3 sentences.)*

```
I decided that my colleagues theory was wrong because I tested it directly: dig confirmed that the relay station's DNS was resolving correctly and relay-station-old.grid.local was an unrelated, intentionally decommissioned host. My colleague's confidence did not math the gathered evidence, so I trusted what could be verified over what was stated with certainty. If I would have accepted their theory and reported it upward, the team would have been chasing a DNS fix on a system that was never broken while the unreachable host went unaddressed.
```

---

## Submission Checklist

- [x] Own address and gateway confirmed (Part A, Step 1)

- [x] Gateway pinged successfully and output recorded (Part A, Step 2)

- [x] Baseline reasoning stated — what a healthy baseline rules out (Part A, Step 3)

- [x] `relay-station.grid.local` pinged by name; resolution and packet loss both read (Part B, Steps 1–2)

- [x] `dig relay-station.grid.local` run; status and A record recorded (Part B, Step 3)

- [x] `10.20.5.30` pinged directly; output recorded (Part B, Step 4)

- [x] The combined proof of the tests stated in writing (Part B, Step 5)

- [x] `traceroute relay-station.grid.local` run; last responding hop identified (Part C, Step 1)

- [x] `traceroute cloud-heights.grid.local` run; all hops listed in order (Part C, Step 2)

- [x] The two traces compared and interpreted (Part C, Step 3)

- [x] `relay-station-old.grid.local` tested and its exact error recorded (Part D, Step 1)

- [x] Explained why the decoy is a different kind of failure (Part D, Step 2)

- [x] Reply to the colleague drafted, grounded in evidence (Part D, Step 3)

- [x] Control test run against a healthy host before escalating (Part D, Step 4)

- [x] **REQUIRED:** `cli-grid-outage.png` uploaded to `assets/screenshots/week-05/` and its filename recorded (Part D, Step 5)

- [x] Incident note complete — what's broken, evidence, ruled out, next step (Part E, Steps 1–4)

- [x] All four Analysis Questions answered (minimum sentence counts met)

- [x] This file is committed to your portfolio repo at `week-05/labs/lab-02-the-grid-outage.md`

---

## GitHub Commit Subsection

This lab's written answers are submitted through the **CyberFoundations Lab Portal**.

1. Go to the CyberFoundations Lab Portal and sign in.
2. Open **Week 5 → Lab 02: The Grid Outage**.
3. Fill in the worksheet fields — they match the commands, outputs, and questions in this file, in the same order.
4. Connect your GitHub account if you haven't already (one-time setup), and select your portfolio repo.
5. Click **Submit to GitHub**. The Portal commits the completed file to `week-05/labs/lab-02-the-grid-outage.md` for you.

**📸 REQUIRED — your evidence screenshot.** Your incident note in Part E needs something to point at:

1. Go to your portfolio repository on GitHub.com and navigate to `assets/screenshots/week-05/` (create the folder if this is your first Week 5 screenshot).
2. Click **Add file → Upload files**, drag in your screenshot, named `cli-grid-outage.png` (lowercase, hyphens, no spaces).
3. Scroll down and click **Commit changes**.
4. Click the uploaded image's filename to open it and confirm your evidence is readable at full size — if the terminal text is too small to read, retake it.
5. Record the filename below so your grader knows to look for it.

The screenshot filename you uploaded:

```
cli-grid-outage.png
```

Your screenshot lives in `assets/screenshots/week-05/` in your repository, alongside the rest of your Week 5 evidence. It does not need to be linked inside this worksheet.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
