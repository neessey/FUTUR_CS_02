# Phishing Forensic & Awareness System

This directory houses the comprehensive security analysis of email threats and the strategic organizational awareness framework designed to counter modern engineering attacks.

---

# Objective

To operate as a Security Operations Center (SOC) Analyst by triaging suspicious emails, analyzing underlying transit headers for authentication failures, mapping modern social engineering trends, and establishing a resilient defense strategy.

---

# Forensic Methodology & Header Analysis

An email header functions as the network "black box" of message transit. To perform trace forensics, analysis is executed from bottom to top (chronologically mapping the hops):

## Sender Policy Framework (SPF)

We verify if the sending mail server's IP address matches the authorized IPs listed in the sender's DNS records. An SPF result of `Fail` or `SoftFail` indicates potential spoofing.

## DomainKeys Identified Mail (DKIM)

We inspect the cryptographic signature embedded in the email header to confirm that the message body was not altered or tampered with during transit.

## Domain-based Message Authentication (DMARC)

We verify alignment. An email fails DMARC when both SPF and DKIM fail or do not align with the domain in the `From:` header.

## Network Path Analysis

We extract the lowest `Received:` header to locate the attacker's true origin IP, bypassing deceptive hop relays.

---

# Threat Classification Framework

## Category 1: Safe

### Criteria

Fully validated SPF, DKIM, and DMARC alignment. The sender domain is known and legitimate, with no high-risk indicators or malicious attachments.

### Security Action

Normal delivery to the user's inbox.

---

## Category 2: Suspicious

### Criteria

Minor header inconsistencies, domain age is under 30 days, or the message contains urgent/coercive language.

### Security Action

Route the email to a quarantine queue. Execute static analysis and test any links inside an isolated dynamic sandbox.

---

## Category 3: Phishing

### Criteria

Explicit DMARC failure, confirmed credential-harvesting links, malicious attachment signatures, or spoofed internal sender identities.

### Security Action

Immediately block the sender, purge the email from all corporate inboxes, and update the firewall and Secure Email Gateway (SEG) blocks with the newly discovered Indicators of Compromise (IoCs).

---

# Modern Threat Landscapes

## AI-Generated Phishing

Attackers leverage Large Language Models (LLMs) to generate linguistically flawless, hyper-personalized emails. This removes traditional indicators like poor grammar and awkward syntax.

## Voice Cloning (Vishing)

Exploiting brief public audio samples (from platforms like LinkedIn or YouTube) to clone an executive's voice. This is used in multi-channel attacks to validate fraudulent wire transfers.

## Quishing (QR Code Phishing)

Embedding malicious QR codes inside emails. Secure Email Gateways often fail to scan images, leaving the user vulnerable when scanning the code with a personal mobile device.

## Multi-Factor Authentication (MFA) Fatigue

Bombarding a target with endless MFA push notifications until they approve the request out of frustration, allowing attackers to hijack active sessions.

---

# Security Metrics & The Human Firewall

We shift organizational defense performance from passive avoidance to proactive detection. The primary KPI is no longer the Click-Through Rate, but rather the Reporting Rate.

Our target is to build an active defense where the Reporting Ratio exceeds 2:

```math
Ratio = \frac{\text{Reported Emails}}{\text{Clicked Links}} > 2
