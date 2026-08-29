# Week 7 Lab 02 — Read the Door Ledger

**Student Name:** Tiffany Haynes

**Date Completed:** 8/29/2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 7  
**Submission Path:** `week-07/labs/lab-02-read-the-door-ledger.md`

> ## Cloud Heights Protected-Rules Safety Rule
> Four baseline rules are protected: **100** (`allow-ssh-from-bastion`), **110** (`allow-icmp-intra-vnet`), **120** (`deny-ssh-student-subnet`), and **1000** (`deny-tcp8080-student-subnet` — Inbound Deny TCP from `10.60.6.0/26` to port `8080`). **You never modify, delete, replace, or use a protected rule as a troubleshooting target.** Create or edit student rules only in priorities **200–999**. The priority **1000** fallback deny sits after your band on purpose, so a narrower Allow you create in 200–999 is evaluated first. A mistake in your student range is recoverable and is not a grading penalty when you diagnose it honestly.

> **Evidence safety:** Never include a Cloud Heights password or Bastion shareable URL. Crop browser address bars and login information before committing screenshots.

---

## Mission

Learn to read inbound and outbound rule ledgers in evaluation order. Translate a rule from field values into plain English, then predict which matching rule makes the decision.

## What You Already Know

A network security rule is a decision about traffic. Rules are evaluated from the lowest priority number to the highest, and the first matching rule wins. Inbound and outbound traffic use separate ledgers. A configured service, a security rule, a test result, and an evidence screenshot answer different questions.

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Cloud Heights → Security Rules |
| Change level | Read-only reasoning |
| Evaluation | Lower priority number first; first match wins |
| Time | 25–30 minutes |

- [x] I am using my assigned `cf-student-XX` VM through the CyberFoundations Lab Portal.

- [x] The VM shows **Running**.

- [x] I can identify the four protected baseline rules at priorities 100, 110, 120, and 1000.

- [x] I understand that my editable priority range is 200–999.

### Cloud Heights Idle Stop

Cloud Heights may warn you that the VM is idle. Return to the Lab Portal and choose **I'm still working** if you are active. If the VM is stopped or deallocated, it was not deleted: restart it from **My Lab Environment**. Your disk files and saved configuration remain.

## Predict First

The priorities **250**, **300**, and **350** used in this lab are **hypothetical examples on paper only — do not create them**. This lab is prediction-only: do not add, edit, or delete rules, and do not run **Test My Rule** unless your instructor tells you to.

Remember the live baseline also contains the protected priority **1000** `deny-tcp8080-student-subnet` fallback (Inbound Deny TCP from `10.60.6.0/26` to port 8080), which is reached only when no earlier rule matches.

Two inbound rules match TCP 8080 from the same source: priority 250 is **Deny** and priority 300 is **Allow**. Predict the verdict before reading further.

```text
Traffic that is routed via TCP port 8080 is going to matched to rule priority 250 which will deny access every time. 250 (Deny) has priority over 300 (allow) so that traffic will never reach its destination unless the priority of the allow rule changes.  
```

## Guided Steps

### Step 1 — Separate the Ledgers

Scroll below the yellow protected-rules summary to the detailed lists headed **INBOUND — EVALUATION ORDER** and **OUTBOUND — EVALUATION ORDER**. Read the inbound list, then the outbound list. Record one sentence explaining why an inbound allow does not automatically create an outbound allow.

```text
An inbound allow does not automatically create an outbound allow because there are variable factors that affect the creation of a rule such as: destination of the traffic, the destination port, protocol and most importantly the priority of the rule. Things that were allowed inbound may be denied when it's outbound.
```

### Step 2 — Translate a Rule

Choose one visible protected rule and translate it using this form:

> Read in the [direction] ledger at priority [number], [allow/deny] [protocol] traffic from [source]:[source port] to [destination]:[destination port].

```text
Read in the inbound ledger at priority 100, allow TCP traffic from 192.168.10.128/26: Port 22 to Cloud Heights VM: Port 22.
```

### Step 3 — Evaluate in Order

For each hypothetical scenario below, list the rules in evaluation order, identify the first matching rule, and state the verdict. These rules are imaginary — do not create them.

1. Priority 250 Deny TCP from `10.60.6.4` to port 8080; priority 300 Allow the same traffic.
2. Priority 300 Allow TCP from `10.60.6.4` to port 8080; priority 350 Deny TCP from any source to port 8080.
3. An inbound Allow exists, but the traffic being evaluated is outbound.

