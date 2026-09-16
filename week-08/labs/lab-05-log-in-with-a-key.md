# Week 8 Lab 05 — Log In with a Key

**Student Name: Tiffany Haynes 
**Date Completed: September 15, 2026  
**Module:** 3 — Practical Cryptography  
**Submission Path:** `week-08/labs/lab-05-log-in-with-a-key.md`

> ## STOP — Protect secrets
> Never paste, upload, commit, or screenshot a password, passphrase, or private key. Do not run `cat` on any private-key file. Submit only the worksheet and the named screenshots. Keep all cryptographic files on the VM.

## How to Use This Lab

- **[TERMINAL]** means type or paste the command into the Cloud Heights terminal.
- **[WORKSHEET]** means type your response in this lab worksheet.
- Run commands in the order shown. Do not type the sample output.
- If your result does not match the stated success check, stop at the Troubleshooting box. Do not improvise with `sudo`, package installation, Azure settings, or SSH server configuration.

## Mission

Authorize the public key created in Lab 03 and prove that `analyst@localhost` authenticates with that key without falling back to the account password.

## What You Already Know

In Week 6, SSH used the `analyst` account password. Here, the server stores the public key in `authorized_keys`, while the private key stays in the client account. The same user, protocol, and destination remain; only the authentication method changes.

## Lab Environment / Pre-Lab Check

**Required prerequisite:** Complete Lab 03 first.

**[TERMINAL] Run:**

```bash
whoami
cf-week8-check
test -f ~/.ssh/week8_analyst_ed25519 &&   test -f ~/.ssh/week8_analyst_ed25519.pub &&   echo "READY: Lab 03 key pair exists" ||   echo "STOP: complete Lab 03 before continuing"
```

**Continue only if:** `whoami` prints `analyst`, all checks report `PASS`, and the final line begins with `READY`.

### If the VM Stops

Return to **My Lab Environment** in the Lab Portal and start your assigned VM. A stopped or deallocated VM is not deleted; saved disk files remain.

## Predict First

**[WORKSHEET]** Does the private key need to be copied into `authorized_keys`? Explain.

```text
The private key should never be copied into "authorized_keys" only the public key. Due to security reasons the private key should be kept local or within the sole custody of the owner. The private key shouldn't be shared, uploaded or posted anywhere.
```

## Guided Steps

### Step 1 — Prepare `authorized_keys`

**[TERMINAL] Run:**

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

### Step 2 — Add the Week 8 Public Key Once

**[TERMINAL] Run:**

```bash
grep -qxF "$(cat ~/.ssh/week8_analyst_ed25519.pub)" ~/.ssh/authorized_keys   || cat ~/.ssh/week8_analyst_ed25519.pub >> ~/.ssh/authorized_keys
```

This command adds the key only if an identical line is not already present.

### Step 3 — Confirm the Entry and Safe Permissions

**[TERMINAL] Run:**

```bash
grep -c 'week8-analyst-key$' ~/.ssh/authorized_keys
stat -c '%a %n' ~/.ssh ~/.ssh/authorized_keys
```

**Required result:** The count is `1`; permissions show `700` for `.ssh` and `600` for `authorized_keys`.

**Evidence moment:** Capture this result as `week08-lab05-authorized-key-permissions.png`.

### Step 4 — Run the Public-Key-Only Proof

**[TERMINAL] Run:**

```bash
ssh   -o PreferredAuthentications=publickey   -o PasswordAuthentication=no   -i ~/.ssh/week8_analyst_ed25519   analyst@localhost   'echo "AUTH_TEST=PUBLICKEY_SUCCESS"; whoami; hostname'
```

If SSH asks whether to trust the host fingerprint, type `yes` and press Enter. If no fingerprint question appears, continue; `localhost` was already known. Do not delete `known_hosts` to force the question.

At `Enter passphrase for key`, enter the **Lab 03 key passphrase**. Do not enter the `analyst` account password.

**Required result:**

```text
AUTH_TEST=PUBLICKEY_SUCCESS
analyst
cf-student-XX
```

The final hostname will contain your assigned VM number instead of `XX`.

**Evidence moment:** Capture the three result lines as `week08-lab05-publickey-auth-success.png`.

## Stop & Check

If the command asks for the `analyst` account password, reports `Permission denied`, or does not print `AUTH_TEST=PUBLICKEY_SUCCESS`, do not submit. The options in the proof command disable account-password fallback for this one test only; they do not change the VM server.

### Troubleshooting

1. Run `grep -c 'week8-analyst-key$' ~/.ssh/authorized_keys`; required value is `1`.
2. Run `stat -c '%a %n' ~/.ssh ~/.ssh/authorized_keys`; required values are `700` and `600`.
3. Run `ssh-keygen -lf ~/.ssh/week8_analyst_ed25519.pub`; it must show the Lab 03 key.
4. If any check fails, stop and contact the instructor. Do not edit `/etc/ssh/sshd_config`, use `sudo`, disable password authentication, or delete `known_hosts`.

## Explain

**[WORKSHEET]** In 4–5 sentences, compare Week 6 password authentication with this key-based test. State what changed and what stayed the same.

```text
(write your explanation here)
There are two different SSH authentication processes used for week 6 (password) and week 8 (public key challenge). In week 6, the account password travels through the SSH connection for authentication. In week 8, the challenge and proof cross the connection, but the private key never travels and is never on the server. The server stores only the authorized public key and the analyst keeps the private key locally. The things that stayed the same for both weeks are the analyst account, same VM, same SSH protocol, same localhost destination.
```

## Analysis Questions

1. What changed between the Week 6 and Week 8 SSH logins? The SSH authentication process changed between week 6 and 8 SSH logins. In week 6, the account password travels through the SSH connection for authentication. In week 8, the challenge and proof cross the connection, but the private key never travels and is never on the server.
2. What stayed the same? The things that stayed the same for both weeks are the analyst account, same VM, same SSH protocol, same localhost destination.
3. Why does `authorized_keys` contain the public key rather than the private key? The public key is designed to be distributed whenever it is needed for an interaction; therefore, it is the key that is placed in authorized_keys. Authorized_keys is the public key's registry. The private key is not to be shared, uploaded or emailed; therefore, it is to be kept under the owner's sole exclusive control.
4. Why is the forced proof stronger evidence than a normal successful SSH login? The force proof is stronger evidence than a normal successful SSH login because it forces the public-key method to confirm proof was made — without exposing private key material. 
5. How is the key passphrase different from the `analyst` account password? The passphrase protects your local key file. The analyst account password authenticates the analyst to the server. The server never sees your passphrase.

## Required Evidence

- `assets/screenshots/week-08/week08-lab05-authorized-key-permissions.png`
- `assets/screenshots/week-08/week08-lab05-publickey-auth-success.png`

## Submission Checklist

- [x ] Lab 03 was completed first.
- [x ] The authorized-key count is exactly `1`.
- [x ] Permissions are `700` and `600`.
- [x ] The public-key-only proof printed all three required lines.
- [x ] Both screenshots use the exact filenames.
- [x ] No password, passphrase, private key, or Bastion URL appears.
- [x ] Every worksheet response is complete.

## GitHub / Lab Portal Submission

1. In the Lab Portal, open the matching Week 8 lab.
2. Complete every **[WORKSHEET]** response.
3. Upload only the required screenshots to `assets/screenshots/week-08/`.
4. Select **Submit to GitHub**.
5. Open the committed worksheet and screenshots on GitHub. Confirm they are readable and contain no secrets.

**Never submit:** `.pem` files, files from `~/.ssh/`, passwords, passphrases, private-key contents, or a Bastion shareable URL.
