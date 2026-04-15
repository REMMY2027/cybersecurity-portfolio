# Security+ SY0-701 — My Domain 1 Notes

**Resource:** Professor Messer's SY0-701 course (free on YouTube)  
**Progress:** Videos 1–9 complete  
**Last updated:** April 2026  

---

## A note on how I'm writing these

These aren't copy-pasted slides. I write these notes after watching each video, in my own words, because that's the only way I actually remember things. If something confused me I explain it the way it finally made sense to me. If something connected to something else I mention that too.

---

## Video 2 — 1.1 Security Controls

### The basic idea

Security controls are the things an organisation puts in place to prevent attacks, minimise damage when attacks happen, and limit how far an attacker can get. The assets being protected include data, physical equipment, and computer systems.

### Four categories of control

Think of categories as *who or what does the controlling:*

**Technical** — the system does the controlling
- Firewalls, anti-virus, operating system controls
- Example: A firewall blocking traffic on port 23

**Managerial** — policies and procedures do the controlling
- Security policies, standard operating procedures, administrative rules
- Example: A policy saying all passwords must be 12+ characters

**Operational** — people do the controlling
- Security guards, security awareness training, human procedures
- Example: A guard checking IDs at the door

**Physical** — physical barriers do the controlling
- Fences, locks, badge readers, guard shacks
- Example: A key card required to enter the server room

### Six types of control

Think of types as *what the control is trying to achieve:*

**Preventive** — stops the event before it happens
- "You shall not pass" energy
- Firewall rules, door locks, security policy enforcement

**Deterrent** — discourages an attempt but doesn't stop it directly
- Makes an attacker think twice
- Warning signs, application splash screens, front reception desk, threat of demotion

**Detective** — identifies and logs after or during an event
- Can't always prevent access, but spots what's happening
- System logs, login reports, motion detectors, property patrols

**Corrective** — fixes things after an event is detected
- Restoring from backups after ransomware
- Applying a patch after discovering a vulnerability
- Creating policies to report security issues

**Compensating** — alternative control when the main one isn't enough
- A workaround when you can't fix the root problem
- Blocking an application at the firewall instead of patching it
- Generator when the power goes out

**Directive** — directs people toward compliance
- Relatively weak — relies on people following instructions
- "Store all sensitive files in a protected folder" sign
- Compliance training, "Authorised Personnel Only" notices

### The matrix that ties it together

This table was on the exam notes and I think it's genuinely useful to memorise:

| | Preventive | Deterrent | Detective | Corrective | Compensating | Directive |
|---|---|---|---|---|---|---|
| **Technical** | Firewall | Splash screen | System logs | Backup recovery | Block app instead of patch | File storage policy |
| **Managerial** | On-boarding policy | Demotion | Review login reports | Reporting policy | Separation of duties | Compliance policy |
| **Operational** | Guard shack | Reception desk | Property patrols | Contact authorities | Require multiple staff | Security training |
| **Physical** | Door lock | Warning signs | Motion detectors | Fire extinguisher | Power generator | "Authorised Personnel Only" sign |

**Exam tip:** Controls can belong to multiple categories simultaneously. A firewall is Technical AND Preventive. Don't overthink it — just understand what each one is trying to do.

---

## Video 3 — 1.2 The CIA Triad

The three fundamentals. Everything in security protects at least one of these.

### Confidentiality — keeping secrets

Information should only be accessible to people who are authorised to see it.

How we protect it:
- **Encryption** — scrambles data so only the right person with the right key can read it
- **Access controls** — restricts who can open, view, or edit a resource
- **Two-factor authentication** — adds a second proof of identity before allowing access

### Integrity — keeping things accurate

Data should only be changed by authorised people, and any unauthorised change should be detectable.

How we protect it:
- **Hashing** — creates a fingerprint of data. Change one character and the fingerprint completely changes. This is how you know if a file has been tampered with.
- **Digital signatures** — mathematical proof that data came from a specific person and hasn't changed since
- **Non-repudiation** — proof that an action happened and can't be denied (more on this below)

### Availability — keeping things running

Systems need to be up and accessible when people need them.

How we protect it:
- **Redundancy** — have backups so if one thing fails, another takes over
- **Fault tolerance** — build systems that keep running even when components fail
- **Patching** — close security holes and fix bugs that could cause downtime

**The way I remember this:** CIA. If an attacker steals your data, that's a Confidentiality breach. If they change your data without you knowing, that's an Integrity breach. If they take your website down, that's an Availability breach.

