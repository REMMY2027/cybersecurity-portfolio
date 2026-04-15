# Offensive Security Intro

**Room:** TryHackMe - Offensive Security Intro  
**Path:** Cyber Security 101 → Start Your Cyber Security Journey  
**Completed:** April 2026  
**Difficulty:** Easy  
**Time to complete:** 10 minutes  

---

## My honest first impression

I wasn't expecting to actually hack something in my first proper room. But that's exactly what happened. I hacked a fake bank website called FakeBank, found a hidden admin page that should not have been accessible, and transferred money into an account. Legally. In a safe environment. And it took about 5 minutes.

That experience changed how I think about security. It's one thing to read "websites can have hidden pages." It's another thing to discover one yourself using a tool in a real terminal.

---

## What I did, step by step

### Task 1 - Think like a Hacker

Short intro task. The key idea: **offensive security is about thinking like an attacker to find weaknesses before real hackers do.** You're not doing anything malicious, you're doing it so the company can fix it before someone with bad intentions finds it.

> **Q: Which term describes simulating a hacker's actions to find system vulnerabilities?**  
> **A: Offensive Security**

---

### Task 2 — Starting the Lab

Launched the FakeBank virtual environment in the browser. It looked like a real banking application; login page, account balances, transaction history. My job was to find a way in that a normal user wouldn't know about.

> **Q: What is the bank account number shown in the FakeBank application?**  
> **A: 8881**

---

### Task 3 — Find Hidden Pages

This is where it got interesting. The task introduced me to **dirb** - a tool that finds hidden pages on websites by trying thousands of common names automatically.

I opened the terminal and ran:

```bash
dirb http://fakebank.thm
```

What dirb does is take a list of common directory and page names, things like `/admin`, `/login`, `/dashboard`, `/bank-transfer` and tries each one against the target website. Any that return a valid response get reported back to you.

The output showed:
- `http://fakebank.thm/images` - nothing interesting
- `http://fakebank.thm/bank-transfer`- **this was the one**

A hidden admin page. Not linked anywhere on the site. Not protected by any login. Just sitting there waiting to be found.

> **Q: What is the other hidden URL dirb found?**  
> **A: http://fakebank.thm/bank-transfer**

---

### Task 4 — Attack the Admin Page

I navigated directly to `http://fakebank.thm/bank-transfer`. The page loaded — no login required, no authentication at all. It was an admin panel that let anyone transfer money between accounts.

I selected account **8881**, deposited **$2000**, clicked "Deposit Money", and a flag appeared:

> **Flag: BANK-HACKED**

Done. I'd just demonstrated a real vulnerability — an unprotected admin panel discoverable by automated scanning — in under 10 minutes.

---

## The tool that made this possible

| Tool | What it does |
|---|---|
| `dirb http://fakebank.thm` | Tries thousands of URL paths and reports which ones exist |

The `+` symbol in dirb's output means a page was found. Any line starting with `+` is worth investigating.

---

## What clicked for me

The most important thing I learned from this room isn't the tool — it's the mindset. **Hiding a page by not linking to it does not protect it.** If the page exists on the server and has no access control, a scanner will find it in seconds.

This is exactly why organisations hire penetration testers — to find things like this before real attackers do.

---

## Why this matters for my SOC career

Here's what really stuck with me: the attack I just performed leaves very obvious traces in web server logs. When dirb scans a site, it fires hundreds of requests in rapid succession. In the logs, that looks like this:

```
32.x.x.x - "GET /admin HTTP/1.1" 404
32.x.x.x - "GET /login HTTP/1.1" 404
32.x.x.x - "GET /dashboard HTTP/1.1" 404
32.x.x.x - "GET /bank-transfer HTTP/1.1" 200
```

Hundreds of 404s from one IP, then a sudden 200 on something sensitive. That pattern is exactly what a SIEM alert would flag. Having performed the attack myself means I'll recognise that pattern immediately when I see it as a SOC analyst — I won't need to guess what it means.
