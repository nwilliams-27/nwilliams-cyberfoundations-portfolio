# CyberFoundations Week 7 — Cloud Heights: The Guard Post

**Module 2 — Networking & Cloud Foundations**

Week 7 moves from reaching and entering a cloud VM to controlling which network sources may reach a service. Students read NIC-level security rules, create one least-privilege TCP 8080 allow, prove both allowed and denied outcomes, troubleshoot a controlled priority fault, and document the evidence.

## Week 7 Labs

1. **Meet the Guard** — inspect the protected baseline and editable range.
2. **Read the Door Ledger** — translate rule fields and apply first-match-wins.
3. **Build the Narrowest Door** — allow only Grid Beacon `10.60.6.4` to TCP 8080.
4. **Prove the Guard Works ★ Deliverable 2** — prove the intended source is allowed and the unintended source is denied.
5. **Break It, Explain It, Fix It** — diagnose and remediate a safe priority failure.
6. **Night Watch — Optional Stretch** — optional evidence-analysis stretch using only Portal-exposed information.

## Protected-Rules Rule

Priorities 100 (`allow-ssh-from-bastion`), 110 (`allow-icmp-intra-vnet`), and 120 (`deny-ssh-student-subnet`) are protected. Student work is restricted to priorities 200–999.

## Portfolio Paths

```text
week-07/labs/
assets/screenshots/week-07/
week-07/notes.md
week-07/reflection.md
```

## Access and Evidence Safety

Do not publish passwords or Bastion shareable URLs. Crop browser address bars and login details from every screenshot. Students use the CyberFoundations Lab Portal and their assigned `cf-student-XX` VM; no Azure Portal steps are required.

## Folder Contents

- `Student/` — six student-facing labs, submissions guide, notes, and reflection.
- `Instructor/` — matching guides, grading references, exemplars, and the consolidated Word guide.
- `notes.md` and `reflection.md` — root copies of student templates.

*CyberVisionaries Institute · CyberFoundations · Tier I*
