# Week 3 — Student Submission Guide

**CyberFoundations · Tier I**

---

## Overview

Week 3 has three lab submissions, living in this repo:

```
week-03/
└── labs/
    ├── lab-01-command-line-navigation.md
    ├── lab-02-two-shells-same-job.md
    └── lab-03-command-line-scavenger-hunt.md
```

All three labs are different from Weeks 1–2: instead of settings screens and Task Manager, you'll be working inside the CyberFoundations CLI Simulator — a browser-based practice terminal, no install required. Submitting each lab has one required part and one optional part:

1. **Written answers:** Fill out that lab's worksheet in the CyberFoundations Lab Portal and click Submit to GitHub. The Portal writes the completed markdown file straight to your repo — no typing, no manual commit needed for this part.
2. **Optional screenshot:** If you'd like a visual record of a CLI Simulator session for your portfolio, you can upload one to `assets/screenshots/week-03/` and embed it, the same way Week 2's screenshots worked. This is not required for any of the three labs.

All three labs are graded — submitted through the Lab Portal and checked against your repo — just not one of the program's five flagship GitHub Deliverable portfolio checkpoints. Those start in Week 4.

The three labs build on each other, so complete them in order:
- **Lab 01** walks you through each of Lessons 3A/3B's five commands step by step, in whichever shell you prefer.
- **Lab 02** has you repeat the same task in both bash and PowerShell, back to back — framed as a two-pass Incident Response investigation, also having you create and back up an investigation note using Lesson 3C's commands.
- **Lab 03** is the week's wrap-up challenge — a deeper, more independent scavenger hunt with less hand-holding, including organizing what you find into a folder of your own making.

---

## Step-by-Step: Submitting Written Answers via the Lab Portal

1. Go to the CyberFoundations Lab Portal and sign in with your student Microsoft account.
2. Open Week 3, then select the worksheet for the lab you're working on (Lab 01, Lab 02, or Lab 03).
3. Load that lab's CLI Simulator scenario and work through its Parts directly in the simulator.
4. Fill in the worksheet fields with the commands you ran and the output you saw.
5. Connect your GitHub account the first time you're prompted, and select your portfolio repo.
6. Click Submit to GitHub. Repeat for each of the three labs, plus the Notes and Reflection worksheets.
7. Log each lab as complete in the lab tracker at labsubmission.cybervisionariesinstitute.org — separate from the Lab Portal, and how your completion actually gets recorded.

---

## Optional: Adding a Screenshot on GitHub.com

**This entire section is optional.** Skipping it will not affect your grade on any of the three labs.

1. Take a screenshot of your CLI Simulator session.
2. Go to your portfolio repository on GitHub.com and navigate to `assets/screenshots/week-03/`.
3. Click Add file → Upload files, drag in your image, and give it a descriptive name.
4. Commit changes, then click the uploaded image's filename to open it.
5. Right-click directly on the image and choose Copy image address (Chrome/Edge) or Copy Image Link (Firefox).
6. Go to the matching lab file, open the pencil (edit) icon, and paste the copied link into the embed line.

---

## Troubleshooting & Common Questions

| Common Issue | How to Handle It |
| :--- | :--- |
| I don't know which shell to use | For Labs 01 and 03, either is fine. Lab 02 specifically requires both. |
| I typed something wrong and got an error | Expected and harmless — the CLI Simulator is consequence-free. Read the error, then try again. |
| My backup file disappeared (Lab 02) | You probably used `mv`/`Move-Item` instead of `cp`/`Copy-Item`. Use `cp` for a backup. |
| My original files are still scattered (Lab 03) | You probably used `cp`/`Copy-Item` instead of `mv`/`Move-Item`. Use `mv` to actually move them. |
| How do I verify everything worked? | Go to your portfolio repo on GitHub.com and check `week-03/labs/` for all three lab files. |
| Do I need to do anything besides Submit to GitHub? | Yes — also log each lab as complete in the lab tracker at labsubmission.cybervisionariesinstitute.org. It's a separate system from the Lab Portal, and it's how your instructor confirms you're done. |

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