---

## Video 4 — 1.2 Non-repudiation

Non-repudiation means you can't deny that you did something. Like signing a legal contract — your signature proves you agreed, and you can't later claim you didn't sign it.

### How hashing proves integrity

A hash is like a fingerprint for data. Run the same data through a hash algorithm and you always get the same result. Change even one character and the result is completely different.

Example from the video: Take an 8.1MB encyclopedia. Change one character somewhere. The hash changes completely. If someone sends you a file and the hash matches what they told you, you know the file wasn't tampered with in transit.

**Important:** A hash tells you if data changed. It doesn't tell you who changed it.

### How digital signatures add proof of origin

Digital signatures combine hashing with public key cryptography to prove both that data wasn't changed AND who sent it.

**Creating a signature (Alice sending to Bob):**
1. Alice hashes her message
2. She encrypts that hash with her **private key** — this creates the digital signature
3. She sends the message + the signature together

**Verifying (Bob receiving):**
1. Bob decrypts the signature using Alice's **public key** — this reveals the hash Alice created
2. Bob hashes the message himself
3. If Bob's hash matches the decrypted hash — the message is untampered and it definitely came from Alice

**The key insight:** Only Alice has her private key, so only Alice could have created that signature. And if the message had been changed after signing, the hashes wouldn't match.

---

## Video 5 — 1.2 Authentication, Authorisation, and Accounting (AAA)

### The AAA framework

| Component | What it means | Simple example |
|---|---|---|
| **Identification** | Who you claim to be | "My username is kirumira" |
| **Authentication** | Prove it | Enter password, scan fingerprint |
| **Authorisation** | What are you allowed to do? | Read access to folder A, no access to folder B |
| **Accounting** | Track what you did | Login time, files accessed, logout time |

### Authenticating devices, not just people

This one I hadn't thought about before. A laptop can't type a password. So how do you verify that a device is actually authorised to connect to the VPN?

Answer: **digitally signed certificates.** The organisation has a Certificate Authority (CA) that issues certificates to approved devices. The certificate is digitally signed by the CA. When the device connects, it presents its certificate. The system verifies the CA's signature. If it checks out — the device is authenticated.

### Authorisation models

Without a model: User → Resource (direct assignment). Doesn't scale. Hard to manage.

With a model: User → Role/Organisation/Attribute → Resource. 

The model sits in the middle and defines rules about who gets what. Much easier to manage when you have hundreds of users and hundreds of resources.

---

## Video 6 — 1.2 Gap Analysis

### What it is

Gap analysis is figuring out where your security posture is now versus where it needs to be. The "gap" is the difference between the two.

### How it works

1. **Pick a framework** — NIST SP 800-171, ISO/IEC 27001, or an internal standard
2. **Look at your people** — what experience do they have? What training have they done?
3. **Look at your processes** — what IT systems exist? What security policies are documented?
4. **Compare** — where are the weaknesses? What's missing?
5. **Write the report** — current state, what needs to change, how long and how much it will cost

Gap analysis takes weeks or months. It's not a quick check — it's a proper investigation involving emails, interviews, data gathering, and technical research.

---

## Video 7 — 1.2 Zero Trust

### The problem with traditional security

Old-school network security: big firewall on the edge, everything inside is trusted. If you get through the firewall, you're in. Problem: once an attacker gets inside, there's nothing stopping them from moving around freely.

### Zero trust: trust nothing by default

Zero trust means: **nothing is inherently trusted.** Every device, every user, every connection has to prove itself every single time.

That means: MFA, encryption, strict permissions, additional internal firewalls, continuous monitoring, analytics.

### Two planes

| Plane | What it handles |
|---|---|
| **Data plane** | Actually moves the data — forwarding packets, encryption, NAT |
| **Control plane** | Makes the decisions — routing tables, policies, what the data plane is allowed to do |

### The components of zero trust

| Component | Role |
|---|---|
| **Policy Engine** | Makes the decision to grant, deny, or revoke access based on rules |
| **Policy Administrator** | Tells the Policy Enforcement Point what to do — generates tokens, allows or blocks |
| **Policy Enforcement Point (PEP)** | The actual gatekeeper — allows, monitors, and terminates connections |

Think of it like a nightclub: the Policy Engine is the decision-maker in the back office, the Policy Administrator is the manager communicating the decision, and the PEP is the bouncer at the door.

### Security zones

