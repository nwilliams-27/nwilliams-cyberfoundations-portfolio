# Week 4 Lab — Build Your First Virtual Machine (VM Builder Simulator) ★ Deliverable 1

**Student Name:** Na'Ketta WIlliams

**Date Completed:** August 11, 2026

**Module:** 1 — Digital Infrastructure & CLI | **Week:** 4  
**Submission Path:** `week-04/labs/lab-03-vm-builder.md`

---

## Overview

Lessons 3 and 4 taught you what a virtual machine is and how one lives and dies. This capstone lab hands you the keys: in the VM Builder Simulator you'll provision a machine of your own through the full five-question wizard (Part A), handle whatever provisioning throws at you (Part B), and run the complete lifecycle — stop, start, snapshot, delete — while a billing meter runs (Part C). This lab is the heart of **★ Deliverable 1: VM concepts + CLI screenshots** — its two screenshots join the two from Labs 01 and 02 in your portfolio repo.

**The simulator will push back on purpose.** Taken names, refused passwords, quota limits, and a region that sometimes fails are all part of the exercise — reading an error calmly and fixing the right thing *is* the skill being graded. Errors here are progress, not mistakes.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | CyberFoundations VM Builder Simulator — runs entirely in your web browser; nothing to install, no account needed, no real servers, no real money |
| Prerequisite | Week 4, Lessons 3 and 4 completed; Labs 01 and 02 recommended first |
| Concept Checks | The simulator gates progress on four Concept Checks — all four are covered in Lessons 3 and 4 |
| Time | Plan for 30–45 minutes, including this worksheet |

### How to Open the Simulator — Step by Step

1. Open your web browser (Chrome, Edge, Firefox, or Safari all work — use a computer, not a phone, so your screenshots capture the full screen).
2. Go to this address (you can also click the **VM Builder Simulator** link on the Lab Portal's Week 4 page — same destination):

```
https://cybervisionariesinstitute.github.io/cyberfoundations-simulators/vm-builder.html
```

3. Confirm you're in the right place: you should see a dark purple header reading **"Foundry District Cloud Annex"** and a page titled **"Mission Briefing: Provision Your First Virtual Machine."** If you see anything else, re-check the address.
4. Read the Mission Briefing all the way down — especially the **"How to use this simulator"** box. It explains the six steps, the Concept Checks, and the two screenshot moments.
5. Get your screenshot tool ready before you begin: **Windows:** press `Win + Shift + S` · **Mac:** press `Cmd + Shift + 4`, then drag to capture. You will need it twice, at moments the simulator announces with a 📸 banner.
6. Keep this worksheet open in a **second browser tab**, side by side with the simulator — you'll record answers as you go.

**⚠️ One thing to know before you start:** refreshing the simulator page **resets the entire simulation** — nothing is saved between visits. That's safe (it's a training environment), but capture each screenshot when prompted, before moving on, and don't refresh mid-run unless you want a fresh start.

**Also before you start:** have Lesson 4's Resource Pack open to its Quick Reference page (the lifecycle/billing table). You'll want it.

---

## Part A — Provision Your Machine

### Step 1 — Name It Like a Professional

Work through the Basics screen: choose a VM name that passes the naming rules *and* would tell a stranger what this machine is for. Note: at least one obvious name is already taken — if you hit **NameNotAvailable**, that's the simulator doing its job; pick another and record what happened.

The name you chose, and whether you hit the taken-name error first:

```
cybersecurity-lab-vm

I didn't get the name taken error.
```

### Step 2 — Choose a Region, and Say Why

Pick a region on the Basics screen. Each option describes a trade-off (latency, capacity). Record your choice and one sentence of reasoning — professionals never pick a region at random.

Your region and your reasoning:

```
Foundry Central

I chose this region because it was recommended. It's closest to me and has the lowest latency.
```

### Step 3 — Choose Your Guest OS

Pick an operating system and record why. There's no wrong answer, but there is a *reasoned* answer — think about which shell you'd rather manage it with, and what the license fee note tells you.

Your OS choice and reasoning:

```
Linux

I chose this one because there's no per hour license fee.
```

### Step 4 — Size It, and Do the Money Math

Pick a size tier. Record its specs and hourly rate, then do Lesson 4's monthly reflex: hourly rate × 24 × 30. Would you leave this machine running for a month?

Your size tier, its specs, and its hourly rate:

