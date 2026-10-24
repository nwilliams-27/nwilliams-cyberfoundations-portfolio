# Lab 05 — Joiner, Mover, Leaver

**Learner:** N. Williams  
**Attempt:** attempt 34C82D · seed chic-w11-v1 · state revision 133 · text revision 376

> Training simulation — fictional Cloud Heights accounts and resources. Your actions change only your Week 11 learning attempt. This is not a live AD domain, Microsoft Entra tenant or Azure subscription.

> **DRAFT — incomplete.** Missing:
> - T502: run Alex's after-transfer tests in both directories.

## Objective

Complete four lifecycle tickets across the two directory surfaces, plus a short Azure scope comparison.

## My answers

### T501 decision table — Add / Retain / Remove / Deny / Escalate (write 'not applicable — reason' where needed)

Add

I created the cloud account cl-Jamie for Jamie under T501. I turned on MFA and enabled the account. Next, I added Jamie to Junior-Analysts and Case-Readers. Jamie now has read access to Analyst Landing and read access to Case Summaries.

Retain

T501 says keep ad-Jamie, which is Jamie’s existing Active Directory account. I did not make any changes to it for this particular lab.

Remove

'Not applicable.' This does not apply here because T501 because  approved adding access, therefore I did not remove any access.

Deny

Jamie does not have update access to Case Summaries. Due to APP-501 approving read-only access, the test was denied (AT-34C82D-80-1). Read access was allowed (AT-34C82D-79-1).

Escalate

'Not applicable.' This does not apply here because the identity verification passed using the callback evidence (CB-501) and HR ID CH-008, so there was nothing to escalate.

### T502 decision table — Add / Retain / Remove / Deny / Escalate

Add

Audit access was added for Alex under T502. In the Active Directory. ad-alex was added to Audit-Team (LC-34C82D-89), which gives read access to the Audit Share. In Cloud Identity cl-alex was added to Audit-Reviewers (LC-34C82D-95), which gives read access and export access to Audit Evidence.

Retain

Alex’s access to handbook was kept. ad-alex remained in All-Staff and a test showed that read access to the Staff Handbook is still allowed (AT-34C82D-93-1). In addition, Alex’s department was changed to Audit on the person record, ad-alex and cl-alex (LC-34C82D-86-1, 86-2 and 86-3). Lastly, ad-alex was moved to the Audit OU (LC-34C82D-87).

Remove

Alex’s old Response access was removed. In the Active Directory, I removed ad-alex from Response-Team (LC-34C82D-88). In Cloud Identity, I removed cl-alex from Response-Operators (LC-34C82D-94). Afterwards, the tests show that read access to the Response Share is denied (AT-34C82D-91-1) and read access to the Response Queue is denied (AT-34C82D-97-1).

Deny

APP-502 says Response operational access should end, therefore Alex no longer has any Response access. Also, Alex has no delete access to Audit Evidence because T502 approves audit access and nothing else.

Escalate

'Not applicable.' There was nothing to escalate because HR requested the transfer, the HR / Audit lead approved it and HR-502 and APP-502 support it.

### T503 decision table — Add / Retain / Remove / Deny / Escalate

Add

Not applicable. T503 only approved ending access, therefore I did not add any access or any groups.

Retain

As T503 required, I kept all records and logs and did not delete anything. ad-finley and cl-finley still exist but the status is disabled and Finley’s person record (P06) is still there with links to both accounts. ad-finley is now in Disabled Accounts.

Remove

Finley’s access in both directories was removed under T503. In Active Directory, I disabled ad-finley (LC-34C82D-102), revoked sessions (LC-34C82D-103), removed All-Staff (LC-34C82D-104) and Support-Team (LC-34C82D-105) and moved the account to Disabled Accounts (LC-34C82D-109). In Cloud Identity, I disabled cl-finley (LC-34C82D-106), revoked sessions (LC-34C82D-107) and removed Case-Readers (LC-34C82D-108).

Deny

Finley is now denied everything. The old session was denied Case Summaries read access because the account is disabled (AT-34C82D-110-1) and a new sign-in failed with ACCOUNT_DISABLED (LC-34C82D-111). Before the I made the change, the same read access was allowed (AT-34C82D-101-1).

Escalate

Not applicable. There was nothing to escalate because HR requested and approved the termination was and it's supported by HR-503.

### T504 decision table — Add / Retain / Remove / Deny / Escalate

Add

Not applicable because T504 did not approve any access, so I did not add anything.

Retain

Blair’s account was kept exactly as it was. cl-blair is still enabled, it is still in Junior-Analysts and Case-Readers, with credential version 1 and MFA enrolled. The audit line for the reset request shows that the account is unchanged.

