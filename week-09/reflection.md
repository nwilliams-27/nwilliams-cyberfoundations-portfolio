# Week 9 Reflection - Vault Exchange Digital Trust

**Student Name:** N. Williams

**Week:** 9

1. What clicked for you this week?

```text
What clicked for me is that a certificate has to pass separate checks by the  name matching the SAN and the issuer being one that I trust. Running the wrong-name and wrong-CA tests on the same certificate showed me that getting one right does not fix the other.
```

2. What's still confusing?

```text
I'm still trying to understand how the wildcard names in the SAN list work. Foe example, why does *.example.com cover an extra label but not the bare domain? I also don't quite understand how revocation gets checked in real browsers because our file checks did not do it. 
```

3. How does this week's material connect to a cybersecurity career path you're interested in?

```text
This week's material connects to PKI and IAM because certificates are how systems prove identity and decide who to trust. It also connects to ethical hacking because the wrong-name and wrong-CA tests showed how a client should reject a certificate that does not match the expected name or issuer. Security testers check to see if real systems are enforcing those rejections and an attacker who can get a client to accept a bad certificate has the ability to impersonate a trusted server. Lastly, It connects to GRC and audit work because I had to separate what I checked myself from what the browser reported and avoid claiming checks I did not perform.
```

4. One thing you would tell a friend just starting this course:

```text
If a check fails, read the exact message before changing anything. The message will usually tell you which part failed, such as the name, the issuer or the connection.
```

## Professional Growth Check

- [x] I can explain why a working key’s label does not prove who owns it.

- [x] I can read the digital badge and describe the list of signing offices I actually saw.

- [x] I can explain the service’s key, its badge application, the office’s key, and the finished badge.

- [x] I can explain why correct information passed and why an intentionally wrong input was refused.

- [x] I can tell the difference between a service not answering and its badge failing a check.

- [x] I can share useful screenshots while keeping private keys and passwords secret.

## Portfolio Deliverable 3 Reflection

Write 5–7 sentences. What can a correctly checked digital badge tell you about the service? What can it NOT promise? Describe one problem using the message you actually saw, and explain what you checked next. Compare the real website with your practice service. You may start with “I used to think…”, “My check showed…”, and “I now know…”.

```text
My check showed that my practice certificate passed against the Week 9 Lesson CA with the name localhost, which confirmed that the name matched the SAN, the dates were current and it traced back to the office I told the client to accept. When I changed only the expected name to wrong.test, I received verify error:num=62:hostname mismatch, so I checked the SAN list and found that it only contained localhost. The real website was different because example.com's certificate chained up to SSL.com TLS ECC Root CA 2022, which my browser already trusted, while my practice certificate was signed by Week 9 Lesson CA and my VM accepted it because I told the check to use it. A certificate that has been checked correctly is not a promise that the website's content or downloads are safe and my file checks never confirmed whether a certificate had been revoked. I now know a passing check tells me the badge fits the name I meant and comes from an office I accept, not that I can trust everything that the service offers.
```
