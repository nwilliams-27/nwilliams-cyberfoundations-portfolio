# Week 7 Lab 03 — Build the Narrowest Door

**Student Name:** N. Williams

**Date Completed:** September 2, 2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 7  
**Submission Path:** `week-07/labs/lab-03-build-the-narrowest-door.md`

> ## Cloud Heights Protected-Rules Safety Rule
> Four baseline rules are protected: **100** (`allow-ssh-from-bastion`), **110** (`allow-icmp-intra-vnet`), **120** (`deny-ssh-student-subnet`), and **1000** (`deny-tcp8080-student-subnet` — Inbound Deny TCP from `10.60.6.0/26` to port `8080`). **You never modify, delete, replace, or use a protected rule as a troubleshooting target.** Create or edit student rules only in priorities **200–999**. The priority **1000** fallback deny sits after your band on purpose, so a narrower Allow you create in 200–999 is evaluated first. A mistake in your student range is recoverable and is not a grading penalty when you diagnose it honestly.

> **Evidence safety:** Never include a Cloud Heights password or Bastion shareable URL. Crop browser address bars and login information before committing screenshots.

---

## Mission

Run a temporary web service on TCP 8080 and create the least-privilege inbound rule: only Grid Beacon (`10.60.6.4`) may reach the service. You will predict the outcome before changing the ledger and verify the result afterward.

Your Allow at a student priority (recommended `300`) is evaluated **before** the protected priority **1000** `deny-tcp8080-student-subnet` fallback. So before you create the Allow, Grid Beacon's TCP 8080 traffic is denied by that priority 1000 fallback; after your narrow Allow exists and the listener is running, the same traffic is allowed because the lower priority number wins.

## What You Already Know

A network security rule is a decision about traffic. Rules are evaluated from the lowest priority number to the highest, and the first matching rule wins. Inbound and outbound traffic use separate ledgers. A configured service, a security rule, a test result, and an evidence screenshot answer different questions.

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Service command | `python3 -m http.server 8080` |
| Allowed source | Grid Beacon — `10.60.6.4` |
| Test | Portal **Test My Rule**, fixed TCP 8080 |
| Time | 35–45 minutes |

- [x] I am using my assigned `cf-student-XX` VM through the CyberFoundations Lab Portal.

- [x] The VM shows **Running**.

- [x] I can identify the four protected baseline rules at priorities 100, 110, 120, and 1000.

- [x] I understand that my editable priority range is 200–999.

### Cloud Heights Idle Stop

Cloud Heights may warn you that the VM is idle. Return to the Lab Portal and choose **I'm still working** if you are active. If the VM is stopped or deallocated, it was not deleted: restart it from **My Lab Environment**. Your disk files and saved configuration remain.

## Predict First

Before starting the service or creating a rule, predict both conditions:

1. What verdict should Grid Beacon receive while no student Allow exists?
2. What verdict should it receive after the narrow Allow is created and the service is listening?
3. Which rule decides each case — your student Allow, or the protected priority 1000 fallback deny?

```text
Verdict 1 -> Grid Beacon should receive Deny because the instruction states "before you create the Allow, Grid Beacon's TCP 8080 traffic is denied by the priority 1000 fallback. Due to no student rule being in place, the only rule that would apply is the protected priority 1000 deny-tcp8080-student-subnet fallback.

Verdict 2 -> Once the Allow (priority 800) is created and python is listening the Grid Beacon should receive Allow because the Allow rule is set at priority 800. Being 800 is a lower number than 1000 the Allow rule is evaluated first and this matches the Grid Beacon's traffic (10.60.4, TCP 8080) and wins which stops the evaluation of priority 1000 before it is even reached.

Before the student Allow rule exists the verdict to Deny is decided by the protected priority 1000 fallback rule. Once the narrow Allow exists and python is listening, the verdict to Allow is decided by the priority 800 student Allow rule.
```

## Guided Steps

### Step 1 — Start the Listener

Open the terminal on your assigned VM and run:

```bash
python3 -m http.server 8080
```

The terminal stays busy while the server runs. Leave it open. `Ctrl+C` stops it when the lab is complete.

### Step 2 — Stop & Check the Service State

Confirm the terminal shows that it is serving on port 8080. If the Portal later reports `SERVICE_NOT_LISTENING`, return here before changing rules.

### Step 3 — Create the Narrow Rule

In Security Rules, add one student rule with these values:

