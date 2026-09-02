# Week 7 Lab 01 — Meet the Guard

**Student Name:** N. Williams

**Date Completed:** August 26, 2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 7  
**Submission Path:** `week-07/labs/lab-01-meet-the-guard.md`

> ## Cloud Heights Protected-Rules Safety Rule
> Four baseline rules are protected: **100** (`allow-ssh-from-bastion`), **110** (`allow-icmp-intra-vnet`), **120** (`deny-ssh-student-subnet`), and **1000** (`deny-tcp8080-student-subnet` — Inbound Deny TCP from `10.60.6.0/26` to port `8080`). **You never modify, delete, replace, or use a protected rule as a troubleshooting target.** Create or edit student rules only in priorities **200–999**. The priority **1000** fallback deny sits after your band on purpose, so a narrower Allow you create in 200–999 is evaluated first. A mistake in your student range is recoverable and is not a grading penalty when you diagnose it honestly.

> **Evidence safety:** Never include a Cloud Heights password or Bastion shareable URL. Crop browser address bars and login information before committing screenshots.

---

## Mission

Inspect the existing NIC-level security rules on your assigned VM without changing anything. Your goal is to recognize the guardrails, separate protected rules from student-editable space, and map each visible field to the firewall mental model.

## What You Already Know

A network security rule is a decision about traffic. Rules are evaluated from the lowest priority number to the highest, and the first matching rule wins. Inbound and outbound traffic use separate ledgers. A configured service, a security rule, a test result, and an evidence screenshot answer different questions.

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | My Lab Environment → Cloud Heights → Security Rules |
| Change level | Read-only; do not add, edit, or delete rules |
| Expected protected rules | 100 `allow-ssh-from-bastion`; 110 `allow-icmp-intra-vnet`; 120 `deny-ssh-student-subnet`; 1000 `deny-tcp8080-student-subnet` |
| Time | 15–20 minutes |

- [x] I am using my assigned `cf-student-XX` VM through the CyberFoundations Lab Portal.

- [x] The VM shows **Running**.

- [x] I can identify the four protected baseline rules at priorities 100, 110, 120, and 1000.

- [x] I understand that my editable priority range is 200–999.

### Cloud Heights Idle Stop

Cloud Heights may warn you that the VM is idle. Return to the Lab Portal and choose **I'm still working** if you are active. If the VM is stopped or deallocated, it was not deleted: restart it from **My Lab Environment**. Your disk files and saved configuration remain.

## Predict First

Before opening the rule list, predict why a course environment would protect its access and safety rules from student edits.

```text
A course environment would protect its access and safety rules from student edits because students are still learning and do not fully understand what they are doing. Protecting the access and safety rules from student edits prevent accidental changes that can break the network. 
```

## Guided Steps

### Step 1 — Open the Guard Post

Start your VM from **My Lab Environment** first. The **Live Azure lab** card is only a launcher — all rule work happens in the Lab Portal's **Security Rules** panel. Do not work in the Azure Portal.

In Cloud Heights, scroll **below** the yellow *Protected rules — do not modify* summary to the detailed list headed **INBOUND — EVALUATION ORDER**. That detailed list, not the yellow summary, is what you inventory and capture.

### Step 2 — Inventory the Baseline

Record each protected rule exactly as shown.

| Priority | Rule name | Direction | Protocol | Source | Destination/port | Action | Protected? |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 100 | allow-ssh-from-bastion | Inbound | TCP |  192.168.10.128/26 | 22 | Allow | Yes |
| 110 |  allow-icmp-intra-vnet | Inbound | ICMP | VirtualNetwork | * | Allow | Yes |
| 120 | deny-ssh-student-subnet | Inbound | TCP | 10.60.6.0/26 | 22 | Deny | Yes |

### Step 3 — Map the Fields

For each field, write the question it answers: direction, source, source port, destination, destination port, protocol, action, and priority.

```text
Direction -> Is this traffic coming in or going out?

Source -> Where is this traffic coming from?

Source Port -> Which port on the sending machine originated this traffic?

Destination -> Where is this traffic going?

Destination Port -> Which port on the receiving machine is this traffic trying to reach?

Protocol -> What kind of traffic is this (ICMP, TCP, UDP, etc.)?

Action -> Should this traffic be allowed or denied?

Priority -> When multiple rules apply, which rule gets evaluated first?

```

