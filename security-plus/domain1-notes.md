# Security+ SY0-701 — My Domain 1 Study Notes

**Course:** Professor Messer's SY0-701 Security+ (free on YouTube)  
**Videos covered:** 1 through 9  
**Last updated:** April 2026  

---

I'm using Professor Messer's free YouTube course alongside my TryHackMe labs to study for the Security+. These are my personal notes — written after watching each video, in my own words. I don't copy slides. I write what actually made sense to me, what confused me at first, and what finally clicked.

---

## Video 2 — Security Controls (Objective 1.1)

This was the first proper technical video and honestly it threw a lot at me at once. But once I understood the structure it made sense.

### What I learned: controls have two dimensions

Before this video I thought "security control" just meant something like a firewall or a lock. Turns out there's a whole system for categorising them — by **what does the controlling** and by **what the control is trying to achieve**.

**What does the controlling — the four categories:**

- **Technical** — the technology itself enforces the control. Firewall blocks traffic. Anti-virus catches malware. The system does it without a human having to be involved.
- **Managerial** — policies and procedures. A written rule that says "all passwords must be 12+ characters." The control is the policy, not the system enforcing it.
- **Operational** — humans do it. A security guard checking IDs. An awareness training session. People are the control.
- **Physical** — something physical restricts access. Fences, locks, badge readers, guard shacks. You literally can't get through.

**What the control is trying to achieve — the six types:**

This one took me longer to get my head around because some of them feel similar. Here's how I eventually separated them:

- **Preventive** — stops something before it happens. A locked door stops an intruder. A firewall stops unauthorised traffic. "You shall not pass."
- **Deterrent** — doesn't actually stop anything, just makes someone think twice. A warning sign on a fence. A "threat of demotion" policy. An attacker might decide it's not worth the risk, but if they push through anyway, the deterrent does nothing.
- **Detective** — notices something is wrong, but might not prevent it. System logs, motion detectors, login reports. By the time the detective control fires, something may have already happened.
- **Corrective** — fixes things after the fact. Restoring from a backup after ransomware. Patching a vulnerability after it's been exploited. It's the "clean up" control.
- **Compensating** — a workaround when you can't use the ideal control. Can't patch the app right now? Block it at the firewall instead. Generator when the power goes out. It's not perfect, but it fills the gap.
- **Directive** — the weakest one. Just tells people what to do and hopes they comply. "Store sensitive files in the protected folder." A compliance training session. An "Authorised Personnel Only" sign. No enforcement — just direction.

### The thing that made it click

I was getting confused between deterrent and preventive until I thought about it like this: a lock on a door is **preventive** — it physically stops you. A "beware of dog" sign is **deterrent** — it might make you turn around, but if there's no dog, nothing stops you.

### What I need to remember for the exam

Controls can belong to multiple categories at once. A firewall is Technical AND Preventive. A security guard is Operational AND Deterrent AND Preventive. The exam might describe a scenario and ask which type applies — focus on what the control is *doing*, not what it *is*.

---

## Video 3 — The CIA Triad (Objective 1.2)

Short video but probably the most foundational concept in all of security. Everything — every control, every policy, every tool — is ultimately protecting one or more of these three things.

### What I learned: three things worth protecting

**Confidentiality** — keeping secrets. Only authorised people should see certain information.

What protects it: encryption (scrambles the data so only someone with the key can read it), access controls (restricts who can open a file or system), and two-factor authentication (makes sure you really are who you claim before letting you in).

**Integrity** — making sure information is accurate and hasn't been tampered with.

What protects it: hashing (creates a fingerprint of data — if even one character changes, the fingerprint completely changes), digital signatures (mathematical proof of who sent something and that it wasn't modified), and non-repudiation (you can't deny that you sent it).

**Availability** — systems need to actually work when people need them.

What protects it: redundancy (backup systems so if one fails another takes over), fault tolerance (systems designed to keep running through failures), and patching (keeping software stable and secure).

### How I'm remembering this

Every time I read about a security incident I now ask: which part of CIA was breached?

- Data stolen = Confidentiality
- Data changed without authorisation = Integrity  
- Website taken down = Availability

Most attacks hit more than one. Ransomware hits all three simultaneously — it makes data unavailable, it's been modified (encrypted), and it exposes what data exists (confidentiality).

---

## Video 4 — Non-repudiation (Objective 1.2)

I'll be honest — before this video I had no idea what non-repudiation meant. The word sounds complicated. The concept is actually simple.

### What I learned: you can't deny what you did

Non-repudiation means there's proof that you did something and you can't claim you didn't. Like signing a legal contract — your signature is proof. You can't later say "I never agreed to that."

