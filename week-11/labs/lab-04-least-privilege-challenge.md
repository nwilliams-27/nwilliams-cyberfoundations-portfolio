# Lab 04 — Least Privilege Challenge

**Learner:** N. Williams  
**Attempt:** attempt 34C82D · seed chic-w11-v1 · state revision 133 · text revision 376

> Training simulation — fictional Cloud Heights accounts and resources. Your actions change only your Week 11 learning attempt. This is not a live AD domain, Microsoft Entra tenant or Azure subscription.

## Objective

Configure three job-role assignments, remove excess access, and test role-based and attribute-based access.

## My answers

### Blair, Casey, Emi: job, required actions, forbidden actions

Job: Blair is a Junior Analyst.

Required actions: Read access to Case Summaries and the Analyst Landing page. 

Forbidden Actions: Blair must not have update or delete access to Case Summaries. Blair must not have access to any resources except Case Summaries and the Analyst Landing page and any temporary audit access must be kept in the final state.

Job: Casey is a  Response Agent that works the Response Queue and reads the Internal Response Note.

Required Actions: Casey needs read access and update access to the Response Queue as well as read access to the Internal Response Note.

Forbidden Actions: Casey must not have access to delete any audit records and must not have directory privileges or directory administration access. Lastly, Casey must not have update access to the Internal Response Note.

Job: Emi is an Audit Reviewer that reviews audit evidence.

Required Actions: Emi needs read access and export access to Audit Evidence.

Forbidden Actions: Deletion is not approved so Emi must not have delete access to Audit Evidence.

### Prediction for the three what-if cases, then which attribute changed each result

1. Prediction:

I predict that all three what-if tests will be DENY. I am expecting the unmanaged device trust, the Audit department and Confidential classification to block Casey from reading the Internal Response Note.

2. Which attribute changed each result:

No attributes changed each result. Device trust = unmanaged, Department = Audit (DEP-AUD) and Classification = Confidential were all DENY, which is the same as Casey’s normal test (AT-34C82D-48-1). The tests were denied because Casey did not have permission to read the note. Her group, Privileged-Operators, gave her no access to it, so changing the device, department or classification did not change the results.

### RBAC and assignment scope in your own words

RBAC (role based access) means that people get access through a role and not one person at a time. A role is a list of things that a person is allowed to do. For example, the Response Agent role allows a person read and update the Response Queue.

A role does nothing until it is given to a group. When it is given to a group, it is attached to a scope. The scope is the thing that the role applies to, such as the Response Queue or Audit Evidence.

Casey was in a group called Privileged-Operators. That group had one very powerful role attached to four different scopes. I removed  the directory scope because Casey’s ticket says no directory access. The role still exists and the other three scopes are still attached.

### Least privilege — what you removed and why

Least privilege means a person gets only the access that their job needs. I removed two things.

First, I removed Casey’s directory administration access, AS-09, from Privileged-Operators. Casey's ticket, T402, says no directory privileges, therefore, Casey should not have had it. Afterwards a test was denied for manage_sensitive_groups, which showed that the removal worked.

Next, I removed Blair from Audit-Reviewers. T401 allowed that audit access only as a temporary experiment and a junior analyst only needs read access to cases. A retest of Audit Evidence read was denied showing that Blair no longer had temporary audit access.

### Why sign-in didn't settle authorization, and how ABAC adds conditions

Signing in proves that the account is who it says it is. Signing in does not decide what the account is allowed to do.

Blair signed in successfully and was still denied update acess on Case Summaries (AT-34C82D-33-1), because no group gave her that action. Emi signed in and was denied read acess, export acess and delete access on Audit Evidence, because she has no groups. Casey signed in and was denied the Internal Response Note, because Privileged-Operators does not have a path assigned to it. Each denial result said “Sign-in succeeded; no matching grant permits this action.”

ABAC (attribute based access control) adds conditions on top of a grant. A grant says who may do an action. A condition checks an attribute, such as the device, the department or the classification, before the action is allowed. Access then needs the grant and the condition to pass.

I tested this with three what-if cases on Casey: device trust set to unmanaged, department set to Auditvand classification set to Confidential. All three were denied with the reason NO_MATCHING_GRANT. Casey has no grant to the note, therefore no condition got a chance to change the result.

## Evidence

- **E08 · EV-34C82D-012** — Three job configuration snapshots with rationales (captured at revision 29)  
  _What this proves:_ The snapshots show cl-blair in Junior-Analysts and Case-Readers with read access to Analyst Landing and Case Summaries, cl-casey in Privileged-Operators with broad access and cl-emi with no groups and no access. The audit log shows configuration reviews of Junior-Analysts, Response-Operators and Audit-Reviewers with no changes made to either.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-012)
- **E09 · EV-34C82D-013** — The nine required access tests (captured at revision 42)  
  _What this proves:_ cl-blair is allowed read access and denied update access on Case Summaries. cl-casey is allowed read and update access on the Response Queue and delete access on Audit Evidence and is denied manage_sensitive_groups in the Directory Administration Sandbox. cl-emi is denied read, export and delete access on Audit Evidence.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-013)
- **E10 · EV-34C82D-014** — Temporary grant → allowed test → revoke → denied retest (captured at revision 46)  
  _What this proves:_ The audits show cl-blair was added to a group and later removed from it. Both changes were accepted under T401. The tests show cl-blair was allowed to read Audit Evidence while the group membership existed and denied after the group membership was removed.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-014)
- **E11 · EV-34C82D-015** — Four ABAC cases (current + three what-ifs) (captured at revision 55)  
  _What this proves:_ The current-state test for Casey's Internal Response Note (read) was DENY with no matching path and the three what-if tests (device trust, department and classification overrides) were also DENY with no matching path. All four tests returned the same decision with the same reason which is, no matching grant permits the action.  
  See [evidence index](evidence/week11-evidence-index.md#ev-34c82d-015)
