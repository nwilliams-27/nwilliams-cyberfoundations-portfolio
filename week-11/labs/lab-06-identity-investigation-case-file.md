# Lab 06 — Identity Investigation + Case File (Deliverable 4)

**Learner:** N. Williams  
**Attempt:** attempt 34C82D · seed chic-w11-v1 · state revision 133 · text revision 376

> Training simulation — fictional Cloud Heights accounts and resources. Your actions change only your Week 11 learning attempt. This is not a live AD domain, Microsoft Entra tenant or Azure subscription.

## Objective

Correlate historical logs and assemble the Cloud Heights IAM Investigation Case File — Portfolio Deliverable 4.

## My answers

### Starting hypothesis before filtering

When I first looked at the table, I noticed that Casey’s account had five failed sign-ins in a row from an unknown device in Remote-B (S004 to S008). Casey then had a successful sign in from the same place (S009). I also noticed that Alex signed in from Office-A (S010) and then from Remote-C two minutes later (S011). Another thing that I noticed is that Drew’s sign-in failed because the account is disabled (S012) and Drew's sign in attempt came from an unknown device in Remote-B near Casey's IP address. My starting hypothesis before filtering is that the password on Casey’s account may have been guessed because there was no MFA recorded and Alex’s two sign in locations may be a shared session or something ordinary like a VPN. At the moment, I do not know if Casey’s successful sign in was the real user or if Alex’s second location is a problem.

## IAM Investigation Case File (Portfolio Deliverable 4)

_This is a technical case file. Week 12 uses it as source material to produce the professional Technical Incident Report and Executive Summary and to finalize and present the portfolio._

### 1. Scope and environment

This investigation examined identity and access activity in the CVI Demo Lab which is a simulated environment with Active Directory, Cloud Identity (Entra) and Azure resource access. It covers my work for Week 11 in Labs 01 to 05 (account setup, access tests, job-role configurations, ABAC tests, joiner, mover and leaver changes and an unverified reset request) along with the September 21 historical sign-in records (S001 to S012) and audit records (A001 to A004) in Lab 06. The boundaries are that all records are fabricated for training and the case file describes what the records show without identifying real people or confirming a real compromise.

### 2. Identity/directory trace and AD DS–Entra–Azure comparison

A person, for example, Alex Morgan (P01), is an HR record. That person can have two separate accounts: ad-alex in Active Directory and cl-alex in Cloud Identity (Entra). The two accounts have their own status, credentials, groups and sessions. Nothing synchronizes between the two accounts. Access does not come from the account alone. In Active Directory, an account joins a security group and the group is listed on a resource’s access control list. For example, before the transfer, ad-alex was in Response-Team, which gave read access on the Response Share (AT-34C82D-83-1). An OU only files the account and does not  grant any access. In Cloud Identity, an account joins a group and the group holds a role at a scope. For example, before the transfer, cl-alex was in Response-Operators, which held the Response Agent role at the queue scope, which gives read access on the Response Queue (AT-34C82D-85-1). Azure works the same way but it is separate. In T505, Azure Reader was assigned to Training-Resource-Readers at RG-LAB, and cl-blair was added to that group. Blair was able read metadata on the Training-VM (AT-34C82D-118-1) but not on the Other-Team-VM (AT-34C82D-121-1) because the scope limited the role. A role does not give a person access by itself. Access starts when the role is assigned to a group at a scope and a person is in that group.

### 3. Blair authentication-versus-authorization investigation

The sign-in and access records show that signing in and accessing a resource are separate checks. Blair’s account (cl-blair) signed in successfully on a managed device with MFA and a current credential, so authentication worked. Before the repair an access test for Case Summaries read was denied (AT-34C82D-3-1), with the reason that sign-in succeeded but no matching grant permitted the action. At the time, Blair’s only group then Junior-Analysts which gave read access on Analyst Landing (AT-34C82D-2-1, allowed) and nothing for Case Summaries. Blair was added to Case-Readers (LA-34C82D-5-1) under ticket T201. Afterward, Case Summaries read was allowed (AT-34C82D-6-1), and update was denied (AT-34C82D-7-1) which matched the read-only approval. The password, device and MFA did not change between the denied and allowed tests. The group membership is the only thing that changed. This shows that a successful sign-in proves who the account is and a matching grant decides what it can do.

