# Week 10 Notes — Security Fundamentals and Risk

**Student Name:** N. Williams

**Date:** September 29, 2026

Use your own words and everyday examples. These notes support your thinking; they are not your lab answers. Do not copy clinic scenario answers here — those belong in the Demo Lab.

## 1. Assets and Business Purpose

An asset is something a business depends on. Pick an everyday example (a bakery, a gym, a school).

```text
My everyday business: A public library.
One asset it depends on: The library depends on a database to track which books exist, where they are located and who has them checked out at the moment. This database can also be referred to as the catalog and lending system.
Why the business needs that asset: Without the database, the library staff does not know what is on the shelves, what's overdue or who has what checked out.  Although people would still be able to walk in and browse the library won't be able to reliably lend books, track returns or know what is available.
```

## 2. Confidentiality, Integrity, Availability

Define each goal in your own words, then describe one situation where more than one goal is affected and explain why.

```text
Confidentiality means: Confidentiality means ensuring information is seen only by the people who are supposed to see it.
Integrity means: Integrity means ensuring information is accurate and has not been changed.
Availability means: Availability means ensuring information is available when it is needed.
A situation where goals overlap, and why: Unauthorized access to a company's database affects confidentiality because the person gaining entry will be to view private information that they should not see, such as names, addresses and account details. This unauthorized access affects integrity because the person gaining entry would be able to alter information such as addresses or account balances. Gaining unauthorized access to a company database affects both goals at once because having access to that system allows the stored information to be viewed and changed.
```

## 3. Event, Weakness, Consequence and Risk

Use your OWN non-clinic example to separate these four ideas.

```text
My example setting: A mobile tax preparation firm.
Threat / event (what could happen): The employee's laptop can be stolen from their car while they are running errands after work.
Vulnerability (the weakness that lets it cause harm): There is no disk encryption on the laptop and the screen lock does not require a password every time  it wakes up so anyone who picks up the device while it is still awake or briefly locked can gain access without knowing the password.
Consequence (what goes wrong if it happens): The tax returns and financial records that are stored on the laptop are exposed to whoever has access and the tax firm has to notify the affected clients that their data may have been compromised which leads to the firm's reputation and client trust taking a huge hit.
Risk (how the pieces combine into something to manage): Due to the laptop not being encrypted and not having a consistent screen lock, it becomes more than a financial loss, it also becomes a data exposure risk that the firm has to manage by assessing what data was on it, notifying the affected clients and possibly facing regulatory and reputational consequences. The risk exists because  of the weak device protection (vulnerability) and the theft (threat) combined.
```

## 4. Observed, Inferred and Unknown

Evidence you saw is different from a guess. If you did not see a safeguard, it is unknown — not proof it is missing.

```text
Something I observed directly: The employee reporting that their laptop was stolen while running errands after work.
Something I inferred from it: The theft was likely opportunistic because the laptop was seen through the window of a locked care and while the employee was running errands. More than likely, the attack was not targeted at the the firm or that particular employee.
Something that is still unknown: It is still unknown whether or not the laptop had any client files open or locally cached when it was stolen instead of being stored directly on the firm's server.
How I will label a safeguard I did not see: I would label a safeguard that I did not see as Not confirmed" or "unknown if present" instead of stating that it is missing. Since no one checked if the remote wipe feature was enabled before it was stolen, I would state the status of the remote wipe as unknown instead of saying the laptop has no remote wipe capability at all.
```

## 5. Warning Signs and Safe Reporting

Suspicious is not the same as proven.

```text
Warning signs I would look for: A sense of urgency requesting me to act immediately, a sender's address that does not match who it claims to be from, unexpected password or personal information requests and unexpected links or attachments.
How I would safely verify without clicking or replying: I would hover over links without clicking, check the sender's full email address and not just their display name and in order to confirm if a request is real I can contact the person directly by phone or via a website that I know is legitimate and not use any of the contact information provided in the message.
Who I would report to, and how: I would report it to my IT department through the company's normal reporting channel such as the dedicated email address for reporting phishing.
Why suspicion alone does not prove compromise: A message can look suspicious and still turn out to be legitimate or it can look real and still be malicious, so noticing warning signs only means that something is worth checking and not that something has gone wrong. If I overreact to something suspicious and it turns out to be harmless I've wasted effort but if I dismiss a warning sign just because it is not proven I risk missing a real attack. Suspicion should lead to checking and reporting and not panic or nonreaction.
```

