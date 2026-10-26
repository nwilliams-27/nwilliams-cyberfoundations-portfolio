# Cloud Heights Identity Investigation — Technical Incident Report

> Fictional training scenario: Cloud Heights Identity Center (simulated). No real organisation, people or systems.

- Analyst: N. Williams
- Source case file: [../week-11/labs/lab-06-identity-investigation-case-file.md](../week-11/labs/lab-06-identity-investigation-case-file.md)
- Evidence index: [../week-11/labs/evidence/week11-evidence-index.md](../week-11/labs/evidence/week11-evidence-index.md)

## 1. Purpose / incident context
This report documents the investigation of the September 21 historical sign-in records (S001 to S012) and audit records (A001 to A004) in the CVI Demo Lab that were reviewed for signs of suspicious identity activity. The review was prompted by the records themselves. The records include repeated failed sign-ins for one account, a sign-in from a second location for another and changes to a group made by different actors. The lab did not give any labeled alerts or reports, so there was no single trigger. The purpose is to document what the records show, what they do not show and what should be checked next, using only the evidence that is in my Week 11 Case File.

## 2. Scope
The investigation covered the September 21 historical sign-in records (S001 to S012) and audit records (A001 to A004) in the CVI Demo Lab. These records involve the accounts for Alex, Blair, Casey and Drew, the CaseDesk application along with the simulated Active Directory and Cloud Identity environment. The time window is September 21, 2026, from 13:00 to 14:25 UTC, which is the span of the records provided. I reviewed Drew’s single failed sign-in to a disabled account (S012) but it did not become a finding. The investigation also covered one live simulator chain from my earlier work (LA-34C82D-43-1, AT-34C82D-44-1, LA-34C82D-45-1 and AT-34C82D-46-1).b. The live simulator chain is kept separate from the historical records. The records do not include any activity before or after that window, any real people or systems, or what any account did after these events so these items were not covered.

## 3. Evidence reviewed
The following records were reviewed:

Historical sign-in log (September 21): events S001 to S012, showing Alex, Blair, Casey and Drew, with fields for time, account, application, IP address, device, location, outcome, reason, authentication method, MFA detail and correlation ID.

Historical audit log (September 21): events A001 to A004, showing group changes and a refused password reset request, with fields for actor, activity, target, before and after values, result, approval ticket and correlation ID.

Raw records: the raw record for S009 and A002 was opened to confirm the exact fields and the shared correlation ID C009.

Live simulator records from my earlier work: the audit entries and access tests in one chain (LA-34C82D-43-1, AT-34C82D-44-1, LA-34C82D-45-1 and AT-34C82D-46-1), kept separate from the historical records.

Captured evidence: saved captures from the labs, including E17 (the two findings with correlation), E18 (the benign comparison) and E19 (the completed case file and versioned export) and my Week 11 Case File.

Evidence IDs (EV-…) and event IDs (S…, A…, C…) refer to records in my Week 11 evidence index; full evidence is not duplicated here.

## 4. Timeline
13:00:00 - S001 - alex@cvi-training.example signed in to CaseDesk from Office-A (192.0.2.10) on managed-alex with password plus MFA; success.

13:01:00 S002 blair@cvi-training.example failed to sign in from Office-A (192.0.2.11) on managed-blair; reason incorrect_password.

13:02:00 S003 blair@cvi-training.example signed in from the same IP and device with password plus MFA; success.

13:10:00 A001 helpdesk@training.example added Blair to Junior-Analysts under ticket CHG-1101; success.
14:00:00 — S004 — casey@cvi-training.example failed to sign in from unknown-device (203.0.113.77, Remote-B); incorrect_password.

14:00:30 S005 same account, device and IP; incorrect_password.

14:01:00 S006 same account, device and IP; incorrect_password.

14:01:30 S007 same account, device and IP; incorrect_password.

14:02:00 S008 same account, device and IP; incorrect_password.

14:03:00 S009 casey@cvi-training.example signed in successfully from the same IP and device using password only; no MFA challenge recorded; correlation ID C009.

14:04:00 A002 casey@cvi-training.example added itself to Privileged-Operators; no approval ticket recorded; correlation ID C009.

14:10:00 S010 alex@cvi-training.example signed in from Office-A (192.0.2.10) on managed-alex using an existing session.

14:12:00 S011 alex@cvitraining.example signed in from Remote-C (198.51.100.20) on managed-alex using an existing session.

14:15:00 A003 helpdesk@training.example removed Alex from Old-Response-Team under ticket CHG-1102; success.

14:20:00 S012 drew@cvi-training.example failed to sign in from unknown-device (203.0.113.88, Remote-B); reason account_disabled.

14:25:00 — A004 — helpdesk@training.example’s password reset request for Blair was refused under REQ-1103.

## 5. Findings
### Finding 1 (RF1) — confidence: likely

The logs show five failed sign-ins for casey@cvi-training.example from an unknown-device in Remote-B (S004 to S008), followed by a password-only successful sign in with no MFA challenge recorded (S009), then one minute later the account added itself to Privileged-Operators with no approval ticket (A002). The logs do not show who was using the account.

