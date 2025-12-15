# OpenVPT Minor Standard 1.0 — Threat Model & Misuse Resistance (Non-Normative)

**Status:** Informative / Non-Normative  
**Scope:** OpenVPT Minor Standard 1.0 (Experimental Policy Profile)  
**Author:** Vojtěch Sejkora  
**Date:** 2025-12-12  

---

## 1. Goal

This document explains common abuse scenarios relevant to minors and how the
**OpenVPT Minor Standard** reduces risk through:

- minimal disclosure,  
- non-linkability,  
- short lifetimes,  
- issuer selection and enforcement.  

It does **not** claim to eliminate all abuse.

---

## 2. Assets to protect

1. **Children’s privacy** (no identity, no tracking)  
2. **Platform integrity** (resistance to account farms and manipulation)  
3. **Guardian privacy** (no linkage guardian ↔ child ↔ platform)  
4. **Issuer integrity** (prevent token forgery and replay)  
5. **Regulatory compliance** (data minimization, retention limits)  

---

## 3. Adversaries

- **Account farms** (cheap creation of many accounts)  
- **Coordinated influence operations** (mass narratives using fake personas)  
- **Fraudsters** (stolen credentials / compromised devices)  
- **Curious platforms** (attempting to correlate children across services)  
- **Network attackers** (MITM, replay, token theft)  

---

## 4. Threats and mitigations

### T1 — Mass creation of “minor” accounts (account farms)

**Threat:** Create thousands of minor accounts cheaply.  

**Mitigations in OpenVPT Minor:**
- Guardian authorization via regulated AAA introduces friction and cost.  
- Attestations SHOULD be time-limited (e.g., 6 months).  
- Tokens MUST be short-lived (minutes).  
- No recovery reduces long-term continuity for farms.  

**Residual risk:** Abuse can shift to adult credentials or stolen attestations.

---

### T2 — Cross-platform correlation of a child

**Threat:** Platforms or issuers attempt to link a child across services.  

**Mitigations:**
- Token subject identifier (`sub`) MUST be **pairwise per RP**.  
- No global identifier is allowed.  
- Prohibited claims block stable identifiers (device IDs, hashes).  

**Residual risk:** Side-channels (IP address, browser fingerprinting) are out of scope;
platforms must mitigate these separately.

---

### T3 — Guardian identification or tracking

**Threat:** A platform learns who the guardian is or builds a parent graph.  

**Mitigations:**
- Guardian authenticates only with the AAA.  
- Issuer and RP receive **no guardian data**.  
- Data retention rules explicitly forbid guardian records.  

**Residual risk:** If the AAA itself is compromised or violates policy, privacy could
be impacted. Oversight and governance are required.

---

### T4 — Token forgery

**Threat:** An attacker fabricates VPT tokens claiming `13+`.  

**Mitigations:**
- Strong cryptographic signature verification (per OpenVPT core).  
- Issuer metadata validation.  
- Key rotation expectations.  
- RPs MUST verify `iss`, signature, expiration, and `policy_profile`.  

**Residual risk:** Misconfigured RPs remain a risk; operational guidance is necessary.

---

### T5 — Token replay or theft

**Threat:** A valid token is stolen and reused.  

**Mitigations:**
- Very short token expiration (`exp`, minutes).  
- `jti` uniqueness with optional replay cache at the RP.  
- TLS everywhere and hardened issuer endpoints.  

**Residual risk:** Stolen devices may still permit misuse; this cannot be fully solved
without reducing privacy guarantees.

---

### T6 — “Truth scoring” misuse or censorship narrative

**Threat:** Misinterpretation that OpenVPT Minor labels content as “true” or “false”.  

**Mitigations:**
- Documentation clarifies **signal weighting**, not truth scoring.  
- OpenVPT Minor defines eligibility claims only (age bracket, assurance).  

**Residual risk:** Platforms may implement policies poorly; transparency is recommended.

---

## 5. RP controls: issuer selection and trust policy

A core protection mechanism is **RP-controlled issuer allowlisting**:

- RPs decide which issuers are accepted for the minor profile.  
- RPs may require jurisdictional constraints (e.g., EU-regulated issuers).  
- RPs may require `assurance_level = high` for sensitive features.  

This avoids a single global authority and keeps policy decisions at the platform level.

---

## 6. Recommended RP policy patterns (examples)

- **Allow** minor accounts only with accepted issuers and high assurance.  
- **Restrict** high-risk actions for minors (public posting, DMs, live streaming).  
- **Rate-limit** interactions from newly verified minor handles.  
- **Disable amplification** for minor-originated content by default (optional).  
- **Transparency:** clearly disclose how age eligibility affects features.  

---

## 7. Security vs privacy trade-offs (explicit)

OpenVPT Minor prioritizes privacy and non-linkability over convenience:

- No recovery reduces fraud but increases re-verification.  
- No device registry prevents tracking but reduces device-level controls.  

This trade-off is intentional.

---

## 8. Conclusion

The OpenVPT Minor Standard reduces scalable abuse by:

- raising the cost of mass account creation,  
- preventing cross-platform correlation,  
- minimizing retention and sensitive data collection.  

It is best deployed with clear RP policies and transparent, user-facing rules.

---

# End of OpenVPT Minor Threat Model
