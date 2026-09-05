# Week 6 Lab 04 — Reading the Blueprints

**Student Name:** Tiffany Haynes

**Date Completed:** 8/23/2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 6  
**Submission Path:** `week-06/labs/lab-04-reading-the-blueprints.md`

---

> ### 🔒 Cloud Heights Security Rule
> Your Bastion link and Cloud Heights password are **private access credentials**. Never paste either into a worksheet, screenshot, GitHub repository, Circle post, or chat message. When taking screenshots, crop out the browser address bar and all login information.

---

## Overview

**This is a SHORT lab — 15 to 20 minutes.** It is deliberately small. You already have the commands; this lab is about matching a drawing to reality.

The **Cloud Heights Network Blueprint** is displayed at the top of this lab page in the portal. Everything you write about the network's architecture comes from that blueprint or from your own machine — never from a guess.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Cloud Heights — live Ubuntu 22.04 VM, reached through Azure Bastion |
| Source of truth | The Cloud Heights Network Blueprint shown at the top of this lab page |
| Commands used | `ip addr`, `ip route` |
| Known value | Student subnet: **`10.60.6.0/26`** |

---

## Part A — Read the Drawing

### Step 1 — Record the Architecture Values

From the blueprint at the top of this page, record each value **exactly as drawn**. If a value is not shown on the blueprint, write "not shown on blueprint" — do not guess.

| Item | Value from the blueprint |
| --- | --- |
| VNet name | vnet-cf-labs |
| VNet address space | 10.60.6.0/24 |
| Student subnet range | 10.60.6.0/26 |

---

## Part B — Verify Against Your Own Machine

### Step 1 — Confirm Your Address Lives in the Subnet

Run `ip addr` and find your private IPv4 address.

Command and output:

```
analyst@cf-student-14:~$ ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 7c:ed:8d:55:bb:c2 brd ff:ff:ff:ff:ff:ff
    inet 10.60.6.33/26 metric 100 brd 10.60.6.63 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::7eed:8dff:fe55:bbc2/64 scope link 
       valid_lft forever preferred_lft forever
3: enP58206s1: <BROADCAST,MULTICAST,SLAVE,UP,LOWER_UP> mtu 1500 qdisc mq master eth0 state UP group default qlen 1000
    link/ether 7c:ed:8d:55:bb:c2 brd ff:ff:ff:ff:ff:ff
    altname enP58206p0s2
```

Your private IP:

```
10.60.6.33/26
```

Explain how you know your address falls inside `10.60.6.0/26` — what range does that prefix actually cover:

```
Both addresses are in the same building and on the same floor, they are just in different rooms.
Range: 10.60.6.0-10.60.6.63
My private address 10.60.6.33/26 falls within the range.

```

### Step 2 — Confirm Route Behaviour

Run `ip route`.

Command and output:

```
analyst@cf-student-14:~$ ip route
default via 10.60.6.1 dev eth0 proto dhcp src 10.60.6.33 metric 100 
10.60.6.0/26 dev eth0 proto kernel scope link src 10.60.6.33 metric 100 
10.60.6.1 dev eth0 proto dhcp scope link src 10.60.6.33 metric 100 
168.63.129.16 via 10.60.6.1 dev eth0 proto dhcp src 10.60.6.33 metric 100 
169.254.169.254 via 10.60.6.1 dev eth0 proto dhcp src 10.60.6.33 metric 100 
```

What the default route tells you about traffic that is not destined for your own subnet:

```
The default route tells you that the any traffic that gets routed outside of my own subnet goes through the gateway 10.60.6.1.
```

### Step 3 — Capture Your Evidence

**Required filename:** `blueprint-verified.png`

This must be **your own `ip addr` and `ip route` output** — not a re-screenshot of the blueprint. Crop out the address bar and any login information.

---

## Part C — How Traffic Actually Moves

### Step 1 — No Public IP

