# Week 6 Lab 05 — Layer Detective

**Student Name:** Tiffany Haynes

**Date Completed:** 8/23/2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 6  
**Submission Path:** `week-06/labs/lab-05-layer-detective.md`

---

## Overview

**This is a SHORT lab — 20 to 30 minutes — and it needs no VM.** No Cloud Heights session, no simulator, no screenshot. This is a thinking lab: you take the evidence you have already collected in Weeks 5 and 6 and sort it into layers.

This is an **independent** lab.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | This worksheet only — nothing to start, nothing to connect to |
| Prerequisite | Week 5 labs and Week 6 Labs 01–04 |
| Screenshot | None required |

---

## Part A — The Seven-Row Table

Fill in every row. For the last column, name one **real thing you personally saw** in Weeks 5–6 that belongs at that layer.

| # | Layer name | One-line job | Real thing from Weeks 5–6 |
| --- | --- | --- | --- |
| 7 | Application | The thing you actually interact with-the page, the request, the shell | The readable HTTP request that I opened in the packet inspector |
| 6 | Presentation | puts data in a shape the other side can use-Formatting, encoding, encryption | The encrypted capture you opened right after the readable one — same tool,  nothing you could read |
| 5 | Session | start, maintain and end conversation | Walking away from your remote shell and coming back to find the prompt still waiting |
| 4 | Transport | Ports and delivery | SSH Port 22, TCP handshake |
| 3 | Network | Gets a message between networks — addresses, masks, gateways, routes | running the commans ip addr & ip route, ping, traceroute |
| 2 | Data link | Gets a message across one local hop — your machine to the thing next to it | The MAC address on your adapter — a hardware address that never leaves your local segment |
| 1 | Physical | Moves raw signal — electricity, light, radio | Every time you plugged in a cable or joined Wi-Fi |

---

## Part B — Case Files

For each case, name the layer where the problem lives, and name the evidence proving the layers **below** it were already working.

### Case File 1 — The Name That Went Nowhere

A hostname lookup fails, but pinging the machine's IP address directly succeeds.

Layer:

```
Layer 7- The application layer, Name resolution protocols such as Domain Name System (DNS) operate at layer 7 (hostname lookup) 
```

Evidence that the layers below were working:

```
The name fails but the ping succeeds. The ping command successfully executing lets you know that the network layer (layer 3) is working. 
```

### Case File 2 — Permission Denied

`ssh` to a host returns `Permission denied` after a password prompt.

Layer:

```
Layer 7- Application
```

Evidence that the layers below were working:

```
You actually got to the machine so layers 1-4 are working. The problem is getting into the machine which is a layer 7 issue. The machine talked to you and said no.
```

### Case File 3 — The Cable Story

A machine reports no link on its interface and has no address at all.

Layer:

```
Layer 1-physical layer
```

Evidence and reasoning:

```
Since there's no link on the interface and the machine has no address at all, there can be a few things that can be causing the problem. The problem could exist with the NIC (network interface card) not getting a signal. This can mean a cable may not be plugged in or there's no wi-fi signal. We know it's not a layer 2 problem since there's no MAC address.
```

### Case File 4 — Ping Works, The Page Does Not

`ping` to a server succeeds, but `curl http://<that server>` returns nothing useful.

Layer:

```
Layer 7- Application
```

Evidence that the layers below were working:

```
Curl is a command that interacts directly with the application so layer 7 is the issue. Getting to the machine was not an issue so layers 1-4 are working properly. 
```

### Case File 5 — Wrong Neighbourhood

A machine has an address, but its default route points somewhere that cannot forward its traffic.

Layer:

```
Layer 3-Network
```

Evidence and reasoning:

```
If a machine has an address, then that means that layer 2 is working. MAC addresses are located on layer 2 of the OSI model.
```

---

## Part C — The Silent Gateway Case

In Lab 03 the Azure default gateway did not answer your ping. However, your VM had a valid default route configured, and your local communication with the Grid Beacon — the ping replies, the HTTP banner, and `TRACE ID: CF-NET-0604` — succeeded.

A failed gateway ping is one piece of evidence — not automatically proof of a gateway or network failure. But the evidence you weigh against it has to be the right kind of evidence.

The Grid Beacon at `10.60.6.4` sits on the same local subnet as your VM (`10.60.6.0/26`). Reaching it proves **local-subnet connectivity** — that traffic never crosses the default gateway, so beacon success alone cannot prove the gateway forwarded anything. Your `ip route` output proves a **default route is configured** — your VM knows where it intends to send non-local traffic — but it does not prove the gateway forwarded that traffic. The evidence that demonstrates the **default path is functioning** is successful communication with a destination outside `10.60.6.0/26`, such as the outbound internet access through NAT that you examined in Lab 04.

### Step 1 — Rule on the Case

Is the failed gateway ping enough evidence to declare a network-layer failure? Explain your answer using the other evidence you collected. In your response, distinguish between:

- evidence that proves **local-subnet connectivity**
- evidence that proves a **default route is configured**
- evidence that supports **successful off-subnet connectivity**

