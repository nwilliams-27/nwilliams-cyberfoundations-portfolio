# Week 5 Submissions Guide

How to complete and submit each piece of Week 5 — The Grid. **Every item below is graded.** Week 5 has no flagship portfolio piece (your next one, ★ Deliverable 2, lands in Week 7), so this week's job is simpler: do the labs well, commit them, and build the evidence habits that Week 7 will lean on.

---

## Labs at a Glance

| Lab | What it covers | Submitted via | Screenshot |
|---|---|---|---|
| Lab 01 — Finding Your Place on the Grid | `ip addr`, `ip route`, `ping`, `dig` · `ipconfig`, `Resolve-DnsName`, `Test-Connection` | Lab Portal → Submit to GitHub | `cli-grid-address.png` — **optional** |
| Lab 02 — The Grid Outage | The Ladder Rule, name vs. IP testing, `traceroute`, your first incident note | Lab Portal → Submit to GitHub | `cli-grid-outage.png` — **required** |
| Lab 03 — Reading the Grid's Mail | Packet Inspector: filters, a DNS lookup, the TCP handshake, plaintext HTTP | Lab Portal → Submit to GitHub | `packet-dns-query.png` + `packet-http-plaintext.png` — **both required** |
| Lab 04 — The Real Grid (stretch) | Your own machine, the real internet, and redaction | Lab Portal → Submit to GitHub | `stretch-real-traceroute.png` — **required if you submit the lab**, redacted |

Each lab file carries its own Submission Checklist. **Those checklists are the source of truth** — this guide is the overview. Check every box before you submit.

---

## Where Everything Goes

All four labs commit to your portfolio repository at these exact paths:

```
week-05/labs/lab-01-finding-your-place.md
week-05/labs/lab-02-the-grid-outage.md
week-05/labs/lab-03-reading-the-grids-mail.md
week-05/labs/lab-04-stretch-the-real-grid.md
```

Screenshots go in one folder, for the whole week:

```
assets/screenshots/week-05/
```

**Filenames are part of the grade.** `cli_grid_outage.png` and `CLIGridOutage.PNG` are not the spec. Lowercase, hyphens, no spaces, `.png` — every time. Renaming a file on GitHub takes about a minute; getting it right on upload takes none.

---

## Lab 01 — Finding Your Place on the Grid

**What to include:** your full `ip addr` output, your IPv4 address, your prefix translated into a subnet mask (`/24` → `255.255.255.0` — record **both** forms, not just the slash), your full `ip route` output and the gateway you read off the `default` line, and your plain-English summary of all three numbers.

Then both pings from Part B with their loss and latency, your `dig` output with the A record **and** the SERVER line including the port number after the `#`, and the by-IP ping from Part C, Step 4.

**Don't skip Part C, Step 5.** The two pings landing on the same machine is the whole point of the lab, and you're asked to say what that proves in your own words. A summary of the steps you took isn't the same thing as saying what you learned.

Part D needs all three PowerShell commands run and recorded, plus your bash-to-PowerShell translation table.

**Screenshot — `cli-grid-address.png` — is optional this week.** Skipping it costs you nothing at all. If you have the energy, do it anyway: Week 7 asks for evidence exactly like this, and the habit is much easier to build now than under a deadline.

---

## Lab 02 — The Grid Outage

**What to include:** every rung of the ladder, in order, with its output — your own address and gateway, the successful gateway ping, the by-name ping of `relay-station.grid.local`, the by-IP ping of `10.20.5.30`, both traceroutes, and the decoy test in Part D.

Two things carry most of the weight:

- **Part B, Step 2.** Record what the *first line* of the ping showed as well as what the summary line showed. Those two lines disagree with each other, and noticing that is the finding.
- **Part E, the incident note.** All four fields filled: what's broken, what evidence proves it, what you ruled out and how, and what should happen next. Keep it short and specific — a paragraph you can defend beats an essay you can't.

**Screenshot — `cli-grid-outage.png` — is required.** It must show your *failure* evidence: at minimum the failed ping by name, the failed ping by IP, and the failed traceroute. A screenshot of your healthy Part A baseline doesn't cover it, because the baseline isn't what you're claiming.

---

## Lab 03 — Reading the Grid's Mail

**What to include:** your packet total counted with the filter bar **empty**, all five protocol names, all four columns explained in your own words, and the workstation address with your reasoning.

From Part B: the two DNS packet numbers, the full hostname queried, which address asked and which was asked, the IP address that came back, and the port number.

From Part C: the packets left after `tcp.port == 443`, the three handshake packets **with their directions** (one of them travels back the other way — check the Source column rather than assuming), both port numbers with the server's identified, every readable line of packet 14 including the exact staff code string, and your description of what packet 10 shows instead.

**Both screenshots are required:**

| Screenshot | Must show | Filename |
|---|---|---|
| Part B, Step 6 | The `dns` filter applied, list reduced, a DNS packet selected | `packet-dns-query.png` |
| Part C, Step 7 | Packet 14 **expanded**, readable lines visible | `packet-http-plaintext.png` |

A screenshot with packet 14 merely highlighted but still collapsed doesn't work — the readable text *is* the evidence.

---

## Lab 04 — The Real Grid (stretch lab)

**This lab is optional, and it is rated when you submit it.** If you complete it, it's read and scored like any other lab.

**Skipping it costs you nothing.** Labs 01, 02 and 03 are the complete graded path for Week 5. A locked-down work laptop, a Chromebook, a shared family machine, a device you don't administer, endpoint software that blocks ping — all of these are ordinary, legitimate reasons, and none of them says anything about you as a student. Choose freely and don't spend a second feeling behind.