| Field | Required value |
|---|---|
| Name | `allow-grid-beacon-8080` (or a clearly equivalent name) |
| Priority | One unused value from 200–999; recommended `300` |
| Direction | Inbound |
| Action | Allow |
| Protocol | TCP |
| Source | `10.60.6.4` |
| Source port | Any |
| Destination | Your assigned VM / displayed default |
| Destination port | `8080` |
| Description | Week 7 least-privilege test service |

Do not broaden the source to the subnet or Any.

### Step 4 — Record the Change

```text
Name -> allow-grid-beacon-8080
Priority -> 300
Direction -> Inbound
Action -> Allow
Protocol -> TCP
Source -> 10.60.6.4
Source port -> *
Destination -> 10.60.6.21
```

## Test

Choose **Test My Rule**, select **Grid Beacon (10.60.6.4)**, and run the fixed TCP 8080 test. Wait at least 10 seconds before repeating a test.

Expected verdict: `ALLOWED`.

If you see `SERVICE_NOT_LISTENING`, the rule may be correct but the Python listener is not running. If you see `TEST_ERROR`, verify the VM is running, wait 10 seconds, and retry once before requesting support.

## Capture Evidence

Capture the completed narrow rule and the Grid Beacon `ALLOWED` result. The images must show the source and destination port clearly.

![Completed narrow rule — week07-lab03-rule-created.png](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-07/week07-lab03-rule-created.png)

![Grid Beacon ALLOWED result — week07-lab03-beacon-allowed.png](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-07/week07-lab03-beacon-allowed.png)

## Explain

Explain why allowing one source to one TCP port is narrower than allowing the whole subnet or Any source, even though all three might make the intended test pass.

```text
Allowing one source to one TCP port is narrower than allowing the whole subnet or any source, even though all three might make the intended test pass because it only allows the traffic that is needed instead of opening the door for larger machines that also work for the test. Allowing the whole subnet or any source would let the test succeed if the traffic falls within the broader range
```

## Required Evidence

Save screenshots in `assets/screenshots/week-07/`:

- `week07-lab03-rule-created.png`
- `week07-lab03-beacon-allowed.png`

Open each image at full size before submission. Confirm that no password, Bastion shareable URL, browser address bar, or unrelated private information is visible.

## Analysis Questions

**Analysis Question 1.** The Python service and the NSG rule perform different jobs. What does each one control? (Minimum 3 sentences.)

```text
The Python service is controls if is running and listening on the port and is ready to respond to requests. The NSG rule controls whether traffic is allowed to reach the service. If both don't work together, nothing will happen because in order to be reached the service needs allowed traffic and in order to respond allowed traffic needs a service.
```

**Analysis Question 2.** Why is `10.60.6.4` a stronger source value than Any for this task? (Minimum 4 sentences.)

```text
10.60.6.4 is a stronger source value than Any for this task because it follows least privilege by limiting access to one known machine instead of every possible source. 'Any' would  also pass the test because the Grid Beacon's traffic is included within 'Any.' It would also let anyone with or without a legitimate reason reach the server as well. Creating a narrow source shrinks the number of machines that could attempt to reach it and this secures the rule so that it is not only sufficient for the test.
```

**Analysis Question 3.** What does an `ALLOWED` result prove, and what does it not prove about other sources? (Minimum 3 sentences.)

```text
An 'ALLOWED' result proves that the tested traffic on a port was permitted under the current rule. A narrow scope is not proven because a much broader rule would produce the same result. Also, ir doesn't say anything about other sources because the depends on the actual configuration of the rule, not only on one test.
```

## Submission Checklist

- [x] Listener started before testing

- [x] Narrow inbound Allow created

- [x] Source is exactly `10.60.6.4` and destination port exactly `8080`

- [x] Grid Beacon result is `ALLOWED`

- [x] `week07-lab03-rule-created.png` captured

- [x] `week07-lab03-beacon-allowed.png` captured

- [x] Protected priorities 100, 110, 120, and 1000 were not changed.

- [x] Every rule I created or edited used priority 200–999.

- [x] No password, Bastion URL, or browser address bar appears in my files.

- [x] This worksheet is committed to `week-07/labs/lab-03-build-the-narrowest-door.md`.

## GitHub / Lab Portal Submission

1. Open **Week 7 → Lab 03: Build the Narrowest Door** in the CyberFoundations Lab Portal.
2. Complete every worksheet field and confirm the listed evidence filenames.
3. Upload screenshots to `assets/screenshots/week-07/`.
4. Confirm your portfolio repository is connected, then choose **Submit to GitHub**.
5. Open the committed worksheet and each image on GitHub to verify formatting, legibility, and redaction.

*CyberVisionaries Institute · CyberFoundations · Tier I*
