# Week 7 Submissions Guide — Cloud Heights: The Guard Post

Week 7 is a Deliverable 2 week. The evidence standard is paired: prove what should be allowed and what should be denied, and show the rule that caused those results.

## Labs at a Glance

| Lab | Focus | Required evidence |
|---|---|---|
| 01 — Meet the Guard | protected baseline + editable range | `week07-lab01-security-rules-baseline.png` |
| 02 — Read the Door Ledger | evaluation order + first match | `week07-lab02-evaluation-order.png` |
| 03 — Build the Narrowest Door | Grid Beacon-only TCP 8080 Allow | rule + beacon ALLOWED screenshots |
| 04 — Prove the Guard Works ★ | Deliverable 2 paired proof | final rule + ALLOWED + DENIED screenshots |
| 05 — Break, Explain, Fix | controlled priority fault + remediation | broken, denied, fixed, retest evidence |
| 06 — Logbook / Night Watch | optional evidence-chain analysis | `week07-lab06-evidence-set.png` if submitted |

## Where Everything Goes

```text
week-07/labs/lab-01-meet-the-guard.md
week-07/labs/lab-02-read-the-door-ledger.md
week-07/labs/lab-03-build-the-narrowest-door.md
week-07/labs/lab-04-prove-the-guard-works.md
week-07/labs/lab-05-break-explain-fix.md
week-07/labs/lab-06-stretch-night-watch.md
assets/screenshots/week-07/
```

## Non-Negotiable Safety

- Never modify protected priorities 100, 110, or 120.
- Create/edit student rules only in priorities 200–999.
- Never publish a password or Bastion shareable URL.
- Crop browser address bars and login details.
- Use the Lab Portal and your assigned VM, not the Azure Portal.

## Test My Rule Facts

The Portal always tests TCP 8080 to your own VM. Available sources are Grid Beacon `10.60.6.4` and Other Test Source `10.60.6.10`. Start `python3 -m http.server 8080` first. Verdicts are `ALLOWED`, `DENIED`, `SERVICE_NOT_LISTENING`, and `TEST_ERROR`. Wait at least 10 seconds between tests.

## Notes and Reflection

```text
week-07/notes.md
week-07/reflection.md
```
