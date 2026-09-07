# Week 7 Notes — Cloud Heights: The Guard Post

**Student Name:** Tiffany Haynes

**Week:** 7

## Firewall and Security Group

```text
A firewall is a tool that evaluates network traffic based on a specific set of instructions. That traffic can either be allowed or denied. 
```

## Rule Anatomy

```text
(priority, direction, source, destination, protocol, port, action)
```

## First Match Wins

```text
The system finds the first matching instruction and executes it immediately. Rules are evaluated in order of priority, starting from the lowest numerical value. The lower numerical priority is evaluated first, which it then stops at the first match. 
```

## Least Privilege

```text
Least privilege- required access while limiting everything else. Give enough access- but no more.
Does this rule allow only the traffic required?

90% of security breaches involves some form of unnecessary access. 
```

## Testing and Evidence

```text
Evidence needs context- Observe first. Interpret Second.
1) NSG Rule- Configuration Phase 1- Strategic Planning & Setup
2) Connection Test- Functionality Phase 2- Success Metrics & Execution
3) Network-Flow Record- Visibility Phase 03- Contextual Detail & Insights
```

## Troubleshooting and Remediation

```text
Diagnosis is troubleshooting. Diagnose before Broadening. Identify root cause before permanent fix.
```

## Questions I Still Have

```text
Majority of my questions are from week 6 in regard to the OSI model. I'm still trying to connect what happens at each level. 
```
