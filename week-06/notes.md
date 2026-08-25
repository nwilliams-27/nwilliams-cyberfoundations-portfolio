# Week 6 Notes — Cloud Heights: Cloud VMs, SSH, VNets & Layers

**Student Name:** N. Williams

**Date Completed:** August 25, 2026

Summarize this week's key concepts in your own words — not copy-pasted definitions. This week moved from the simulated Grid into a real cloud environment, so focus on what you personally observed as well as what each term means.

> **Cloud Heights Security Rule:** Your Bastion shareable link and Cloud Heights password are private access credentials. Never paste either into this file, a screenshot, your GitHub repository, Circle, or a chat message.

## Key Concepts This Week

- **Cloud** — other people's computers, professionally operated and reached over a network
- **Datacenter** — the physical facility where cloud computing equipment lives
- **Region** — a geographic area where a cloud provider operates datacenters
- **Virtual machine (VM)** — a computer created in software; in Cloud Heights, your VM runs on hardware in a real datacenter
- **IaaS / PaaS / SaaS** — different levels of cloud service: rent the room, rent the workshop, or rent the finished service
- **Shared responsibility model** — the cloud provider secures the building and underlying platform; the customer is still responsible for what belongs to them
- **Provisioning** — creating and preparing a resource so it is ready to use
- **Golden image / snapshot** — a known starting point that can be used to create consistent machines
- **Snapshot vs backup** — a snapshot is a point-in-time copy used for recovery or cloning; a backup is a separate recovery copy with a different purpose
- **Azure Bastion** — the guarded front desk that gives you browser-based SSH access without giving your VM a public IP
- **Bastion shareable link** — sensitive access information that must never be committed to GitHub or exposed in screenshots
- **SSH (Secure Shell)** — remote command-line access to another machine
- **SSH client and server** — the client starts the connection; the server listens and answers
- **Port 22** — the standard numbered door used by SSH
- **Host / fingerprint verification** — the verify-before-approve habit when connecting to a host for the first time
- **Authentication** — proving that you are the account you claim to be
- **Remote session / remote shell** — the live command-line session running on another machine
- **Getting TO vs getting INTO a machine** — network reachability and authentication are different problems
- **`hostname`** — asks which machine you are on
- **`whoami`** — asks which account you are using
- **`pwd`** — asks where you are in the filesystem
- **Private IP address** — an address used inside a private network rather than directly on the public internet
- **Virtual network (VNet)** — the private cloud neighborhood where resources communicate
- **Subnet** — a smaller address range inside a VNet; a floor inside the larger building
- **NAT / outbound translation** — lets a privately addressed machine communicate outward without giving the machine its own public IP
- **Network Security Group (NSG)** — the network guard post that controls what traffic is allowed; you take control of these rules in Week 7
- **Known-good reference point** — a target whose expected behavior gives you something reliable to compare against
- **Grid Beacon** — the known-good Cloud Heights host at `10.60.6.4`
- **The silent Azure gateway** — Azure's default gateway may not answer ICMP ping even when the network is healthy
- **OSI model** — the seven-layer vocabulary used to organize network and application behavior
- **TCP/IP model** — the more compact layer model commonly used by practitioners
- **Layers** — a way to separate different jobs in a communication path so troubleshooting can be systematic
- **Encapsulation** — information travelling inside other information, like a letter inside an envelope inside a mailbag
- **The Ladder Rule in the real cloud** — work outward, prove what works, use the route and a known-good target, and never let one silent tool response choose the culprit by itself

## My Cloud Heights Command Table

You used these commands on a real Ubuntu machine this week. Instead of memorizing syntax, write down the **question each command answers** or the job it performs.