### 4. Directory administration and sensitive-action verification

I created and managed accounts only under approved tickets and checked identity before sensitive actions during directory administration. Under T301, I created ad-jamie (LA-34C82D-9-1), added All-Staff and Support-Team (LA-34C82D-10-1 and 11-1) as well as enabled the account under the activation approval ACT-301 (LA-34C82D-14-1). Identity verification passed using the callback evidence CB-302 and HR ID CH-008 (LA-34C82D-12-1) before I reset the credential (LA-34C82D-13-1) under T302. No password was set or shown. I created cl-jamie (LA-34C82D-70-1), then verification passed using CB-501 and HR ID CH-008 (LA-34C82D-71-1) before I enrolled MFA (LA-34C82D-72-1) and enabled the account (LA-34C82D-73-1) under T501. An unrecognized caller claimed employee ID CH-099, but the HR record shows CH-002 for Blair under T504. The verification failed (LA-34C82D-113-1), I did not reset the credential and the ticket was escalated (LA-34C82D-114-1).

### 5. Three job-role configurations, scopes and business rationales

The role configuration was scoped to three job roles under T401, T402 and T403, each ticket approved the minimum access that the job needs.

Blair (junior analyst, T401): Junior-Analysts gives read access on Analyst Landing (RL-LAND at R-LAND), and Case-Readers gives read access on Case Summaries (RL-READ at R-CASE). Update access on Case Summaries was denied (AT-34C82D-33-1) because the business need is reading cases only.

Casey (response agent, T402): Response-Operators holds RL-RESP at R-QUEUE and R-ABAC, giving read access and update access on the Response Queue and read access on the Internal Response Note. Casey started in Privileged-Operators, which gave her more access than her job needed. She was able to delete items in the Response Queue, view and change Case Summaries, read, export and delete Audit Evidence and manage users and groups in the directory. I removed the directory assignment AS-09 (LA-34C82D-30-1), added Casey to Response-Operators (LA-34C82D-60-1) and removed Casey from Privileged-Operators (LA-34C82D-61-1). Audit Evidence delete access was then denied (AT-34C82D-64-1) and Internal Response Note read access was allowed (AT-34C82D-63-1).

Emi (audit reviewer, T403): Audit-Reviewers holds RL-AUD at R-AUD which gives read access and export access on Audit Evidence. I added Emi to the group (LA-34C82D-62-1). Read access and export access was allowed (AT-34C82D-65-1 and 66-1) and delete access was denied (AT-34C82D-67-1).

Least privilege means each person only gets only access that the job needs and nothing more. Each of these configurations matched the approved scope and any excess access was removed.

### 6. Access test matrix and temporary grant/revoke evidence

I tested access by running one access test for each person, resource and action, followed by comparing the decision with the ticket’s approved scope. Each test used a signed-in session and each result was recorded with a test ID.

Blair (T401): Case Summaries read access was allowed (AT-34C82D-32-1) and update access was denied (AT-34C82D-33-1).

Casey (T402): prior to the fix, Response Queue read access and update access was allowed (AT-34C82D-35-1 and 36-1), Audit Evidence delete access was allowed (AT-34C82D-37-1) and directory manage_sensitive_groups was denied after AS-09 was removed (AT-34C82D-38-1). After the fix, Internal Response Note read access was allowed (AT-34C82D-63-1) and Audit Evidence delete access was denied (AT-34C82D-64-1).

Emi (T403): prior to the fix, Audit Evidence read access, export access and delete access was denied (AT-34C82D-40-1, 41-1 and 42-1). After the fix, read access and export access was allowed (AT-34C82D-65-1 and 66-1) and delete access was denied (AT-34C82D-67-1).