In cybersecurity, this is done through **digital signatures**.

### How digital signatures actually work

This is the part that I had to watch twice, but it finally made sense:

**When Alice sends a message to Bob:**

1. Alice takes her message and runs it through a hash algorithm — this creates a short "fingerprint" of the message
2. She then encrypts that fingerprint using her **private key** — this encrypted fingerprint is the digital signature
3. She sends Bob the original message AND the digital signature together

**When Bob receives it:**

1. Bob decrypts the digital signature using Alice's **public key** — this gives him the fingerprint Alice created
2. Bob runs the message through the same hash algorithm himself — this gives him his own fingerprint
3. He compares the two fingerprints — if they match, the message is untampered and it definitely came from Alice

### Why this works

Only Alice has her private key. So only Alice could have created that signature. And if anyone had changed the message after Alice signed it, the fingerprints wouldn't match — Bob would know immediately.

### What confused me at first

I kept mixing up which key does what. Here's my rule now: **sign with private, verify with public**. Alice signs with her private key. Anyone with her public key can verify. Simple.

---

## Video 5 — Authentication, Authorisation and Accounting (Objective 1.2)

The AAA framework. Three steps that happen every time you try to access anything.

### What I learned: three separate things happen when you log in

**Identification** — you tell the system who you are. Usually a username. This is just a claim — you haven't proved anything yet.

**Authentication** — you prove you are who you claim. Password, fingerprint, smart card, one-time code. Now the system believes you.

**Authorisation** — based on who you are, what are you allowed to do? Your username might give you read access to folder A but no access to folder B. This is entirely separate from authentication.

**Accounting** — the system tracks everything you do. Login time, files accessed, changes made, logout time. This is the audit trail.

### The bit that surprised me: authenticating devices

I hadn't thought about this before — systems and devices need to be authenticated too, not just people. A laptop can't type a password. So how does a VPN know that a device is authorised to connect?

The answer is **certificates**. The organisation has a Certificate Authority (CA) that issues digitally signed certificates to approved devices. When a device connects, it presents its certificate. The system checks the CA's digital signature. If it checks out — the device is in.

This is why you sometimes can't connect to the company VPN from a personal laptop — your personal device doesn't have the right certificate.

### Authorisation models

Without a model, you'd have to manually assign every user's access to every resource. Doesn't scale at all.

With a model (Role-Based, Attribute-Based, etc.) you define rules in the middle — users get roles, roles get permissions. Add a new employee, assign them a role, they automatically inherit the right permissions. Much more manageable.

---

## Video 6 — Gap Analysis (Objective 1.2)

Shorter video but important for the exam and for real-world security work.

### What I learned: understanding where you are vs where you need to be

A gap analysis is exactly what it sounds like — you look at your current security posture, compare it to where you need to be (based on a framework or standard), and identify the gaps.

The process:
1. Pick a framework to measure against — NIST SP 800-171, ISO/IEC 27001, or internal standards
2. Look at your people — what experience do they have, what training have they done, do they know the policies?
3. Look at your processes — what systems exist, what policies are documented, what's actually being followed?
4. Compare current state to the target — where are the weaknesses?
5. Write the gap analysis report — here's where we are, here's where we need to be, here's what it will take to get there

### What I took from this

Gap analysis isn't a quick checklist. It takes weeks or months, involves lots of people, and requires real research. The output is a formal report with specific recommendations. It's how organisations figure out where to invest their security budget.

---

## Video 7 — Zero Trust (Objective 1.2)

This one felt the most relevant to how security actually works in 2026. It's a big shift from how networks used to be designed.

### What I learned: the old way was broken

Traditional network security: put a big firewall on the edge, everyone inside the network is trusted. Once you're through the firewall, you can move around relatively freely.

The problem: if an attacker gets through the firewall — either by phishing an employee, exploiting a vulnerability, or getting physical access — they're inside the trusted zone and can reach almost anything.

### Zero trust flips this completely

Zero trust says: **nothing is inherently trusted.** Not users. Not devices. Not even internal systems. Everything has to prove itself every single time.

That means: multi-factor authentication everywhere, encryption on all traffic even internal traffic, strict permissions, additional internal firewalls, continuous monitoring, and analytics watching for unusual behaviour.

### The two planes

I didn't know networks were split into planes until this video:

- **Data plane** — actually moves the data. Forwarding packets, encrypting traffic, NAT. It does the work.
- **Control plane** — tells the data plane what to do. Routing tables, policies, rules. It makes the decisions.

### The zero trust components I need to know for the exam

