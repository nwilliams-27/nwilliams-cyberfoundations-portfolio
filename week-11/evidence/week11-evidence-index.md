# Week 11 evidence index

Simulator captures from attempt 34C82D. Hash detects content changes; it is not proof of live AD administration.

## EV-34C82D-001

- Slot E01 · M01 · revision 0
- Directory trace: Alex person, both accounts, OU, one AD grant path, one cloud role path
- SHA-256 2ffc4a88aa8239ce91515c8b5db0003b29a9ce6323c3dded8876b528520d83a5
- Caption: The records show one that person, Alex Morgan, has two separate accounts: ad-alex in the Active Directory and cl-alex in the cloud identity directory. Both accounts are showing as being enabled.

## EV-34C82D-002

- Slot E01 · M01 · revision 0
- Directory trace: Alex person, both accounts, OU, one AD grant path, one cloud role path
- SHA-256 2ffc4a88aa8239ce91515c8b5db0003b29a9ce6323c3dded8876b528520d83a5
- Caption: The records show one person, which is Alex Morgan with two accounts: ad-alex in the Active Directory and cl-alex in the cloud identity directory. The status of both accounts show them as being enabled.

## EV-34C82D-003

- Slot E02 · M01 · revision 0
- Comparison of Blair, Finley and Drew
- SHA-256 1831331695c963eefe026e966522f5d90e0d7fdc0acc79ff8cf9c97134b87e4c
- Caption: cl-blair is in Junior-Analysts, it grants read on Analyst Landing and cl-finley is in Case-Readers, it grants read on Case Summaries and both accounts are enabled. Drew’s accounts, ad-drew and cl-drew, are disabled with no groups and no effective access.

## EV-34C82D-004

- Slot E03 · M02 · revision 3
- Successful sign-in + the access result before your change
- SHA-256 a8fff59f597fae168e957a0f7ea16871195e3b1ee3eab32c26dd214edd1ae206
- Caption: Blair was able to sign in successfully. Blair was denied access when trying to read Case Summaries.

## EV-34C82D-005

- Slot E04 · M02 · revision 7
- Membership/access change audit + read result + update result
- SHA-256 c383a710b88325eb50b01fc200368038e312d04344024654a4d97e058652bb31
- Caption: Before the change, read on Case Summaries was denied (AT-34C82D-3-1) because Junior-Analysts, which is Blair’s only group, had no path to it. After adding Blair to Case-Readers under T201, read was allowed (AT-34C82D-6-1) through Case-Readers → Case Reader → R-CASE and update was denied (AT-34C82D-7-1) with no matching path. Blair now has the read access the ticket approved and no update rights.

## EV-34C82D-006

- Slot E03 · M02 · revision 8
- Successful sign-in + the access result before your change
- SHA-256 38d2072f3677eb7c1b01480acbbf1df18aa95303dd9e372529d9edc67c549530
- Caption: Before the change I ran access test AT-34C82D-3-1 for cl-blair on Case Summaries (read). It was denied: the sign-in succeeded but no matching grant allowed it. Blair’s only group was Junior-Analysts, it gave read on Analyst Landing (test AT-34C82D-2-1, allowed) and nothing for Case Summaries.

## EV-34C82D-007

- Slot E05 · M03 · revision 13
- Created account with OU and groups
- SHA-256 56ab48e50bbeeac7a70eb2050a3e6ff65bd800324bd48a2bbe1053a1744550dd
- Caption: This snapshot shows that ad-jamie, which is Jamie Ellis's Active Directory account, was placed in the Support OU (OU-SUP). Jamie's account is a member of the All-Staff and Support-Team groups and the status is disabled.

## EV-34C82D-008

- Slot E06 · M03 · revision 13
- Passed verification + reset version change
- SHA-256 0d664cd53a4306d8177bb8357e8ca09adce5d771beaf14a71a0b8d3ee5b67bfd
- Caption: The records show that the identity verification for T302 passed and afterwards a credential reset was accepted for ad-jamie under T302. The reset changed the credential version from 1 to 2, set the account to change its credential at next sign-in and no password was set or shown.

## EV-34C82D-009

- Slot E07 · M03 · revision 24
- Disable / re-enable / session validation sequence
- SHA-256 b14a60588881825cdc999bfe41abf7ec0706a36517c6dc40bfd522269d5435de
- Caption: The records show ad-jamie as being disabled under T301, a sign-in refused because the account was disabled and a Staff Handbook read test denied for the same reason. The records then show the account as re-enabled under T301, a new sign-in succeeding in session SES-34C82D-23 and the same read test allowed.

## EV-34C82D-010