Zero trust divides networks into zones: trusted, untrusted, internal, external, VPN, specific departments. Traffic from an untrusted zone to a trusted zone is blocked by default. This limits how far an attacker can move even if they get inside the network.

---

## Video 8 — 1.2 Physical Security

Before attackers can get to your data, they sometimes try to get to your building. Physical security stops that.

### Access control methods

**Barricades / bollards** — concrete posts or barriers that stop vehicles. Channels people through specific access points where they can be checked.

**Access control vestibules** — interlocking doors. When one door opens, the other locks. Prevents tailgating — someone following an authorised person through a secure door without being checked.

**Fencing** — perimeter control. Transparent so guards can see through it. Robust so it can't be cut easily. Razor wire on top to prevent climbing.

**CCTV** — not just recording, modern systems can do motion recognition and object detection. Cameras are networked together and records are kept.

**Guards + access badges** — physical validation at entry points. Two-person integrity means no single person has sole access to a high-security asset — reduces insider threat risk. Badges are electronically logged.

**Lighting** — more light means more security. Attackers avoid well-lit areas. Also important for facial recognition cameras to work properly.

### Sensors

| Type | How it works |
|---|---|
| Infrared | Detects body heat — works in complete darkness |
| Pressure | Detects weight — floor tiles, window sensors |
| Microwave | Detects movement across large areas |
| Ultrasonic | Sends sound waves, detects reflection — movement and collision |

---

## Video 9 — 1.2 Deception and Disruption

This section is really interesting from a SOC perspective because these are detection tools that SOC teams actually use.

### Honeypots

A honeypot is a fake system designed to look real enough to attract attackers. The attacker thinks they've found something valuable and starts interacting with it — meanwhile, every action they take is being logged and studied.

The "attacker" is often an automated machine running reconnaissance scripts, not a human. Honeypots are especially useful for learning about attacker techniques and catching automated threats early.

### Honeynets

A network of honeypots. Multiple fake systems — servers, workstations, routers — all designed to look like a real corporate network. Gives more data and looks more convincing.

### Honeyfiles

Fake files placed in shared folders. A classic example: a file called `passwords.txt` sitting in a file share. If anyone accesses it — legitimate users have no reason to — an alert fires immediately.

It's a virtual bear trap. The moment an attacker touches it, you know someone is in your network who shouldn't be.

### Honeytokens

Fake data with tracking built in. Examples:

- **Fake API credentials** — look real but don't work. When someone tries to use them, a notification fires.
- **Fake email addresses** — monitor the internet to see if they get posted anywhere (data breach detection).
- **Fake database records** — if they appear somewhere outside your network, you know data was stolen.
- **Browser cookies** — track if they're used outside your network.

### Why this matters for SOC work

Alerts from honeypots and honeyfiles are **high-confidence incidents.** Unlike a regular SIEM alert that might be a false positive, if someone triggers a honeyfile — that's almost certainly a real attacker. Legitimate users have no reason to be in a decoy file. When I see this kind of alert in a real SOC, I treat it as a confirmed incident from the start.

---

## Terms to know for the exam

| Term | What it means |
|---|---|
| CIA Triad | Confidentiality, Integrity, Availability — the foundation of security |
| Non-repudiation | Can't deny you did something — proof of origin |
| Hash | A fixed-length fingerprint of data — changes if data changes |
| Digital signature | Encrypted hash proving who sent something and that it wasn't changed |
| AAA | Authentication, Authorisation, Accounting |
| Zero trust | Nothing is inherently trusted — everything must be verified |
| PEP | Policy Enforcement Point — the gatekeeper |
| Gap analysis | Current security posture vs target posture |
| Honeypot | Fake system designed to attract attackers |
| Honeyfile | Fake file that triggers an alert when accessed |
| Honeytoken | Fake data with tracking built in |
| Deterrent control | Discourages but doesn't prevent |
| Compensating control | Alternative when the main control isn't enough |

---

## My progress through Professor Messer's course

| Video | Topic | Done? |
|---|---|---|
| 1 | Introduction | ✅ |
| 2 | 1.1 Security Controls | ✅ |
| 3 | 1.2 CIA Triad | ✅ |
| 4 | 1.2 Non-repudiation | ✅ |
| 5 | 1.2 AAA Framework | ✅ |
| 6 | 1.2 Gap Analysis | ✅ |
| 7 | 1.2 Zero Trust | ✅ |
| 8 | 1.2 Physical Security | ✅ |
| 9 | 1.2 Deception and Disruption | ✅ |
| 10+ | Continuing... | 🔄 |
