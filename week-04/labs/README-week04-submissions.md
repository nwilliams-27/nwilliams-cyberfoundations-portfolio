# Week 4 Submissions Guide

How to complete and submit each piece of Week 4 — including your first flagship deliverable. Every item below is graded.

---

## Labs at a Glance

| Lab | What it covers | Submitted via | Required screenshot |
|---|---|---|---|
| Lab 01 — File Permissions: The Badge Audit | `ls -l`, symbolic `chmod`, `Get-Acl` | Lab Portal → Submit to GitHub | `cli-permissions-audit.png` |
| Lab 02 — The Archive Investigation | Wildcards, `grep`/`Select-String`, find → check → lock down | Lab Portal → Submit to GitHub | `cli-search-investigation.png` |
| Lab 03 — Build Your First Virtual Machine ★ | The VM Builder Simulator: wizard, errors, lifecycle, meter | Lab Portal → Submit to GitHub | `vm-config-summary.png` + `vm-dashboard-running.png` |

Each lab file carries its own Submission Checklist — treat those as the source of truth, and check every box before submitting.

---

## ★ Deliverable 1 — VM Concepts + CLI Screenshots

Your first portfolio flagship. It is complete when **all four screenshots** exist in `assets/screenshots/week-04/` with these exact filenames (lowercase, hyphens, no spaces):

1. `cli-permissions-audit.png` — Lab 01's final corrected `ls -l`
2. `cli-search-investigation.png` — Lab 02's Part C sequence (search, check, fix)
3. `vm-config-summary.png` — Lab 03's Review screen, before Create
4. `vm-dashboard-running.png` — Lab 03's dashboard: Running, with at least one snapshot visible

…and the two VM screenshots are embedded in your committed Lab 03 file.

**Uploading a screenshot (same method as Week 3):** on GitHub.com, open `assets/screenshots/week-04/` (create it on your first upload) → **Add file → Upload files** → drag the image in → **Commit changes**. To embed: click the uploaded image's filename, right-click directly on the image, choose **Copy image address** (Chrome/Edge) or **Copy Image Link** (Firefox), and paste that link into the embed placeholder in your lab file. If right-click doesn't offer it, click the download-arrow icon and copy the URL from the address bar.

**Commit message:** "Add Deliverable 1: VM lifecycle and CLI evidence" — meaningful and descriptive, per your Professional Growth Check.

---

## Notes and Reflection

Both are filled in through the Lab Portal (Week 4 → Notes / Reflection) and committed to `week-04/notes.md` and `week-04/reflection.md`. Same four reflection prompts as every week — consistency is the point.

---

## Common Issues to Avoid

- **Missing "before" checks.** Lab 01 and Lab 02's permission fixes require `ls -l` before AND after each `chmod` — a fix showing only the final state comes back for revision.
- **A pattern that catches too much.** Lab 02, Part A Step 3 requires your output to show the matched files *and nothing extra* — test with `ls` first.
- **Empty search ≠ absent word.** If `grep denied` returns nothing, check your case before concluding — the logs speak in CAPS.
- **Screenshot 2 in the wrong state.** `vm-dashboard-running.png` must show status **Running** and a snapshot in the list. Stopped, or no snapshot = recapture (the simulator re-runs in minutes).
- **Filename drift.** `vm_config_summary.png` and `VMdashboard.png` are not the spec. Renaming on GitHub takes one minute — exactness is part of the grade.
- **Passwords in worksheets.** Lab 03 never asks for your simulator password. If you typed one anywhere, edit it out before submitting.
- **Refreshing the VM Builder mid-run.** The simulator resets completely on refresh — capture each screenshot when prompted, not later.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
