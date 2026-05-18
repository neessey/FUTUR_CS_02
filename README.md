# Phishing Forensic & Awareness System

## Overview
This project presents a complete security analysis framework focused on phishing detection, email forensics, and organizational awareness against modern social engineering attacks.

The objective is to simulate the role of a Security Operations Center (SOC) Analyst by investigating suspicious emails, analyzing email headers, identifying authentication failures, and building a resilient security awareness strategy.

---

# Objectives

- Analyze suspicious emails using forensic methodologies
- Detect spoofing and phishing indicators
- Investigate email authentication mechanisms
- Understand modern phishing techniques
- Design an organizational defense and awareness framework
- Improve threat detection and incident response efficiency

---

# Forensic Methodology & Header Analysis

Email headers act as the “black box” of message transmission.  
Analysis is performed chronologically from the lowest `Received:` field upward to trace the original source of the email.

## SPF (Sender Policy Framework)
Verifies whether the sender’s mail server IP address matches the authorized IPs defined in the domain DNS records.

### Indicators
- `Pass` → Legitimate sender validation
- `Fail` or `SoftFail` → Potential spoofing attempt

---

## DKIM (DomainKeys Identified Mail)
Checks the cryptographic signature embedded within the email header to ensure that the message content has not been altered during transit.

### Purpose
- Validates message integrity
- Prevents email tampering

---

## DMARC (Domain-based Message Authentication, Reporting & Conformance)
Ensures alignment between SPF, DKIM, and the visible `From:` domain.

### Failure Conditions
An email fails DMARC when:
- SPF fails or does not align
- DKIM fails or does not align

---

## Network Path Analysis
The original sender IP is identified through deep inspection of the `Received:` headers to bypass relay obfuscation techniques.

---

# Threat Classification Framework

## Category 1 — Safe

### Criteria
- Valid SPF, DKIM, and DMARC
- Legitimate sender domain
- No malicious indicators detected

### Security Action
- Allow normal inbox delivery

---

## Category 2 — Suspicious

### Criteria
- Minor header inconsistencies
- Newly registered domains
- Urgent or manipulative language patterns

### Security Action
- Quarantine email
- Perform static analysis
- Open links in isolated sandbox environments

---

## Category 3 — Phishing

### Criteria
- DMARC failure
- Credential harvesting attempts
- Malicious attachments
- Spoofed internal identities

### Security Action
- Block sender immediately
- Purge emails from corporate inboxes
- Update firewall and Secure Email Gateway (SEG) indicators

---

# Modern Threat Landscape

## AI-Generated Phishing
Attackers use Large Language Models (LLMs) to create highly convincing and grammatically flawless phishing emails.

### Risks
- Personalized targeting
- Reduced human detection capability

---

## Voice Cloning (Vishing)
Cybercriminals clone executive voices using short public audio samples from platforms such as LinkedIn or YouTube.

### Attack Goal
- Fraudulent financial approvals
- Wire transfer manipulation

---

## Quishing (QR Code Phishing)
Malicious QR codes are embedded inside emails to bypass traditional email security scanners.

### Risk
Users scanning QR codes on personal devices may unknowingly access phishing websites.

---

## MFA Fatigue Attacks
Attackers repeatedly send Multi-Factor Authentication push requests until the victim accidentally approves access.

### Impact
- Session hijacking
- Unauthorized account access

---

# Security Metrics & Human Firewall

This framework promotes proactive threat reporting rather than passive avoidance.

## Primary KPI
The main performance indicator is the **Reporting Rate** instead of the traditional click-through rate.

### Target Metric

```math
Reported Emails / Clicked Links > 2