Temporary grant and removal (T401): I added Blair to Audit-Reviewers as a temporary audit-access experiment (LA-34C82D-43-1) under T401. Audit Evidence read access was allowed during the grant (AT-34C82D-44-1). Blair was then from the group (LA-34C82D-45-1) and the same test was denied afterwards (AT-34C82D-46-1). As T401 requires, temporary access did not remain.

### 7. ABAC current-policy and what-if findings

The current-policy test showed that Casey was denied read access to the Internal Response Note (AT-34C82D-48-1). Her only group, Privileged-Operators, had no path to the note which is the reason that no matching grant permitted the action.

I then ran three what-if tests on the same record, changing one attribute at a time and returning the other two to current:

Device trust set to unmanaged was denied (AT-34C82D-53-1).
Department set to Audit (DEP-AUD) was denied (AT-34C82D-54-1).
Classification set to Confidential was denied (AT-34C82D-55-1).

All three results gave the same reason as the current-policy test: no matching grant permits this action. No attribute changed the result because Casey had no grant to the note, therefore no condition was reached.

After Casey was added to Response-Operators under T402 (LA-34C82D-60-1), the normal test for the same note was allowed (AT-34C82D-63-1). That result showed the attributes department=DEP-RESP, deviceTrust=managed and classification=Internal. The what-if tests were not reran after that change, so the records do not show whether any attribute would have changed the result once Casey had the grant.

### 8. Joiner/mover/leaver and unverified-reset decisions

For the lifecycle requests I checked the ticket’s approved scope and the attached evidence before each change, then I tested the result.

Joiner (T501, Jamie): HR-501 linked Jamie to HR ID CH-008 and CB-501 showed that the callback details matched. Verification passed (LA-34C82D-71-1). I created cl-jamie (LA-34C82D-70-1), enrolled MFA (LA-34C82D-72-1), enabled the account (LA-34C82D-73-1) and added Junior-Analysts and Case-Readers (LA-34C82D-77-1 and 78-1). Case Summaries read access was allowed (AT-34C82D-79-1) and update access was denied (AT-34C82D-80-1), which matched the read-only approval.

Mover (T502, Alex): HR-502 and APP-502 approved the move from Response to Audit. I saved the before state first (E13), then changed the department on the person record and both accounts (LA-34C82D-86-1, 86-2 and 86-3) and moved ad-alex to OU-AUD (LA-34C82D-87-1). In Active Directory, I removed Response-Team (LA-34C82D-88-1) and added Audit-Team (LA-34C82D-89-1). In Cloud Identity, I removed Response-Operators (LA-34C82D-94-1) and added Audit-Reviewers (LA-34C82D-95-1). The tests afterwards showed Response access was denied (AT-34C82D-91-1 and 97-1) and Audit and handbook access was allowed (AT-34C82D-92-1, 93-1 and 98-1).

Leaver (T503, Finley): HR-503 showed HR that approved the termination. I disabled both accounts (LA-34C82D-102-1 and 106-1), revoked sessions (LA-34C82D-103-1 and 107-1), removed all memberships (LA-34C82D-104-1, 105-1 and 108-1) and moved ad-finley to Disabled Accounts (LA-34C82D-109-1). Nothing was deleted. The old session was denied because the account is disabled (AT-34C82D-110-1)and a new sign-in attempt failed with ACCOUNT_DISABLED (LS-34C82D-111-1).

Unverified reset (T504, Blair): an unrecognized caller claimed employee ID CH-099 but the HR-LOOKUP showed Blair’s ID is CH-002. The callback number came from the caller and CB-504 showed no callback to the HR-recorded number. Verification failed (LA-34C82D-113-1), I did not reset the credential and the ticket was escalated (LA-34C82D-114-1).

### 9. Historical identity findings (see findings below)