## Stop & Check

- Can you edit a protected rule? You should not be able to — all four are locked.
- Where may student rules be created? Priorities 200–999.
- Which value is read first: 200 or 900? The lower number, 200.

## Test

This is a read-only lab: do not add, edit, or delete any rule. Your test is visual verification — confirm all four protected rules remain present and that no student rule was created.

## Capture Evidence

Capture the detailed **INBOUND — EVALUATION ORDER** view showing all four protected rules (100, 110, 120, 1000) and no student rule. If it does not fit in one image, use two clearly named images and explain why.

![Security rules baseline — week07-lab01-security-rules-baseline.png](https://raw.githubusercontent.com/nwilliams-27/nwilliams-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-07/week07-lab01-security-rules-baseline.png)

## Explain

In 3–4 sentences, explain how protected baselines and a separate student priority band reduce accidental lockout while still allowing meaningful practice.

```text
Protected baselines are in place so that the critical security rules, which are the rules that are necessary for an environment to function safely, cannot be accidentally changed or deleted by students. This measure prevents a mistake from locking everyone out or breaking the environment. The separate student priority band provides students with a dedicated space to add, test and modify their own rules without touching or conflicting with the baseline protected rules. When combined, protected baselines and a separate student priority band provides meaningful hands on practice because students get to create and test their rules while the underlying safety configurations and essential access remains protected from being accidentally broken.
```

## Required Evidence

Save screenshots in `assets/screenshots/week-07/`:

- `week07-lab01-security-rules-baseline.png`

Open each image at full size before submission. Confirm that no password, Bastion shareable URL, browser address bar, or unrelated private information is visible.

## Analysis Questions

**Analysis Question 1.** Why is a priority number part of rule behavior rather than just an identifier? (Minimum 3 sentences.)

```text
A priority number determines the order in which rules are evaluated. Due to NSG (National Security Group), processing stops at the first matching rule and the priority number decides which rule takes effect when rules such as allowing and blocking the same traffic conflict. Changing the priority number of a rule can change the outcome of the traffic and not just how the rules are identified, making priority number part of rule behavior rather than just an identifier.
```

**Analysis Question 2.** Explain the difference between a rule being visible, editable, and protected. (Minimum 3 sentences.)

```text
Visible means that I can see that a rule exists and view the configuration but I may not have to ability to change it. Editable means that I can modify or the delete the rule and requires a higher level of access than just having the ability to view it. Protected means that a rule is locked, it cannot be changed or removed no matter who's viewing it.
```

**Analysis Question 3.** Which baseline rule protects your current administrative path, and why must it never be used as a troubleshooting target? (Minimum 3 sentences.)

```text
Baseline rule 100 allow-ssh-from-bastion protects my current administrative path because this is the rule that is allowing the SSH connection and how I access the Cloud Heights VM. This baseline rule should never be used as a trouble shooting target because if it is modified, disabled or deleted I would lose access immediately and I would not be able to reconnect or fix anything afterwards. If this rule is removed or broken while troubleshooting for an unrelated issue is taking place, I could lock myself and possibly everyone else out of the machine without a back back in through normal means, which would turn a small troubleshooting mistake into a much larger problem that is hard to recover from.
```

## Submission Checklist

- [x] Baseline inventory completed without changes

- [x] All visible rule fields mapped to their security questions

- [x] Editable range 200–999 identified

- [x] `week07-lab01-security-rules-baseline.png` captured

- [x] Protected priorities 100, 110, 120, and 1000 were not changed.

- [x] I did not create, edit, or delete any security rules during this read-only lab.

- [x] No password, Bastion URL, or browser address bar appears in my files.

- [x] This worksheet is committed to `week-07/labs/lab-01-meet-the-guard.md`.

## GitHub / Lab Portal Submission

1. Open **Week 7 → Lab 01: Meet the Guard** in the CyberFoundations Lab Portal.
2. Complete every worksheet field and confirm the listed evidence filenames.
3. Upload screenshots to `assets/screenshots/week-07/`.
4. Confirm your portfolio repository is connected, then choose **Submit to GitHub**.
5. Open the committed worksheet and each image on GitHub to verify formatting, legibility, and redaction.

*CyberVisionaries Institute · CyberFoundations · Tier I*
