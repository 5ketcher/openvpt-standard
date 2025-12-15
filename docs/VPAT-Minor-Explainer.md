# OpenVPT Minor Standard 1.0 — Explainer (Non‑Normative)
Privacy‑Preserving Age Verification for Minors

**Status:** Informative / Non‑Normative Explainer  
**Applies to:** OpenVPT Minor Standard 1.0 (Experimental Policy Profile)  
**Author:** Vojtěch Sejkora  
**Date:** 2025-12-12  

---

## 1. What problem this solves (in human terms)

Platforms need a way to enforce age gates (e.g., **13+ / 15+ / 18+**) without forcing children to:
- upload documents,
- create permanent “child identities”,
- or become trackable across the internet.

The **OpenVPT Minor Standard** provides a way for a platform to learn only this:

> “This user is old enough for the feature / service.”

…and learn **nothing else**.

---

## 2. What OpenVPT Minor is (and is not)

### It *is*
- **Age category verification** (e.g., `13+`).
- A **privacy‑first profile** of the OpenVPT standard.
- Designed to minimize data and linkability.

### It is *not*
- A child digital identity.
- A child account system.
- A parental tracking mechanism.
- A “truth scoring” system for content.

---

## 3. The five roles (who does what)

### Subject (Minor)
A child user who wants to pass an age gate.  
Key rule: **no persistent account is required**.

### Legal Guardian
A parent/guardian who authorizes the age attestation.  
Key rule: the guardian **is not revealed** to the Issuer or the platform.

### Age Attestation Authority (AAA)
A regulated authority (e.g., **bank**, **national eID**, **EUDI Wallet**) that can confirm age eligibility.  
Key rule: AAA returns only:
- **age bracket** (e.g., `13+`),
- **assurance** (how strong the check is),
- **validity** (how long it remains acceptable).

AAA **never issues OpenVPT or VPT tokens**.

### VPT Issuer (Identity Provider)
Issues the **Verified Person & Age Token (VPT)** using the AAA’s attestation.  
Key rule: the Issuer stores **no personal data about the child** and relies on the attestation result only.

### Relying Party (RP)
The platform/service that consumes the token.  
Key rule: RP receives **no bank data, no guardian data, no identity** — only eligibility.

---

## 4. The key privacy rules (why it’s safe)

### Rule A — “Age‑only”
Only the age bracket is shared (example: **13+**).  
No birth date, no name.

### Rule B — “No persistent identity”
There is no global “child identifier”. The child does not become a long‑term record.

### Rule C — “No cross‑platform correlation”
The token subject identifier (`sub`) is **pairwise per platform**.  
Even if two platforms compare notes, they cannot prove it’s the same child.

### Rule D — “No recovery by design”
If the device/handle is lost, there is no “account recovery”.  
This prevents long‑term identity accumulation and reduces abuse potential.

### Rule E — “Minimal retention”
Issuers and platforms store the minimum required and delete after expiry + grace.

---

## 5. How the flow works (a story)

1. A child starts an age verification on a platform (RP).
2. The platform redirects the child to a VPT Issuer.
3. The Issuer generates a **local pseudonym/handle** (random) and stores it only on the device.
4. The guardian authorizes age attestation using a regulated authority (AAA).
5. AAA checks eligibility and returns only:
   - age bracket (e.g., `13+`)
   - assurance (e.g., high)
   - validity (e.g., 6 months)
6. Issuer records only the attestation result (minimal).
7. Issuer issues a short‑lived VPT token for the platform (minutes).
8. Platform validates the token and applies its policy (allow / restrict / deny).

---

## 6. What platforms can do with it (policy examples)

A platform can safely implement policies such as:
- Allow account creation only with `age_bracket >= 13+`.
- Restrict features for minors (e.g., public posting, DMs, discoverability).
- Apply different moderation or amplification rules for minor accounts.
- Require stronger assurance (`assurance_level = high`) for sensitive features.

**Important:** OpenVPT Minor does not tell a platform what to do — it enables policy without tracking.

---

## 7. Abuse prevention (how this makes attacks harder)

OpenVPT Minor does not “solve” all propaganda or bot issues — but it makes *mass abuse* harder by raising the cost:

- To create large numbers of “minor‑eligible” accounts, an attacker would need repeated guardian‑authorized attestations from regulated authorities.
- Tokens are short‑lived; attestations should be time‑limited.
- No recovery means farms lose continuity easily.
- Pairwise identifiers prevent building cross‑platform profiles.

This shifts abuse from “cheap and scalable” to “expensive and hard to scale”.

---

## 8. “Signal weighting” vs “truth scoring”

OpenVPT Minor is **not** a mechanism to label content as “true/false”.  
Instead, it supports **signal weighting**:

> “This interaction comes from a minor‑eligible account with a certain assurance.”

Platforms may choose to limit amplification or sensitive actions — not because minors are “untrustworthy”, but because minors are a protected category and the platform must act responsibly.

---

## 9. What the platform learns (and what it cannot learn)

### The platform can learn
- Age bracket (e.g., `13+`)
- Assurance level (e.g., high)
- That the token is valid and issued by a trusted Issuer

### The platform cannot learn
- Child identity
- Guardian identity
- Bank/eID details
- Cross‑platform linkage

---

## 10. Limits and honest caveats

- A determined adversary may still abuse systems using adult credentials or compromised flows.
- Policy decisions remain the platform’s responsibility.
- The model is intentionally privacy‑heavy; convenience trade‑offs are expected.

---

## 11. Where this fits in OpenVPT

- OpenVPT Core defines token and assurance mechanics.
- **OpenVPT Minor** is a constrained profile focused on minors, where privacy dominates.
- Additional profiles may exist (e.g., adult profiles with recovery).

---

## 12. One‑sentence summary

**OpenVPT Minor Standard enables platforms to enforce age gates without creating child identities or tracking children across the internet.**