```
D4s_v3

vCPUs - 16 GB RAM - 256 GB disk

$0.220/hr
```

Your monthly math (rate × 24 × 30):

```
$0.220 × 24 × 30 = $158.40
```

### Step 5 — Create the Admin Account

Create the administrator username and password. The simulator blocks guessable usernames and refuses weak passwords — if it pushes back, record what it rejected and what you learned from the rejection.

What (if anything) got rejected, and your final username (never record the password):

```
Nothing was rejected.
```

### Step 6 — Capture Screenshot 1 (REQUIRED — Deliverable 1)

On the Review & Create screen — before you click Create — take the screenshot the simulator prompts for: your full configuration summary, including the total hourly cost. Name it exactly **`vm-config-summary.png`**. Upload instructions are in the GitHub Commit section.

---

## Part B — Survive Provisioning

### Step 1 — Create, and Read What Happens

Click **Create Virtual Machine** and watch the provisioning stages. Depending on your Part A choices, provisioning may fail with a readable error — **QuotaExceeded** (your size is bigger than the subscription allows) or **AllocationFailed** (your region ran out of capacity). If it fails: read the error, identify which wizard choice it points at, fix *that one thing*, and retry.

What happened on your first Create attempt (success, or the exact error name):

```
 QuotaExceeded 
```

If you hit an error: what it told you, and what you changed:

```
The error told me that my subscription allows a maximum of 2 vCPUs per VM in this region. I changed to a smaller VM.
```

### Step 2 — Confirm You're Running

Once provisioning completes, confirm on the dashboard: status **Running**, and the billing meter ticking at the rate your size card promised. Record the rate the meter shows.

The running rate shown on your dashboard:

```
$.110/hr
```

---

## Part C — Run the Full Lifecycle

Complete all four lifecycle tasks on the dashboard, in this order, and answer the simulator's Concept Checks as they appear.

### Step 1 — Stop, and Watch the Meter

Stop (deallocate) your VM. Watch what happens to the billing rate — it should not go to zero. Record the stopped rate and what it's paying for.

The stopped rate, and what a stopped VM still pays for:

```
The stopped rate is $0.002/hr and it only pays for the disk storage.
```

### Step 2 — Start It Again

Start the VM and confirm the full rate resumes. One sentence: where did your files go while it was stopped?

Your one-sentence answer:

```
While the VM was stopped my files went to the virtual disk.
```

### Step 3 — Take a Snapshot

Take a snapshot and record its name from the snapshot list. One sentence: what exactly did you just photograph, and when would you be glad you have it?

Snapshot name and your one-sentence explanation:

```
snapshot-1

I took a snapshot of my dashboard showing the current state of the VM and I would be glad to have it if the VM is ever accidentally changed, deleted or misconfigured. 
```

### Step 4 — Capture Screenshot 2 (REQUIRED — Deliverable 1)

With your VM **Running** and at least one snapshot visible, take the dashboard screenshot. Name it exactly **`vm-dashboard-running.png`**.

### Step 5 — Delete, and Read the Warning

Delete your VM. Read the confirmation dialog before you click — record what it warns you is about to happen, then confirm and record your final total cost from the completion banner.

What the delete warning said, in your own words:

```
If I delete this VM the virtual disk will be permanently destroyed. This change cannot be undone, the disk is gone forever. It is important to have a snapshot of my dashboard so that I can rebuild my VM. Also, the only way to stop billing is to delete the VM.
```

Your final simulated cost:

```
$117.18
```

---

## Analysis Questions

**Analysis Question 1.** Your stopped VM kept billing a small amount. Explain the "locker fee" in your own words — what physical thing still exists when a VM is stopped, and why is deletion the only true zero? *(Minimum 2 sentences.)*

```
The locker fee is like a holding fee for my VM. Although I stopped the VM, the disk is still being used for storage and I still need access to it at a later time so that locker fee is charged. Deletion is the only true zero because the disk is permanently deleted. 
```

**Analysis Question 2.** Lesson 4 revealed that your real Weeks 6–12 lab machines are stamped from golden snapshots your instructor built. Using what you did in Part C, Step 3, explain how a golden snapshot works and why it means every student's machine starts identical. *(Minimum 3 sentences.)*