## 6. Likelihood and Impact

Likelihood and impact are each rated 1–3. The classroom score is Likelihood x Impact. Bands: 1–2 low, 3–4 medium, 6–9 high. These are classroom judgments, not measured probabilities.

```text
What a likelihood of 1, 2 or 3 means to me: A likelihood of 1 means the event is unlikely to happen due to there being little exposure to it or because any existing protections would probably stop it. A likelihood of 2 means that there is real exposure and a known weakness but there is some protection that still exists, so it is possible but not routine. A likelihood of 3 means that the event happens regularly or the weakness is completely unprotected so it would be realistic to expect that it could happen at almost any time.
What an impact of 1, 2 or 3 means to me: An impact of 1 means that the consequences would be minor and a brief inconvenience with no real disruption or sensitive information is involved. An impact of 2 means that part of normal operations would be disrupted or that some sensitive information would be involved and in order to fix it real effort is required. An impact of 3 means that operations would stop, sensitive information would be seriously exposed or lost or recovery would be difficult and uncertain.
Existing controls vs proposed controls, in my words: Existing controls are protections that are in place right now, things I can point to as already happening and not something that I'm hoping will help later. Proposed controls are changes I am recommending to reduce a risk but they are not in place yet so they should not be used to justify a lower likelihood or impact score. Only what is protecting the business today counts toward the rating.
Why recording my reason matters more than the number: The number alone does not specify why I chose it and it is possible for different people to pick different numbers for the same situation. Explaining my reasoning in writing allows someone else can check if my judgment makes sense, point out something I may have missed or understand my thinking even if they would have scored it differently. The number is really just a shorthand for the reasoning behind it. It is not a fact on its own.
```

## 7. Controls and Residual Risk

Use a non-clinic example.

```text
A specific control: Requiring a second verification step such as a code sent via text message in addition to a password whenever an employee logs into the coffeeshop's point-of-sale or inventory system.
How it helps: If an employee's password is stolen, guessed or reused from another site that suffers a breach the attacker would not be able to log in without having access to that employee's phone, so requiring a second verification step makes it much harder for a stolen password to lead to unauthorized access.
Risk remaining afterwards: An employee can still be tricked into approving the second-step code themselves or their phone could be lost or stolen with their password, therefore unauthorized access is not impossible it is just significantly less likely to happen than with only a password.
```

## 8. Talking to a Manager

```text
How I would explain a risk in plain language to a non-technical manager: I would explain in plain terms what could go wrong, why it's possible right now and what it would cost if it were to happen. I would not use technical terms like vulnerability or attack vector. For example, instead of saying me saying "our point-of-sale system lacks multi-factor authentication," I would say something like "right now, anyone who figures out an employee's password can get into the sales and customer data and there is nothing stopping them once they have it." Next, I would explain the fix in the same plain terms, like adding a code sent via text-message when logging in and be upfront that no fix makes the risk go away completely, the risk would just smaller and easier to catch. Keeping the focused on real consequences such as losing customer trust or having to notify people their information was exposed would a non-technical manager actually understand why it matters instead of them getting lost in how the technology works.
```

## 9. Questions and Terms to Revisit

```text
Terms I want to review: There are no terms that I want to review right now because risk assessment and identifying vulnerabilities is one of my favorite parts of cybersecurity so and this section actually felt clear.
Questions for my instructor: How do cybersecurity professionals decide between a likelihood of 2 versus 3 when the evidence does not clearly point to one obvious answer, is there a standard tiebreaker that's used or is it always a judgment call?

Who owns the decision to accept residual risk instead of paying for another control?
```
