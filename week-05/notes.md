# Week 5 Notes — The Grid: Addresses, Names, Ports, and Diagnostics

**Student Name:** N. Williams

**Date Completed:** August 19, 2026

Summarize this week's key concepts in your own words — not copy-pasted definitions.

## Key Concepts This Week

- IP addresses — the dotted-quad number every device on a network needs (`10.20.5.42` on The Grid)
- The subnet mask — the answer to "which addresses are my neighbours?" (`/24` = `255.255.255.0`)
- The default gateway — the door out of your neighbourhood (`10.20.5.1` on The Grid)
- Private vs public addresses — `10.x`, `172.16–31.x`, and `192.168.x` are *inside* addresses
- DNS — the Grid's Directory Board: a name goes in, an IP address comes out
- NXDOMAIN vs a host that resolves but is down — two different failures with two different causes
- DHCP — the Address Office: leases, why addresses change, why a laptop "just works" on a new network
- Ports — the numbered doors on a building: 22 SSH, 53 DNS, 80 HTTP, 443 HTTPS, 3389 RDP, 25 SMTP
- TCP vs UDP — a confirmed conversation vs a shout across the room
- The TCP handshake — SYN → SYN-ACK → ACK (packets 7, 8 and 9 in Lab 03)
- The diagnostic toolkit — `ping` (is it alive?), `traceroute` (where does it stop?), `dig` (what number is behind that name?)
- **THE LADDER RULE** — check yourself → check your gateway → check the target by NAME → check the target by IP → trace the path. *Work outward, one rung at a time, and let the evidence pick the culprit.*

## My Command Table

You learned the same five jobs twice this week — once in bash, once in PowerShell. Fill the pairs in from memory if you can, and check them afterwards. This table is worth keeping.

The bash command and its PowerShell equivalent for each job — show my own address, show my default gateway, test reachability, trace the path, look up a name:

```

                             bash           PowerShell
show my address:			 ip addr		ipconfig
show my default gateway:     ip route	    ipconfig
test reachability: 			 ping			Test-Connection
trace the path: 			 traceroute		tracert
look up a name: 			 dig			Resolve-NnsName

```

## In My Own Words

Your machine has three numbers: an address, a subnet mask, and a default gateway. Explain what each one is for, the way you'd explain it to someone who has never heard those words.

```
The address is your machine's unique identity on the network. Every device on a network needs a unique address so that other devices know who to send data to and who a message came from. Think about when you send a letter to a friend through the mail, you need a to and from address and both addresses must be unique, right? Right.. So, your address would be who the message came from and your friend's address would be who the data is being sent to.

The subnet mask determines if the address of another machine is in the same local network (like a neighbor) or somewhere outside of the local area network. Think about that letter you sent, if your friend lives in your neighborhood, that would be the same local area network but if your friend lives in another city, that would be outside of your local area network.

The default gateway is the exit point. It's the device  your machine sends traffic to when the final destination is not a neighbor. So think about that letter again, the mailman would be the default gateway because that's how your letter to that friend in another city is going to get out of your mailbox and start making it's way to the city where your friend lives (the final destination).
```

What does DNS actually do? Include the difference between a name that comes back "Name or service not known" (NXDOMAIN) and a name that resolves perfectly well to a host that never answers.

```
The DNS translates site names, like google.com, into an IP address that computers use to coonect. It basically turns a name into a number. 

"Name or service not known" (NXDOMAIN) means that the DNS has no record for that name at all, the name does not exist. 

A name that resoles but never answers means that the DNS found a valid record and a real IP address but the machine at that address did not respond. The DNS is working properly, the problem is with the host or the network.

```

An IP address gets your traffic to the right building. What does a port number add to that, and why would a defender care how many doors are open?

```
The port number tells the machine which door the traffic should enter once it arrives at the building. A defender would care how many doors are open because every open port is an entry point for an attacker. Therefore, the defender would want apply least privilege by only opening the ports, access and services that are needed; making sure that as few doors as possible are open.
```

Write out THE LADDER RULE — all five rungs, in order — and say why running them in that order matters more than running them fast.

```
Rung 1 -> ip addr
Rung 2 -> ping 10.x.x.x
Rung 3 -> ping relay-station
Rung 4 -> ping 10.x.x.x
Rung 5 -> traceroute relay-station

Running them in order instead of running them fast matters because the results of each rung determine what needs to be checked next and eliminates everything that is not the answer to the problem.
```

What is DHCP, and why does your laptop get an address automatically on a network it has never joined before, while a server like `grid-dns` keeps the same address permanently?

```
DHCP (Dynamic Host Configuration Protocol) is the service that automatically assigns an IP address to my device when it joins a network. My laptop automatically gets an address on a network it has never joined before because it is a new device on that network. A server like grid-dns keeps the same address permanently because other machines  depend on it always being in the same place. If the address changes, everything that points to it would break.
```

---

## Submission Checklist

- [x] I summarized each concept in my own words, not copied definitions

- [x] I completed the bash-to-PowerShell command table

- [x] I answered all five "In My Own Words" prompts

- [x] This file is committed to my portfolio repo at `week-05/notes.md`

---

*CyberVisionaries Institute — Cyber Foundations, Tier I*
