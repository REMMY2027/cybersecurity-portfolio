# Search Skills

**Room:** TryHackMe — Search Skills  
**Path:** Cyber Security 101 → Start Your Cyber Security Journey  
**Completed:** April 2026  
**Difficulty:** Easy  

---

## Why searching is a real skill

Before this room I thought "researching something" just meant typing it into Google. Turns out there's a whole discipline around effective searching — especially in cybersecurity, where you're often looking for very specific technical information about threats, vulnerabilities, or tools.

This room taught me how to search properly, which tools to use beyond Google, and where to find reliable technical information fast.

---

## Advanced Google operators

These are ways to make Google far more precise than a normal search:

| Operator | How to use it | What it does |
|---|---|---|
| `site:` | `site:gov.uk cybersecurity` | Searches only within a specific website |
| `filetype:` | `filetype:pdf penetration testing` | Finds specific file types |
| `intitle:` | `intitle:"index of" passwords` | Searches page titles only |
| `"exact phrase"` | `"buffer overflow" tutorial` | Forces an exact match |
| `-word` | `python tutorial -youtube` | Excludes a word from results |

The `site:` operator is particularly useful for SOC work — if I need to verify information about a CVE, I can search `site:cve.mitre.org CVE-2024-XXXX` and go straight to the official source.

---

## Specialist tools I learned about

### Shodan (shodan.io)

This one genuinely surprised me. Shodan is a search engine for internet-connected devices — not websites, but actual devices. You can search for:

- All internet-facing servers running a specific software version
- All devices with a specific port open
- All devices of a specific type in a specific country

From a defender's perspective: this is how attackers find targets at scale. They search Shodan for organisations running vulnerable software and attack them automatically. Knowing this, I understand why patching quickly and reducing your attack surface matters so much. If your vulnerable server shows up on Shodan, it's only a matter of time before someone tries it.

### CVE Database (cve.mitre.org)

CVE stands for Common Vulnerabilities and Exposures. Every known vulnerability gets a CVE number — like `CVE-2024-12345`. This is the universal language of vulnerabilities. When a new vulnerability is reported, the CVE number is how everyone refers to it consistently.

As a SOC analyst, if an alert references a specific exploit or attack, I'll often need to look up the associated CVE to understand what it does, what systems it affects, and how severe it is.

### Exploit-DB (exploit-db.com)

A database of publicly available exploits. Security researchers publish exploit code here after responsible disclosure. Useful for understanding what an attacker might be using against a specific vulnerability.

### OSINT Framework

A collection of open-source intelligence tools organised by category. Useful for investigating threat actors, domains, email addresses, usernames, and more.

---

## How I use these in practice

Here's the kind of searching I'd do during an actual SOC investigation:

**Scenario:** Alert fires, attacker IP is `32.122.195.63`

1. Search `32.122.195.63 threat intelligence` on Google to get community reports
2. Look up the IP on Shodan to see what services it's running and where it's hosted
3. Check AbuseIPDB and VirusTotal for community flagging
4. If the attacker appears to be exploiting a specific CVE, look it up on `cve.mitre.org`

All of that takes about 3–5 minutes. Being fast and accurate at this kind of research is what separates a good Tier 1 analyst from a slow one.

---

## What I didn't expect

Shodan blew my mind a bit. The fact that you can find internet-connected industrial control systems, hospital equipment, and home routers all through a search engine — it made the concept of "attack surface" feel very real and very serious. Organisations that don't know what they're exposing to the internet are essentially leaving their front door open.

Understanding this from the defender's side — knowing what your organisation looks like on Shodan — is something I want to learn more about.
