# Junior Security Analyst Intro

**Room:** TryHackMe — Junior Security Analyst Intro  
**Path:** Cyber Security 101 → Start Your Cyber Security Journey  
**Completed:** April 2026  
**Difficulty:** Easy  
**Time to complete:** About 20 minutes  

---

## Why this room hit differently

This wasn't just a learning room. It was a simulation of an actual working day as a junior SOC analyst. I played Joe again — this time dealing with a more serious scenario involving a machine on the internal network communicating with a known malicious IP address.

By the end I had triaged an alert, investigated the activity, identified the attacker, blocked them at the firewall, and written up my findings. That's a complete incident response cycle, done by me, in under 20 minutes.

---

## The scenario

Joe is a Tier 1 SOC analyst on shift. The SIEM dashboard has fired an alert: an internal machine is connecting to a suspicious external IP address. Joe needs to investigate and respond before it becomes a serious incident.

---

## What I did

### 1. Reviewed the alert

Opened the monitoring dashboard. The alert showed an internal host making repeated outbound connections to an IP I didn't recognise. High frequency, regular intervals — the kind of pattern you'd expect from malware phoning home to a Command and Control (C2) server.

---

### 2. Investigated the source IP

I looked up `32.122.195.63` in a threat intelligence tool. It came back flagged — associated with C2 activity. Not a false positive. This was real.

At this point I knew:
- **Who the attacker was:** IP `32.122.195.63`
- **What the machine was doing:** communicating with a C2 server
- **How long it had been happening:** the connection had been active for 2 hours before the alert fired

---

### 3. Identified the affected machine and user

Traced the alert back to the specific internal machine and user account responsible for the outbound connection. This is critical information for the incident report — you need to know exactly what was compromised.

---

### 4. Contained the threat

Blocked `32.122.195.63` at the firewall. No more outbound communication to the C2 server. The malware on the affected machine could no longer receive instructions or exfiltrate data.

---

### 5. Documented and escalated

Wrote up the incident:
- Time of initial connection
- Attacker IP and threat intelligence verdict
- Affected machine and user account
- Duration of compromise (2 hours before alert)
- Actions taken (firewall block)
- Recommended next steps (Tier 2 review, full forensic investigation of the affected machine)

Escalated to Tier 2 with the full incident notes attached.

---

## Key concepts this room taught me

**True positive vs False positive**

| Type | What it means | What I do |
|---|---|---|
| True positive | A real threat — the alert is correct | Investigate, contain, document, escalate |
| False positive | Normal behaviour that triggered an alert | Close the alert, document why it was a FP, tune the rule if needed |

Most SOC alerts are false positives. Getting good at quickly distinguishing between the two — without dismissing real threats — is the core skill of a Tier 1 analyst.

**What a C2 server is**

A Command and Control server is infrastructure that attackers use to remotely control malware on a victim's machine. Once malware is installed on a device, it "calls home" to the C2 server to receive instructions — exfiltrate data, download more malware, move to other machines on the network. Cutting that communication (by blocking the C2 IP) is the first priority in containment.

**Escalation is not failure**

Tier 1 analysts are not expected to resolve everything. My job was to triage, contain immediately, and pass a complete and accurate handoff to Tier 2. A bad escalation — missing information, unclear findings, incorrect verdict — wastes everyone's time. A good escalation gives the next analyst everything they need to pick up where I left off.

---

## The thing that stood out most

Documentation. The room made it very clear that writing down every action I take during an investigation is not optional — it's core to the job. If I block an IP without documenting why, future analysts won't know what decision was made or what evidence it was based on. If I'm ever questioned about my actions, my documentation is my defence.

This applies to everything: what alert I received, what I looked up, what I found, what I decided, what I did, and when. Time-stamped, clear, and complete.

---

## SOC workflow I now understand from experience

```
Alert received
    ↓
Is this a true positive or false positive?
    ↓ (true positive)
What is the scope? What machine, what user, what attacker IP?
    ↓
How long has this been happening? What data might have been exposed?
    ↓
Contain immediately — block IP, isolate machine if needed
    ↓
Document everything with timestamps
    ↓
Escalate to Tier 2 with complete incident notes
    ↓
Follow up — did Tier 2 confirm? What was the root cause?
```

I ran through every step of this in this room. That's the closest thing to real SOC experience I've had so far.
