# Week 3 — Windows, Linux, and Your First Commands

---

## Focus

Week 3 continues Module 1 — Digital Infrastructure & CLI. Five lessons carry the week: Opening the Hood (files, folders, the file-system tree), Windows vs. Linux: A File System Tour, Navigating the Shell (3A), Reading and Getting Help (3B), and Creating and Organizing (3C). By the end of the week you'll know ten commands total, in both bash and PowerShell, and you'll have used every one of them yourself inside the browser-based CLI Simulator — no install, no VM, nothing that can break anything real.

**This is also the first week you build your own week folder.** `week-01/` and `week-02/` came pre-built in your repo. Starting this week, you create `week-03/` yourself — see `HOW-TO-CREATE-YOUR-WEEK-FOLDER.md` at the repo root before you do anything else below.

---

## Outcomes

By the end of this week, you'll be able to:

- Explain what a file and a folder are, and read a file path out loud, root to file
- Tell a Windows-style path from a Linux-style path, and explain case-sensitivity in one sentence
- Find your location, look around, and move through a folder tree using `pwd`/`Get-Location`, `ls`/`dir`, and `cd`
- Read a file's contents and look up an unfamiliar command using `cat`/`type` and `--help`/`man`/`Get-Help`
- Create, copy, move, and delete files and folders using `mkdir`/`touch`, `cp`/`mv`, and `rm`

---

## Environment

No VM or cloud environment this week. Weeks 2–4 use the browser-based Lab Portal — no local install required.

| Component | Details |
|---|---|
| Environment | CyberFoundations CLI Simulator (browser-based, inside the Lab Portal) |
| Shells | Both bash and PowerShell — pick whichever you prefer for Labs 01 and 03; Lab 02 requires both |

**Prerequisite:** You create the `week-03/` folder yourself this week (see the note above) — matching the same naming convention as `week-01/` and `week-02/`: lowercase, hyphenated, two-digit week number. The content for this file and for `week-03/labs/README-week03-submissions.md` is available right here in this Reference section — you don't need to retype anything from scratch.

---

## What You'll Submit

| Deliverable | Location |
|---|---|
| Lab 01 — Navigate Your First File System | `week-03/labs/lab-01-command-line-navigation.md` |
| Lab 02 — Two Shells, Same Job (Incident Response Edition) | `week-03/labs/lab-02-two-shells-same-job.md` |
| Lab 03 — Command Line Scavenger Hunt | `week-03/labs/lab-03-command-line-scavenger-hunt.md` |
| Notes | `week-03/notes.md` |
| Reflection | `week-03/reflection.md` |

All five of these are graded — submitted through the Lab Portal and checked against your repo. None of them is a flagship GitHub Deliverable, though — that's a separate portfolio checkpoint that starts in Week 4.

**How you'll submit:** Fill out each lab's, notes', and reflection's worksheet in the Lab Portal, then click Submit to GitHub — it commits the completed file to your repo automatically for all five of these. You do not need to manually create `notes.md` or `reflection.md` yourself; the Portal creates them for you the first time you submit. Screenshots of your CLI Simulator sessions are optional for all three labs. After submitting each item, also log it as complete in the lab tracker at labsubmission.cybervisionariesinstitute.org — separate from the Lab Portal, and how your completion gets recorded.

---

## Checklist

- [ ] Created my own `week-03/` folder, `README-week03-root.md`, and `labs/README-week03-submissions.md` following `HOW-TO-CREATE-YOUR-WEEK-FOLDER.md`
- [ ] Completed Lessons 1, 2, 3A, 3B, and 3C
- [ ] Attended (or watched the recording of) the Week 3 live session, "Finding Your Way"
- [ ] Completed Lab 01 and submitted it via the Lab Portal
- [ ] Completed Lab 02 (both bash and PowerShell) and submitted it via the Lab Portal
- [ ] Completed Lab 03 and submitted it via the Lab Portal
- [ ] Submitted my Notes worksheet via the Lab Portal
- [ ] Submitted my Reflection worksheet via the Lab Portal
- [ ] Logged each submission in the lab tracker at labsubmission.cybervisionariesinstitute.org

---

## What "Good" Looks Like

- Your `week-03/` folder matches `week-01/` and `week-02/` exactly in naming — `week-03`, not `Week3` or `week-3`.
- You check your location with `pwd`/`Get-Location` immediately after every `cd` move, not just at the end — this habit is called out explicitly in Lesson 3A and graded in Lab 01/02's checklists.
- Before deleting or moving anything, you look first with `ls`/`dir` — no surprises, no "wait, where did that go?"

---

## Connecting to Prior Weeks

- **Week 2** — You were introduced to the OS layer at a surface level; this week goes hands-on with that same layer through the command line for the first time.
- **Week 4** — Builds directly on this week's file/folder commands, adding permissions and your first look at what a virtual machine actually is. This is also the first week with a real GitHub portfolio deliverable.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
