# Week 6 Lab 02 — Knocking on Door 22

**Student Name:** Tiffany Haynes

**Date Completed:** 8/22/2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 6  
**Submission Path:** `week-06/labs/lab-02-knocking-on-door-22.md`

---

> ### 🔒 Cloud Heights Security Rule
> Your Bastion link and Cloud Heights password are **private access credentials**. Never paste either into a worksheet, screenshot, GitHub repository, Circle post, or chat message. When taking screenshots, crop out the browser address bar and all login information.

---

## Overview

Week 5 told you SSH is how administrators reach a machine over the network, and that it knocks on **port 22**. This week you knock yourself. You are already inside Cloud Heights through Bastion — now you will open a second, nested SSH session from your machine *to itself* and watch every step of what SSH does before it lets you in.

Starts **guided**, finishes **independent**. Expect 30–40 minutes.

**This lab uses password authentication only.** SSH keys are Week 8. Do not go looking for them yet.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Cloud Heights — live Ubuntu 22.04 VM, reached through Azure Bastion |
| Username | `analyst` |
| Password | Provided separately. Never typed into this worksheet. |
| Commands used | `ssh`, `whoami`, `hostname`, `pwd`, `exit` |
| Prerequisite | Week 6 Lab 01 completed |

**Before you start:** open **My Lab Environment**, start your VM if needed, wait for **Running**, then open Cloud Heights.

---

## Part A — Two Ways Into the Same Room

### Step 1 — Name the Path You Already Used

You reached Cloud Heights through a browser session. Something else handled the network hop for you.

Describe, in your own words, what the Bastion/browser path did on your behalf:

```
The bastion/browser helped me to reach the machine or start a connection. The bastion also helped me to prove my identity by sending my credentials.
```

### Step 2 — Predict the Manual Path

You are about to type an SSH command by hand. Before you run it, write what you expect to happen and what you expect to be asked for:

```
Once the SSH command is executed, I expect for the screen to give me an output that states whether or not access was granted and/or authorization is required. I may be asked for my username and password.
```

---

## Part B — Knocking

### Step 1 — Run the SSH Command

In your Cloud Heights terminal, run:
```
ssh analyst@localhost
```

**Stop before typing anything else.**

### Step 2 — Read the First-Connection Prompt

The first time SSH connects to a host it has never seen, it shows you the host's **fingerprint** and asks whether you want to continue connecting. This is not an error. It is SSH telling you: *I have no record of this machine yet — do you recognise it?*

Paste the prompt you saw (fingerprint line included — a fingerprint is not a credential):

```
Last login: Fri Aug 21 01:22:57 2026 from 192.168.10.134
analyst@cf-student-14:~$ ssh analyst@localhost
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ED25519 key fingerprint is SHA256:ofoya/v5AJ4fq768x8CzJhMhec+A16GrbNdszTd96zY.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? 
```

Explain why you were willing to answer `yes` here — what makes this an expected first connection rather than a suspicious one:

```
I'm willing to answer yes to the question "are you sure you want to continue connecting" since I know this is the very first time connecting and the machine doesn't know the host. This is not suspicion just routine. Going forward, I won't be prompted with that question since there will be an established or saved fingerprint for the host. 
```

### Step 3 — Enter Your Password

Type `yes`, then enter your password when prompted.

**Linux does not echo password input.** No characters, no dots, no asterisks appear as you type. The terminal looks frozen. It is not — type the password and press Enter.

What did the screen show while you typed:

```
The screen showed the white cursor, but it didn't move as I typed the password.
```

### Step 4 — Prove You Are in the Nested Session

Inside the new session run each of these and record the output:
```
whoami
```

```
analyst
```

```
hostname
```

```
cf-student-14
```

```
pwd
```

```
/home/analyst
```

### Step 5 — Notice the Prompt

Compare the prompt now to the prompt before you ran `ssh`. Describe anything that changed and anything that looks identical, and explain why it looks that way given where you connected to:

```
The only noticeable change I recognized was the machine name changed from analyst@localhost to analyst@cf-student-14. Since we were not connecting to a remote host but rather local host the destination name changed to the actual host name.
```

### Step 6 — Capture Your Evidence

Screenshot the terminal showing the first-connection prompt and the successful session.

**Required filename:** `ssh-first-connection.png`

