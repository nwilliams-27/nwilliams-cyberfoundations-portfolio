# Lab 02 — Authentication vs. Authorization: A Valid Badge Doesn't Open Every Door

**Learner:** N. Williams  
**Attempt:** attempt 34C82D · seed chic-w11-v1 · state revision 133 · text revision 376

> Training simulation — fictional Cloud Heights accounts and resources. Your actions change only your Week 11 learning attempt. This is not a live AD domain, Microsoft Entra tenant or Azure subscription.

## Objective

Diagnose and repair Blair's missing case access without granting update rights.

## My answers

### Prediction before testing: will Blair be able to read cases? Why?

I predict that Blair will be denied read access on Case Summaries. Blair can sign in but Blair's only group, Junior Analysts gives access to Analyst Landing and not to Case Summaries. Case Summaries supports read, update and delete. Blair only needs read. I predict that by adding Blair to Case-Readers, Blair would be able to read Case Summaries but not update because access to Care-Readers is read only.

### Identification — using the actual account

Blair’s account is listed as cl-blair in the cloud directory. In the HR Record it belongs to person P02 in the Support department. The account name tells the system which account is acting.

### Authentication — using your sign-in event

I ran the sign-in test for Blair. The device was managed, the credential was current, and MFA passed. Blair’s sign-in works, which matches the ticket. This means that the problem is not signing in.

### Authorization — using your access tests

Before the change I ran access test AT-34C82D-3-1 for cl-blair on Case Summaries (read). It was denied: the sign-in succeeded but no matching grant allowed it. Blair’s only group was Junior-Analysts, it gave read on Analyst Landing (test AT-34C82D-2-1, allowed) and nothing for Case Summaries.

### Accountability — using the audit record

The audit log is showing that sim-junior-analyst added cl-blair to GC-READ (Case-Readers). Blair’s groups changed from GC-JUN to GC-JUN and GC-READ. The change was accepted and linked to ticket T201. This means the clinic can show who changed Blair’s access, what changed and why.

## Evidence

- **E03 · EV-34C82D-004** — Successful sign-in + the access result before your change (captured at revision 3)  
  _What this proves:_ Blair was able to sign in successfully. Blair was denied access when trying to read Case Summaries.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-004)
- **E04 · EV-34C82D-005** — Membership/access change audit + read result + update result (captured at revision 7)  
  _What this proves:_ Before the change, read on Case Summaries was denied (AT-34C82D-3-1) because Junior-Analysts, which is Blair’s only group, had no path to it. After adding Blair to Case-Readers under T201, read was allowed (AT-34C82D-6-1) through Case-Readers → Case Reader → R-CASE and update was denied (AT-34C82D-7-1) with no matching path. Blair now has the read access the ticket approved and no update rights.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-005)
- **E03 · EV-34C82D-006** — Successful sign-in + the access result before your change (captured at revision 8)  
  _What this proves:_ Before the change I ran access test AT-34C82D-3-1 for cl-blair on Case Summaries (read). It was denied: the sign-in succeeded but no matching grant allowed it. Blair’s only group was Junior-Analysts, it gave read on Analyst Landing (test AT-34C82D-2-1, allowed) and nothing for Case Summaries.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-006)
