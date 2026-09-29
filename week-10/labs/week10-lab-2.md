# Week 10 — Lab 2: Prioritize Risks and Recommend Controls

Learner: N. Williams
Case: Cloud Heights Family Clinic — Risk & Threat Investigation
Scenario date: Friday 13 March 2026, 09:00 (clinic local time)
Report generated: 2026-09-29T17:08:07.009Z
Study mode: Guided (hints available)

Completion checklist: all required work for Lab 2 is present.

## Risk ratings

| ID | Asset | Likelihood | Why | Impact | Why | Score (L x I) | Classroom band |
| --- | --- | --- | --- | --- | --- | --- | --- |
| SC-01 | Appointment scheduling account | 2 | EV\-REC\-01, EV\-REC\-02 and EV\-REC\-03 reveal that this type of message is received weekly and the staff has caught it before but the account does not require a second sign in step and a process for reporting or acting on this pattern is not available, which only offer partial protection that relies on individual staff noticing. | 2 | If the account is accessed appointments can be viewed, moved or cancelled for the entire clinic which would disrupt scheduling and expose patient appointment information. Because this account does not hold medical records, there is no evidence of account access fully stopping clinical operations. | 4 | Medium (3–4) |
| SC-02 | Patient records application | 3 | EV\-REC\-01, EV\-REC\-02 and EV\-REC\-03 reveal a shared login with a password that is written down on a piece of paper, has not been changed in 8 months and two former employees still know the information. There is no control in place to catch or limit this exposure, it is routine and ongoing and it is not an isolated incident. | 3 | The patient records application not only houses scheduling information, it also houses clinical patient information. If this information is accessed inappropriately, sensitive patient records are exposed because the account is shared, and there is no way to identify who accessed what which makes any response or recovery more difficult. | 9 | High (6–9) |
| SC-03 | Staff laptops | 2 | EV\-WKS\-01 shows that 2 out of 9 laptops \(CHC\-04, CHC\-07\) have been unpatched for 90 days with no owner chasing the fix and EV\-WKS\-03 shows that a laptop was left unlocked and signed in in a workspace that opens to the patient corridor. This is a real, evidenced weakness, however, it is bounded to specific laptops and one observed instance, noy a routine clinic wide condition. | 2 | Both of the affected laptops are used on a daily basis for email and the records application so an unexpected vulnerability or an unattended, unlocked session can expose or alter patient information and disrupt the work on those devices, even though nothing in the evidence suggests that full clinic operations will stop. | 4 | Medium (3–4) |
| SC-04 | Public information website | 3 | EV\-WEB\-01 shows that the certificate expirees 7 days from the scenario date. EV\-WEB\-02  shows that the last two renewals only happened after the site started warning visitors and there is still no calendar reminder or named owner. Due to no protection or process change in place, the smae failure is likely to repeat. | 1 | EV\-WEB\-03 confirmed that the website has no sign in, no patient portal and no connection to the records application, holding public information only. A lapsed certificate causes a browser warning and can deter visitors but does not expose sensitive data or stop clinic operations. | 3 | Medium (3–4) |
| SC-05 | Backup archive | 3 | EV\-BAK\-01 shows that the last successful backup was 14 days ago with job entries that have failed 6 times consecutively. EV\-BAK\-03 shows that the single drive stays permanently attached and there is no second, offsite or cloud copy anywhere. There is no control in place to catch this pattern, it is ongoing as well as unaddressed. | 3 | EV\-BAK\-02 confirms that a restore has never been tested, so whether or not the backup will work is unknown. EV\-BAK\-03 shows that the drives stays permanently attached, which means that it could be affected by the same event that damages the source data. If recovery is needed, it would be uncertain and potentially unavailable, which is a serious operational risk. | 9 | High (6–9) |

Bands (1–2 low, 3–4 medium, 6–9 high) are a classroom teaching aid, not a compliance standard.

## Priority risks
- SC-02 — Patient records application
- SC-05 — Backup archive

**Why these:** SC-02 and SC-05 received the highest possible score of 9, which placed them together in the High risk band and neither one outranked the other. SC-02 involves the clinic's clinical patient data directly, with no way to attribute access to an individual because the account is shared and no protection has caught this in 8 months. SC-05 threatens the clinic's ability to recover if anything goes wrong because the only backup is untested, has been unreliable for the last two weeks and it stored on the same machine it would need to recover from. These two risks together compound each other: if patient records are compromised or lost through the shared-account weakness or through a device failure, the backup that is meant to recover them has never been proven to work.

## Recommended controls
### SC-02
- **Control:** Replace the shared frontdesk login with individual accounts for each of the four reception staff employees and set a requirement for the shared account to be disabled the same day anyone leaves, tying it to the badge-return process that already exists.
- **How it helps:** This process would restore attribution, the clinic will be able to tell which staff member accessed which record which makes misuse easier to spot after the fact and easier to deter in the first place. It also closes the former staff retain working access weakness because the login changes would now be tied to the same process that already reclaims the badge.
- **Risk remaining afterwards:** Individual accounts do not stop an employee from misusing their own legitimate access and the evidence never established if records can be accessed from outside of the clinic, so that particular exposure would remain unaddressed. The risk would move from routine and unattributable to occasional and traceable but it is not eliminated.

### SC-05
- **Control:** Keep a second backup copy on a drive that is disconnected after each backup is completed instead of it remaining permanently attached and perform one test restore to confirm that the copies work.
- **How it helps:** Whatever damages or encrypts the reception workstation can't affect the disconnected drive due to it no longer being attached most of the time. A tested restore converts a "we assume that it works" into something is confirmed which addresses the core unknown in the evidence.
- **Risk remaining afterwards:** A single additional drive is still a clinic's worth of backups that is sitting on-site, it's not an offsite or cloud copy. Therefore, a fire, a theft or even a building wide event has the possibility to destroy the original data and both of the backup copies. Recovery is more likely to succeed but the clinic would still not be protected against every kind of loss.

## Owner briefing
Word count: 206 (guide: 100–150)

There are two risks that stand out the most for the clinic right now. First, all four of your front desk staff share one login for patient records and the password is written on paper. The password has not changed in eight months or after two staff members left. This means that we can't tell who accessed a given record and anyone who ever saw that password still has the ability get in. Second, the backup drive stays plugged into the same computer that it protects, it hasn't backed up successfully in two weeks, several recent backup attempts have failed.  and we have never tested whether or not it would restore our data if we needed it.

My recommendation is to give each front desk staff member their own login, which is tied to the same process we already use when someone leaves. I also recommend adding a second backup drive that can be disconnected after each backup and testing a restore to confirm that it works.

Neither fix is perfect. We would still need to check if records can be accessed from outside of the clinic and a fire or theft could still affect both backup copies since they are stored in the same building.