Your VM has a private address and **no public IP**. Explain what that means for who can reach it directly from the internet:

```
Since the VM has a private address and no public IP that means no one can reach it directly from the internet. The only way someone would be able to reach it is if they built a direct path to the VM.
```

### Step 2 — Outbound vs. Inbound

Outbound internet traffic from your VM leaves through address **translation (NAT)**. Inbound access for you arrives through **Azure Bastion**, not through a public address on the VM.

Explain both directions in your own words:

```
So that a machine (VM) with a private address can hold a conversation on the public internet; the VM uses the Network Address Translation (NAT) which converts the private address to a shared public address as it leaves the machine to reach the internet. For inbound, no can reach the VM's private IP address directly; instead, the Azure Bastion is like your secured front desk which is the only way in to reach your machine. There is no public address used to reach the machine.
```

### Step 3 — The Guard Post You Do Not Touch Yet

Each student machine sits behind its own **network security group** — a per-student guard post that decides what traffic is allowed in.

**In Week 6 you do not configure it.** Week 7 is when you take control of those rules.

Write one sentence naming what the guard post does and one sentence stating what you are *not* doing with it this week:

```
The Network security group or the guard post is responsible for evaluating the traffic based on a specific set of instructions. The guard post will either allow or deny access based on those instructions. This week we are not creating or configuring any rules for the guard post to evaluate traffic against.
```

---

## Analysis Questions

**Analysis Question 1.** Why would an organization put every student machine in one small subnet instead of giving each machine a public address? *(Minimum 3 sentences.)*

```
An organization would want to put every student machine in one small subnet instead of giving each machine a public address for two reasons: security purposes and organization. By grouping or keeping all the student machines on one small subnet reduces configuration errors. For security purposes, forcing traffic to cross a boundary before crossing over into another segment, there's a limit as to how far an issue can travel. The boundaries are where defenders can act: inspect, allow or deny traffic. 
```

**Analysis Question 2.** Segmentation means separating a network into parts that cannot freely reach each other. Give one concrete benefit of segmentation during a security incident. *(Minimum 3 sentences.)*

```
When a network is segmented into subnets, every boundary between floors becomes a control point. Defenders can inspect, allow, or deny traffic exactly where it crosses from one segment to another — rather than letting it flow unchecked through an open space. One concrete benefit of segmentation during a security incident is that the attackers would only have access to one segmented area and not to other areas within the system.

```

**Analysis Question 3.** A diagram and a live machine disagree about an address range. Which do you trust, what do you do next, and why? *(Minimum 2 sentences.)*

```
If a diagram and a live machine disagree about an address range, I would trust what the live machine states is the correct address range.  On the live machine, I would run the two commands ip addr and ip route which confirms what any diagram asserts. A diagram is just a claim about reality, and the live machine is the true source.
```

---

## Submission Checklist

- [x] VNet name, address space, and subnet range recorded from the blueprint (Part A)

- [x] `ip addr` run and own private IP confirmed inside `10.60.6.0/26` (Part B, Step 1)

- [x] `ip route` run and default route behaviour explained (Part B, Step 2)

- [x] `blueprint-verified.png` captured from your own terminal, cropped, uploaded to `assets/screenshots/week-06/` (Part B, Step 3)

- [x] Private address / NAT / Bastion explained (Part C, Steps 1–2)

- [x] Per-student guard post identified — and explicitly not configured this week (Part C, Step 3)

- [x] All three Analysis Questions answered (minimum sentence counts met)

- [x] This file is committed to your portfolio repo at `week-06/labs/lab-04-reading-the-blueprints.md`

---

## GitHub Commit Subsection

1. Open **Week 6 → Lab 04: Reading the Blueprints** in the Lab Portal.
2. Fill in the worksheet fields and upload `blueprint-verified.png` to `assets/screenshots/week-06/`.
3. Click **Submit to GitHub** — the Portal commits to `week-06/labs/lab-04-reading-the-blueprints.md`.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