- **Policy Engine** — evaluates whether to grant or deny access based on policy and context
- **Policy Administrator** — communicates the decision to the enforcement point, generates access tokens
- **Policy Enforcement Point (PEP)** — the actual gatekeeper. Allows, monitors, and terminates connections based on what it's told
- **Policy Decision Point (PDP)** — where the engine and administrator live together

My way of remembering it: the Policy Engine is the judge, the Policy Administrator is the clerk communicating the verdict, and the Policy Enforcement Point is the security guard at the door acting on it.

### Security zones

Zero trust uses zones — trusted, untrusted, internal, external, specific departments like Finance or HR. Traffic moving from an untrusted zone to a trusted zone is blocked by default unless explicitly allowed. Even trusted-to-trusted traffic might need verification depending on what's being accessed.

---

## Video 8 — Physical Security (Objective 1.2)

I found this video interesting because physical security is something you can see and touch — it's easy to understand intuitively.

### What I learned: physical security is layered

**Barricades and bollards** — those concrete posts you see outside buildings. Stops vehicles from ramming through. Channels pedestrians through specific checkpoints where they can be screened.

**Access control vestibules** — interlocking doors. Open the outer door and the inner door locks. This prevents tailgating — someone authorised letting an unauthorised person slip through behind them. You can only get through one at a time.

**Fencing** — perimeter control. Has to be transparent enough for guards to see through it, robust enough to resist cutting, and high enough (with razor wire) to prevent climbing.

**CCTV** — modern systems don't just record, they analyse. Motion recognition, object detection, facial recognition. All cameras are networked and all footage is stored.

**Guards and access badges** — physical human verification. Two-person integrity is interesting — some high-security areas require two people to be present at all times so no single person has unchecked access to a sensitive asset.

**Lighting** — more light means more security. Attackers avoid well-lit areas. Also important for cameras to work effectively, especially for facial recognition.

### The sensors

These come up in exam questions so I made sure to understand each one:

- **Infrared** — detects body heat. Works in complete darkness. Used in motion detectors.
- **Pressure** — detects weight or force. Floor tiles that sense someone walking on them. Window sensors that detect the glass flexing.
- **Microwave** — detects movement across large open areas.
- **Ultrasonic** — sends out sound waves and detects the reflection. Used for motion and collision detection.

---

## Video 9 — Deception and Disruption (Objective 1.2)

This was my favourite video so far because it felt the most creative. The idea that you can actually trap attackers using fake systems and fake data is brilliant.

### What I learned: you can make attackers reveal themselves

**Honeypots** — fake systems designed to look real and attract attackers. The attacker thinks they've found something valuable and starts poking around. Every action they take is being logged. You learn their techniques, their tools, and how they operate — while they think they're making progress.

What I found interesting: most honeypot "attackers" aren't humans — they're automated scanning tools. Bots probing the internet looking for vulnerable systems. Honeypots catch these automatically.

**Honeynets** — a whole fake network. Multiple honeypots together — fake servers, workstations, routers — all designed to look like a real corporate environment. More convincing, more data.

**Honeyfiles** — fake files placed where attackers might look. The classic example is a file called `passwords.txt` sitting in a shared folder. Legitimate users have no reason to open it. The moment anyone accesses it — alert fires. Virtual bear trap.

**Honeytokens** — fake data with tracking built in. This is where it gets clever:

- **Fake API credentials** — look real, do nothing, but send a notification the moment someone tries to use them
- **Fake email addresses** — monitor the internet to see if they get posted anywhere (which would mean data was leaked)
- **Fake database records** — if they appear outside your network, you know someone stole the data
- **Browser cookies** — track if they're used from an unexpected location

### Why this matters for my SOC career

Alerts from honeypots and honeyfiles are high-confidence. Unlike a regular SIEM alert that might be a false positive triggered by normal behaviour, a honeyfile alert almost certainly means a real attacker — because no legitimate user should ever be touching a decoy file. When I eventually work in a SOC and see one of these alerts, I'll treat it as a confirmed incident from the start, not something to triage cautiously.

---

## My progress so far

| Video | Topic | Status |
|---|---|---|
| 1 | Course Introduction | ✅ Done |
| 2 | 1.1 Security Controls | ✅ Done |
| 3 | 1.2 CIA Triad | ✅ Done |
| 4 | 1.2 Non-repudiation | ✅ Done |
| 5 | 1.2 AAA Framework | ✅ Done |
| 6 | 1.2 Gap Analysis | ✅ Done |
| 7 | 1.2 Zero Trust | ✅ Done |
| 8 | 1.2 Physical Security | ✅ Done |
| 9 | 1.2 Deception and Disruption | ✅ Done |
| 10+ | Continuing... | 🔄 In progress |
