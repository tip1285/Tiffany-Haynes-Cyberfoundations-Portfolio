# Week 4 Notes — Permissions, Searching, and Virtual Machines

**Student Name:** Tiffany Haynes

**Date Completed:** 8/9/2026

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
The owner can read and write the file.
The group can read the file.
Others don't have permission to do anything with the file.
```

**What is a hypervisor, and what are its two jobs?**

```
The Hypervisor sits between the hardware and the guests; it divides the host's real cpu time, memory and disk among the guests. The hypervisor also tracks the resources that each guest uses in real time (bookeeping). 
```

**A stopped VM still costs a little money. What is it paying for, and what's the only way to reach a true zero?**

```
A stopped VM only stops the compute charges not the locker fee. The locker fee is for the disk which is still running and is billed for storage.
The only way to reach a true zero is to delete the VM.
```

---

## Submission Checklist

- [x] I summarized each concept in my own words, not copied definitions

- [x] I answered all three "In My Own Words" prompts

- [x] This file is committed to my portfolio repo at `week-04/notes.md`