My historical findings are supported by the September 21 sign-in and audit records and each finding rests on the exact event IDs that I selected.

Finding 1 (Casey, High priority): sign-in records S004, S005, S006, S007 and S008 show five failed password sign-ins from an unknown-device at 203.0.113.77 in Remote-B. Sign-in record S009 shows a password-only success from the same IP and device, with no MFA challenge recorded. Audit record A002 shows that the account added itself to Privileged-Operators with no approval ticket recorded. The logs support that these events happened in this order. The logs do not show who was using the account or why.

Finding 2 (Alex, Medium priority): sign-in records S010 and S011 show Alex signing in on an existing session from Office-A (192.0.2.10) and then signing in from Remote-C (198.51.100.20) two minutes later, both on managed-alex. Audit record A003 shows that the helpdesk removed Alex from Old-Response-Team under ticket CHG-1102. A003 has no correlation ID and came after both sign-ins so the logs do not link it to them. The logs do not show if the Remote-C sign-in was Alex or why the location changed.

#### Finding 1: Casey - Multiple failed sign ins followed by a no MFA successful sign in and a self added privileged group. 

- **Events:** S004, S005, S006, S007, S008, S009, A002
- **Observation:** Casey’s account (casey@cvi-training.example) had five failed sign-ins with incorrect_password from unknown-device at 203.0.113.77 in Remote-B between 14:00:00 and 14:02:00 (S004 to S008), 30 seconds apart. At 14:03:00 the same account signed in successfully from the same IP and device using only the password only with no MFA challenge recorded (S009). At 14:04:00 the account added itself to Privileged-Operators with no approval ticket recorded (A002, correlation ID C009).
- **Hypothesis:** The password on Casey's account may have been guessed after several failed attempts from an unknown device. The success without an MFA challenge could mean that MFA was not required for this sign-in or that it was not logged. The self-added Privileged-Operators membership may mean that the person using the account was trying to gain more access. There's also a possibility that the real user mistyped the password several times before getting in.
- **Supported conclusion:** The logs support that casey@cvi-training.example had five failed password sign-ins from an unknown device in Remote-B, followed by a successful password-only sign-in with no MFA challenge recorded. The log also supports that the account then added itself to Privileged-Operators with no approval ticket recorded. The shared correlation ID C009 links the successful sign-in (S009) to the group change (A002). The logs do not show who was using the account or why.
- **Uncertainty / missing information:** The logs don't show who was at the keyboard for S004 to S009 or if the person was the real Casey. They do not show why no MFA challenge was recorded for S009: MFA may not have been required or it may not have been logged. They do not show whether Casey’s account was locked out or reset before the successful sign-in. Lastly, the logs do not show what Casey did with Privileged-Operators after A002 or if anyone reviewed or approved the change afterwards.
- **Next action:** The next action would be for someone to contact Casey through a trusted channel, such as the phone number on the HR record, to confirm if she made the sign-ins from 203.0.113.77 and added herself to Privileged-Operators. Next, Casey's account needs to be reviewed to check if it should be in Privileged-Operators and remove it if no approval exists. Casey’s credential would need to be reset if the identity verification passes. Lastly, the sign-in logs need to be reviewed for other activity from 203.0.113.77 and for what the account did after A002.
- **Priority:** high — High priority was chosen because the logs show a possible account takeover pattern with a privilege change: five failed password sign-ins from an unknown device, a password-only success with no MFA challenge recorded followed by the account adding itself to Privileged-Operators with no approval ticket. Privileged-Operators gave broad access in this lab, which includes delete access and directory administration, so an unauthorized member could cause serious harm. A higher level was not chosen because the logs do not show who was using the account or what was done after the group change.

#### Finding 2: Alex: two locations in two minutes and an unlinked ticketed change.

