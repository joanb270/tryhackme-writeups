# 🐧 Linux Fundamentals Part 2 – Writeup (TryHackMe)

## 📌 Overview

- **Platform:** TryHackMe  
- **Room:** Linux Fundamentals Part 2  
- **Difficulty:** Beginner  
- **Category:** Linux / Foundations  
- **Focus:** SSH access, command flags, filesystem operations, users, permissions, and security-relevant directories

This writeup documents what I learned in **Linux Fundamentals Part 2**, written with a **SOC / Blue Team** mindset (verify, document, and think like an investigator).

---

## 🎯 Learning Objectives

By completing this room, I aimed to:

- Understand **SSH** and how remote command execution works
- Use command **flags/switches** and locate help via `--help` and `man`
- Perform core filesystem actions (create/copy/move/delete + identify file types)
- Understand **permissions** (`rwx`) for files and directories
- Understand users/groups basics and switching users (`su`, `su -l`)
- Recognize **security-relevant Linux directories** and their purpose

---

## 🧠 SSH (Secure Shell)

### What SSH is (in simple terms)

SSH is an encrypted protocol used to connect to a remote machine’s command line.  
It allows you to run commands on another system securely over a network.

### Why it matters in cybersecurity (SOC)

- Many servers are administered via SSH (often with no GUI)
- Attackers also target SSH (brute force, stolen keys, misconfigurations)
- SSH activity is commonly visible in authentication logs

### Environment mental model (TryHackMe)

- **AttackBox** = your working machine (like your laptop in the cloud)
- **Target Linux machine** = the remote system you connect to

### SSH login syntax

```bash
ssh username@MACHINE_IP
````

Example:

```bash
ssh tryhackme@MACHINE_IP
```

**Notes:**

* The first time you connect, you may be asked to trust the host key (normal behavior).
* When typing the password, the terminal shows **no characters** (this is normal).

### Post-login verification (SOC habit)

After connecting, it’s good practice to confirm who/where you are:

```bash
whoami
hostname
pwd
```

**SOC relevance:** prevents confusion between local vs remote context and helps document evidence correctly.

---

## 🧩 Flags & Switches (Command Options)

Most Linux commands support arguments called **flags/switches** to modify behavior.

### Example: listing hidden files with `ls`

Default behavior:

```bash
ls
```

Hidden files (starting with `.`) won’t appear by default.

Show everything:

```bash
ls -a
```

Equivalent long form:

```bash
ls --all
```

### Getting help quickly

Most commands support:

```bash
command --help
```

Example:

```bash
ls --help
```

### Man pages (manual pages)

For deeper documentation:

```bash
man command
```

Example:

```bash
man ls
```

**Navigation tips:**

* Press `q` to quit
* Use `/text` to search inside the manual

**SOC relevance:** Being able to self-serve documentation is essential when you encounter unfamiliar commands on real systems.

---

## 🛠️ Filesystem Actions (Create / Copy / Move / Delete / Identify)

### Create empty files: `touch`

```bash
touch note
```

### Create directories: `mkdir`

```bash
mkdir mydirectory
```

### Copy files: `cp`

```bash
cp note note2
```

### Move or rename: `mv`

Rename:

```bash
mv note2 note3
```

Move into a directory:

```bash
mv note3 mydirectory/
```

### Delete files/directories: `rm`

Delete a file:

```bash
rm note
```

Delete a directory recursively:

```bash
rm -R mydirectory
```

⚠️ **Warning:** `rm` is permanent (no recycle bin).

### Identify real file type: `file`

```bash
file suspicious_file
```

**SOC relevance:** file extensions can lie. Always verify file type before executing or trusting it.

---

## 🔐 Permissions 101 (Files vs Directories)

Linux permissions control what actions are allowed for:

* **Owner (user)**
* **Group**
* **Others (everyone else)**

### Viewing permissions with `ls -l`

```bash
ls -lh
```

You’ll see permission strings like:

```text
-rw-r--r--
drwxr-x---
```

### Meaning of `rwx`

* `r` = read (read file / list names in directory)
* `w` = write (modify file / create-delete items in directory)
* `x` = execute (run file / **enter directory**)

### Critical directory rule (common confusion)

For directories:

* `x` means you can **enter** the directory
* Without `x`, you cannot access directory contents properly—even if you have `r`

**SOC relevance:** permission misconfigurations are a major source of data exposure and privilege abuse.

---

## 👤 Users, Groups, and Switching Users (`su`)

### Users vs Groups (simple model)

* **User** = individual identity
* **Group** = a set of users that can share access rules

### Switching users: `su`

```bash
su user2
```

This switches user but may keep parts of the previous environment.

### Switching users with login environment: `su -l`

```bash
su -l user2
```

This simulates a full login as that user (environment variables, home directory, etc.).

**SOC relevance:** switching users (especially to privileged ones) is often part of attacker workflows; understanding it helps investigation.

---

## 🗂️ Common Security-Relevant Directories

### `/etc` — System configuration

* Stores important system configs
* Common security-related files:

  * `/etc/passwd` (user list info)
  * `/etc/shadow` (password hashes; restricted)
  * `/etc/sudoers` (who can run sudo)

**SOC relevance:** changes here often indicate misconfiguration or compromise.

---

### `/var` — Variable data (especially logs)

* Frequently written data by services
* **Logs** are commonly in `/var/log`

**SOC relevance:** logs are primary evidence sources in incident response.

---

### `/root` — Root user home

* Home directory of the `root` user

**SOC relevance:** unexpected tools or files here can indicate privileged activity.

---

### `/tmp` — Temporary files

* Often cleared on reboot
* Typically writable by many users

**SOC relevance:** attackers may stage scripts/tools here during compromise.

---

## 🧠 Key Takeaways (SOC-Focused)

* SSH is the standard way to manage Linux remotely—and a common attack surface
* Flags/switches and man pages reduce reliance on memorization
* File operations (`cp`, `mv`, `rm`) explain both admin workflows and attacker behavior
* Permissions (`rwx`) are foundational for security and access control
* `/etc`, `/var/log`, `/root`, `/tmp` are high-value directories for investigation

---

## 🏁 Conclusion

Linux Fundamentals Part 2 strengthened my ability to work on Linux systems in a realistic way (remote access, permissions, and evidence-oriented navigation). These skills map directly to SOC workflows such as log review, user/account investigation, and identifying suspicious artifacts.

---

## ⚠️ Disclaimer

This writeup is for educational purposes only. It focuses on learning Linux fundamentals from a defensive (SOC/Blue Team) perspective and does not include exploit walkthroughs or sensitive data.

```
```
