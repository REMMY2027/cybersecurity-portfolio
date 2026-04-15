# Defensive Security Intro

**Room:** TryHackMe — Defensive Security Intro  
**Path:** Cyber Security 101 → Start Your Cyber Security Journey  
**Completed:** April 2026  
**Difficulty:** Info  
**Time to complete:** 15 minutes  

---

## What made this room special

In the previous room I was the attacker — scanning FakeBank and exploiting an unprotected admin page. In this room I flipped sides. I played **Joe**, an apprentice SOC analyst on his first solo shift, and I had to deal with an active attack as it happened.

The connection between the two rooms hit me immediately: the attack I was now *defending* against was exactly the same type I had just *performed*. Directory enumeration. The same tool, the same technique, just seen from the other side. That made everything in this room click instantly.

---

## The investigation, step by step

### Task 1 — Think like a Defender

Quick framing task to establish the defender's mindset:

- **Offensive security:** Find the weaknesses
- **Defensive security:** Detect the attacks and respond before damage is done

Unlike offensive security, defenders don't break into systems. They monitor, protect, detect, and respond.

> **Q: What is the main goal of defensive security?**  
> **A: Detect and respond to attacks**

---

### Task 2 — Detect Suspicious Activity

The SOC monitoring dashboard lit up. Something was wrong on the network.

I opened the dashboard and reviewed the alerts. One source IP was generating traffic that didn't look right — too many requests, too fast, all from the same address.

I isolated it:

> **Q: Which source IP address is generating the suspicious traffic?**  
> **A: 32.122.195.63**

That's step one of any investigation — figure out where the traffic is coming from. In a real SOC this would kick off an automatic enrichment process: is this IP known malicious? What country is it from? Has it been seen before?

---

### Task 3 — Identify the Attack

Knowing the IP isn't enough. I needed to understand what they were actually *doing*.

I checked the "URL Discovery Attempts" log in the dashboard — a record of every URL the attacker had tried to access on our system. The list was long. Hundreds of paths, most of them returning 404. And then, at the bottom:

> **Q: What is the latest URL the attacker tried to find?**  
> **A: https://fakebank.com/admin**

There it was. Directory enumeration. The attacker was running the exact same `dirb`-style scan I had run in the previous room. They were looking for an unprotected admin panel.

Knowing the attack type changes everything. It tells you what the attacker was looking for, whether they might have found it, and what the impact could be if they had.

**Attack type confirmed: directory enumeration / web scanning**

---

### Task 4 — Stop the Attack

Joe knows who the attacker is and what they're doing. Time to contain it.

The immediate priority in defensive security when an attack is actively happening is **containment** — stopping the damage as quickly as possible.

I went to the firewall management section of the dashboard, entered the attacker's IP (`32.122.195.63`), selected **BLOCK**, and clicked Apply.

> **Flag: THM{FAKEBANK-SECURED}**

The attack was stopped. No more requests from that IP would reach the server.

---

## The full picture of what just happened

```
Alert fires → I identify the source IP: 32.122.195.63
           → I check what they were doing: URL Discovery Attempts log
           → I identify the attack type: directory enumeration
           → I see their target: https://fakebank.com/admin
           → I contain the threat: block IP at firewall
           → I confirm containment: flag received
           → Next steps: document everything, write the report, escalate if needed
```

That sequence — detect, investigate, identify, contain, document — is the Tier 1 SOC analyst workflow. I just ran through the whole thing.

---

## What I'm taking away from this

**One:** Doing both rooms back to back was deliberate and brilliant. I performed the attack in one room and defended against it in the next. I understood the URL Discovery Attempts log immediately because I had just created the same kind of trail myself. This is why blue teamers benefit from understanding offensive techniques.

**Two:** The firewall block is containment, not resolution. In a real SOC, blocking the IP is step one. After that comes documentation, a full incident report, and potentially escalation to Tier 2 if the attacker had already accessed something sensitive. The shift isn't over when you click "Block."

**Three:** Documentation is part of the job. Every action I took — every IP I looked up, every log I reviewed, every firewall rule I applied — needs to be recorded. That record is evidence. It protects the organisation and it protects me.
