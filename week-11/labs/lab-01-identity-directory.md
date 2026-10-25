# Lab 01 — Identity Directory: Who Is Who?

**Learner:** N. Williams  
**Attempt:** attempt 34C82D · seed chic-w11-v1 · state revision 134 · text revision 378

> Training simulation — fictional Cloud Heights accounts and resources. Your actions change only your Week 11 learning attempt. This is not a live AD domain, Microsoft Entra tenant or Azure subscription.

## Objective

Investigate why one person may have different access in different identity systems.

## My answers

### Difference 1 between the AD and cloud views (what you saw, and why it matters)

In the AD, the access is coming from a group that is listed on the resource, for example Response-Team (GA-RESP) → AD ACL → Response Share (read, update). In the cloud, access is coming through a group and then a role at a scope, for example Response-Operators (GA-RESP) → RL-RESP @ R-QUEUE → Response Queue. This matters because access has to be traced differently in each directory. If I only checked the AD, I could miss what the cloud grants and if I only checked the cloud I would miss what the AD grants.

### Difference 2 between the AD and cloud views

The AD has OU's and the cloud doesn't. ad-alex is in OU-RESP but cl-alex does not show an OU. The OU is only used to organize accounts, it grants nothing. Access comes from groups in both.

### What do Junior-Analysts and Case-Readers actually grant? How did you check?

The Junior -Analysts (GA-JUN) grants read access on Analyst-Landing. The case Readers (GC-READ) grants read access on Case Summaries. Both are read-nly. Even though both are read-only, they reach different resources. I checked the Grants line on the Group page for each group and compared it to a member's effective access (cl-blair and cl-finley).

### Why does Drew's person record still exist?

Even though Drew's employment status is showing as departed Drew's record still exists because it does actually exist and links to ad-drew and cl-drew. Both of the accounts are disabled and have no access but the record stays because it is in the organization's history of who the person was and the information is needed for audit trails. Disabling the account only removes the access, it does not remove the history.

## Directory trace

| Concept | Object |
|---|---|
| ou | OU-RESP Response |
| role | RL-RESP Response Agent @ R-QUEUE |
| group | GA-RESP Response-Team |
| person | P01 Alex Morgan |
| account | ad-alex (alex@ad.cloudheights.example) |
| identity | P01 Alex Morgan |
| permission | queue → update |

## Evidence

- **E01 · EV-34C82D-001** — Directory trace: Alex person, both accounts, OU, one AD grant path, one cloud role path (captured at revision 0)  
  _What this proves:_ The records show one that person, Alex Morgan, has two separate accounts: ad-alex in the Active Directory and cl-alex in the cloud identity directory. Both accounts are showing as being enabled.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-001)
- **E01 · EV-34C82D-002** — Directory trace: Alex person, both accounts, OU, one AD grant path, one cloud role path (captured at revision 0)  
  _What this proves:_ The records show one person, which is Alex Morgan with two accounts: ad-alex in the Active Directory and cl-alex in the cloud identity directory. The status of both accounts show them as being enabled.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-002)
- **E02 · EV-34C82D-003** — Comparison of Blair, Finley and Drew (captured at revision 0)  
  _What this proves:_ cl-blair is in Junior-Analysts, it grants read on Analyst Landing and cl-finley is in Case-Readers, it grants read on Case Summaries and both accounts are enabled. Drew’s accounts, ad-drew and cl-drew, are disabled with no groups and no effective access.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-003)