```

A golden snapshot is a saved copy of the VM's disk at that exact moment. It captures the operating system, installed software and settings at that point in time. Creating a new VM from that golden snapshot allows for it to be built from the state captured in the snapshot instead of from blank machine. Every student's VM is created from the same saved snapshot so all of the VM's start out identical, regardless of who's building it or when making the golden snapshot a consistent starting point for each machine that is being built.

```

**Analysis Question 3.** If you hit a provisioning error in Part B (or even if you didn't): why do you think this lab *wants* you to encounter errors in a simulator before Week 6 hands you real infrastructure? *(Minimum 2 sentences.)*

```
I think this lab wants me to build and encounter errors on a practice VM first because making mistakes here, such as a wrong naming choice don't have real consequences. Making a mistake on a real infrastructure can expose a system or cost money. Therefore,  practicing low-stakes infrastructure creates the habit of getting it right before it matters. 
```

**Analysis Question 4.** Defend your Part A size choice to an imaginary manager watching the budget: why was your tier the right rent for this job, and what would have made you pick a bigger or smaller one? *(Minimum 2 sentences.)*

```
I originally selected D4s_v3-4 vCPUs, 16 GB RAM, 256 GB disk because I wanted to be sure the that machine could comfortably handle company needs without any performance issues. In reviewing my choice, I realized that may have more capacity than the company needs and in efforts to keep costs down, I chose a smaller machine. I now know to base the size of the VM on the workload instead of automatically choosing a larger machine on a just in case basis. Doing so controls costs while still getting the job done.
```

---

## Submission Checklist

- [x] VM named within the rules, taken-name error handled if encountered (Part A, Step 1)

- [x] Region chosen with recorded reasoning (Part A, Step 2)

- [x] Guest OS chosen with recorded reasoning (Part A, Step 3)

- [x] Size tier recorded with hourly rate and monthly math (Part A, Step 4)

- [x] Admin account created; any rejections recorded — password NOT written anywhere (Part A, Step 5)

- [x] **REQUIRED:** `vm-config-summary.png` captured at the Review screen (Part A, Step 6)

- [x] First Create attempt recorded — success or exact error + fix (Part B)

- [x] Stop / start / snapshot completed with meter observations recorded (Part C, Steps 1–3)

- [x] **REQUIRED:** `vm-dashboard-running.png` captured with VM running + snapshot visible (Part C, Step 4)

- [x] VM deleted; warning paraphrased and final cost recorded (Part C, Step 5)

- [x] All four Concept Checks passed in the simulator

- [x] All four Analysis Questions answered (minimum sentence counts met)

- [x] This file is committed to your portfolio repo at `week-04/labs/lab-03-vm-builder.md`

---

## GitHub Commit Subsection — ★ Deliverable 1

This lab completes **Deliverable 1: VM concepts + CLI screenshots**. Two things get committed:

**1. This worksheet, via the Lab Portal:**

1. Go to the CyberFoundations Lab Portal and sign in.
2. Open **Week 4 → Lab 03: Build Your First Virtual Machine**.
3. Fill in the worksheet fields and click **Submit to GitHub**. The Portal commits the completed file to `week-04/labs/lab-03-vm-builder.md`.

**2. Your four Deliverable 1 screenshots, uploaded to `assets/screenshots/week-04/`:**

| Screenshot | From | Filename |
|---|---|---|
| Permissions audit | Lab 01 | `cli-permissions-audit.png` |
| Archive investigation | Lab 02 | `cli-search-investigation.png` |
| VM configuration summary | Lab 03, Part A | `vm-config-summary.png` |
| VM dashboard, running + snapshot | Lab 03, Part C | `vm-dashboard-running.png` |

For each: on GitHub.com, navigate to `assets/screenshots/week-04/`, click **Add file → Upload files**, drag the image in (exact filenames above — lowercase, hyphens, no spaces), and **Commit changes**. Then open each uploaded image, right-click directly on it, choose **Copy image address** (Chrome/Edge) or **Copy Image Link** (Firefox), and paste the two VM links into the embeds below:

![VM configuration summary](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-04/vm-config-summary.png)

![VM dashboard — running, with snapshot](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-04/vm-dashboard-running.png)

**If right-click doesn't show that option:** click the small download-arrow icon in the top-right of the image preview instead, then copy the URL from your browser's address bar.

**Commit message tip (from Lesson 4):** when GitHub asks for a commit message on your uploads, write one that says what the work is — *"Add Deliverable 1: VM lifecycle and CLI evidence"* — not "stuff." An employer reading your repo sees discipline in details like that.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
