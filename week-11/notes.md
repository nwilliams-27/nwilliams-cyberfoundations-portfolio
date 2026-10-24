# Week 11 Notes — IAM & Active Directory

**Student Name:** N. Williams

**Date:** October 24, 2026

Use your own words and everyday examples. These notes support your thinking; they are not your lab answers. Do not copy your lab answers here — those belong in the Cloud Heights Identity Center.

## 1. Person vs Identity vs Account

```text
A person is: A person is a real human being.
An identity is: An identity is the set of facts that tells an organization who that person is, such as their name, job and employee ID.
An account is: An account is a login that lets that person use one system. One person can have several accounts.
An everyday example that shows the difference: An everyday example that shows the difference would be: a nurse is a person. Their employee profile is their identity. Their email login, scheduling-app login and building badge are their accounts.
```

## 2. OU vs Group vs Role vs Permission

```text
An organizational unit (OU) is: An organizational unit (OU) is a folder in a directory that keeps accounts organized, like a department folder. It does not give anyone access by itself.
A group is: A group is a list of accounts that are managed together such as a team roster.
A role is: A role is a named bundle of job permissions such as a “cashier” or “auditor.”
A permission is: A permission is one action on one thing such as “read this report” or “delete this file.”
How they fit together in one sentence: Accounts are filed in an OU for organization, placed in groups, and the groups are given roles, and the roles contain the permissions.
```

## 3. Authentication vs Authorization

```text
Authentication means: Authentication means proving who you are with something like a password, a badge or a fingerprint.
Authorization means: Authorization means deciding what you are allowed to do after you have proven who you are.
A real-life analogy (for example, an airport or an office building): A real-life analogy would be: at an airport, showing your passport and boarding pass proves who you are. The gate agent then checks if your ticket lets you on that flight. Getting through security does not mean that you can board any plane.
```

## 4. Least Privilege

```text
Least privilege means: Least privilege means giving a person only the access that their job needs and nothing more.
One place in the labs where giving more access would have been risky: One place in the labs where giving more access would have been risky: one account was in a group with broad rights, including the ability to delete audit records and manage users. Her job only needed to work a queue, so the extra access could have let someone erase evidence or change other people’s accounts.
```

## 5. RBAC

```text
Role-based access control means: Role-based access control means access comes from the role someone has, not from permissions handed out to each person one by one.
One role from the labs and what it allowed: An audit reviewer role allowed reading and exporting audit evidence and nothing else.
```

## 6. ABAC

```text
Attribute-based access control means: Attribute-based access control means access depends on facts about the person, the device or the thing being accessed, such as their department, if their device is managed or how sensitive the record is.
How it differs from RBAC: RBAC asks “what role do you have?” ABAC adds conditions and asks “do the facts about this request fit the rules?” A role can say yes but the attributes can still say no.
```

## 7. AD DS vs Entra ID

```text
AD DS is: AD DS is Microsoft’s directory that runs on an organization’s own servers. It keeps accounts, groups and OUs for the on-premises network.
Microsoft Entra ID is: Microsoft Entra ID is Microsoft’s cloud directory. It keeps accounts and groups for cloud apps and uses roles assigned at a scope.
One similarity and one difference: Both store accounts and groups and decide who can sign in. AD DS uses OUs and access lists on resources, while Entra ID has no OUs and uses roles.
```

## 8. Azure Resource RBAC

```text
What Azure resource RBAC controls: What someone can do to cloud resources like virtual machines, such as view their details or stop them. It works through a role assigned at a scope, like a resource group.
How it relates to what I did in the simulator: I gave one group a role that could only view details of a machine in one resource group. That person could see that machine but could not stop it or see machines in other groups.
```

## 9. Joiner / Mover / Leaver

```text
Joiner (what happens when someone starts): Their accounts are created, identity is checked, and they get only the groups and access their job needs.
Mover (what happens when someone changes roles): Their department is updated, the old access is removed and the new access is added. Both steps matter or old access will pile up.
Leaver (what happens when someone leaves): Their accounts are disabled, sessions are ended, memberships are removed and records are kept.
Why the leaver step matters most for security: A former employee with working access is a risk no one is watching. Leftover access can be misused and any activity looks like it came from a trusted account.
```

## 10. Sign-in Logs vs Audit Logs

```text
Sign-in logs show: Sign-in logs show who tried to sign in, when, from where, on what device and if it worked.
Audit logs show: Audit logs show changes made to accounts, groups, roles and tickets and who made them.
One question each log can answer: The sign-in log can answer “did this account sign in from an unusual place?” The audit log can answer “who added this account to a privileged group?”
```

## 11. Useful Terms and Questions

```text
Terms I want to review: ABAC and how attribute conditions interact with a grant, since in my tests no attribute changed a result until a grant existed.
Questions for my instructor: None right now because I feel comfortable with the material.
```