- **Events:** S010, S011, A003
- **Observation:** At 14:10:00 UTC, alex@cvi-training.example signed in to CaseDesk from 192.0.2.10 in Office-A on managed-alex while using an existing session (S010). At 14:12:00 the same account signed in from 198.51.100.20 in Remote-C on managed-alex using an existing session (S011) as well. At 14:15:00, helpdesk@training.example removed alex@cvi-training.example from Old-Response-Team under approval ticket CHG-1102 (A003). A003 has no correlation ID.
- **Hypothesis:** The second sign-in for Alex from Remote-C may be an ordinary sign in, such as a VPN or a shared session carried over from the office because the device is still managed-alex and both sign-ins used an existing session. It is also a possibility that the session was used from another location by someone other than Alex. The removal from Old-Response-Team (A003) could be routine cleanup because it has an approval ticket and it may be unrelated to the sign-ins.
- **Supported conclusion:** The logs support that alex@cvi-training.example signed in twice using an existing session, first from Office-A (192.0.2.10) and then from Remote-C (198.51.100.20) two minutes later. Both sign ins were on managed-alex. The logs also support that helpdesk@training.example removed Alex from Old-Response-Team under ticket CHG-1102 at 14:15:00. The logs do not link A003 to either sign-in, since A003 has no correlation ID and came after both. Lastly, the logs do not show why the second location was used or if it was Alex.
- **Uncertainty / missing information:** The logs do not show if the Remote-C sign-in (S011) was Alex or why the location changed. They do not show if Alex used a VPN, if the session was shared or how the session reached 198.51.100.20. They do not show why the second sign-in came from a different IP while the device stayed managed-alex. Lastly,the logs do not show if A003 had any connection to the sign-ins or what access Alex lost beyond Old-Response-Team.
- **Next action:** The next actions would be to contact Alex through a trusted channel, such as the phone number on the HR record to confirm whether the Remote-C sign-in (S011) was theirs. Check if Alex was using a VPN or a documented remote-access method at 14:12. Review the sign-in logs for other activity from 198.51.100.20 and for what the session did after S011. Lastly, confirm with the helpdesk that A003 (ticket CHG-1102) was routine cleanup and unrelated to the sign-ins.
- **Priority:** medium — Medium priority was chosen because a sign-in from a second location two minutes after the first on an existing session is worth confirming, but the records show no failed attempts, no missing MFA and no privilege change tied to it. The device stayed managed-alex so a VPN or shared session is a plausible ordinary explanation. I did not choose High priority because nothing in the logs suggests misuse and I did not choose Low because the logs cannot show who used the session from Remote-C.

#### Finding 3: 

- **Events:** none
- **Observation:** _Not written yet._
- **Hypothesis:** _Not written yet._
- **Supported conclusion:** _Not written yet._
- **Uncertainty / missing information:** _Not written yet._
- **Next action:** _Not written yet._
- **Priority:** not set — _Not written yet._

### 10. Sign-in/audit correlation and one benign/ambiguous comparison

The selected records are linked by correlation ID C009. Sign-in record S009 (Casey, password only, no MFA challenge recorded, 14:03:00) and audit record A002 (Casey’s account adding itself to Privileged-Operators, no approval ticket, 14:04:00) share C009. This supports that the two records are linked in the logs and that the group change was recorded one minute after the sign-in. It does not prove who was using the account, why the change was made or that the sign-in was malicious. No other sign-in and audit pair shares a correlation ID: A001, A003 and A004 show not_available.

Benign or ambiguous comparison (Blair): Blair had one failed sign-in with an incorrect password (S002), followed by a successful sign-in with password plus MFA one minute later from the same IP and managed device in Office-A (S003). A001 shows that the helpdesk added Blair to Junior-Analysts under approval ticket CHG-1101. This looks like it could be an ordinary typo and it differs from Casey’s pattern: one failure, not five, on a managed device, with MFA challenged. The records cannot show why the first password was wrong or who typed it, therefore one failure followed by a success is not enough to claim compromise or malicious activity.

