# Week 4 — Permissions, Searching, and Your First Virtual Machine ★ Deliverable 1

---

## Focus

Week 4 closes Module 1 — Digital Infrastructure & CLI, and the Foundry District arc. Four lessons carry the week: File Permissions (who can touch what), Wildcards & Searching (finding anything fast), What Is a Virtual Machine?, and The VM Lifecycle & the Road Ahead. The command line goes deeper instead of wider — permissions and search build directly on Week 3's ten commands — and then the week pivots to the concept the whole second half of this course stands on: virtualization.

**This is your first flagship deliverable week.** Deliverable 1 — VM concepts + CLI screenshots — is assembled across all three labs and committed to your portfolio. Every week's work is graded; this week's also becomes the first showcase piece in the portfolio you'll present in Week 12.

**You build `week-04/` yourself,** same as Week 3 — see `HOW-TO-CREATE-YOUR-WEEK-FOLDER.md` at the repo root. The content for this file and the submissions guide is available in the Lab Portal's Week 4 Reference section — no retyping needed.

---

## Outcomes

By the end of this week, you'll be able to:

- Read any file's permission string from `ls -l` and decode it audience by audience (owner / group / other)
- Change permissions deliberately with `chmod` — symbolic and numeric — following THE GATEKEEPER'S RULE (check before, check after)
- Read a Windows ACL with `Get-Acl` and translate an access entry into plain English
- Match many files at once with `*`, `?`, and `[ ]` patterns — and always test a pattern with `ls` before acting on it
- Search inside files with `grep`/`Select-String`, including case-insensitive and multi-file sweeps
- Explain host, guest, hypervisor, Type 1 vs. Type 2, and isolation — the vocabulary of virtualization
- Walk the full VM lifecycle (create → start/stop → snapshot → delete) and say what's on the billing meter at every stage

---

## Environment

No local install this week — same as Weeks 2–3, plus one new destination.

| Component | Details |
|---|---|
| CLI Simulator | Labs 01–02, in the Lab Portal — new "Badge Audit" and "Archive Investigation" scenarios, plus Permissions & Search warm-up modules |
| VM Builder Simulator | Lab 03 — browser-based, at `https://cybervisionariesinstitute.github.io/cyberfoundations-simulators/vm-builder.html` (also linked from the Week 4 page) |

---

## What You'll Submit

| Deliverable | Location |
|---|---|
| Lab 01 — File Permissions: The Badge Audit | `week-04/labs/lab-01-file-permissions.md` |
| Lab 02 — The Archive Investigation | `week-04/labs/lab-02-wildcards-and-searching.md` |
| Lab 03 — Build Your First Virtual Machine ★ | `week-04/labs/lab-03-vm-builder.md` |
| Notes | `week-04/notes.md` |
| Reflection | `week-04/reflection.md` |
| ★ Deliverable 1 screenshots (4) | `assets/screenshots/week-04/` — see the submissions guide for exact filenames |

---

## Weekly Checklist

- [ ] Created `week-04/` and `week-04/labs/` folders (repo-root guide)
- [ ] Watched all four lessons and their resource packs
- [ ] Warmed up in the CLI Simulator's Permissions & Search modules
- [ ] Completed Labs 01, 02, and 03 through the Lab Portal
- [ ] Uploaded all four Deliverable 1 screenshots with exact filenames
- [ ] Committed notes.md and reflection.md
- [ ] Checked every box in each lab's own Submission Checklist

---

## What "Good" Looks Like

Permission fixes with the before-and-after checks recorded — not just the final state. Patterns tested with `ls` before anything acted on them. Analysis answers in your own words, meeting the minimum sentence counts. Screenshots that are legible, complete, and named exactly to spec. Commit messages that say what the work is. Deliverable 1 isn't graded on artistry — it's graded on exactly the professional discipline this list describes.

---

## Connecting to Prior Weeks

Week 2 taught you what a computer is; Week 3 gave you ten commands to run one. Week 4 finishes the arc: who's allowed to touch what (permissions), how to find anything (patterns + search), and how computers themselves became something you can summon and dismiss (VMs). Next week the course crosses the bridge to The Grid & Cloud Heights — networking — and in Week 6 the Lab Portal hands you a real Azure machine stamped from a golden snapshot. You'll recognize exactly how it was made, because you built one in simulation this week.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
