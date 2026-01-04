# 🐧 Linux Fundamentals Part 1 – Writeup (TryHackMe)

## 📌 Overview

- **Platform:** TryHackMe  
- **Room:** Linux Fundamentals Part 1  
- **Difficulty:** Beginner  
- **Category:** Linux Fundamentals  
- **Goal:** Build core Linux CLI skills needed for cybersecurity (SOC/Blue Team)

This writeup documents what I learned in **Linux Fundamentals Part 1** from a **security-first** perspective. The focus is not just completing tasks, but understanding how each command relates to real-world investigation, evidence handling, and system navigation.

---

## 🎯 Learning Objectives

By the end of this room, I aimed to be comfortable with:

- Understanding where Linux is used in the real world
- Using the terminal to interact with a Linux system
- Navigating the filesystem efficiently
- Reading files and understanding basic file operations
- Searching for files and searching inside files
- Practicing a SOC-style mindset: **verify, don’t assume**

---

## 🧠 Why Linux Matters in Cybersecurity

Linux is a core OS in cybersecurity because it powers:

- Web servers and web applications
- Cloud infrastructure and containers
- Security tools and monitoring systems
- Many enterprise back-end systems

For SOC/Blue Team work, Linux knowledge is required to:
- locate logs and configuration files quickly
- validate system state from the command line
- identify suspicious files and misconfigurations
````markdown

## 📂 Filesystem Navigation (Core CLI)

Linux investigation often starts with answering: **Where am I? What’s here? Where should I go next?**  
These commands are the foundation for that workflow.

### ✅ Commands Used

| Command | Meaning | What it does |
|---|---|---|
| `pwd` | Print Working Directory | Shows the full path of your current location |
| `ls` | List | Lists files and directories in the current location |
| `cd` | Change Directory | Moves you into another directory |

---

### `pwd` — Where am I?

```bash
pwd
````

**Why it matters (SOC):**
When you’re investigating a system, knowing your exact path prevents mistakes (e.g., editing or deleting the wrong file) and helps you document evidence properly.

---

### `ls` — What’s here?

```bash
ls
```

**Why it matters (SOC):**
You quickly identify what exists in a directory: scripts, logs, configs, suspicious files, or unexpected folders.

---

### `cd` — Move around

```bash
cd Documents
cd /var/log
cd ..
```

* `cd Documents` moves into `Documents`
* `cd /var/log` moves to an absolute path
* `cd ..` moves one level up

**Why it matters (SOC):**
A SOC analyst often needs to jump to log directories, application folders, and user home directories quickly during triage.

---

## 📄 Reading Files (Basic Evidence Handling)

A lot of investigation involves reading files safely (without executing anything).

### `cat` — Display file contents

```bash
cat todo.txt
```

**Why it matters (SOC):**
Config files, notes, logs, and sometimes even exposed credentials can be found in plain text. `cat` is a quick way to confirm what a file contains.

````markdown
## 🔍 Searching for Files (Fast Enumeration)

When working on real systems, manually browsing every folder is inefficient. Linux provides built-in tools to locate files quickly.

### `find` — Locate files by name

Find a specific file (when you know the name):

```bash
find -name passwords.txt
````

Find files by extension using a wildcard:

```bash
find -name "*.txt"
```

**Why it matters (SOC):**

* Quickly locate potentially sensitive files (e.g., `passwords.txt`, backups, configs)
* Speed up triage when investigating user directories or application folders
* Useful in post-compromise analysis to identify dropped tools or artifacts

---

## 🔎 Searching Inside Files (Log & IOC Hunting)

Logs can be large. Reading them line-by-line isn’t realistic. `grep` allows filtering for specific indicators such as IPs, usernames, error messages, or suspicious endpoints.

### `grep` — Search for text inside files

Search for a specific value (e.g., an IP address) in a log file:

```bash
grep "81.143.211.90" access.log
```

