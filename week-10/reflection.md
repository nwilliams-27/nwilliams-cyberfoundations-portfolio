# Week 10 Reflection — Thinking Like a Security Professional

**Student Name:** N. Williams

**Date:** September 30, 2026

Answer in your own words. A few sentences each is enough. You do not need to redo your lab answers.

1. What clicked for you this week?

```text
What clicked for me this week was how to tell the difference in a vulnerability, a threat and a consequence when they're all sitting in the same space. My mobile tax firm laptop theft example made this concrete: the theft was the event, the missing encryption was the vulnerability and the exposed client data was the consequence.
```

2. What is still confusing?

```text
Nothing for this week is really confusing, this was one of the clearer weeks.
```

3. Which assumption changed during the labs, and what evidence changed it?

```text
It was assumed that a certificate or control appearing to be valid on the surface meant that something was actually safe. The clinic's evidence changed that, the website's certificate was valid right up until it expired with no one tracking it and the backup drive looked like it was working because a light blinked but it had never been tested. Evidence showed me that what "looks fine" and what's "proven to work" are not the same thing.
```

4. Describe one connection you made to confidentiality, integrity or availability.

```text
The patient records scenario at the clinic proved how one weakness can affect multiple CIA goals at once. The shared login threatened confidentiality because anyone who knew the password could view records they shouldn't. It also meant that no one could verify integrity, because there was no way to tell who actually viewed or changed a given record due to every action in the log being tied to the same shared account instead of each employee.
```

5. Describe one priority you chose and the tradeoff behind it.

```text
I prioritized the patient records risk over the website certificate risk, even though the certificate issue would also score as a real concern. The tradeoff was that the certificate problem only affects public trust and availability because the website holds no patient data at all but the records risk directly exposes clinical information with no way to identify who accessed it. I gave more attention to the risk with the more serious consequence. The certificate problem is easier and cheaper to fix but it's impact was much lower because the website does not hold patient data.
```

6. Name one control you recommended and the risk that remains afterwards.

```text
I recommended that the shared frontdesk login be replaced with individual accounts for each reception staff member. The remaining risk is that individual accounts do not prevent someone from misusing their own access and the evidence never established if the records system could be reached from outside the clinic, leaving that exposure unaddressed even after the fix.
```

7. How does this week connect to a cybersecurity career you are interested in?

```text
This week connects to ethical hacking and penetration testing because finding a vulnerability like the shared login or the unpatched laptops is exactly what a tester would be looking for before an attacker does. It also connects to GRC and IAM because rating the risk and recommending a fix without pretending that it disappears completely is the other half of that work, a pentester finds the weakness, but someone still has to decide how serious it is and what to do about it.
```

8. What is your next step to keep building these skills?

```text
My next step is to get more hands-on practice by testing systems in a safe, legal environment, such as a practice lab or a platform built for this kind of training. Reading about a vulnerability is different from finding and exploiting one myself, which is the hands-on experience is that I need.
```

## Professional Growth Check

- [x] I can explain what clicked and name what is still confusing.

- [x] I can describe an assumption I changed because of evidence.

- [x] I can connect a risk to confidentiality, integrity or availability.

- [x] I can explain a priority I chose and its tradeoff.

- [x] I can describe a control and the risk that remains after it.

- [x] I can connect this week's work to a cybersecurity career.

- [x] I have a clear next step.

## Optional Synthesis

In 5–7 sentences, describe how you moved from observations to a recommendation. This is optional and not graded separately.

```text
I walked through the evidence and noted specific details, such as the password for the shared login being written on paper and not being changed for 8 months. For each scenario, I separated the vulnerability, the threat it caused and the consequences if it were to happen. Then I rated the likelihood and impact using only what the evidence supported, which clearly showed that two risks were more serious than the other risks. I chose those two because their scores were highest and their consequences involved patient data or the clinic's ability to recover. For each risk I recommended a realistic control and I named the risk that was still left afterward because no fix removes risk completely.
```