```
A failed gateway ping is not enough evidence to declare a network-layer failure and evidence that proves this follows:
1)The Grid Beacon at 10.60.6.4 sits within the same subnet as the VM 10.60.6.0/26. Evidence shows that I was able to reach the Grid Beacon which means there was local-subnet connectivity. 
2)After running the command ip route it proved that I had a default route configured. All non-local traffic should be routed to 10.60.6.1 the gateway.
3)The VM is able to reach an IP outside of the subnet which was enabled through the use of NAT. The private address of the VM was converted to a public address which is used to connect to the internet; therefore, off-subnet connectivity was successful.
```

### Step 2 — Name the Correct Conclusion

For each of these four results, state what it actually proves: the Grid Beacon at `10.60.6.4` answering, the default route shown by `ip route`, a successful connection to a destination outside your local subnet, and the gateway's failed ping. Then state the rule you would give a junior colleague about the difference between an observation ("the gateway did not answer my ICMP probe") and a diagnosis ("the gateway is broken"):

```
1)Reaching the Grid Beacon 10.60.6.4 proves that route between the host and 10.60.6.4 is working or is operational.
2)The default route is confirmed by running the command ip route. This tells us where all non-local traffic should be routed.
3)A successful connection to a destination outside of the local subnet lets you know that the gateway isn't broken if it's properly routing traffic.
4)The failed gateway ping is to be expected since we know that ICMP ping does not return an answer from Port 22. 

An observation is just a collection of things that we gather to determine a final diagnosis. Never go off of an opinion or assumptions, always rule out everything (one rung at a time) before you determine the final diagnosis. 
```

---

## Part D — Two Models, One Job

The OSI model has seven layers. The practical TCP/IP model most engineers speak day to day has four or five.

### Step 1 — Map Them

Briefly show how the seven OSI layers collapse into the practical model:

```
OSI Model                        VS                 TCP/IP
7-Application                                     Application-Combines Layers 7-5 of the OSI Model
6-Presentation                                    Transport 
5-Session                                         Internet
4-Transport                                       Network-Combines layers 1-2 of the OSI model 
3-Network                                          
2-Data Link
1-Physical                                          
```

### Step 2 — When Each Is Useful

Explain when the seven-layer vocabulary helps and when the practical model is the better tool:

```
The seven-layer vocabulary helps when trying to understand what is going on at each level of the OSI model. The name of each level in the OSI model helps to correlate the issues that may happen at that level and it removes any confusion. The practical model or the TCP/IP model is better tool for troubleshooting.
```

---

## Analysis Questions

**Analysis Question 1.** Explain the Ladder Rule using layer language. What does "test the near thing first" mean when the rungs are layers? *(Minimum 3 sentences.)*

```
In layer language "Test the near thing first" means are you able to send a message within the local network. Starting with the first rung which is "check yourself" (do you have a valid address?), this first rung covers layers 1-2 on the OSI model which are the physical and data-link layers. Once you confirm that you have a valid address, then you move to the second rung which is check your gateway (can you get out at all?), this rung happens on both OSI layers 2-3. Test the near thing first should be addressed on OSI layer 2 when you ping the Gateway. The gateway is within your local network; therefore, it's one of the first and closest thing you test.
```

**Analysis Question 2.** Why is "which layer is this?" a faster question than "what is broken?" when you are under pressure? *(Minimum 3 sentences.)*

```
The question "which layer is this?" is a faster question than "what is broken?" because you are given a concrete place to start investigating or start the process to rule out what is working. The question "what is broken" will have you testing unnecessary things. Once you pinpoint the layer where the problem exists, then you can eliminate or rule out everything that is beneath that layer. Essentially using the question "which layer is this?" will save you time when you are under pressure.
```

**Analysis Question 3.** Pick one case file from Part B and describe the very next command you would run to confirm your ruling, and what result would change your mind. *(Minimum 2 sentences.)*

```
I chose case 1 which is the case where the hostname lookup fails, but pinging the machine's IP address directly succeeds. The very next command I would run after looking up the hostname would be the dig command. The dig command would provide me with the A record and the server who provided the information. By running the dig command, I can confirm that the IP address that I pinged actually matches. If the IP address is different then we have a different issue.
```

---

## Submission Checklist

- [x] All seven rows of the OSI table completed with a real Week 5–6 anchor each (Part A)

- [x] All five case files given a layer and supporting evidence (Part B)

- [x] Silent gateway case ruled on correctly (Part C)

- [x] OSI vs. practical TCP/IP model compared (Part D)

- [x] All three Analysis Questions answered (minimum sentence counts met)

- [x] No screenshot required for this lab

- [x] This file is committed to your portfolio repo at `week-06/labs/lab-05-layer-detective.md`

---

## GitHub Commit Subsection

1. Open **Week 6 → Lab 05: Layer Detective** in the Lab Portal.
2. Fill in the worksheet fields.
3. Click **Submit to GitHub** — the Portal commits to `week-06/labs/lab-05-layer-detective.md`.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