- Evidence / events: S004, S005, S006, S007, S008, S009, A002
- Uncertainty: The logs don't show who was at the keyboard for S004 to S009 or if the person was the real Casey. They do not show why no MFA challenge was recorded for S009: MFA may not have been required or it may not have been logged. They do not show whether Casey’s account was locked out or reset before the successful sign-in. Lastly, the logs do not show what Casey did with Privileged-Operators after A002 or if anyone reviewed or approved the change afterwards.

### Finding 2 (RF2) — confidence: possible

The logs show alex@cvi-training.example signing in twice on an existing session, first from Office-A (S010) and then two minutes later (S011) from Remote-C. Both sign ins are on managed-alex. The logs do not show if the Remote-C sign-in was Alex or why the location changed.

- Evidence / events: S010, S011, A003
- Uncertainty: The logs do not show if the Remote-C sign-in (S011) was Alex or why the location changed. They do not show if Alex used a VPN, if the session was shared or how the session reached 198.51.100.20. They do not show why the second sign-in came from a different IP while the device stayed managed-alex. Lastly,the logs do not show if A003 had any connection to the sign-ins or what access Alex lost beyond Old-Response-Team.

## 6. Impact / risk
If this activity was unauthorized someone could use Casey’s account to get whatever access the Privileged-Operators group gives. The September 21 records show that Casey’s account joined this group (A002) but do not say what the group allows, so it is unknown how far the access reaches. People who depend on the systems and records that group can reach could be affected. The seriousness is High for Casey’s account because the records show repeated failed sign-ins, a password-only success with no MFA challenge recorded and a self-added privileged group with no approval ticket (S004 to S009, A002). The records do not show who was using the account or what it did after A002 so the actual impact is unknown.

The seriousness is for Alex's account is Medium. A sign in from second location two minutes after the first sign in on an existing session and a managed device, could allow someone to use a session that was not theirs but the records show no failed attempts, no missing MFA and there is no privilege change tied to it (S010, S011). Alex's access was reduced with the ticketed removal of Old-Response-Team (A003) and it is not a risk on its own.

## 7. Recommendations / next actions
### Recommendation 1 (R1)

- **What:** The security team will contact Casey through a trusted channel, such as the phone number on the HR within 1 business day to confirm if she made the sign-ins from 203.0.113.77 and added her account to Privileged-Operators. If she did not, or if no approval exists for the membership, the helpdesk will remove the account from Privileged-Operators and the credential will be reset after her identity is verified.
- **Who:** Security team lead, with the helpdesk lead for the membership removal.
- **Why:** The risk that someone other than Casey is using an account that gave itself privileged access with no approval is reduced (S004 to S009, A002).
- **Priority:** high
- **Timeframe:** Within 1 business day
- **Linked findings:** RF1
- **Success measure:** Once Casey confirms or denies the activity, the Privileged-Operators membership is approved with a ticket or removed and the outcome is recorded in the ticket.

### Recommendation 2 (R2)

- **What:** The security team will contact Alex through a trusted channel, such as the phone number on the HR record, within 2 business days to confirm if the Remote-C sign-in (S011) was Alex and if a VPN or documented remote-access method explains it. The removal from Old-Response-Team (A003, ticket CHG-1102) was routine cleanup will also be confirmed with the helpdesk.
- **Who:** Security Team Lead
- **Why:** The risk that someone other than Alex is using an existing session from a second location (S010, S011) is reduced.
- **Priority:** medium
- **Timeframe:** Within 2 business days
- **Linked findings:** RF2
- **Success measure:** Once Alex has confirmed or denied the Remote-C sign-in, the helpdesk has confirmed the purpose of A003, and both answers have been recorded.

## 8. Limitations / unknowns
This Case File is based on a simulated training environment and every record in it is fabricated or generated by the simulator and this limits the investigation. The accounts, people, IP addresses, devices, tickets and approvals are fictional, so the case file does not identify real people and cannot confirm a real compromise or real misuse.

The historical records (S001 to S012 and A001 to A004) are a small, fixed set from one day, September 21. They do not show what happened before or after that day, so they cannot show patterns over time. Correlation IDs link only one sign-in and audit pair (C009), so other records cannot be tied together by ID.

The records show what the accounts did, not who was using them. The logs cannot show a person’s intent, if a password was guessed or known or why a sign-in came from a different location. Several fields, such as MFA detail only show what the simulator recorded and I cannot tell if a missing entry means the control was not required or if it was not logged.

My live records (LA-, AT- and LS- IDs) come from tasks that I ran in the simulator and they are separate from the historical records. The simulator has no automatic synchronization between Active Directory and Cloud Identity and its access tests check one action on one resource at a time. Some saved captures, such as E01 to E04 and E08, show the state at the time they were saved and later changes do not rewrite them. I also ran some tests more than once so the evidence includes repeated results.

The recommendations in this Case File are preliminary and rest only on the records listed.

## 9. Conclusion
Based on the available records, the strongest warning pattern shows on Casey's account: five failed password sign-ins from an unknown device, a password-only success with no MFA challenge recorded along with the account adding itself to Privileged-Operators with no approval ticket (S004 to S009, A002). The second-location sign-in on Alex's is worth confirming but has a plausible ordinary explanation and Blair’s single failed sign-in looks like it could be an ordinary typo. The records cannot show who was using any of these accounts or why so these are findings that need to be verified and not confirmed misuse. The actions are listed in Section 7.
