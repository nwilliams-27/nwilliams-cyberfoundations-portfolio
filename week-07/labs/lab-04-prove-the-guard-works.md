# Week 7 Lab 04 — Prove the Guard Works ★ Deliverable 2

**Student Name:** N. Williams

**Date Completed:** September 3, 2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 7  
**Submission Path:** `week-07/labs/lab-04-prove-the-guard-works.md`

> ## Cloud Heights Protected-Rules Safety Rule
> Four baseline rules are protected: **100** (`allow-ssh-from-bastion`), **110** (`allow-icmp-intra-vnet`), **120** (`deny-ssh-student-subnet`), and **1000** (`deny-tcp8080-student-subnet` — Inbound Deny TCP from `10.60.6.0/26` to port `8080`). **You never modify, delete, replace, or use a protected rule as a troubleshooting target.** Create or edit student rules only in priorities **200–999**. The priority **1000** fallback deny sits after your band on purpose, so a narrower Allow you create in 200–999 is evaluated first. A mistake in your student range is recoverable and is not a grading penalty when you diagnose it honestly.

> **Evidence safety:** Never include a Cloud Heights password or Bastion shareable URL. Crop browser address bars and login information before committing screenshots.

---

## Mission

Assemble Deliverable 2 evidence by proving both halves of least privilege: the intended source is allowed and an unintended source is denied. A single successful test is not enough.

## What You Already Know

A network security rule is a decision about traffic. Rules are evaluated from the lowest priority number to the highest, and the first matching rule wins. Inbound and outbound traffic use separate ledgers. A configured service, a security rule, a test result, and an evidence screenshot answer different questions.

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Required setup | Lab 03 rule present; Python listener still running |
| Allowed source | Grid Beacon — `10.60.6.4` |
| Unintended source | Other Test Source — `10.60.6.10` |
| Deliverable | Security group configuration + verification evidence |
| Time | 30–40 minutes |

- [x] I am using my assigned `cf-student-XX` VM through the CyberFoundations Lab Portal.

- [x] The VM shows **Running**.

- [x] I can identify the four protected baseline rules at priorities 100, 110, 120, and 1000.

- [x] I understand that my editable priority range is 200–999.

### Cloud Heights Idle Stop

Cloud Heights may warn you that the VM is idle. Return to the Lab Portal and choose **I'm still working** if you are active. If the VM is stopped or deallocated, it was not deleted: restart it from **My Lab Environment**. Your disk files and saved configuration remain.

## Predict First

**Prerequisite from Lab 03 (required):** the Python listener is running on TCP 8080, and your narrow inbound Allow for `10.60.6.4` port `8080` exists in priorities **200–999**. If either is missing, finish Lab 03 first.

**Expected results:** Grid Beacon `10.60.6.4` is **ALLOWED** by your student Allow; Other Test Source `10.60.6.10` is **DENIED** by the protected priority **1000** `deny-tcp8080-student-subnet` fallback.

**DO NOT** create an additional deny rule, broaden your allow, or modify any protected rule to produce these results.

Without changing the Lab 03 rule, predict the result from each test source.

| Source | Prediction | Deciding rule/reason |
| --- | --- | --- |
| Grid Beacon `10.60.6.4` | ALLOWED | 300 allow-grid-beacon-8080 |
| Other Test Source `10.60.6.10` | DENIED | 1000 deny-tcp8080-student-subnet |

## Guided Steps

### Step 1 — Verify the Final Configuration

Confirm the listener is running and the student Allow remains inbound TCP 8080 from exactly `10.60.6.4`.

### Step 2 — Test the Intended Source

Select **Grid Beacon (10.60.6.4)** and run **Test My Rule**. Record the verdict and compare it with your prediction.

```text
Verdict -> ALLOWED

I predicted that the traffic would be ALLOWED and the test result confirmed my prediction because the connection was successful.
```

### Step 3 — Test the Unintended Source

Wait at least 10 seconds. Select **Other Test Source (10.60.6.10)** and run the same fixed TCP 8080 test.

```text
Verdict -> DENIED

I predicted that the traffic would be DENIED and the test result confirmed my prediction and proves that the narrow rule excludes the unintended source because it was not allowed to reach the service.
```

Expected verdict: `DENIED`, produced by the protected priority 1000 fallback.

If either result differs from expected, **stop making changes**: capture the complete rule list in evaluation order plus the test result, and report it to your instructor in this worksheet. Do not add a deny rule, broaden the allow, or modify protected rules.

## Stop & Check

Your evidence pair should now prove:

- the intended connection is permitted;
- the unintended connection is not permitted;
- the service was listening during both tests;
- the rule source is narrow rather than Any.

## Test Summary

