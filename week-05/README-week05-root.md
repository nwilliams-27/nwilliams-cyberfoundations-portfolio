# Week 5 — The Grid: Networking Foundations

---

## Focus

Week 5 opens Module 2 — Networking & Cloud Foundations. Module 1 was about the machine in front of you. Module 2 is about everything your machine can *reach*, and The Grid is the district where you learn to find your way around it.

Four lessons carry the week: **Welcome to The Grid** (networks and IP addresses), **Names and Leases** (DNS & DHCP), **Ports & Protocols** (which door you knock on), and **The Diagnostic Toolkit** (`ping`, `traceroute`, `dig`, and THE LADDER RULE). Then you put all of it to work: one guided lab, one incident you have to solve yourself, and one where you read real network traffic packet by packet.

**There's no flagship deliverable this week** — Deliverable 2 arrives in Week 7. Every lab this week is still graded, and every lab still commits to your portfolio repo, exactly as in Week 4. What's different is that this week's job is building the diagnostic habit rather than assembling a showcase piece.

**You build `week-05/` yourself,** same as Weeks 3 and 4 — see `HOW-TO-CREATE-YOUR-WEEK-FOLDER.md` at the repo root. The content for this file and the submissions guide is available in the Lab Portal's Week 5 Reference section — no retyping needed.

---

## Outcomes

By the end of this week, you'll be able to:

- Read your own IPv4 address, subnet mask, and default gateway — with `ip addr` and `ip route` on Linux, and `ipconfig` on Windows — and say in plain English what each number is for
- Tell a private address from a public one, and explain why your home router shows two different numbers
- Explain what DNS does, and prove it: run `dig`, read the ANSWER SECTION, and confirm the name and the number reach the same machine
- Tell apart the two failures that look alike — a name that doesn't resolve at all (NXDOMAIN) versus a name that resolves fine to a host that's dead
- Explain DHCP leases, and why servers get fixed addresses while people get leases
- Name the well-known ports worth knowing — 22, 53, 80, 443, 3389, 25 — and explain what a port adds to an address
- Describe TCP vs UDP, and identify a TCP handshake (SYN → SYN-ACK → ACK) in a real packet capture
- Work **THE LADDER RULE** on an unfamiliar outage: check yourself → check your gateway → check the target by name → check the target by IP → trace the path
- Write a short, structured incident note that states a finding, the evidence for it, and what you ruled out

---

## Environment

No install this week, and no VM yet — your own machine in Cloud Heights arrives in Week 6. Everything happens in your browser.

| Component | Details |
|---|---|
| CLI Simulator | Labs 01 and 02, in the Lab Portal — the "Finding Your Place" and "Outage Response" scenarios on The Grid, in both bash and PowerShell |
| Packet Inspector | Lab 03, in the Lab Portal alongside the CLI Simulator — a recorded minute of Grid traffic, identical for every student. **You will never install Wireshark for this course** |
| Your own computer | The optional stretch lab only, using commands that already exist on Windows and macOS. Nothing to install, ever |

---

## What You'll Submit

| Deliverable | Location |
|---|---|
| Lab 01 — Finding Your Place on the Grid | `week-05/labs/lab-01-finding-your-place.md` |
| Lab 02 — The Grid Outage | `week-05/labs/lab-02-the-grid-outage.md` |
| Lab 03 — Reading the Grid's Mail | `week-05/labs/lab-03-reading-the-grids-mail.md` |
| Stretch Lab — The Real Grid *(optional, rated when submitted)* | `week-05/labs/lab-04-stretch-the-real-grid.md` |
| Notes | `week-05/notes.md` |
| Reflection | `week-05/reflection.md` |
| Screenshots | `assets/screenshots/week-05/` — see the table below for exact filenames |

Screenshot filenames, exactly as written:

| Screenshot | Lab | Required? |
|---|---|---|
| `cli-grid-address.png` | Lab 01 | Optional — portfolio practice for Week 7 |
| `cli-grid-outage.png` | Lab 02 | **Required** |
| `packet-dns-query.png` | Lab 03 | **Required** |
| `packet-http-plaintext.png` | Lab 03 | **Required** |
| `stretch-real-traceroute.png` | Stretch Lab | Required *if* you submit the stretch lab — and it must be redacted |

---

## Weekly Checklist

- [ ] Created `week-05/` and `week-05/labs/` folders (repo-root guide)
- [ ] Watched all four lessons and their resource packs
- [ ] Completed Lab 01 — recorded your address, mask, gateway, and the A record for `foundry-archive.grid.local`
- [ ] Completed Lab 02 — worked the ladder in order and wrote the incident note in Part E
- [ ] Completed Lab 03 — both required screenshots uploaded with exact filenames
- [ ] *Optional:* completed the stretch lab, with every redaction pre-flight line ticked
- [ ] Committed `notes.md` and `reflection.md`
- [ ] Checked every box in each lab's own Submission Checklist

---

## What "Good" Looks Like

Evidence before conclusions. In Lab 02 that means the ladder run in order with each rung's output recorded — a finding that names the right cause but shows no baseline hasn't proved anything yet. It also means testing your colleague's theory instead of accepting it, and saying *what you ruled out and how*, not just what you found.

In Lab 03 it means the specific numbers: 15 packets, five protocols, the handshake at packets 7, 8 and 9, and the exact header value you found in packet 14. In Lab 01 it means being able to explain the aha in your own words — that the name and the number are the same destination.

Everywhere: answers that meet the minimum sentence counts, screenshots that are legible and named exactly to spec, and commit messages that say what the work is. None of this is graded on polish. It's graded on whether the reasoning is visible.

---

## Connecting to Prior Weeks

Week 3 gave you commands. Week 4 taught you who's allowed to touch what, and turned a computer into something you can summon and dismiss. Week 5 is the first week your work leaves the machine entirely: the Foundry archive you searched in Week 4 shows up here as `foundry-archive.grid.local` at `10.20.5.20`, and you reach it across a network you can now read.

The Ladder Rule is this week's version of a habit you already have — Week 3's Golden Rule and Week 4's Gatekeeper's Rule both taught you to check before you act. The ladder just points that same discipline outward.

Next week the course crosses into **Cloud Heights**, and the Lab Portal hands you a real Azure machine of your own. You'll use SSH — the thing you only met as a concept this week — to reach it. And the readable HTTP request you found in Lab 03, sitting there in plain text with a staff code inside it, is a question this course comes back to in Week 8.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
