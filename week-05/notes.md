# Week 5 Notes — The Grid: Addresses, Names, Ports, and Diagnostics

**Student Name:** Tiffany Haynes

**Date Completed:** 8/16/2026

Summarize this week's key concepts in your own words — not copy-pasted definitions.

## Key Concepts This Week

- IP addresses — the dotted-quad number every device on a network needs (`10.20.5.42` on The Grid)
- The subnet mask — the answer to "which addresses are my neighbours?" (`/24` = `255.255.255.0`)
- The default gateway — the door out of your neighbourhood (`10.20.5.1` on The Grid)
- Private vs public addresses — `10.x`, `172.16–31.x`, and `192.168.x` are *inside* addresses
- DNS — the Grid's Directory Board: a name goes in, an IP address comes out
- NXDOMAIN vs a host that resolves but is down — two different failures with two different causes
- DHCP — the Address Office: leases, why addresses change, why a laptop "just works" on a new network
- Ports — the numbered doors on a building: 22 SSH, 53 DNS, 80 HTTP, 443 HTTPS, 3389 RDP, 25 SMTP
- TCP vs UDP — a confirmed conversation vs a shout across the room
- The TCP handshake — SYN → SYN-ACK → ACK (packets 7, 8 and 9 in Lab 03)
- The diagnostic toolkit — `ping` (is it alive?), `traceroute` (where does it stop?), `dig` (what number is behind that name?)
- **THE LADDER RULE** — check yourself → check your gateway → check the target by NAME → check the target by IP → trace the path. *Work outward, one rung at a time, and let the evidence pick the culprit.*

## My Command Table

You learned the same five jobs twice this week — once in bash, once in PowerShell. Fill the pairs in from memory if you can, and check them afterwards. This table is worth keeping.

The bash command and its PowerShell equivalent for each job — show my own address, show my default gateway, test reachability, trace the path, look up a name:

```
Bash/Powershell
ip addr/ipconfig- show my own address
ip route/ipconfig- show my default gateway
ping/Test-Connection- test reachability
traceroute/tracert-trace the path
dig/Resolve-DnsName- look up a name
```

## In My Own Words

Your machine has three numbers: an address, a subnet mask, and a default gateway. Explain what each one is for, the way you'd explain it to someone who has never heard those words.

```
-An IP address is your machine's unique identifier within a network/neighborhood. It consists of 4 octets and are separated by dots (ex. 192.168.1.1). (4 numbers/3 dots)
-The subnet mask tells you who your neighbors are. The mask=255.255.255.0, where the mask says 255 that is equivalent to the street, where it says 0 that's the house number. 
-The default gateway is the IP address that gets you out of the neighborhood.
```

What does DNS actually do? Include the difference between a name that comes back "Name or service not known" (NXDOMAIN) and a name that resolves perfectly well to a host that never answers.

```
Domain Name System (DNS)- you hand DNS a name and it hands you back a number (IP Address).
A name that comes back "Name or service not known" that name isn't on the board at all it doesn't exist.
A name that resolves perfectly well to a host and it never answers could mean that the machine is powered off or offline. 
```

An IP address gets your traffic to the right building. What does a port number add to that, and why would a defender care how many doors are open?

```
An IP address gets your traffic to the right building, but a port number gets you to the right door. A defender cares about how many doors are open because every open port adds to the attack surface. Every open door means a door to configure, patch and watch.
```

Write out THE LADDER RULE — all five rungs, in order — and say why running them in that order matters more than running them fast.

```
Rung 1- ip addr- Do I have a valid address?
Rung 2- ping- Can I get out at all? Is the gateway healthy?
Rung 3- ping name- Does the name work?
Rung 4- ping number- Does the number work?
Rung 5- traceroute-How far do I get?
Work outward. One rung at a time and let the evidence pick the culprit. 
Start with yourself and move outward, that way you eliminate everything that is not the answer.
```

What is DHCP, and why does your laptop get an address automatically on a network it has never joined before, while a server like `grid-dns` keeps the same address permanently?

```
Dynamic Host Configuration Protocol (DHCP) is the address office it automatically gives your laptop an address whenever it joins a network it's never been on before. grid-dns is a server (static); therefore, it keeps the same IP address.
```

---

## Submission Checklist

- [x] I summarized each concept in my own words, not copied definitions

- [x] I completed the bash-to-PowerShell command table

- [x] I answered all five "In My Own Words" prompts

- [x] This file is committed to my portfolio repo at `week-05/notes.md`

---

*CyberVisionaries Institute — Cyber Foundations, Tier I*