| Evidence question | Result |
| --- | --- |
| Is the service listening? | YES |
| Is Grid Beacon allowed? | YES |
| Is Other Test Source denied? | YES |
| Which rule produces the intended Allow? | 300 allow-grid-beacon-8080 |

## Capture Evidence

Capture the final rule plus both result cards. Screenshots must show the selected source and verdict. These images are the core evidence for Deliverable 2.

![Final rule — week07-lab04-final-rule.png](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-07/week07-lab04-final-rule.png)

![Grid Beacon ALLOWED result — week07-lab04-grid-beacon-allowed.png](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-07/week07-lab04-grid-beacon-allowed.png)

![Other Test Source DENIED result — week07-lab04-other-source-denied.png](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-07/week07-lab04-other-source-denied.png)

## Explain — Deliverable 2 Statement

Write a concise professional statement covering what you configured, the source/port scope, the two tests, and how the results prove least privilege.

```text
I configured a narrow rule at priority 300 that only allowed traffic from the Grid Beacon 10.60.6.4 to port 8080. When testing the rule I had my Python web server actively running on port 8080. This rule took precedence over the protected priority 1000 fallback deny rule because lower priority numbers are evaluated first. This configuration was tested in two parts, connecting from the Grid Beacon first followed by connecting from an unintended source that did not match the address of the rule. The result from the Grid Beacon was ALLOWED which confirmed that the intended source was able to reach the service. The result from the unintended source was DENIED which confirmed that traffic from outside of the specified scope was blocked correctly. Together, the results proved that least privileged was enforced properly because the rule only permitted the traffic that it was designed for and excluded everything else. This could not be proven on a successful connection alone because it is possible for a broader rule to produce the same passing result without being narrowly scoped.
```

## Required Evidence

Save screenshots in `assets/screenshots/week-07/`:

- `week07-lab04-final-rule.png`
- `week07-lab04-grid-beacon-allowed.png`
- `week07-lab04-other-source-denied.png`

Open each image at full size before submission. Confirm that no password, Bastion shareable URL, browser address bar, or unrelated private information is visible.

## Analysis Questions

**Analysis Question 1.** Why are one ALLOWED result and one DENIED result stronger evidence together than either result alone? (Minimum 4 sentences.)

```text
A single ALLOWED result does not prove that unintended sources are blocked because a broad rule would produce the same result. A single DENIED result does not prove that the intended source can get through when it should. However, both together proves that the rule only allows the right traffic and nothing more. This confirms both sides of least privilege, sufficient access for the right source and restricting everyone else.
```

**Analysis Question 2.** If the Other Test Source were ALLOWED, what would you inspect before changing anything? (Minimum 4 sentences.)

```text
If the Other Test Source was ALLOWED I would check to see if the rule's source field species 10.60.6.10 first. I would also check to see if a different rule with a lower number priority might be matching that traffic first instead of my intended rule. I would need to identify the true cause before changing anything because fixing the wrong rule would not solve the problem. This ensures that any corrections made address the real source of the unexpected result.
```

**Analysis Question 3.** How does this evidence distinguish configuration from observed enforcement? (Minimum 3 sentences.)

```text
Configuration is what a rule is set to say, for example, a specific source value but that alone does not prove that it is behaving as intended. Observed enforcement is the result of real traffic that has been tested, which confirms if the rule is working as written. The evidence distinguishes the two because a configuration that looks correct can still fail due to rule ordering or a hidden misconfiguration. Only testing reveals the truth.
```

## Submission Checklist

- [x] Final rule screenshot shows narrow source and TCP 8080

- [x] Grid Beacon `ALLOWED` evidence captured

- [x] Other Test Source `DENIED` evidence captured

- [x] Deliverable 2 statement completed

- [x] `week07-lab04-final-rule.png` captured

- [x] `week07-lab04-grid-beacon-allowed.png` captured

- [x] `week07-lab04-other-source-denied.png` captured

- [x] Protected priorities 100, 110, 120, and 1000 were not changed.

- [x] Every rule I created or edited used priority 200–999.

- [x] No password, Bastion URL, or browser address bar appears in my files.

- [x] This worksheet is committed to `week-07/labs/lab-04-prove-the-guard-works.md`.

## GitHub / Lab Portal Submission

1. Open **Week 7 → Lab 04: Prove the Guard Works ★ Deliverable 2** in the CyberFoundations Lab Portal.
2. Complete every worksheet field and confirm the listed evidence filenames.
3. Upload screenshots to `assets/screenshots/week-07/`.
4. Confirm your portfolio repository is connected, then choose **Submit to GitHub**.
5. Open the committed worksheet and each image on GitHub to verify formatting, legibility, and redaction.

*CyberVisionaries Institute · CyberFoundations · Tier I*
