# Linux Fundamentals Part 1

**Room:** TryHackMe — Linux Fundamentals Part 1  
**Path:** Cyber Security 101 → Linux Fundamentals  
**Completed:** April 2026  
**Difficulty:** Easy  
**Badge earned:** 🐧 cat linux.txt  
**Over 1.1 million people have completed this room**  

---

## My starting point

I'll be honest — before this room, opening a black terminal window and typing commands into it felt intimidating. I'd seen people do it in films and YouTube videos and it always looked like they were doing something impossibly technical.

After this room, it doesn't feel that way anymore. The terminal is just another way of talking to a computer. Instead of clicking icons, you type instructions. That's it.

---

## Why Linux matters for SOC work

Linux powers most of the internet. Web servers, cloud infrastructure, security tools, SIEM backends, firewalls — most of it runs on Linux. As a SOC analyst I'll be spending a lot of time in Linux terminals, reading log files, running queries, and investigating incidents. There's no way around it.

This room was my first proper session in a real Linux terminal. Not a simulator. An actual Ubuntu machine running in the cloud, accessible through my browser.

---

## What I learned, task by task

### Task 2 — Background on Linux

Linux isn't one thing — it's a family of operating systems all based on UNIX. The specific version matters: Ubuntu, Debian, Kali all do different things. This series uses Ubuntu, which is the most common in enterprise environments.

One fact that surprised me: Ubuntu Server can run on just **512MB of RAM**. That's why it's used everywhere — it's incredibly lightweight.

> **Q: What year was the first Linux OS released?**  
> **A: 1991** — created by Linus Torvalds

---

### Task 3 — My First Linux Machine

Clicked "Start Machine" and a real Ubuntu terminal appeared in my browser. The machine details:

```
Title: linuxfundpt1
IP: 10.10.144.238
```

I'm connected to a real machine hosted in TryHackMe's cloud. Everything I do in this terminal happens on that machine, not mine. That's important — if I mess something up, it doesn't affect my laptop.

---

### Task 4 — First Commands

This is where I typed my very first Linux commands:

**`echo`** — prints text to the screen
```bash
tryhackme@linux1:~$ echo Hello
Hello

tryhackme@linux1:~$ echo "Hello Friend!"
Hello Friend!
```

One thing to note: single words don't need quotes. Anything with spaces needs to be in double quotes.

**`whoami`** — tells you which user you're logged in as
```bash
tryhackme@linux1:~$ whoami
tryhackme
```

> **Q: Command to output "TryHackMe"?** → `echo TryHackMe`  
> **Q: What username am I logged in as?** → `tryhackme`

---

### Task 5 — Interacting With the Filesystem

The four most essential commands for navigating Linux:

| Command | Full name | What it does |
|---|---|---|
| `ls` | listing | Show what's in the current directory |
| `cd` | change directory | Move into a folder |
| `cat` | concatenate | Read a file |
| `pwd` | print working directory | Show exactly where I am |

**Real examples I ran:**

```bash
# See what folders exist
tryhackme@linux1:~$ ls
'Important Files'  'My Documents'  Notes  Pictures

# Move into a folder
tryhackme@linux1:~$ cd Pictures
tryhackme@linux1:~/Pictures$

# List what's inside without navigating there
tryhackme@linux1:~$ ls Pictures
dog_picture1.jpg  dog_picture2.jpg  dog_picture3.jpg  dog_picture4.jpg

# Read a file
tryhackme@linux1:~/Documents$ cat todo.txt
Here's something important for me to do later!

# Find out exactly where I am
tryhackme@linux1:~/Documents$ pwd
/home/ubuntu/Documents
```

The practical exercise for this task was actually finding things on the deployed machine:

> **Q: How many folders on the Linux machine?** → **4**  
> **Q: Which directory contains a file?** → **folder4**  
> **Q: What are the file's contents?** → **Hello World!**  
> **Q: What is the path after navigating to folder4?** → **/home/tryhackme/folder4**

---

### Task 6 — Searching for Files

This is where things got genuinely useful.

**`find`** — searches for files without manually navigating everywhere

```bash
# Find a specific file by name
tryhackme@linux1:~$ find -name passwords.txt
./folder1/passwords.txt

# Find all .txt files (using wildcard *)
tryhackme@linux1:~$ find -name *.txt
./folder1/passwords.txt
./Documents/todo.txt
```

**`grep`** — searches the *contents* of files

The problem: `access.log` has 244 lines. Reading through all of them manually would take ages.

```bash
# Count lines first
tryhackme@linux1:~$ wc -l access.log
244 access.log

# Search for a specific IP
tryhackme@linux1:~$ grep "81.143.211.90" access.log
81.143.211.90 - - [25/Mar/2021] "GET / HTTP/1.1" 200 417

# Search recursively through ALL files in a directory
tryhackme@linux1:~$ grep -R "PRETTY_NAME" /etc/
/etc/os-release:PRETTY_NAME="Ubuntu"
```

For the practical exercise, I used grep to find a hidden flag in access.log:

```bash
grep "THM" /home/tryhackme/access.log
```

> **Flag found: THM{ACCESS}**

This is the moment grep became my favourite command. 244 lines, found in under a second.

---

### Task 7 — Shell Operators

Four operators that make commands much more powerful:

| Operator | What it does |
|---|---|
| `&` | Run a command in the background |
| `&&` | Run two commands in sequence (second only runs if first succeeds) |
| `>` | Write output to a file (overwrites everything) |
| `>>` | Append output to a file (keeps existing content) |

**The difference between `>` and `>>` is critical:**

```bash
# Creates file with "hey"
echo hey > welcome

# File now contains "hey"
cat welcome
hey

# APPENDS "hello" — doesn't delete "hey"
echo hello >> welcome

# File now contains both
cat welcome
hey
hello
```

> **Q: Operator to run a command in background?** → `&`  
> **Q: Replace "passwords" file contents with "password123"?** → `echo password123 > passwords`  
> **Q: Add "tryhackme" without losing "passwords123"?** → `echo tryhackme >> passwords`

---

## Complete command cheat sheet from this room

| Command | Example | Use in SOC work |
|---|---|---|
| `echo` | `echo "hello"` | Write notes to files during investigations |
| `whoami` | `whoami` | Confirm which account I'm operating as |
| `ls` | `ls /var/log` | See what log files exist |
| `cd` | `cd /var/log` | Navigate to log directory |
| `cat` | `cat auth.log` | Read authentication logs |
| `pwd` | `pwd` | Confirm my current location |
| `find` | `find -name "*.log"` | Locate all log files on a system |
| `wc -l` | `wc -l auth.log` | Count how many events are in a log |
| `grep` | `grep "Failed" auth.log` | Find all failed login attempts |
| `grep -R` | `grep -R "malware.exe" /var/log/` | Search all logs for a filename |
| `>` | `echo "IP blocked" > notes.txt` | Start an investigation notes file |
| `>>` | `echo "timestamp" >> notes.txt` | Add to investigation notes |

---

## Flag captured

| Task | Flag |
|---|---|
| Task 6 — grep on access.log | **THM{ACCESS}** |

---

## Honest reflection

The `grep -R` command was the moment this room went from "useful" to "wow." Being able to search every file in every subdirectory for a specific string with one command — that's how a SOC analyst hunts for indicators of compromise across an entire server. One command, seconds, done.

I'm going to be using every command in this room for the rest of my career.