**Why it matters (SOC):**

* Locate indicators of compromise (IOCs) in logs
* Track suspicious IP activity
* Find authentication failures or unusual requests quickly

---

## ⚙️ Linux Operators (Efficiency Boosters)

Linux operators help combine commands, run processes in the background, and redirect output.

### `&` — Run a command in the background

```bash
some_command &
```

**Use case (SOC):**
Run a long command while continuing analysis in the same terminal session.

---

### `&&` — Chain commands (only run the next if the first succeeds)

```bash
command1 && command2
```

**Use case (SOC):**
Safer automation. Example: only search logs if the directory change worked.

---

### `>` — Redirect output (overwrite)

```bash
echo "hello" > welcome
```

* Creates `welcome` if it doesn’t exist
* Overwrites the file if it already exists

**Use case (SOC):**
Capture outputs into a file to document evidence.

---

### `>>` — Redirect output (append)

```bash
echo "another line" >> welcome
```

* Appends to the file without deleting previous content

**Use case (SOC):**
Build investigation notes incrementally without overwriting prior results.

````markdown
## 🛠️ File & Directory Management (Create, Move, Copy, Delete)

These commands are used constantly in Linux administration and security work. Understanding them helps with investigation (handling evidence) and also understanding attacker behavior (dropping tools, moving payloads, deleting traces).

### `touch` — Create an empty file

```bash
touch note
````

* Creates a blank file named `note` (or updates its timestamp if it already exists)

**Security relevance (SOC):**
Attackers may create files to store output, scripts, or temporary data. Seeing unexpected new files can be a clue.

---

### `mkdir` — Create a directory (folder)

```bash
mkdir mydirectory
```

**Security relevance (SOC):**
New directories may be used for persistence, staging tools, or hiding data (sometimes with dot-prefixed names like `.hidden`).

---

### `cp` — Copy files or directories

Copy a file:

```bash
cp note note2
```

**Security relevance (SOC):**
Copying is useful to preserve evidence (e.g., copying logs for review) without modifying the original file.

---

### `mv` — Move or rename files/directories

Rename a file:

```bash
mv note2 note3
```

Move a file into a directory:

```bash
mv note3 mydirectory/
```

**Security relevance (SOC):**
Attackers may rename files to disguise them (e.g., pretending a script is an image). Renaming also affects investigation timelines.

---

### `rm` — Remove files (and directories with flags)

Remove a file:

```bash
rm note
```

Remove a directory recursively:

```bash
rm -R mydirectory
```

⚠️ **Warning:** `rm` permanently deletes (no recycle bin).
**Security relevance (SOC):**
Attackers often delete artifacts and logs to cover tracks. Unexpected deletions can indicate malicious activity.

---

## 🧾 Determine the Real File Type

Extensions can be misleading in Linux. The `file` command checks what a file actually is.

### `file` — Identify file type

```bash
file note
file suspicious_file
```

**Security relevance (SOC):**
A file named `photo.jpg` might actually be a script or binary. Always verify before interacting or executing.

---

## 🧠 Key Takeaways (Security-Focused)

* Linux CLI skills are essential for SOC/Blue Team work
* Efficient navigation and search (`pwd`, `ls`, `cd`, `find`, `grep`) speed up investigations
* Operators (`&`, `&&`, `>`, `>>`) help automate and document analysis
* File operations (`touch`, `mkdir`, `cp`, `mv`, `rm`) explain both admin workflows and attacker behaviors
* File extensions are not trustworthy—use `file` to confirm file types

---

## 🏁 Conclusion

Linux Fundamentals Part 1 established a solid foundation for working confidently in Linux environments. These skills directly support real SOC tasks such as log triage, evidence gathering, and basic system investigation.

---

## ⚠️ Disclaimer

This writeup is for educational purposes only and focuses on learning Linux fundamentals from a defensive (SOC/Blue Team) perspective.

```
```

