# Week 2 Lab 01 — Cybersecurity Landscape & Digital Infrastructure Overview

**Student Name:** Na'Ketta Williams

**Date Completed:** July 26, 2026

**Module:** 1 — Digital Infrastructure & CLI | **Week:** 2  
**Submission Path:** `week-02/labs/lab-01-hardware-os-software-diagram.md`

---

## Overview

In this lab, you build a working mental model of the system you'll be securing throughout this course: the hardware, operating system, and software layers that make up every computer, and where the cybersecurity field fits around them. This lab has two parts. Part A connects this week's material to the CyberFoundations City map. Part B has you build and explain a diagram of how a computer's hardware, OS, and software layers interact.

**No terminal or command line is required this week** — that starts in Week 3.

---

## Lab Environment

| Component | Details |
|---|---|
| Environment | Browser-based Lab Portal (Module 1 orientation) |
| Required Materials | CyberFoundations City map; a diagram tool of your choice (hand-drawn and photographed, or any digital tool) |

**Prerequisite:** Portfolio repo created from the CyberFoundations student template in Week 1. Fill out this worksheet here in the Lab Portal, then hit Submit to commit it directly to your repo at `week-02/labs/lab-01-hardware-os-software-diagram.md`.

---

## Part A — CyberFoundations City & the Cybersecurity Landscape

The CyberFoundations City map is your visual guide to the next 11 weeks. Each district represents a module of this course. This part connects this week's material to the map you were introduced to in Week 1.

### Step 1 — Open the Lab Portal Orientation Module

From your Student Dashboard, open the **Module 1 orientation** module.

### Step 2 — Complete the Orientation Walkthrough

Work through the orientation content. It covers the same hardware/OS/software material as this week's lessons from a different angle — use it to check your understanding, not to replace the lessons.

### Step 3 — Locate This Week's District on the City Map

Open the CyberFoundations City map (introduced in Week 1, Lesson 6). Identify which district corresponds to Module 1 — Digital Infrastructure & CLI.

**District name:**

```
Foundry District
```

**Why this district fits this week's topics (1–2 sentences):**

```
This district fits this week's topics because it relays the foundation of the computer and explains the attack surface. Knowing the cybersecurity landscape, what's inside a computer and the operating systems are all key components in understanding cybersecurity.
```

---

## Part B — Hardware, OS, and Software Diagram

A computer is a stack of layers: physical hardware at the bottom, an operating system managing that hardware in the middle, and the software you actually use on top. This part has you draw that stack and explain it in your own words.

### Step 1 — Identify the Layers

Before drawing anything, name one example of what lives at each layer.

**Hardware layer — one example component (e.g., CPU, RAM, or storage):**

```
RAM - Random Access Memory - fast, short term memory that is holding what I'm using right now.
```

**Operating system layer — name an OS (e.g., Windows, Linux, or macOS):**

```
Windows - proprietary operating system - owner controls access to the source code and distribution.
```

**Software layer — one example application (e.g., a web browser or word processor):**

```
Microsoft Word - word processing software
```

### Step 2 — Sketch Your Diagram

Sketch a simple diagram (hand-drawn and photographed, or built in any digital tool) showing how the hardware, OS, and software layers stack and interact. Arrows or labels showing "what talks to what" matter more than visual polish. If you'd like a free browser-based option instead of hand-drawing, try [draw.io](https://www.drawio.com/) — no account required to get started.

### Step 3 — Upload and Embed Your Diagram

This step happens directly on GitHub, not through this worksheet — there's no upload field here, since screenshots and diagrams are added through GitHub's own upload UI, the same way as every other week.

1. Go to your portfolio repository on GitHub.com and navigate to `assets/screenshots/week-02/`.
2. Click **Add file → Upload files**, then drag in your diagram image (name it something descriptive, like `hardware-os-software-diagram.png` — no spaces, lowercase, no timestamps).
3. Commit the upload.
4. Click on the uploaded image to open it, then click the **Raw** button. Copy the URL from your browser's address bar.
5. After you submit this worksheet, it will be committed to your repo. Go back to GitHub, open the committed file, click the pencil (edit) icon, and paste your raw URL into the embed line below:

```markdown
![Hardware/OS/software diagram](paste your raw image URL here)
```

**My Diagram** (added directly on GitHub after you submit):

### Step 4 — Explain Your Diagram

In your own words — not a copied definition — explain how the three layers interact. Reference your own diagram directly.

**Explanation (minimum 3 sentences):**

```
I open the software (Chrome or Word). The software communicates its intent with the operating system (Linux, Windows). The operating system communicates with the hardware (CPU, RAM, SSD) by executing the intent of the software. 
```

---

## Analysis Questions

Answer each question in your own words. These questions connect what you did in Parts A and B to the bigger picture of this course.

### Analysis Question 1

If the operating system crashed on the computer you diagrammed, which layer(s) would stop working, and which (if any) would keep working? Explain your reasoning.

```
If the operating system crashed the hardware would still work because my laptop would still be powered on. It would still work because I didn't lose electricity.
```

### Analysis Question 2

Pick one piece of software you use daily. Trace it down through the OS to the hardware it ultimately depends on. What would happen to that software if the hardware layer failed?

```
If the hardware layer failed Chrome would not work at all. The failed CPU would produce a black screen, failed RAM means everything in the Chrome and Windows memory is gone because I was actively using both programs when it failed. SSD failure means that I would not be able to load a new page or save a download. The failed GPU (graphics processing unit) means that I would not be able to see anything on my display (monitor). When the motherboard fails, nothing works at all because this is equivalent to my laptop being powered off.
```

### Analysis Question 3

Explain, in your own words, why a cybersecurity professional needs to understand all three layers — hardware, OS, and software — rather than just the software layer where most visible attacks (like phishing emails) happen.

```
It is important for a cybersecurity professional to understand all three layers (hardware, OS and software) because each layer is vulnerable to an attack. An attacker only needs one vulnerable layer to gain access. Understanding only one layer isn't enough, a cybersecurity professional has to know all three layers in order to catch and stop an attack.
```

---

## Lab Report Questions

Answer each question in complete sentences.

**1. What is the cybersecurity landscape, and why does it matter to someone starting this course?**

```
The cybersecurity landscape identifies what is included in cybersecurity. Cybersecurity includes an attack surface and four moving parts. The attackers, they disrupt the environment; the defenders, they protect the environment; the infrastructure, what's being protected and the everyday users, students, employees, etc.
```

**2. Which CyberFoundations City district did you identify in Part A, and how does its theme connect to the hardware/OS/software material in Part B?**

```
I identified the Foundry District. The theme connects to the to the hardware/OS/software material in Part B because it relates to the foundation of the computer and explains the attack surface. 
```

**3. Of the three layers (hardware, OS, software), which one do you think is hardest to secure, and why?**

```
From a beginners standpoint, I think that the software layer would be the hardest to secure because software like Chrome is easily accessible by anyone and easier access creates higher risk for vulnerabilities.
```

---

## Submission Checklist

- [x] Lab Portal Module 1 orientation completed

- [x] District identified and explained

- [x] Hardware, OS, and software layer examples listed

- [x] Diagram uploaded to `assets/screenshots/week-02/` via GitHub and embedded directly in the committed file

- [x] Diagram explanation written in your own words (minimum 3 sentences)

- [x] All three Analysis Questions answered (minimum sentence counts met)

- [x] All three Lab Report Questions answered in complete sentences

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