- Slot E05 · M03 · revision 24
- Created account with OU and groups
- SHA-256 7ebdf744951ee10ebaae566abc10e0addbd2bf648a12239206fc31597e41b9d0
- Caption: This snapshot shows that ad-jamie, which is Jamie Ellis's Active Directory account, was placed in the Support OU (OU-SUP). Jamie's account is a member of the All-Staff and Support-Team groups and the status is disabled.

## EV-34C82D-011

- Slot E05 · M03 · revision 24
- Created account with OU and groups
- SHA-256 7ebdf744951ee10ebaae566abc10e0addbd2bf648a12239206fc31597e41b9d0
- Caption: This snapshot shows that ad-jamie, which is Jamie Ellis's Active Directory account, was placed in the Support OU (OU-SUP). Jamie's account is a member of the All-Staff and Support-Team groups and the status is disabled.

## EV-34C82D-012

- Slot E08 · M04 · revision 29
- Three job configuration snapshots with rationales
- SHA-256 7d983280a2313030de6ef46c987566a80ef275704341516c410e91a52599c133
- Caption: The snapshots show cl-blair in Junior-Analysts and Case-Readers with read access to Analyst Landing and Case Summaries, cl-casey in Privileged-Operators with broad access and cl-emi with no groups and no access. The audit log shows configuration reviews of Junior-Analysts, Response-Operators and Audit-Reviewers with no changes made to either.

## EV-34C82D-013

- Slot E09 · M04 · revision 42
- The nine required access tests
- SHA-256 715d83e1f3f7db9095ab642b79d071ab2713e5e77421b22dd588219638716582
- Caption: cl-blair is allowed read access and denied update access on Case Summaries. cl-casey is allowed read and update access on the Response Queue and delete access on Audit Evidence and is denied manage_sensitive_groups in the Directory Administration Sandbox. cl-emi is denied read, export and delete access on Audit Evidence.

## EV-34C82D-014

- Slot E10 · M04 · revision 46
- Temporary grant → allowed test → revoke → denied retest
- SHA-256 073b896f01846d65f16996ec087ac30e0cffbf82ecd3ed9700160e764ac530c0
- Caption: The audits show cl-blair was added to a group and later removed from it. Both changes were accepted under T401. The tests show cl-blair was allowed to read Audit Evidence while the group membership existed and denied after the group membership was removed.

## EV-34C82D-015

- Slot E11 · M04 · revision 55
- Four ABAC cases (current + three what-ifs)
- SHA-256 b887f70bbd9f1ba89260d0ca9445a8619f5ab090fd6880aa0db7ad6ffbdb98fd
- Caption: The current-state test for Casey's Internal Response Note (read) was DENY with no matching path and the three what-if tests (device trust, department and classification overrides) were also DENY with no matching path. All four tests returned the same decision with the same reason which is, no matching grant permits the action.

## EV-34C82D-016

- Slot E12 · M05 · revision 80
- Jamie joiner chain
- SHA-256 7e8e9f41a6ecbdc30454f490573f2e0de0c1f26dc9fd407304b4fa6d9befed0c
- Caption: A cloud account was created for Jamie, it was protected with MFA, enabled and added to two groups (Junior-Analysts & Case-Readers). All of these changes were accepted under T501. Jamie's access tests show that the read access for Case Summaries was ALLOW and update access was DENY, which means that Jaimie can read Case Summaries but cannot update them.

## EV-34C82D-017

- Slot E13 · M05 · revision 85
- Alex before/after access and changes
- SHA-256 520ce49335825fe561b7bb4e96ba35553973314bea2fac6e7bcd1b7eceab0477
- Caption: Before the transfer occurs, ad-alex (Active Directory) and cl-alex (Cloud Identity) are in the Response department, with ad-alex in All-Staff and Response-Team and cl-alex in Response-Operators. ad-alex was allowed read access to the Response Share and cl-alex was allowed read access the Response Queue.

## EV-34C82D-018

- Slot E13 · M05 · revision 98
- Alex before/after access and changes
- SHA-256 f68dd3dd2469385a7b57210e93c7dbe86ec50bdb363b42403b5ee5a7e9d332ba
- Caption: Under T502, Alex’s department was changed from Response to Audit on the person record, ad-alex and cl-alex. ad-alex was moved to OU-AUD, the old Response memberships were removed and audit memberships were added in both directories. Before the transfer Alex had read access to Response Share and Response Queue. After the transfer, Alex was denied read access to Response Share and Response Queue and allowed read access on Audit Share, Audit Evidence and Staff Handbook.

## EV-34C82D-019

- Slot E13 · M05 · revision 98
- Alex before/after access and changes
- SHA-256 f68dd3dd2469385a7b57210e93c7dbe86ec50bdb363b42403b5ee5a7e9d332ba
- Caption: Under T502, Alex’s department was changed from Response to Audit on the person record, ad-alex and cl-alex. ad-alex was moved to OU-AUD, the old Response memberships were removed and audit memberships were added in both directories. Before the transfer Alex had read access to Response Share and Response Queue. After the transfer, Alex was denied read access to Response Share and Response Queue and allowed read access on Audit Share, Audit Evidence and Staff Handbook.

