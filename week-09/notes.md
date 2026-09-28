# Week 9 Notes - Vault Exchange Digital Trust

**Student Name:**

**Week:** 9

Use your own words. Short notes and everyday examples are welcome. You do not need to memorize commands.

## From a Key to an Identity

Ivy has two keys with the same label. Why is the label not enough? How can checking a visitor badge help explain the problem?

```text
My explanation: Two keys sharing the same label is not enough because anyone can create a label and the label does not prove anything about the identity. A visitors badge is not accepted just because it has a name on it, the guard checks who issued it and if the office is trusted. This is similar to a key claiming to belong to someone, the claim means nothing until it is tied to a trusted, verified certificate that vouches for its identity.
```

## Certificate Fields

A certificate is a digital badge. Explain the name list (SAN), signing office (issuer), start/end dates, public key, signature method, and allowed job (purpose). Which field would you check to see whether the badge covers the right website?

```text
Field and its job: SAN lists the names that the certificate is allowed to cover.

Issuer names the office that the certificate.

Start/end dates  define the valid time window of the certificate.

The public key is what the certificate vouches for.

The signature method is how the issuer signed the certificate.

Purpose (EKU) defines what job the certificate is authorized to do.

To check whether the badge covers the right website I would check the SAN field, not just the Subject's Common Name.
```

## Issuers and Accepted Trust

Who signed the badge? Who chooses whether to accept the top office? Explain why an office signing its own badge is not enough to make your browser trust it.

```text
My explanation: The CA (issuing office) signed the badge but my browser or operating systems makes a separate decision ahead of time if it is to be included in its trusted store. An office signing its own badge is not enough on its own because anyone can sign self sign a certificate and claim any name that they want. Real trust comes from the pre-existing, deliberate decision to accept it, not the signature itself.,
```

## Key CSR and Certificate Roles

The CSR is a badge application. Explain the separate jobs of the service’s private key, application, office’s private key, and finished certificate. Who signs the application? Who signs the finished badge?

```text
My explanation: The CSR (service's private key) signs the application which proves that the applicant hold that specific key. The office's private key signs the finished certificate, which vouches for the applicant's identity. So the service signs the application and the office signs the finished badge, which are two separate keys doing two different jobs.
```

## TLS and Verification Evidence

Compare looking at a certificate, checking its saved file, and connecting to a running service. TLS sets up a protected connection. What did you observe in each activity?

```text
I looked at: The certificate fields directly in the browser (Subject, SAN, Issuer, dates).
I checked: The saved certificate file offline by using openssl verify against the specified CA without a live connection.
I connected to: The running TLS service on 127.0.0.0:8443 and confirmed a live handshake and certificate check together.
```

## Troubleshooting and Remediation

These words mean finding and fixing a problem. Record an actual message, what it meant, and your next step. Consider a wrong name, expired dates, wrong office, canceled certificate, or service that is not running. Label situations you only discussed; do not claim to have tested them.

```text
Message or situation: I received a hostname mismatch error when checking an expected name that was not in the certificate's SAN.
What it means: It means that the certificate is valid and trusted but it does not cover the specific name being checked,
What I would check or fix: I would confirm the SAN list and reissue the certificate with the correct name if it's really missing.
Which check I would repeat: I would repeat the offline verification check and then the live TLS connection check.
```

## Questions I Still Have

```text
A word or step I want explained again: 1. How do browsers check if a certificate has been revoked and how fast does it catch a certificate that has been canceled?

2. What does a real CA do to verify domain control before issuing a certificate since signing a CSR does not prove that someone owns a website name?

3. How do organizations renew and deploy new certificates without downtown or forgetting that a certificate is about to expire?
```