**Benign/ambiguous comparison** (S002, S003, A001): Blair’s failed sign-in with an incorrect password (S002) looks unusual because it can resemble password guessing. However, it came from the same IP and managed device in Office-A as the successful sign-in one minute later using password plus MFA (S003) and A001 shows a ticketed group add by the helpdesk (CHG-1101). That fits an ordinary typo. The records do not show why the password was wrong or who typed it and one failure followed by a success is not enough to claim compromise or malicious activity.

**Live simulator chain (current attempt, not historical):** LA-34C82D-43-1 
AT-34C82D-44-1
LA-34C82D-45-1 
AT-34C82D-46-1

This is a live simulator chain from my work in  Lab 04 for Week 11. This work is separate from the historical September 21 records. Under ticket T401, I added cl-blair to Audit-Reviewers as the temporary audit-access experiment (LA-34C82D-43-1). Next, an access test for Audit Evidence read then allowed (AT-34C82D-44-1). I removed cl-blair from Audit-Reviewers under T401 (LA-34C82D-45-1) and the same test was denied afterwards (AT-34C82D-46-1). The chain shows that the temporary access was granted, worked and then taken away, with each change tied to a ticket.

### 11. Preliminary response/remediation recommendations, priority and uncertainty

The next action I recommend checking is if Casey’s account (casey@cvi-training.example) was used by Casey for the sign-ins from 203.0.113.77 and for adding itself to Privileged-Operators (S004 to S009, A002). Contact Casey through a trusted channel, such as the phone number on the HR record. Review whether or not the Privileged-Operators membership has an approval and remove it if none exists. Reset Casey’s credential after identity verification passes. Review the sign-in logs for other activity from 203.0.113.77 and for what the account did after A002. Priority: High. The logs show a possible account takeover pattern with a privilege change but they do not show who was using the account or what was done after the group change.

The second action is to confirm with Alex, through a trusted channel, if the Remote-C sign-in (S011) was Alex and if a VPN or documented remote-access method explains it. Confirm with the helpdesk that A003 (CHG-1102) was routine cleanup and unrelated to the sign-ins. Priority: Medium. A second location two minutes later on an existing session is worth confirming but the records do not show any failed attempts, no missing MFA and no privilege change tied to it.

Blair’s failed sign-in followed by a success (S002, S003) needs no action beyond noting it. It has an ordinary explanation and the records show one failure on a managed device with MFA challenged.

What remains uncertain: who was at the keyboard for Casey’s sign-ins (S004 to S009) and for Alex’s Remote-C sign-in (S011), why no MFA challenge was recorded for Casey’s success (S009), if the Privileged-Operators change (A002) was later reviewed or approved and what any of these accounts did after the events. These are preliminary recommendations based only on the records.

### 12. Evidence index notes

This Case File references the following saved evidence captures and the event IDs used in each section.

Saved evidence captures:

E01 (directory trace: snapshots of ad-alex and cl-alex, captured at the starting state) and E02 (comparison snapshots of cl-blair, ad-alex, cl-alex and cl-finley, captured at the starting state): used in Section 2.

E03 (Blair’s successful sign-in and the access result before the change) and E04 (the membership change audit with the read and update results): used in Section 3.

E05 (ad-jamie with OU and groups), E06 (T302 verification and credential reset) and E07 (disable and re-enable sequence): used in Section 4.

E08 (cl-blair, cl-casey and cl-emi snapshots with configuration reviews) and E09 (the nine access tests): used in Sections 5 and 6.

E10 (Blair’s temporary Audit-Reviewers grant, allowed test, removal and denied retest): used in Section 6.

E11 (Casey’s current-policy and what-if tests): used in Section 7.

E12 (Jamie’s joiner chain), E13 (Alex’s before and after access and changes), E14 (Finley’s leaver evidence) and E15 (the failed reset verification and escalation): used in Section 8. E15 is also used in Section 4.

E16 (Blair’s Azure scoped tests): used in Section 2.

E17 (the two historical findings with correlation) and E18 (the benign comparison): used in Sections 9 and 10.