Remove

Not applicable because nothing needed to be removed. I did not reset the credential or change any of Blair's access.

Deny

I denied the password reset. T504 had no reset approval and no approver. The caller claimed employee ID CH-099, but the HR record shows that Blair Chen (P02) has CH-002. The callback number came from the caller and CB-504 says no callback to the HR-recorded number was made. The verification failed because request-only details are not trusted verification (LC-34C82D-113) and the reset request was refused.

Escalate

T504 was set to escalate (LC-34C82D-114) because the request could not be verified. Someone should contact Blair through the HR-recorded number and review the unverified request.

### Why is an Azure resource role/scope different from a cloud directory role?

An Azure resource role is different from a cloud directory role because it controls actions on cloud resources, such as virtual machines and it only works at the scope where it is assigned. A cloud directory role controls what someone can do inside an app or directory, such as reading Case Summaries and it applies to app resources, not Azure resources.

In this lab, Azure Reader gave Blair read_metadata on the Training-VM only because it was assigned at RG-LAB. Blair could not stop the VM and she was denied on the Other-Team-VM (AT-34C82D-121-1) because the assignment only covers RG-LAB. A directory role like Case Reader gave her read access to Case Summaries.

## Evidence

- **E12 · EV-34C82D-016** — Jamie joiner chain (captured at revision 80)  
  _What this proves:_ A cloud account was created for Jamie, it was protected with MFA, enabled and added to two groups (Junior-Analysts & Case-Readers). All of these changes were accepted under T501. Jamie's access tests show that the read access for Case Summaries was ALLOW and update access was DENY, which means that Jaimie can read Case Summaries but cannot update them.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-016)
- **E13 · EV-34C82D-017** — Alex before/after access and changes (captured at revision 85)  
  _What this proves:_ Before the transfer occurs, ad-alex (Active Directory) and cl-alex (Cloud Identity) are in the Response department, with ad-alex in All-Staff and Response-Team and cl-alex in Response-Operators. ad-alex was allowed read access to the Response Share and cl-alex was allowed read access the Response Queue.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-017)
- **E13 · EV-34C82D-018** — Alex before/after access and changes (captured at revision 98)  
  _What this proves:_ Under T502, Alex’s department was changed from Response to Audit on the person record, ad-alex and cl-alex. ad-alex was moved to OU-AUD, the old Response memberships were removed and audit memberships were added in both directories. Before the transfer Alex had read access to Response Share and Response Queue. After the transfer, Alex was denied read access to Response Share and Response Queue and allowed read access on Audit Share, Audit Evidence and Staff Handbook.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-018)
- **E13 · EV-34C82D-019** — Alex before/after access and changes (captured at revision 98)  
  _What this proves:_ Under T502, Alex’s department was changed from Response to Audit on the person record, ad-alex and cl-alex. ad-alex was moved to OU-AUD, the old Response memberships were removed and audit memberships were added in both directories. Before the transfer Alex had read access to Response Share and Response Queue. After the transfer, Alex was denied read access to Response Share and Response Queue and allowed read access on Audit Share, Audit Evidence and Staff Handbook.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-019)
- **E14 · EV-34C82D-020** — Finley session/status/revocation evidence (captured at revision 111)  
  _What this proves:_ Both of Finley’s accounts, ad-finley and cl-finley are disabled. The audits show that each account was disabled and had its sessions revoked under T503. Using the old session, Finley’s Case Summaries read access was denied because the account is disabled and a new sign-in attempt failed for the same reason.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-020)
- **E15 · EV-34C82D-021** — Failed reset verification, refusal and escalation (captured at revision 114)  
  _What this proves:_ The verification audit shows that the identity check for cl-blair failed and was refused under T504, using request-only details. The ticket-status audit shows that T504 was escalated, therefore no reset was approved.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-021)
- **E16 · EV-34C82D-022** — AD DS – Entra – Azure comparison and scoped tests (captured at revision 121)  
  _What this proves:_ Azure Reader was given to Blair's cloud account at RG-LAB through Training-Resource-Readers under T505. Blair has read access to the Training-VM’s metadata, cannot stop it and cannot read the Other-Team-VM’s metadata.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-022)
- **E13 · EV-34C82D-023** — Alex before/after access and changes (captured at revision 130)  
  _What this proves:_ Before the transfer occurs, ad-alex (Active Directory) and cl-alex (Cloud Identity) are in the Response department, with ad-alex in All-Staff and Response-Team and cl-alex in Response-Operators. ad-alex was allowed read access to the Response Share and cl-alex was allowed read access the Response Queue.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-023)
