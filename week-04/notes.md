# Week 4 Notes — Permissions, Searching, and Virtual Machines

**Student Name:** Na'Ketta Williams

**Date Completed:** August 11, 2026

Summarize this week's key concepts in your own words — not copy-pasted definitions.

## Key Concepts This Week

- File permissions: read/write/execute × owner/group/other, and reading `ls -l`
- Changing permissions with `chmod` (symbolic and numeric) — and THE GATEKEEPER'S RULE
- Windows ACLs, read with `Get-Acl`/`icacls`
- Wildcards (`*`, `?`, `[ ]`) and searching inside files with `grep`/`Select-String`
- Virtual machines: host vs. guest, the hypervisor, Type 1 vs. Type 2, isolation
- The VM lifecycle: create, start, stop (deallocate), snapshot, delete — and what each costs
- Golden snapshots — how your Weeks 6–12 lab machines are made

## In My Own Words

**Decode `-rw-r-----` audience by audience: who can do what to this file?**

```
The owner can read and write (modify/make changes) the file. The  group only has access to read the file and others have no access at all.
```

**What is a hypervisor, and what are its two jobs?**

```
A hypervisor is the software layer between physical hardware and virtual machines that allow multiple VM's to run on one machine. It divides the hosts CPU, RAM and disk among the VM's and isolate each VM so that problems in one VM don't affect other VM's.
```

**A stopped VM still costs a little money. What is it paying for, and what's the only way to reach a true zero?**

```
Disk storage is still has to be paid for even though the VM is stopped. The only way to reach a true zero is to delete the VM.
```

---

## Submission Checklist

- [x] I summarized each concept in my own words, not copied definitions

- [x] I answered all three "In My Own Words" prompts

- [x] This file is committed to my portfolio repo at `week-04/notes.md`
