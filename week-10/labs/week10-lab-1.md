# Week 10 — Lab 1: Investigate What Needs Protection

Learner: N. Williams
Case: Cloud Heights Family Clinic — Risk & Threat Investigation
Scenario date: Friday 13 March 2026, 09:00 (clinic local time)
Report generated: 2026-09-29T00:18:07.949Z
Study mode: Guided (hints available)

Completion checklist: all required work for Lab 1 is present.

## Evidence added to my findings
- EV-REC-01 — Email received at the reception mailbox
- EV-REC-02 — Sender address comparison card
- EV-REC-03 — Reception desk log note
- EV-REC-OFF-01 — Records application account list
- EV-REC-OFF-02 — Records access log extract
- EV-REC-OFF-03 — Practice manager statement — leavers
- EV-WEB-01 — Website certificate details (captured 13 March 2026)
- EV-WEB-02 — Renewal process note
- EV-WEB-03 — What the public website actually holds
- EV-WKS-01 — Update status report (IT contractor, 13 March 2026)
- EV-WKS-02 — IT contractor statement
- EV-WKS-03 — Desk photo — signed-in laptop
- EV-BAK-01 — Backup job history
- EV-BAK-02 — Restore testing statement
- EV-BAK-03 — Backup drive photo

## My investigation notebook
AS-01 The reception mailbox continuously receives phishing attempts and the staff has started deleting them without reporting. The scheduling account only needs a password to log in, a second check is not required. Nobody knows if IT is aware this is happening or if anyone has ever entered a password by mistake.

AS-02 Reception shares one login for patient records and the password is written on a piece of paper and has not been changed for 8 months, even through staff departures. The access log only shows the shared account name and never identifies a person.

AS-03 the two laptops are 90 days overdue on updates with no one owning the fix. In addition, a laptop was found signed in, unlocked, and unattended in a room patients walk through.

AS-04 The website's certificate is renewed by hand but only after someone notices a browser warning, with no calendar reminder and no named owner. It's due to expire in 7 days from the scenario date and the last two renewals only happened reactively.

AS-05 The clinic only has one backup, it is always plugged into the same workstation that it protects and there is no second copy or offsite copy.

## Risk scenarios