```text
1. Priority 250 Deny TCP from 10.60.6.4 to port 8080; Priority 300 Allow the same traffic.

250 Deny TCP from 10.60.6.4 to port 8080
300 Allow TCP from 10.60.6.4 to port 8080
Priority 250 will be the first matching rule out of the set since it's the lowest number out of the two. The lower the number the higher the priority. All TCP traffic from 10.60.6.4 to port 8080 will be denied access.

2.Priority 300 Allow TCP from 10.60.6.4 to port 8080; priority 350 Deny TCP from any source to port 8080.
300 Allow TCP from 10.60.6.4 to port 8080
350 Deny TCP from Any Source to port 8080
Priority 300 will be the first matching rule out of the set since it's the lowest number out of the two. The lower the number the higher the priority. All traffic from 10.60.6.4 to port 8080 will be allowed access.

3.An inbound Allow exists, but the traffic being evaluated is outbound.
If there are no outbound rules and just an inbound rule to allow access, then the traffic may be subject to the deny all rule. The outbound traffic will be blocked or denied by default.
```

## Stop & Check

If you find yourself reading the Allow you want and ignoring an earlier matching Deny, restart at the lowest priority number. The ledger stops at the first match.

## Test

Use the displayed rule list to predict whether Grid Beacon TCP 8080 would currently be explicitly allowed by a student rule. Do not press **Test My Rule** yet unless your instructor directs you; the service may not be listening.

## Capture Evidence

Capture the detailed **INBOUND — EVALUATION ORDER** view (and **OUTBOUND — EVALUATION ORDER** if your evidence needs it), then name the first matching hypothetical rule and the resulting verdict for each scenario in your worksheet.

## Explain

Write a five-sentence explanation of first-match-wins that a classmate could use without memorizing Azure terminology.

```text
Whenever security rules are created, they are added to the policy in any order. Each rule is assigned a number which is considered the priority. The priority determines the order in which the rule is evaluated. The top priority always starts with the lowest number; for example, rule 100 will be evaluated before rule 200. The top-down processing stops at the first match. The system finds the first matching instruction and executes it immediately. Anything after that first match is irrelevant.
```

## Required Evidence

Save screenshots in `assets/screenshots/week-07/`:

- `week07-lab02-evaluation-order.png`

Open each image at full size before submission. Confirm that no password, Bastion shareable URL, browser address bar, or unrelated private information is visible.

## Analysis Questions

**Analysis Question 1.** If a Deny at priority 300 and an Allow at priority 400 both match, which wins and why? (Minimum 3 sentences.)

```text
If a Deny at priority 300 and an Allow at priority 400 both match, priority 300 wins. The first match principle evaluates and matches to priority 300 first since it's the lowest number out of 300 and 400. The system finds the first matching instruction and executes it immediately.
```

**Analysis Question 2.** Why do inbound and outbound rules have to be reasoned about separately? (Minimum 3 sentences.)

```text
Inbound and outbound rules have to be reasoned separately because the source, the destination, the port, the protocol all affect the behavior of the rule. Some inbound rules may allow or deny certain things that an outbound rule may not. I would think that inbound rules would be stricter than outbound rules since a network would be cautious allowing outside traffic from an unknown and even known source.
```

**Analysis Question 3.** How can an Allow rule be correct by itself but ineffective in the full ledger? (Minimum 3 sentences.)

```text
An allow rule can be correct by itself but ineffective in a full ledger due to priority rules. If the rule has a high priority number such as 300 or 400, then the lower numbers will be evaluated before it gets to the 300 priority. Also, in the ledger there may be a deny rule that evaluated before it gets to the allow rule.
```

## Submission Checklist

- [x] Three scenarios evaluated in order

- [x] One live rule translated to plain English

- [x] Inbound and outbound ledgers distinguished

- [x] `week07-lab02-evaluation-order.png` captured

- [x] Protected priorities 100, 110, 120, and 1000 were not changed.

- [x] Every rule I created or edited used priority 200–999.

- [x] No password, Bastion URL, or browser address bar appears in my files.

- [x] This worksheet is committed to `week-07/labs/lab-02-read-the-door-ledger.md`.

## GitHub / Lab Portal Submission

1. Open **Week 7 → Lab 02: Read the Door Ledger** in the CyberFoundations Lab Portal.
2. Complete every worksheet field and confirm the listed evidence filenames.
3. Upload screenshots to `assets/screenshots/week-07/`.
4. Confirm your portfolio repository is connected, then choose **Submit to GitHub**.
5. Open the committed worksheet and each image on GitHub to verify formatting, legibility, and redaction.

*CyberVisionaries Institute · CyberFoundations · Tier I*
