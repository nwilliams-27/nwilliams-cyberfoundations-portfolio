# Week 9 Submissions Guide - Vault Exchange Digital Trust

Week 9 completes Portfolio Deliverable 3: a collection of your work showing what you learned. Complete the two worksheets, add screenshots, and explain the results in your own words. Some checks should refuse deliberately wrong information. That refusal is useful evidence that the check worked.

A **repository** is your project folder on GitHub. A **commit** is a saved set of changes. **Evidence** means the messages, screenshots, and observations that support your explanation. Use the same save/upload method you practiced in Week 7; ask your instructor to demonstrate it if needed.

In the Lab Portal, every worksheet has a **Save Progress** button that stores your answers and a **Submit to GitHub** button that commits the finished file to your portfolio repository. Notes and Reflection are separate worksheets on the Week 9 page and are saved and submitted the same way.

## Labs at a Glance

| Lab | Focus | Required evidence |
|---|---|---|
| 01 - Investigate a Certificate | Read a real website’s digital badge; check its name, dates, and signing offices | Dated observation, completed field/chain tables, fields and chain screenshots |
| 02 - Build and Test Digital Trust | Make a key and badge application; issue and check a badge; try a protected practice connection; explain problems | CSR/certificate/public-key evidence; offline pass/name fail/CA fail; listener/TLS pass/TLS fail/stopped listener; comparison and explanations |

## Where Everything Goes

| File | Repository destination |
|---|---|
| Lab 01 worksheet | `week-09/labs/lab-01-investigate-a-certificate.md` |
| Lab 02 worksheet | `week-09/labs/lab-02-build-and-test-digital-trust.md` |
| Notes | `week-09/notes.md` |
| Reflection | `week-09/reflection.md` |
| Evidence images | `assets/screenshots/week-09/` |

Keep worksheet filenames unchanged. Use the image names listed in each worksheet. From a worksheet in `week-09/labs/`, this relative Markdown image link opens a repository image:

```markdown
![Certificate fields](../../assets/screenshots/week-09/week09-lab01-certificate-fields.png)
```

Add a caption explaining the evidence. Replace this example with the correct filename for each image.

## Portfolio Deliverable 3 Checklist

- [ ] **Certificate investigation:** public hostname, date/time zone, issuer, SAN, validity, key and signature roles, purpose, and chain map in Lab 01.
- [ ] **OpenSSL walkthrough:** dedicated key, request, issuance, inspection, verification, and local connection explained in Lab 02.
- [ ] **Trust evidence:** passing and deliberately failing results with exact CA file and expected name; causes explained.
- [ ] **Comparison:** public website versus isolated service, including who accepts each issuing office and where each service runs.
- [ ] **Reflection:** what certificates establish, their limits, and one evidence-supported troubleshooting decision.
- [ ] **Professional evidence:** legible artifacts, exact filenames, no private material.

## Non-Negotiable Safety

- Keep Week 6-8 work and SSH configuration intact.
- Do not add the practice issuing offices to your computer or browser’s accepted list. The supplied `-CAfile` command option chooses an office only for that check.
- TLS listener remains `127.0.0.1:8443` and is stopped afterward.
- Never upload `.key.pem`, `.ssh` contents, passwords, or access URLs.
- Upload only the named documents and reviewed screenshots. Never upload the whole practice folder or use `git add .` from it; that could include secret keys.

## Submission Sequence

1. Complete the two worksheets, notes, and reflection in your own words.
2. Review every screenshot at full size and add captions.
3. Commit only the named documents and reviewed images to your portfolio repository.
4. In the Lab Portal, open each Week 9 worksheet, press **Save Progress** while you work, then press **Submit to GitHub** to commit it.
5. Open the repository files after committing and verify formatting and privacy. Provide the commit link as directed by your instructor.

A missing command, unwritable folder, or VM connection problem should be reported with the exact error. Do not fabricate a passing result. Lab 01 website certificate values are time-sensitive; record your own observation rather than copying a classmate's screenshot.