| ID | Asset | Evidence | Threat / event | Vulnerability | Consequence | CIA | Unknown / question |
| --- | --- | --- | --- | --- | --- | --- | --- |
| SC-01 | Appointment scheduling account | EV\-REC\-01 \(Email received at the reception mailbox\); EV\-REC\-02 \(Sender address comparison card\); EV\-REC\-03 \(Reception desk log note\) | An unknown external sender emails the reception staff a fake urgent message posing as internal IT, asking them to confirm their username and current password. | The scheduling account signs in using only an email address and password, no second step is required. Reception has received similar messages weekly for at least three weeks and has been deleting them instead of reporting them. | If an employee entered their credentials, whoever received them can sign into the scheduling account and view, move, or cancel appointments for the entire clinic using one set of credentials with no second check requirement. | Confidentiality, Availability | Has anyone ever entered a password i response to one of these emails? Has the IT contractor been told about the weekly pattern? |
| SC-02 | Patient records application | EV\-REC\-OFF\-01 \(Records application account list\); EV\-REC\-OFF\-02 \(Records access log extract\); EV\-REC\-OFF\-03 \(Practice manager statement — leavers\) | A former employee or anyone else who has seen the shared password, signs into the front desk account and views patient records without anyone at the clinic knowing it was them. | One login with a password written on a card in a desk drawer is shared by all members of the reception staff and anyone else who has ever seen it, including former employees. This information has not been changed in 8 months. The access log only records the account name, not the name of the person who signed in.<br> | Patient records can be viewed by someone no longer employed at the clinic or by anyone who obtained the written password, with no way to identify who accessed which record, since attribution is impossible with a shared login. | Confidentiality | Can a former employee can access the records application from outside of the clinic? Has this shared account ever been misused? Why does the clinic use individual accounts for all areas except reception. |
| SC-03 | Staff laptops | EV\-WKS\-01 \(Update status report \(IT contractor, 13 March 2026\)\); EV\-WKS\-02 \(IT contractor statement\); EV\-WKS\-03 \(Desk photo — signed\-in laptop\) | Someone walking through the shared workspace that opens directly onto the patient corridor. They use an unattended laptop that is signed in and unlocked to view or change patient records or a laptop missing 90 days of security updates is exploited through a known, unpatched vulnerability while it's used daily for email and records. | The two laptops \(CHC\-04, CHC\-07\) that are used daily for email and records have gone 90 days without any applied security updates because restarts keep getting postponed and no one is responsible for making sure it happens. In addition, laptops in the shared workspace can be left signed in and unlocked while unattended, in a room that patients pass through to reach the waiting area. | An unpatched laptop can be compromised through a known vulnerability that a routine update would have closed, or a stranger passing through the workspace can view or alter patient records on an unattended, unlocked laptop because the room sits on a path patients use. | Confidentiality | Have any alerts, malware detection or incidents been reported on either laptop? How often are laptops are left signed in and unattended? |
| SC-04 | Public information website | EV\-WEB\-01 \(Website certificate details \(captured 13 March 2026\)\); EV\-WEB\-02 \(Renewal process note\); EV\-WEB\-03 \(What the public website actually holds\) | The certificate is valid  until 20 March 2026, which is only 7 days out from the scenario date, so it expires and visitors reach the site to find its address, hours, or services before anyone notices and renews it. The visitors see browser security warning first. | Certificate renewal is done by hand  and there is no calendar reminder and no named owner. The last two renewals only happened after the site had already started warning visitors, which means that the clinic's process only reacts after the certificate has already expired and not before. | Patients or the public trying to find the clinic's address, hours or how to register would see a security warning and may be deterred from continuing or lose confidence in the clinic, even though the underlying pages are only public information. | Availability | The renewal  has already happened twice before so why hasn't it been automated or assigned to someone? Does anyone check the expiration date proactively or does the clinic only find out from a visitor's browser warning? |
| SC-05 | Backup archive | EV\-BAK\-03 \(Backup drive photo\); EV\-BAK\-02 \(Restore testing statement\); EV\-BAK\-01 \(Backup job history\) | Data needs to be recovered after loss or damage \(a ransomware event, hardware failure, accidental deletion\). Any attempts to restore from the backup drive find that the copy is incomplete, corrupted or otherwise unusable because it has never been tested. | Backup only exists on a single external drive that is permanently plugged into the reception workstation, with no second copy, offsite copy or cloud copy anywhere. The last successful backup was 14 days before the scenario date and every attempt since then has completed with errors \("source in use"\). No restore has ever been attempted, so whether or not the data can be recovered is untested and unknown. Due to the drive staying attached, anything able to write to the workstation's files can also write to the backup itself, which means that the backup isn't protected from whatever might damage or encrypt the original. | If the data is lost, damaged or encrypted, the only backup could turn out to be outdated \(missing the last two weeks\) and unusable because restoration has never been tested and the drive being permanently attached means it could be affected by the same event that damaged the original data. | Availability | Why have the recent backup jobs failed with "source in use" and has anyone tried to fix that? Would the clinic actually survive if it needed the backup to occur today? |

## Email analysis
1. The message creates urgency, claiming that the scheduling account will close in 2 hours unless the password is confirmed immediately.
2. The sender address \(appointments@clinic-support.example\) does not match the clinic's domain \(cloudheights-clinic.example\) or the real scheduling vendor's domain \(appointments-vendor.example\). It is a third, unrelated domain.
3. Clinic IT Support is the display name but a display name is typed by the sender and does not prove who actually sent the email, so it means nothing on its own.

**Safe response / reporting step:** I would not click the link, reply or enter any credentials. I would leave the message alone and report it to the IT contractor because that is the person responsible for the clinic's technical security, even though he is only present one day a week. I would not attempt to investigate or test the link myself.

**Suspicious vs proven:** This message proves that someone outside of the clinic sent a fake, urgent request designed to look like internal IT, using a mismatched sender domain and a fake link. It does not prove that anyone entered their password or that the scheduling account was compromised because the evidence explicitly states that if anyone replies or enters a password it is not recorded. In order to know more, I would need to check if the scheduling account shows any unusual sign-ins or changes around that date and ask the reception staff directly whether they interacted with the message at all
