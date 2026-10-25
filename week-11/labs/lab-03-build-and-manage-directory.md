# Lab 03 — Build and Manage the Directory

**Learner:** N. Williams  
**Attempt:** attempt 34C82D · seed chic-w11-v1 · state revision 134 · text revision 378

> Training simulation — fictional Cloud Heights accounts and resources. Your actions change only your Week 11 learning attempt. This is not a live AD domain, Microsoft Entra tenant or Azure subscription.

## Objective

Create Jamie's AD account, place it, grant basic group access, and verify, reset and enable it safely.

## My answers

### What did OU placement change — and not change?

When ad-jamie was placed in Support (OU-SUP) the account was under the Support area of the directory. It did not give Jamie any access. Jamie's access came later, from group membership.

### What did group membership change?

When ad-jamie was added to All-Staff and Support-Team it changed what Jamie could do. All-Staff gave read on the Staff Handbook, which was shown in the access table and in the ALLOW test. Support-Team gave nothing by itself because it has no resource entry.

### What did the credential reset change? Why did verification come first?

The reset under T302 changed the credential version from 1 to 2 and no password was set or shown. Next at Jaime's first sign in a new credential was set (version 2 to 3). The verification came first because a reset is a sensitive action and someone pretending to be Jamie could take over the account. The callback evidence on the ticket (CB-302) showed matching identity details and before the reset occurred, the identity verification passed.

### What did disabling and re-enabling do to sessions and access?

When ad-Jamie was disabled the one open session was revoked and sign in was blocked with ACCOUNT_DISABLED. The Staff Handbook test was denied showing the reason as ‘The target account is disabled,’ even though the groups never changed. Re-enabling ad-Jamie restored access but did not bring back the old session. Jamie had to sign in again and got a new session, which was SES-34C82D-23.

## Evidence

- **E05 · EV-34C82D-007** — Created account with OU and groups (captured at revision 13)  
  _What this proves:_ This snapshot shows that ad-jamie, which is Jamie Ellis's Active Directory account, was placed in the Support OU (OU-SUP). Jamie's account is a member of the All-Staff and Support-Team groups and the status is disabled.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-007)
- **E06 · EV-34C82D-008** — Passed verification + reset version change (captured at revision 13)  
  _What this proves:_ The records show that the identity verification for T302 passed and afterwards a credential reset was accepted for ad-jamie under T302. The reset changed the credential version from 1 to 2, set the account to change its credential at next sign-in and no password was set or shown.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-008)
- **E07 · EV-34C82D-009** — Disable / re-enable / session validation sequence (captured at revision 24)  
  _What this proves:_ The records show ad-jamie as being disabled under T301, a sign-in refused because the account was disabled and a Staff Handbook read test denied for the same reason. The records then show the account as re-enabled under T301, a new sign-in succeeding in session SES-34C82D-23 and the same read test allowed.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-009)
- **E05 · EV-34C82D-010** — Created account with OU and groups (captured at revision 24)  
  _What this proves:_ This snapshot shows that ad-jamie, which is Jamie Ellis's Active Directory account, was placed in the Support OU (OU-SUP). Jamie's account is a member of the All-Staff and Support-Team groups and the status is disabled.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-010)
- **E05 · EV-34C82D-011** — Created account with OU and groups (captured at revision 24)  
  _What this proves:_ This snapshot shows that ad-jamie, which is Jamie Ellis's Active Directory account, was placed in the Support OU (OU-SUP). Jamie's account is a member of the All-Staff and Support-Team groups and the status is disabled.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-011)