| Command | What question does it answer / what does it do? |
| --- | --- |
| `hostname` | shows the name of the machine that I'm on |
| `whoami` | shows the user account that I'm logged in as |
| `pwd` | shows my location in the file system |
| `ip addr` | show my network interfaces and their assigned IP addresses (IPV4 and IPv6) |
| `ip route` | shows my routing table and default gateway |
| `ping` | checks reachability by sending packets and reporting if a response is recived |
| `traceroute` | shows te hop by hop path that my traffic takes to reach a desitination and the response time at each hop |
| `dig` | performs the DNS lookup, translates a name to an UP address |
| `curl` | sends requests to URL/server and shows the results directly in the terminal |
| `ssh` | allows secure remote access to a machine that I'm not sitting at |
| `exit` | closes my current session and takes me back to where I was before |

## In My Own Words

### 1. Getting TO vs Getting INTO

Explain the difference between getting **TO** a machine and getting **INTO** a machine. Use something you personally observed in Cloud Heights as evidence.

```
When packets arrive and respond I am getting to a machine by reaching it over the network, but this does not mean that I have access. When I authenticate the machine and I can control it that means I got into the machine. In Cloud Heights I pinged and trace routes to test reachability, not access. When I ran ssh with the correct credentials, I got into the machine.
```

### 2. The Silent Gateway

Your Azure gateway did not answer `ping`, but your VM was still healthy. Explain how you proved the network was working and what this taught you about interpreting tool output.

```
I proved my network was working by looking beyond the failed ping. When I ran ip addr and ip route it was confirmed that I had a valid IP and the gateway was assigned correctly. These are signs of basic network configuration health independent of a ping receiving a reply. This taught me that the results of one tool, especially negative results, is not enough to draw a conclusion. I need to check the results against other evidence before deciding that something is broken.
```

### 3. Private on the Inside, Connected to the Outside

Explain how your Cloud Heights VM can reach the internet even though it has only a private IP address. Then explain how **you** reach the VM from outside its VNet.

```
Even though it only has a private IP address, my Cloud Heights VM can reach the internet because Azure automatically performs Network Address Translation (NAT) on outbound traffic. When my Cloud Heights VM sends something out to the internet, Azure will translate the private IP address into a public IP address, sends the traffic and then translate the response back to my Cloud Heights VM. Azure Bastion acts as a secure gateway with it's own public access point in order for me to reach the VM outside of its VNet.
```

### 4. VNet vs Subnet

Explain the difference between a VNet and a subnet using the Cloud Heights building/floor analogy. Then explain why separating systems into smaller network ranges can help security.

```
The VNEt is the entire building, the whole private network space. The subnets are the floors in the building, smaller sections that are carved out for a specific group of machines. Smaller ranges help security because splitting a network into subnets limits how far problems can spread. If one subnet is compromises, the rest of the network does not automatically get exposed, similar to an intruder on one floor of the building and not having access to the rest of the floors in the building. Splitting a network into subnets also makes it easier to apply different access controls to each section, which contains potential damage instead of leaving everything exposed.
```

### 5. The Ladder Rule Has a Map Now

The Ladder Rule never used the words OSI or TCP/IP. Explain how the layer models give you a map for the same troubleshooting process you have already been using.

```
In The Ladder Rule, I work outward, one rung at a time, letting the evidence pick the culprit. In the OSI layer model I troubleshoot using the bottom up method, checking for the problem one layer at a time, starting at layer 1. If the problem is suspected at a specific layer I would use the top down method.
```

---

## Submission Checklist

- [x] I summarized the Week 6 concepts in my own words, not copied definitions

- [x] I completed my Cloud Heights command table

- [x] I explained getting TO vs getting INTO a machine

- [x] I documented what the silent Azure gateway taught me

- [x] I explained the Cloud Heights private-network design

- [x] I connected the Ladder Rule to network layers

- [x] I checked that my Bastion shareable URL does not appear anywhere in this file

- [x] I checked that my Cloud Heights password does not appear anywhere in this file

- [x] This file is committed to my portfolio repo at `week-06/notes.md`

---

*CyberVisionaries Institute — Cyber Foundations, Tier I*
