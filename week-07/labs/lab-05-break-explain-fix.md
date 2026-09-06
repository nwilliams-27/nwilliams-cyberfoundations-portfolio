# Week 7 Lab 05 — Break It, Explain It, Fix It

**Student Name:** N. Williams

**Date Completed:** September 4, 2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 7  
**Submission Path:** `week-07/labs/lab-05-break-explain-fix.md`

> ## Cloud Heights Protected-Rules Safety Rule
> Four baseline rules are protected: **100** (`allow-ssh-from-bastion`), **110** (`allow-icmp-intra-vnet`), **120** (`deny-ssh-student-subnet`), and **1000** (`deny-tcp8080-student-subnet` — Inbound Deny TCP from `10.60.6.0/26` to port `8080`). **You never modify, delete, replace, or use a protected rule as a troubleshooting target.** Create or edit student rules only in priorities **200–999**. The priority **1000** fallback deny sits after your band on purpose, so a narrower Allow you create in 200–999 is evaluated first. A mistake in your student range is recoverable and is not a grading penalty when you diagnose it honestly.

> **Evidence safety:** Never include a Cloud Heights password or Bastion shareable URL. Crop browser address bars and login information before committing screenshots.

---

## Mission

Create a controlled priority failure inside your student range, diagnose it from evidence, remove the problem, and retest. The goal is method: UNDERSTAND → PREDICT → CHANGE → TEST → VERIFY, then FORECAST → EXECUTE → VERIFY → REMEDIATE → RETEST.

## What You Already Know

A network security rule is a decision about traffic. Rules are evaluated from the lowest priority number to the highest, and the first matching rule wins. Inbound and outbound traffic use separate ledgers. A configured service, a security rule, a test result, and an evidence screenshot answer different questions.

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Required setup | Lab 03 narrow Allow present; listener running |
| Safe failure | Student-created Deny only; all four protected baselines (100, 110, 120, 1000) untouched |
| Recommended temporary rule | Priority 250 Deny TCP 8080 from `10.60.6.4` |
| Time | 45–55 minutes |

- [x] I am using my assigned `cf-student-XX` VM through the CyberFoundations Lab Portal.

- [x] The VM shows **Running**.

- [x] I can identify the four protected baseline rules at priorities 100, 110, 120, and 1000.

- [x] I understand that my editable priority range is 200–999.

### Cloud Heights Idle Stop

Cloud Heights may warn you that the VM is idle. Return to the Lab Portal and choose **I'm still working** if you are active. If the VM is stopped or deallocated, it was not deleted: restart it from **My Lab Environment**. Your disk files and saved configuration remain.

## UNDERSTAND

Your working Allow is expected at priority 300 or another student value. A new matching Deny with a lower number is evaluated first and makes the later Allow unreachable for that traffic.

## Predict First — FORECAST

Predict Grid Beacon's verdict after adding a priority 250 inbound Deny for TCP 8080 from `10.60.6.4`, while leaving the working Allow in place.

```text
Predicted verdict -> DENIED

First matching rule -> 250 Deny rule

Reason -> Priority 250 is lower than priority 300 and it's gets evaluated first, being that it matches the Grid Beacon's traffic exactly the evaluation stops there. 
```

## CHANGE / EXECUTE

### Step 1 — Create the Temporary Fault

Create a student rule named `deny-grid-beacon-8080-test`:

- priority `250`
- Inbound / Deny / TCP
- source `10.60.6.4`; source port Any
- destination your assigned VM/default; destination port `8080`
- description: intentional Week 7 troubleshooting fault

Do not edit or delete the Lab 03 Allow. Do not touch the protected priorities 100, 110, 120, or 1000 — the priority 1000 `deny-tcp8080-student-subnet` fallback stays exactly as it is throughout this lab.

### Step 2 — Capture the Broken Ledger

Capture both the priority 250 Deny and the later Allow in the same ordered rule view.

## TEST / VERIFY

Wait at least 10 seconds, select Grid Beacon, and run **Test My Rule**. Expected verdict: `DENIED`.

```text
Actual result -> DENIED

Yes, this matched my forecast because the priority 250 Deny rule is a lower priority number than the existing priority 300 Allow rule it was evaluated first, correctly blocking the Grid Beacon's traffic.

```

### Stop & Check — Diagnose Before Fixing

Confirm these healthy facts before remediation:

- VM is Running.
- Python listener is still active on 8080.
- The original Allow is still present and correctly scoped.
- The temporary Deny is evaluated first.

