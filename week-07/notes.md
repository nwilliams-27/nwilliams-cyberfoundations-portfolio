# Week 7 Notes — Cloud Heights: The Guard Post

**Student Name:** N. Williams

**Week:** 7

## Firewall and Security Group

```text
Decides what network traffic is allowed in or denied based on a set of configuration rules.
```

## Rule Anatomy

```text
(priority, direction, source, destination, protocol, port, action)
```

## First Match Wins

```text
Rules are evaluated in order from the lowest priority number to the highest priority number. Once a rule matches the traffic that rule's action is immediately applied and no further rules are checked.
```

## Least Privilege

```text
Least privilege only allows the access that is needed for the job that needs to be complete, reducing the attack surface.
```

## Testing and Evidence

```text
A single successful test is not enough proof because an Allow result (intended source works) and a Deny result (unintended source blocked) are needed to prove that least privilege is enforced and not just assumed.
```

## Troubleshooting and Remediation

```text
When diagnosing use evaluation order and evidence (which rule matched and why) before any changes are made instead of trial and error edits. Doing so identifies the root cause and avoids unnecessary or incorrect
```

## Questions I Still Have

```text
In real production environments how often are temporary test rules added and left in place by accident and what is the standard practice for capture before it becomes a problem?

Is first match wins always this straightforward or are there other factors beyond priority number that can affect which rule wins?

Are there any time constraints when trouble shooting in a live, active outage?
```
