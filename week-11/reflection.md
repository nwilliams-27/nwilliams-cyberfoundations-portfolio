# Week 11 Reflection — IAM & Active Directory

**Student Name:** N. Williams

**Date:** October 25, 2026

Answer in your own words. A few sentences each is enough. You do not need to redo your lab answers.

1. What IAM concept became clearer this week?

```text
The IAM concept that became clearer this week was how a person is actually assigned access. I learned that access is not given to a person directly. It comes from a group and the group is given a role at a scope. The role is a bundle of specific actions, such as read, export or delete and each action has to be allowed on its own. For example, a role can allow read and export on a resource but not delete, so each action has to be checked separately. 
```

2. What is the difference between authentication and authorization, in your own words?

```text
The difference between authentication and authorization is authentication is proving who you are and authorization is deciding what you’re allowed to do after you’ve proven who you are.

Signing in with a password, a managed device and MFA is authentication.

Authentication depends on a matching grant, such as a group that holds a role at a scope.

```

3. Where did least privilege affect one of your decisions?

```text
One of my decisions affected by least privilege was Blair’s access in the Lab 04 ticket T401. The ticket approved read access to Case Summaries and the landing page for a junior analyst and it also allowed a temporary audit-access experiment that could not remain. I added Blair to Audit-Reviewers for the experiment and a test showed Audit Evidence read access was allowed (AT-34C82D-44-1). Next, I removed Blair from the group so that her final access would match the ticket. A retest showed Audit Evidence read access was denied (AT-34C82D-46-1). So even though the extra access worked, it did not stay because a junior analyst only needs to read access cases and that's what least privilege means, only giving a person the access that their job requires.

```

4. What did you learn from a joiner, mover or leaver scenario?

```text
In leaver scenario for Lab 05 when Finley’s contract ended, I learned that disabling the accounts was not enough by itself. The sessions had to be revoked, group memberships had to be removed and Finley's Active Directory account had to be moved to Disabled Accounts. Also, each directory, Active Directory and Cloud Identity had to be handled separately because they do not synchronize. After the changes, Finley's old session was denied and a new sign-in failed because the account was disabled. Nothing was deleted so the records are still there if someone needs to review them later.

```

5. What made an identity event suspicious versus merely unusual?

```text
In the September 21 records, the merely unusual event was Blair’s one wrong password followed by a success with MFA on a managed device. The event was merely unusual because there was a normal explanation and a ticketed change. The events on Casey’s account were suspicious because several warning signs came together: five failed passwords from an unknown device, a success with no MFA recorded, followed by the account adding itself to Privileged-Operators with no approval ticket. The difference was Blair had one odd event with a normal explanation and Casey had several odd events together with no explanation, so hers needed to be checked. The logs still can’t prove who was using the account or why, so misuse is not confirmed but the activity is suspicious so it needs to be checked.
```

6. What evidence helped support a conclusion you reached?

```text
The evidence that supported my conclusion in Casey's finding was a shared correlation ID. The successful sign-in (S009) and the audit record showing Casey’s account adding itself to Privileged-Operators (A002) carried ID C009, followed by the group change one minute after the sign-in. This supported my conclusion that the two records were linked and happened in that order. What it did not prove who was using the account or why, so my conclusion was written carefully and listed what the logs could not show.

```

7. What uncertainty remained, even after your investigation?

```text
After looking closely, I still could not tell who was really using the accounts. The logs show what happened but they do not show who did it or why. not who did it or why. This led to me recommending things to check and not claiming that someone did something wrong.

```

8. What would you explain differently to a nontechnical manager?

```text
Without using technical terms I would explain what happened, why it matters and what should be done next. Instead of saying ‘a password-only sign-in with no MFA challenge from an unknown device followed by a self-added privileged group,’ I would explain it by saying someone tried to log in with a user’s password several times from a device we that we do not recognize, they were able to get in without the extra security step and the account gave itself more power with no approval. Next, I would clearly explain what we don't know, which is we can’t tell who was using the account, so I recommend contacting the user directly to check instead of just saying someone did something wrong.

```

9. What do you want to carry into Week 12, when you turn your case file into an incident report?

```text
The habit of only saying what the records support is something that I want to carry into Week 12. In my case file, every finding listed the exact event IDs, what the logs showed, and what they could not show, like who was using the account. I also want to keep clearly saying what we don’t know and write recommendations as things to check, not as claims that someone did something wrong. Finally, I want to keep tying each action to a ticket and a test because I think that it makes my work easy to check.
```

## Professional Growth Check

- [x] I can explain the difference between authentication and authorization.

- [x] I can describe least privilege with an example from my own work.

- [x] I can walk through a joiner, mover or leaver scenario.

- [x] I can say what made an identity event suspicious, and what evidence supported my conclusion.

- [x] I can name what is still uncertain and what I want to carry into Week 12.