E13 keys: EV-34C82D-017 (before the transfer) and EV-34C82D-023 (after the transfer, including the reruns).

Historical records (September 21):

S004 to S009 and A002: Finding 1 and the correlation (Sections 9, 10 and 11).

S010, S011 and A003: Finding 2 (Sections 9 and 11).

S002, S003 and A001: the benign comparison (Section 10).

Live records from my work:

LA-34C82D-43-1, AT-34C82D-44-1, LA-34C82D-45-1 and AT-34C82D-46-1: Blair’s temporary grant chain (Section 6 and Document live activity).

The other LA- and AT- IDs are cited in the section where each action or test is described.

### Explicit simulation/environment limitations

This Case File is based on a simulated training environment and every record in it is fabricated or generated by the simulator. The accounts, people, IP addresses, devices, tickets and approvals are fictional, so the case file does not identify real people and cannot confirm a real compromise or real misuse.

The historical records (S001 to S012 and A001 to A004) are a small, fixed set from one day, September 21. They do not show what happened before or after that day, so they cannot show patterns over time. Correlation IDs link only one sign-in and audit pair (C009), so other records cannot be tied together by ID.

The records show what the accounts did, not who was using them. The logs cannot show a person’s intent, if a password was guessed or known or why a sign-in came from a different location. Several fields, such as MFA detail only show what the simulator recorded and I cannot tell if a missing entry means the control was not required or if it was not logged.

My live records (LA-, AT- and LS- IDs) come from tasks that I ran in the simulator and they are separate from the historical records. The simulator has no automatic synchronization between Active Directory and Cloud Identity and its access tests check one action on one resource at a time. Some saved captures, such as E01 to E04 and E08, show the state at the time they were saved and later changes do not rewrite them. I also ran some tests more than once so the evidence includes repeated results.

The recommendations in this Case File are preliminary and rest only on the records listed.

### Earlier labs

- [Lab 01](lab-01-identity-directory.md)
- [Lab 02](lab-02-authentication-vs-authorization.md)
- [Lab 03](lab-03-build-and-manage-directory.md)
- [Lab 04](lab-04-least-privilege-challenge.md)
- [Lab 05](lab-05-joiner-mover-leaver.md)

## Evidence

- **E17 · EV-34C82D-024** — Two distinct findings with raw-event references and correlation (captured at revision 133)  
  _What this proves:_ The selected sign-in records show five failed password sign-ins for Casey from unknown-device in Remote-B (S004 to S008) followed by a password-only successful sign in with no MFA challenge recorded (S009) and two sign-ins for Alex from different locations two minutes apart (S010 and S011). The audit records show that Casey added herself to Privileged-Operators with no ticket (A002, sharing correlation ID C009 with S009) and a ticketed removal of Alex from Old-Response-Team that has no correlation ID (A003).  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-024)
- **E18 · EV-34C82D-025** — Benign/ambiguous comparison (captured at revision 133)  
  _What this proves:_ There was one failed sign-in on Blair's account with an incorrect password (S002), followed one minute later by a successful sign-in with password plus MFA from the same IP and managed device in Office-A (S003). The audit record shows that the helpdesk added Blair to Junior-Analysts under approval ticket CHG-1101 (A001), which has no correlation ID.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-025)
- **E19 · EV-34C82D-026** — Completed case file / versioned export (captured at revision 133)  
  _What this proves:_ The selected sign-in records show five failed password sign-ins for Casey from an unknown-device in Remote-B (S004 to S008) followed by a password-only success with no MFA challenge recorded (S009), two sign ins for Alex from different locations two minutes apart (S010 and S011) and Blair’s single failed sign-in followed by a successful sign in with MFA (S002 and S003). The selected audit records show Casey adding herself to Privileged-Operators with no ticket (A002, sharing correlation ID C009 with S009), a ticketed removal of Alex from Old-Response-Team (A003) and a ticketed group add for Blair (A001).  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-026)
