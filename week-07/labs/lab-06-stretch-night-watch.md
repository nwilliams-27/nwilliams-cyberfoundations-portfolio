# Week 7 Lab 06 — Night Watch — Optional Stretch

*The Logbook*

**Student Name:** N. Williams

**Date Completed:** September 6, 2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 7  
**Submission Path:** `week-07/labs/lab-06-stretch-night-watch.md`

> ## Cloud Heights Protected-Rules Safety Rule
> Four baseline rules are protected: **100** (`allow-ssh-from-bastion`), **110** (`allow-icmp-intra-vnet`), **120** (`deny-ssh-student-subnet`), and **1000** (`deny-tcp8080-student-subnet` — Inbound Deny TCP from `10.60.6.0/26` to port `8080`). **You never modify, delete, replace, or use a protected rule as a troubleshooting target.** Create or edit student rules only in priorities **200–999**. The priority **1000** fallback deny sits after your band on purpose, so a narrower Allow you create in 200–999 is evaluated first. A mistake in your student range is recoverable and is not a grading penalty when you diagnose it honestly.

> **Evidence safety:** Never include a Cloud Heights password or Bastion shareable URL. Crop browser address bars and login information before committing screenshots.

---

## Mission

Turn the evidence already exposed by the Lab Portal into a short analyst logbook. The current student Portal does not expose a VNet flow-log viewer, so this lab does not ask you to locate or interpret one. Work only with visible security-rule state and **Test My Rule** results.

**Optional stretch:** Skipping this lab does not reduce your grade.

## What You Already Know

A network security rule is a decision about traffic. Rules are evaluated from the lowest priority number to the highest, and the first matching rule wins. Inbound and outbound traffic use separate ledgers. A configured service, a security rule, a test result, and an evidence screenshot answer different questions.

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Evidence available | Security Rules list and Test My Rule result cards |
| Not available | Student-facing VNet/NSG flow-log viewer |
| Change level | No new rule changes required |
| Time | 20–30 minutes; optional |

- [x] I am using my assigned `cf-student-XX` VM through the CyberFoundations Lab Portal.

- [x] The VM shows **Running**.

- [x] I can identify the four protected baseline rules at priorities 100, 110, 120, and 1000.

- [x] I understand that my editable priority range is 200–999.

### Cloud Heights Idle Stop

Cloud Heights may warn you that the VM is idle. Return to the Lab Portal and choose **I'm still working** if you are active. If the VM is stopped or deallocated, it was not deleted: restart it from **My Lab Environment**. Your disk files and saved configuration remain.

## Predict First

Predict which conclusion can be supported by each artifact: a rule screenshot, an `ALLOWED` result, a `DENIED` result, and the Python listener output.

```text
Rule screenshot -> supports a conclusion about the configuration of a rule -> what is the rule set to do?

ALLOWED result -> supports the conclusion of what the tested traffic is permitting at that time.

DENIED result -> supports the conclusion of what the tested traffic blocked at that time.

Python listener output -> supports the conclusion of what the service was running and available to respond.
```

## Guided Steps

### Step 1 — Build the Evidence Chain

Use your Week 7 screenshots to identify:

1. **Configuration:** what the rule was designed to permit or deny.
2. **Enforcement result:** what the Portal test reported.
3. **Observed behavior:** whether the test source reached TCP 8080.
4. **Evidence artifact:** which screenshot supports the claim.

| Claim type | Claim | Supporting file | Limitation |
| --- | --- | --- | --- |
| Configuration | The rule allows inbound traffic to TCP 8080 from 10.60.4.6 and all other sources are denied by the fallback rule. | week07-lab03-beacon-allowed.png | This only shows the intent, not the actual behavior. |
| Enforcement | The test results were ALLOWED for 10.60.4.6 and DENIED for the unintended source. | week07-lab05-retest-results.png | These are only results of tests that are built into the portal and have not been confirmed as a real world connection. |
| Observed behavior | The results in the test screenshot show if the source reached port 8080. | week07-lab05-retest-results.png | There is no seperate listener log to confirm this and the source of evidence is the same as the enforcement claim. |

### Step 2 — Write a Night-Watch Entry

**Date/time recorded by student:** September 6, 2026 4:02 p.m.

**VM identifier:** student vm

**Rule reviewed:** Priority 300, Allow, Inbound, TCP, source 10.60.6.4, port 8080

**Intended source and port:** 10.60.6.4 (Grid Beacon), TCP port 8080

**Allowed-source result:** ALLOWED

**Unintended-source result:** DENIED

**Conclusion:**

```text
The rule successfully allowed the intended source, Gid Beacon 10.60.6.4 to reach port 8080 and correctly denied the unintended source that attempted the same connection. The results together, confirm that least privilege was enforced by allowing the traffic it was designed for and not being broadly permissive. The proves that the the rule works as it was intended to work and it was not a lucky test that just happened to pass.
```

**Evidence filenames:** week07-lab03-beacon-allowed.png AND week07-lab05-retest-results.png

### Step 3 — Name the Visibility Gap