```text
Diagnosis -> The DENIED result was caused by the new priority 250 rule Deny rule, which matched the Grid Beacon's traffic and was evaluated before the priority 300 Allow rule due to the lower priority number. This ruled out a stopped VM because a connection timeout or unreachable host would have been produced by a stopped VM, instead of a clean NSG level Denied verdict. A stopped service was also ruled out because a stopped Python listener would have caused a connection refusal at the application level instead of a network level Deny which was decided before the traffic even reached the service.

Remediate -> I deleted the temporary deny-grid-beacon-8080-test rule (priority 250). Removing this returned the ledger to its known good least privilege state with only the priority 300 Allow rule (source 10.60.6.4) governing access to port 8080.

Retest result -> ALLOWED. This confirmed that the system was returned to its known good least privilege state after the temporary priority 250 Deny rule was removed. Traffic to the Grid Beacon is now once again permitted by the priority 300 rule, just as it was before the deliberate fault was added.
```

## REMEDIATE

Delete only the temporary `deny-grid-beacon-8080-test` rule you created. Removing the controlled fault returns the ledger to the known-good least-privilege state.

## RETEST

1. Wait at least 10 seconds and retest Grid Beacon: expected `ALLOWED`.
2. Wait at least 10 seconds and retest Other Test Source: expected `DENIED`.

```text
Grid Beacon -> ALLOWED

Other Test Source -> DENIED
```

## Capture Evidence

Your sequence must show broken rules, observed denial, repaired rules, and final retest results.

![Broken rules — week07-lab05-broken-rules.png](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-07/week07-lab05-broken-rules.png)

![Observed denial — week07-lab05-observed-denial.png](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-07/week07-lab05-observed-denial.png)

![Fixed rules — week07-lab05-fixed-rules.png](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-07/week07-lab05-fixed-rules.png)

![Retest results — week07-lab05-retest-results.png](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-07/week07-lab05-retest-results.png)

## Explain — Incident Note

```text
Problem:
Evidence:
Healthy conditions ruled out:
Root cause:
Remediation:
Retest:
Prevention:
```

## Required Evidence

Save screenshots in `assets/screenshots/week-07/`:

- `week07-lab05-broken-rules.png`
- `week07-lab05-observed-denial.png`
- `week07-lab05-fixed-rules.png`
- `week07-lab05-retest-results.png`

Open each image at full size before submission. Confirm that no password, Bastion shareable URL, browser address bar, or unrelated private information is visible.

## Analysis Questions

**Analysis Question 1.** Why did the correct Allow stop working even though it was never edited? (Minimum 4 sentences.)

```text
The Allow rule stopped working even though it was never edited because the new priority 250 rule was evaluated first due to it having a lower priority number. NSG rules use first-match wins logic and evaluation stopped at the Deny rule and the Allow rule was never reached. This shows that the effects of a rule depend on its position relative to other rules and not just its own configuration. The Allow rule stayed correct and was overridden by the rule that was evaluated first.
```

**Analysis Question 2.** Why is diagnosing from evaluation order better than changing rules by trial and error? (Minimum 4 sentences.)

```text
Diagnosing the evaluation order is better than changing rules by trial and error because it identifies the root cause instead of guessing fixes without understanding why the problem happened. Trial and errors risks changing rules that were not the problem and possibly breaking something that was functioning properly. Understanding the evaluation order allows you to predict the outcome of a fix before making it while targeting the real cause instead of blindly testing. Due to the fix being based on evidence instead of chance, the process is faster and more reliable.
```

**Analysis Question 3.** What made this failure safe and recoverable in the course environment? (Minimum 3 sentences.)

```text
This failure safe in the course environment because it was a deliberate created test in a course lab instead of a real production environment with real consequences. It was recoverable because the fault was a clearly identified temporary rule that could be cleanly deleted which immediately restored the known good configuration. The problem was fully resolved by not modifying the Allow rule and removing the conflicting rule.
```

## Submission Checklist

- [x] Forecast written before the change

- [x] Temporary fault stayed in priorities 200–999

- [x] Broken ledger and DENIED result captured

- [x] VM, listener, and original Allow checked before remediation

- [x] Only the temporary Deny removed

- [x] Grid Beacon retested `ALLOWED` and Other Test Source retested `DENIED`

- [x] Incident note completed

- [x] Protected priorities 100, 110, 120, and 1000 were not changed.

- [x] Every rule I created or edited used priority 200–999.

- [x] No password, Bastion URL, or browser address bar appears in my files.

- [x] This worksheet is committed to `week-07/labs/lab-05-break-explain-fix.md`.

## GitHub / Lab Portal Submission

1. Open **Week 7 → Lab 05: Break It, Explain It, Fix It** in the CyberFoundations Lab Portal.
2. Complete every worksheet field and confirm the listed evidence filenames.
3. Upload screenshots to `assets/screenshots/week-07/`.
4. Confirm your portfolio repository is connected, then choose **Submit to GitHub**.
5. Open the committed worksheet and each image on GitHub to verify formatting, legibility, and redaction.

*CyberVisionaries Institute · CyberFoundations · Tier I*