**What to include if you do submit it:** the address command for *your* operating system and what it returned, the extra adapters and addresses your real machine has that the simulator didn't, your traceroute with hop count and first hop, your latency pattern from first hop to last, your reading of any `* * *` lines, your name lookup against a large site with at least two reasons a big service answers with several addresses, and all of Part D.

**Part D is not optional within this lab.** Before anything is committed, cover:

1. Your **public IP address**
2. Your **computer's hostname** — including in the terminal's title bar, which is where it usually hides
3. The **username in your shell prompt**
4. Any **ISP name** in a hop hostname

Redact by **cropping** or by drawing a **solid, opaque filled box**. Do not blur and do not pixelate — both can be reversed, and that has burned real professionals in published reports. Then re-read your *typed* answers too; a pasted traceroute leaks exactly the same things as an image.

**Screenshot — `stretch-real-traceroute.png` — required if you submit this lab, and redacted before upload.** Once an image is committed to a public repository, deleting it later does not reliably remove it from the history. Get it right the first time.

---

## Uploading a Screenshot

Same method as Week 4, for every screenshot this week.

1. Go to your portfolio repository on **GitHub.com** and open `assets/screenshots/week-05/` — create the folder on your first upload of the week.
2. **Add file → Upload files**, drag the image in, named exactly as specified above.
3. Scroll down and click **Commit changes**.
4. Click the uploaded image's **filename** to open it, and check it displays clearly at full size. If terminal text is too small to read, retake the screenshot and re-upload.
5. Back in your lab worksheet, type the filename into the screenshot field near the end of the lab.

That's it — the image lives in your repository and your grader will open it from there. You don't need to link or embed it inside the worksheet.

**Two things that will save you a resubmission:**

- **Name the file exactly as specified**, in lowercase with hyphens and no spaces. `cli-grid-outage.png` is right; `CLI Grid Outage.png` and `Screenshot 2026-08-10 at 9.14.22 AM.png` are not. The filename is how your grader finds it.
- **Redact before you upload, not after.** Anything committed to a public repository should be treated as permanently public, even if you delete it a minute later. This matters most for the stretch lab.

---

## Committing Your Work

Every lab this week commits. The Lab Portal does the commit for you:

1. Sign in to the **CyberFoundations Lab Portal**.
2. Open **Week 5 → the lab you're submitting**.
3. Fill in the worksheet fields — they match your lab file, in the same order.
4. Connect your GitHub account if you haven't already (one-time setup) and select your portfolio repo.
5. Click **Submit to GitHub**. The Portal writes the file to `week-05/labs/…` for you.
6. **Log the completed lab at [labsubmission.cybervisionariesinstitute.org](https://labsubmission.cybervisionariesinstitute.org)** so it's recorded against your name.

**Write a commit message that says what you did.** "Add Week 5 Lab 02 — Grid outage incident note and evidence" tells a reader something; "update" tells them nothing. Employers browse commit history, and this is free credibility.

---

## Notes and Reflection

Both are filled in through the Lab Portal (**Week 5 → Notes / Reflection**) and committed to `week-05/notes.md` and `week-05/reflection.md`.

The four reflection prompts are the same every week — that consistency is deliberate, and it's how you'll see your own growth when you re-read them in Week 12:

1. What clicked for you this week?
2. What's still confusing?
3. How does this week's material connect to a cybersecurity career path you're interested in?
4. One thing you'd tell a friend just starting this course.

Plus the Professional Growth Check boxes — tick them honestly rather than optimistically.

---

## Common Issues to Avoid

- **Recording `/24` without translating it.** Lab 01, Part A, Step 3 asks for the prefix *and* the mask (`255.255.255.0`). Half an answer is the most common Lab 01 return.
- **Missing the port on the `dig` SERVER line.** It's after the `#`. That `53` comes back in Lab 03 — you'll want to have noticed it.
- **Pasting the loopback address.** If your Part A answer starts `127.`, you've read the `lo` interface instead of `eth0`. Loopback is your machine talking to itself, not its place on the network.
- **Wrong shell.** Part D of Lab 01 is PowerShell; the rest is bash. `dig` in PowerShell and `Test-Connection` in bash both produce a command-not-found error — the fix is the shell selector, not the command.
- **"It failed" as your Lab 02 Part B answer.** The by-name ping did two things: it resolved the name *successfully*, then lost every packet. Record both halves. Students who record only the failure almost always reach the wrong conclusion in Part E.
- **Deciding what's broken before Part E.** Lab 02 is built so the evidence assembles the answer. Run every rung, record every result, then conclude.
- **Counting filtered packets in Lab 03.** Part A, Step 1 wants the total with the filter bar **empty**. If you answer 2, 4 or 7, a filter was still applied — clear it and recount.
- **Reversing the SYN-ACK direction.** Packets 7 and 9 travel one way; packet 8 travels back. Read its Source column rather than pattern-matching.
- **Paraphrasing the staff code.** "It had a code in it" isn't the value. Record the exact string. Precision in recording evidence is a habit this week starts building.
- **Screenshots in the wrong state.** Unfiltered DNS view, collapsed packet 14, or a Lab 02 screenshot showing only your healthy baseline — all three come back for a recapture, and all three are cheap to redo.
- **Filename drift.** `cli_grid_outage.png`, `packet-dns.png`, `Screenshot 2026-08-12 at 14.22.png` — none of these are the spec.
- **A filename that doesn't match the uploaded file.** The name you type must match the file in `assets/screenshots/week-05/` exactly — that's how your grader finds it.
- **Unredacted output in Lab 04.** A visible public IP or hostname in a committed image comes back for correction every time, without exception. Redact *before* you upload, not after.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