**Crop rules.** No Bastion URL, no address bar, no password field, no login screen. The fingerprint text is fine.

### Step 7 — Leave

Run:
```
exit
```
What did the prompt look like after exiting, and how do you know you are back in the original session:

```
After typing exit, the screen states "logout" on one line and then the next line states "Connection to localhost closed". If you're no longer connected to the localhost, then you are in the original session.
```

---

## Part C — The Deliberate Failure (Independent)

### Step 1 — Knock With the Wrong Name

Run an SSH command to `localhost` using a username that does not exist on this machine — for example `ssh notauser@localhost`. Enter anything at the password prompt.

Command you ran:

```
ssh cybergurl@localhost
```

Output:

```
analyst@cf-student-14:~$ ssh cybergurl@localhost
cybergurl@localhost's password: 
Permission denied, please try again.
cybergurl@localhost's password: 
```

### Step 2 — Read the Failure Correctly

`Permission denied` is a **failure of authentication**, not a failure of the network.

Explain what the network and SSH already had to do successfully in order for you to be told "permission denied" at all:

```
The network had to have a name resolve first. The SSH had to be listening in order for you to be told "permission denied". The password was probably entered incorrectly and that's why you ended up with the "permission denied" reply.

```

---

## Analysis Questions

**Analysis Question 1.** Distinguish *reach* from *authentication*. Which one had already succeeded when you saw a password prompt, and how do you know? *(Minimum 3 sentences.)*

```
When using SSH, there's one door with two locks. The first lock is how you reach the machine. Reaching the machine means that a name resolves, a path exists, and something is listening on port 22. The second lock is how you get into the machine. The second lock is the authentication process. You show the machine that you are who you claim to be.
```

**Analysis Question 2.** You accepted a host fingerprint today because you knew you had just connected to your own machine. Describe a situation where accepting a fingerprint without thinking would be a real problem. *(Minimum 3 sentences.)*

```
I couldn't think of a situation off the top of my head; therefore, I used Google to help me think of a situation where accepting a host's fingerprint without thinking could lead to a real problem. Google stated that accepting an SSH host fingerprint without checking it leaves you open to a man-in-the-middle attack. An attacker can intercept your network connection. They can steal your passwords, private keys, or sensitive data while pretending to be the real server.
```

**Analysis Question 3.** What changed and what stayed the same when you moved from the outer session into the nested SSH session, and why? *(Minimum 2 sentences.)*

```
The name of the destination changed from localhost to cf-student-14; however, the host is still the same host. The destination is loopback which means that the machine turns around and connects to itself.
```

**Analysis Question 4.** A colleague says "SSH is broken, I got permission denied." Using only what you learned in this lab, what would you tell them is already working, and what would you check next? *(Minimum 3 sentences.)*

```
If a colleague says "SSH is broken, I got permission denied, I will tell them that the first lock or step in the SSH process is done. The first step of the SSH process makes sure the name resolves, that there's a clear path, and port 22 is listening. The second lock which is the authentication lock needs to be proven. I will tell them to make sure they are providing the correct password/credentials to the machine. 
```

---

## Submission Checklist

- [x] Bastion path vs. manual SSH path described (Part A)

- [x] `ssh analyst@localhost` run and the first-connection prompt recorded (Part B, Steps 1–2)

- [x] Password entered; non-echoing input observed and described (Part B, Step 3)

- [x] `whoami`, `hostname`, `pwd` run inside the nested session (Part B, Step 4)

- [x] Prompt change described (Part B, Step 5)

- [x] `ssh-first-connection.png` captured, cropped, uploaded to `assets/screenshots/week-06/` (Part B, Step 6)

- [x] Session exited cleanly (Part B, Step 7)

- [x] Bad-username test run and `Permission denied` output recorded (Part C)

- [x] All four Analysis Questions answered (minimum sentence counts met)

- [x] This file is committed to your portfolio repo at `week-06/labs/lab-02-knocking-on-door-22.md`

---

## GitHub Commit Subsection

1. Open **Week 6 → Lab 02: Knocking on Door 22** in the Lab Portal.
2. Fill in the worksheet fields and upload `ssh-first-connection.png` to `assets/screenshots/week-06/`.
3. Click **Submit to GitHub** — the Portal commits to `week-06/labs/lab-02-knocking-on-door-22.md`.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