## EV-34C82D-020

- Slot E14 · M05 · revision 111
- Finley session/status/revocation evidence
- SHA-256 f74af58f6809e9a5c95f5ce2ab34aa7e154a8ad42730379171876e2288401a28
- Caption: Both of Finley’s accounts, ad-finley and cl-finley are disabled. The audits show that each account was disabled and had its sessions revoked under T503. Using the old session, Finley’s Case Summaries read access was denied because the account is disabled and a new sign-in attempt failed for the same reason.

## EV-34C82D-021

- Slot E15 · M05 · revision 114
- Failed reset verification, refusal and escalation
- SHA-256 485ac4732e0aafb4059430c505b7f51cf0f2baeb06ce8dc2911f7fe1b1023907
- Caption: The verification audit shows that the identity check for cl-blair failed and was refused under T504, using request-only details. The ticket-status audit shows that T504 was escalated, therefore no reset was approved.

## EV-34C82D-022

- Slot E16 · M05 · revision 121
- AD DS – Entra – Azure comparison and scoped tests
- SHA-256 d281faae19390ef9e6a86c19bf1be8e4e2edf8a8f45d3aaebb340ced05572ede
- Caption: Azure Reader was given to Blair's cloud account at RG-LAB through Training-Resource-Readers under T505. Blair has read access to the Training-VM’s metadata, cannot stop it and cannot read the Other-Team-VM’s metadata.

## EV-34C82D-023

- Slot E13 · M05 · revision 130
- Alex before/after access and changes
- SHA-256 7492160190505874e229fabef5af45c71fa43da228bbd7c76fe4d20af841f0ac
- Caption: Before the transfer occurs, ad-alex (Active Directory) and cl-alex (Cloud Identity) are in the Response department, with ad-alex in All-Staff and Response-Team and cl-alex in Response-Operators. ad-alex was allowed read access to the Response Share and cl-alex was allowed read access the Response Queue.

## EV-34C82D-024

- Slot E17 · M06 · revision 133
- Two distinct findings with raw-event references and correlation
- SHA-256 015164d6369930c03958d9542762cd539dbd075af26238208f41a2d3f9ddab68
- Caption: The selected sign-in records show five failed password sign-ins for Casey from unknown-device in Remote-B (S004 to S008) followed by a password-only successful sign in with no MFA challenge recorded (S009) and two sign-ins for Alex from different locations two minutes apart (S010 and S011). The audit records show that Casey added herself to Privileged-Operators with no ticket (A002, sharing correlation ID C009 with S009) and a ticketed removal of Alex from Old-Response-Team that has no correlation ID (A003).

## EV-34C82D-025

- Slot E18 · M06 · revision 133
- Benign/ambiguous comparison
- SHA-256 807718b8ef37c95f1c6d41c8623aa9c41e55f361bb32e1861c0b6a7bb5d8cddd
- Caption: There was one failed sign-in on Blair's account with an incorrect password (S002), followed one minute later by a successful sign-in with password plus MFA from the same IP and managed device in Office-A (S003). The audit record shows that the helpdesk added Blair to Junior-Analysts under approval ticket CHG-1101 (A001), which has no correlation ID.

## EV-34C82D-026

- Slot E19 · M06 · revision 133
- Completed case file / versioned export
- SHA-256 357a3d03b702bab1abaf21b6b58b5744b8c91f14e460c686a4731a1626749119
- Caption: The selected sign-in records show five failed password sign-ins for Casey from an unknown-device in Remote-B (S004 to S008) followed by a password-only success with no MFA challenge recorded (S009), two sign ins for Alex from different locations two minutes apart (S010 and S011) and Blair’s single failed sign-in followed by a successful sign in with MFA (S002 and S003). The selected audit records show Casey adding herself to Privileged-Operators with no ticket (A002, sharing correlation ID C009 with S009), a ticketed removal of Alex from Old-Response-Team (A003) and a ticketed group add for Blair (A001).

## EV-34C82D-027

- Slot E13 · M05 · revision 134
- Alex before/after access and changes
- SHA-256 ce4f4d4c35ab4bb142299f527302af43743fae6fe987dc9d2232cc96c51b418d
- Caption: Under T502, Alex’s department was changed from Response to Audit on the person record, ad-alex and cl-alex. ad-alex was moved to OU-AUD, the old Response memberships were removed and audit memberships were added in both directories. Before the transfer Alex had read access to Response Share and Response Queue. After the transfer, Alex was denied read access to Response Share and Response Queue and allowed read access on Audit Share, Audit Evidence and Staff Handbook. Export access was also allowed in Audit Evidence.