Explain what you cannot conclude without a student-facing flow-log or audit viewer. Do not invent timestamps, packet records, or traffic history that the Portal does not show.

```text
Without a student-facing flow-log or audit viewer the actual timestamp or sequence of the tested traffic cannot be confirmed because the portal only shows the current verdict, it does not show the history. Secondly, I cannot confirm if any untested sources attempted to reach the service or what happened when they or if they did. Thirdly, I cannot confirm that the rule has consistently behaved over time because I only have one point in time result. Lastly, I cannot verify if real packets were sent and received because the portal only reflected the rule logic and not the traffic that was captured.
```

## Stop & Check

A rule list shows intended configuration. A test result shows one controlled test outcome. Neither is a complete history of all traffic.

## Test

No new test is required. If you repeat tests, keep the listener running, wait at least 10 seconds between tests, and use only the two Portal-provided sources.

## Capture Evidence

Create one clearly organized evidence-set screenshot or use the exact existing filenames from Labs 03–05. Do not fabricate a flow-log screenshot.

![Evidence set — week07-lab06-evidence-set.png](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/debdf28c4402d63ebcada31e5f72dfa483360bdf/assets/screenshots/week-07/week07-lab06-evidence-set.png)

## Explain

Write an analyst-style conclusion that clearly separates what is proven, what is inferred, and what remains unknown.

```text
It has been proven that the tested traffic behaved as it was configured to behave. The Grid Beacon was ALLOWED and the unintended source was DENIED on TCP port 8080. This rule inferred that least privilege was enforced correctly because testing an intended source and an unintended source together shows that the rule is narrowly scoped and not coincidently broadly permissive. The rule also infers that is would behave consistently for any future traffic that matches the conditions based on the first match wins evaluation. Whether or not this result would hold for a live connection outside of the simulated test in the portal remains unknown because no independent listener log was captured. Lastly, it is unknown if any other untested sources have attempted to reach this service because it was unable to be confirmed with flow log visibility.  
```

## Required Evidence

Save screenshots in `assets/screenshots/week-07/`:

- `week07-lab06-evidence-set.png`

Open each image at full size before submission. Confirm that no password, Bastion shareable URL, browser address bar, or unrelated private information is visible.

## Analysis Questions

**Analysis Question 1.** How is a security rule different from a record of traffic that actually occurred? (Minimum 3 sentences.)

```text
A security rule is different from a record of traffic that actually occurred based on the configured conditions because it describes intent not history. The record of traffic that occurred is evidence of what happened on the network. This includes real connections, timestamps and sources that are independent of what the rule says should happen. A rule can exist and be configured correctly without any traffic ever testing it but a traffic record only exists if real network activity has taken place.
```

**Analysis Question 2.** What can a single Test My Rule result prove, and what can it not prove? (Minimum 4 sentences.)

```text
A single Test My Rule result can prove how the current configuration of the rule evaluates a specific, tested combination of source, port and protocol at that particular moment in time. A single Test My Rule result cannot prove that the rule has been correctly or narrowly scoped because a broader, less secure rule has the ability to produce the same passing result for that one test. Secondly, a single Test My Rule result cannot prove that the rule would behave for another untested source or   provide confirmation that a real live connection would produce the same outcome as the simulated evaluation in the portal. Lastly, an a single Test My Rule result cannot prove that the behavior of the rule will be consistent over time because it is only a single point in time time, not a record that is on going. 
```

**Analysis Question 3.** Why is naming a visibility gap more professional than filling it with an assumption? (Minimum 3 sentences.)

```text
Naming a visibility gap more professional than filling it with an assumption because itis a representation of the actual evidence limits that are available and do not present a guess as a confirmed fact. Filling a gap with an assumption is taking a risk by stating something as true that has not been verified, which can be misleading to anyone that is relying on the report to make a decision. Identifying what is unknown in a clear manner leads to further investigations being targeted at that specific gap instead of leaving false impressions that the picture has already been completed.
```

## Submission Checklist

- [x] Configuration, enforcement, observed behavior, and evidence distinguished

- [x] Night-watch entry completed

- [x] Portal visibility gap stated accurately

- [x] No flow-log viewer or traffic history invented

- [x] Evidence filenames verified

- [x] Protected priorities 100, 110, 120, and 1000 were not changed.

- [x] Every rule I created or edited used priority 200–999.

- [x] No password, Bastion URL, or browser address bar appears in my files.

- [x] This worksheet is committed to `week-07/labs/lab-06-stretch-night-watch.md`.

## GitHub / Lab Portal Submission

1. Open **Week 7 → Lab 06: Night Watch — Optional Stretch** in the CyberFoundations Lab Portal.
2. Complete every worksheet field and confirm the listed evidence filenames.
3. Upload screenshots to `assets/screenshots/week-07/`.
4. Confirm your portfolio repository is connected, then choose **Submit to GitHub**.
5. Open the committed worksheet and each image on GitHub to verify formatting, legibility, and redaction.

*CyberVisionaries Institute · CyberFoundations · Tier I*
