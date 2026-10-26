# Executive Summary — Cloud Heights Identity Investigation

> Fictional training scenario: Cloud Heights Identity Center (simulated). No real organisation, people or systems.

- Prepared by: N. Williams
- Audience: Cloud Heights leadership
- Length: 341 words (target about 250–400)
- Full technical detail: [technical-incident-report.md](technical-incident-report.md)

## What happened?
One day of sign-in and change records (September21) from a training system was reviewed. In that review, one user account had several failed sign-in attempts from a device that we do not recognize and gained access without the usual second security step. Immediately after (one minute later), that same account gave itself extra access with no approval on record. A second user signed in from two different locations two minutes apart but this may have an ordinary explanation.

## Why does it matter?
An account that has extra access can reach more information and make more changes than its owner should. If someone other than the real user is using it, they are able to view or change important records. We can’t tell if that happened yet, so the risk is real but it is not confirmed.

## What did we find?
A few things worth checking were found. on the first case we found  a pattern that fits someone guessing a password, followed by a self-approved increase in access. This priority is rated as likely and high. On the case we found that the user signed in from two different locations two minutes apart. This may have an ordinary explanation, such as a work VPN, so it was rated as possible and medium priority. A third case shows one wrong password followed by a successful sign-in, which looks like an ordinary typo and needs no action.

## What should happen next?
The first user should be contacted through a trusted channel, such as the phone number on file, to confirm if they made those sign-ins. Check if the extra access was approved and remove it if it wasn’t. Next, it should be confirmed with the second user if they were working remotely. The password should only be reset after the person’s identity has been verified.

## What remains uncertain
The records show what the accounts did but do not show who was using them. We don’t know who made the sign-ins, why the second security step was missing once or what any account did after the access was changed. These are checks that need to be run, not conclusions.
