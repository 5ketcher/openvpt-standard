
# TrustID: Verified Person & Age Token Protocol (VPAT)
**Draft Version:** 1.1  
**Status:** Experimental Specification  
**Date:** 2025-12-10  

**Primary Author:**  
**Vojtěch Sejkora**  
Czech Republic  

---

## Copyright & License
Copyright © 2025 **Vojtěch Sejkora**

This specification is released under the **Apache License 2.0**.  
This means:
- Anyone may read, use, implement, and modify this specification.  
- Attribution to the original author is required.  
- No one may remove the author's name from this document.  
- No one may claim authorship of this work.  

Full license text: https://www.apache.org/licenses/LICENSE-2.0

---

# Abstract
TrustID VPAT (Verified Person & Age Token) is an open, minimal-disclosure protocol for verifying human authenticity and age category on online platforms.

The protocol enables Identity Providers (IdPs) to issue small, privacy-preserving JWT tokens containing boolean or bracketed claims such as:
- `verified_person = true`  
- `age_over_18 = true`  
- `trust_level = 3`  
- `device_certified = true`  

No personal data (name, birthdate, national ID, address) is ever shared.

VPAT helps platforms reduce:
- fake accounts  
- botnets  
- synthetic identities  
- underage access  
- deepfake misuse  

The protocol aligns with eIDAS 2.0, EU Digital Wallet, and global identity frameworks.

---

# 1. Introduction
This document defines the TrustID VPAT Protocol, an interoperable mechanism allowing online platforms (Relying Parties) to validate:

- that a user is a real human,  
- their verified age bracket,  
- the trustworthiness level certified by an IdP,  
- the authenticity of the user device.

VPAT uses JWT, JOSE, and JWKS for cryptographic security.

Intended for:
- social networks  
- gaming platforms  
- content platforms  
- anonymity-sensitive communities  
- AI-moderated networks  

---

# 2. Terminology

| Term | Definition |
|------|------------|
| Subject (User) | The human whose identity/age was verified. |
| Identity Provider (IdP) | Entity verifying identity and issuing proof tokens. |
| Relying Party (RP) | Online service consuming VPAT tokens. |
| Proof Token | Short-lived JWT representing minimal verified attributes. |
| Device Certification | Optional binding of a proof to a trusted device. |
| jti | Unique token identifier used for revocation. |

## 2.1 Abbreviations

| Abbreviation | Meaning | Description |
|--------------|---------|-------------|
| **VPAT** | Verified Person & Age Token | Main protocol for privacy-preserving proof of real person & age. |
| **IdP** | Identity Provider | Entity that verifies user identity and issues VPAT tokens. |
| **RP** | Relying Party | Platform or service that consumes and validates VPAT tokens. |
| **JWT** | JSON Web Token | Cryptographically signed token container used by VPAT. |
| **JOSE** | JSON Object Signing and Encryption | Family of standards defining JWT/JWS/JWE. |
| **JWKS** | JSON Web Key Set | Public key set used by RPs to verify IdP signatures. |
| **JWS** | JSON Web Signature | Signed data structure used within JWT. |
| **KID** | Key Identifier | Identifies which signing key was used to sign a JWT. |
| **SUB** | Subject | Stable pseudonymous user identifier inside the IdP. |
| **JTI** | JWT ID | Unique token identifier used for revocation checks. |
| **LOA** | Level of Assurance | Identity assurance level (eIDAS: low / substantial / high). |
| **eIDAS** | Electronic Identification, Authentication and Trust Services | EU regulatory framework for digital identity. |
| **HSM** | Hardware Security Module | Secure storage for private cryptographic keys. |
| **TLS** | Transport Layer Security | Protocol securing communication between RP and IdP. |
| **API** | Application Programming Interface | Technical interface through which VPAT is implemented. |
| **PII** | Personally Identifiable Information | VPAT avoids sharing these by design. |
| **CSPRNG** | Cryptographically Secure PRNG | Used to generate secure random values such as `jti`. |

---

# 3. Specification Governance

The TrustID VPAT specification is currently maintained by its original
author, **Vojtěch Sejkora**, as an open experimental standard.

- The authoritative version of the specification is the one published at:  
  https://github.com/5ketcher/TrustID-Verified-Person-Age-Token-Protocol-VPAT-

- Proposed changes MAY be submitted as GitHub Issues or Pull Requests.

- Normative changes (those that affect protocol behaviour, token
  structure, required claims or security requirements) MUST be approved
  by the author before they become part of an official version.

- Stable versions are tagged and released as `vX.Y` or `vX.Y-draft` in
  the repository and can be cited as such.

--
# 4. Core Principles

1. Minimal Disclosure  
2. Cryptographic Integrity  
3. Revocability  
4. Interoperability  
5. Privacy  

---

# 5. Identity Provider Internal Model (Private)

Internal structure example:

```
{
  "sub": "stable-pseudo-id-3289fd923",
  "eidas_id_hash": "sha256(...redacted...)",
  "age_bracket": "18+",
  "loa": "eIDAS-high",
  "trust_level": 4,
  "real_person": true,
  "last_verified_at": "2025-12-01T12:34:56Z"
}
```

---

# 6. Token Format

## Standard Claims
iss, sub, iat, exp, jti, policy_profile.

## TrustID Claims
real_person, age_bracket, age_13_plus, age_18_plus, trust_level, loa.

## Social Claims
subject_type, platform, handle, profile_url.

## Device Claims
device_certified, device_type.

Device claims are OPTIONAL and MAY be omitted entirely if the deployment does not require device binding.

## Issuer Metadata Claims (Required)

To enable governance and trust policy enforcement, all VPAT proof tokens
MUST include the following issuer metadata claims:

- `issuer` — URL identifying the issuing IdP  
- `issuer_country` — ISO-3166 country code  
- `issuer_type` — e.g., "government", "bank", "mobile_operator", "kyc_provider"  
- `assurance_level` — e.g., "low", "substantial", "high", or eIDAS-aligned values  
- `policy_profile` — identifier of the profile used for this issuance

These claims allow Relying Parties to perform trust decisions independently
of the core cryptographic validation.

---

# 7. API Endpoints

### POST /tokens/issue/by-handle  
Issues VPAT token.

### POST /verify-token  
Validates VPAT token.

### GET /.well-known/jwks.json  
Public keys.

---

# 8. Token Lifetime Rules
Short-lived (5 minutes).  
Proof tokens up to 1 year.

Relying Parties SHOULD reject proof tokens with an expiration longer than 365 days.

---

# 9. Revocation Model  
revoked, expired, device_revoked.

If an issuer becomes suspended or untrusted according to a Trust Policy,
Relying Parties MUST treat all its tokens as invalid regardless of signature validity.

---

# 10. Security Requirements

IdP must rotate keys, secure private keys, enforce TLS.  
RP must validate signature & expiration.
Relying Parties MUST validate issuer metadata against their configured Trust Policy.
Tokens from untrusted or unapproved issuers MUST be rejected or downgraded.

Privacy rules apply.

---

# 11. Trust Level Framework (TLF 1.0)

The Trust Level Framework defines a unified, interoperable, and auditable scale of assurance used across all implementations of the TrustID VPAT Protocol.  
It provides Relying Parties (RPs) with a predictable and machine-verifiable indicator of the confidence an Identity Provider (IdP) has in the verified identity of a Subject.

Trust Levels represent identity assurance, verification strength, and resistance to impersonation, independent from age bracket or device trust.

Relying Parties MUST evaluate the `trust_level` claim according to this framework.

---

## 11.1 Trust Level Overview

| Trust Level | Name                              | Description                                                                 | Typical Use Cases |
|-------------|-----------------------------------|-----------------------------------------------------------------------------|--------------------|
| **0**       | Unverified                        | No identity verification. User is anonymous or pseudonymous without proof. | Guest access, low-risk features. |
| **1**       | Basic Assurance                   | Low assurance (email, phone). No document verification.                     | Low-risk communities, spam reduction. |
| **2**       | Medium Assurance                  | Document or automated KYC check with facial matching.                       | Standard social verification, gaming, 13+ checks. |
| **3**       | High Assurance                    | Equivalent to *eIDAS Substantial*. Biometric liveness + document + MFA.     | Political posting, 18+ content, monetisation. |
| **4**       | Strong / Government-grade         | Equivalent to *eIDAS High*. National eID, BankID, EUDI Wallet.              | Trusted creators, journalists, high-security services. |
| **5**       | Critical Assurance (Reserved)     | Reserved for future regulated sectors.                                      | Electronic voting, legal services. |

---

## 11.2 Allowed Trust Level Values

The `trust_level` claim MUST contain an integer **0–4** in VPAT v1.0.  
Value **5** is reserved and MUST NOT be used until future revisions.

---

## 11.3 Requirements for Each Trust Level

### 11.3.1 Trust Level 0 — Unverified
IdP MUST ensure:
- No identity verification was performed.
- Subject is anonymous.
- Token MUST NOT assert `real_person = true`.

### 11.3.2 Trust Level 1 — Basic Assurance
IdP MUST:
- Perform low-assurance checks (email, phone, weak KYC).
- Detect duplicate/fraud identities where possible.
- Assert `real_person = true` ONLY if minimal KYC was performed.

### 11.3.3 Trust Level 2 — Medium Assurance
IdP MUST:
- Verify identity using document-based or automated KYC with face match.
- Perform liveness detection or use equivalent anti-spoofing checks.
- Ensure one-to-one identity uniqueness.
- Log verification method and timestamp.

### 11.3.4 Trust Level 3 — High Assurance (eIDAS Substantial)
IdP MUST:
- Perform verification at or above *eIDAS Substantial*.
- Use biometric verification with anti-spoofing.
- Authenticate user with MFA.
- Validate identity against authoritative registries.
- Maintain audit logs and revocation capability.

### 11.3.5 Trust Level 4 — Strong/Government-grade Assurance (eIDAS High)
IdP MUST:
- Use a qualified national identity scheme (eIDAS High, BankID, EUDI Wallet).
- Protect signing keys in an HSM.
- Confirm identity using government-grade processes.
- Meet availability and robustness requirements for national identity providers.

### 11.3.6 Trust Level 5 — Critical Assurance (Reserved)
- Reserved for future versions of VPAT.
- MUST NOT be issued by any IdP under VPAT v1.0.

---

## 11.4 Mapping to External Standards

| TLF Level | eIDAS LoA     | NIST IAL  | Notes |
|-----------|----------------|-----------|-------|
| **TL0**   | —              | IAL0      | No identity verification |
| **TL1**   | —              | IAL1      | Low confidence |
| **TL2**   | —              | IAL1/IAL2 | Medium identity confidence |
| **TL3**   | Substantial    | IAL2      | High identity confidence |
| **TL4**   | High           | IAL3      | Government-grade identity |
| **TL5**   | —              | —         | Reserved |

This mapping ensures VPAT remains interoperable with global digital identity frameworks.

---

## 11.5 Relying Party Behavior Based on Trust Level

RPs SHOULD enforce minimum trust levels based on risk:

- **TL0** → no sensitive actions  
- **TL1** → low-risk posting, non-monetised features  
- **TL2** → commenting, messaging, platform participation  
- **TL3** → political posting, 18+ content, monetisation  
- **TL4** → journalist/creator verification, identity-dependent actions  
- **TL5** → future high-regulation roles  

RPs MAY impose stricter requirements under their Trust Policy.

---

## 11.6 Misuse and Non-compliance

If an IdP falsely inflates trust levels:

- Relying Parties MUST reject its tokens.
- The issuer SHOULD be marked non-compliant.
- The issuer MAY be suspended from TrustID-compatible ecosystems.

Repeated misuse SHOULD result in permanent exclusion.

---

## 11.7 Backward Compatibility

Introducing TLF 1.0 does **not** change any existing VPAT claim fields.  
Tokens remain backward compatible.

If an RP does not recognize or cannot evaluate `trust_level`, it MUST treat the user as **TL0 (Unverified)**.

---

# 12. Threat Model
Mitigates botnets, synthetic identities, deepfakes, underage access, etc.

# 12.1 Handling Untrusted or Hostile Issuers

The VPAT protocol does not restrict which entities may operate as
Identity Providers (IdPs). This ensures global interoperability.

However, some issuers may lack transparency, fail audits, or attempt to
issue synthetic or fraudulent identities.

VPAT mitigates this by requiring issuer metadata within each token.
Relying Parties MUST apply their own Trust Policy to determine which issuers
are acceptable for specific use cases or regulatory environments.

The protocol remains neutral while allowing strict governance at deployment time.


---

# 13. Trust Policy Layer (Issuer Governance)

TrustID separates the technical protocol from trust decisions.  
While the protocol defines how tokens are issued, structured and verified,
it does **not** specify which Identity Providers (IdPs) should be trusted.

Trust decisions are governed by a **Trust Policy**, defined by each platform,
region, or regulatory ecosystem. A Trust Policy determines:

- which IdPs are accepted by the Relying Party (RP),
- which countries or trust schemes qualify,
- required Levels of Assurance (LoA),
- whether additional rules apply for high-risk actions (e.g., political advertising),
- how issuers are audited, onboarded, suspended, or removed.

VPAT remains a **neutral protocol**:  
it supports any issuer, but allows RPs to enforce strict governance.

---

# 14. Policy Profiles

A Trust Policy may define one or more **Profiles**, representing different
assurance requirements, legal constraints, or ecosystem rules.

A Profile defines:
- acceptable IdP types,
- minimum assurance levels,
- expected verification methods,
- allowed or disallowed token claims,
- retention and audit expectations.

Examples:
- `vpat-eu-1.0` — EU-focused profile aligned with eIDAS and regulated KYC providers  
- `vpat-global-0.1` — minimal global interoperability profile  
- `vpat-sandbox` — unrestricted profile for development/testing

The token MUST contain a `policy_profile` claim indicating the profile
under which the IdP issued the token.
Relying Parties MUST evaluate this claim according to their configured Trust Policy.

---

# 15. Protocol Flows

### Verification Flow:
User → RP → IdP → RP (token) → IdP (verify)

### Revocation Flow:
RP → /verify-token → IdP

---

# 16. Interoperability
Compatible with eIDAS2, BankID, national identity systems, FIDO2.

---

# 17. Reference Implementation  
This repository includes example payloads and flows under /examples/.
The full Laravel backend implementation is separate.

---

# 18. Future Extensions
age_over_21, verified_business_account, COSE tokens, device risk scoring.

---

# 19. Recommended Data Storage & Handling Guidelines

This section defines the recommended internal data fields, retention
rules, and handling practices for Identity Providers (IdPs) and Relying
Parties (RPs) implementing the TrustID VPAT protocol.

These recommendations ensure:
- privacy preservation,
- auditability,
- regulatory compliance (including GDPR and eIDAS),
- operational integrity,
- resistance against fraud, botnets, and synthetic identities.

---

## 19.1 Identity Provider (IdP) — Recommended Internal Data Model

IdPs SHOULD internally maintain the following fields:

- `sub` — stable pseudonymous subject identifier  
- `legal_identity_hash` — a salted hash of strong identity attributes  
- `age_bracket` — e.g., "13+", "15+", "18+", "21+"  
- `loa` — level of assurance  
- `trust_level` — integer (0–5)  
- `real_person` — boolean  
- `verified_at` — timestamp of last verified identity event  
- `verification_method` — e.g., "eIDAS-high", "bank", "wallet"  
- `revocation_status` — active, revoked, compromised  
- `device_binding` (optional) — registered trusted devices  
- `audit_log_id` — pointer to audit trail  

Personally identifiable information (PII), such as name, birthdate, ID
numbers, or address, MUST NOT be stored in reversible form.  
Only hashed or derived identifiers SHOULD be kept.

---

## 19.2 Required Logging by IdPs

IdPs SHOULD maintain secure logs of:

- identity verification events  
- token issuance events  
- token revocation events  
- key rotation events  
- fraud or anomaly detection alerts  
- access to administrative APIs  

Logs MUST include timestamps and MUST be tamper-evident or stored in an
append-only format.

Logs MUST NOT contain:
- JWT contents,  
- personal identity data,  
- unnecessary user metadata.

---

## 19.3 Relying Party (RP) — Recommended Validation Records

RPs SHOULD store ONLY:

- the VPAT token (for short-term replay protection),  
- `sub` pseudonymous identifier,  
- `issuer`,  
- `assurance_level`,  
- `policy_profile`,  
- token `jti` (for duplicate detection),  
- timestamp of validation,  
- decision outcome (accepted / rejected / downgraded).

RPs MUST NOT store:

- name, birthdate, ID numbers, or any PII,  
- full JWT contents beyond validation window,  
- historical location or behavioral data tied to the token.

Recommended maximum retention for tokens: **7–30 days**.  
Recommended maximum retention for validation logs: **1 year**, unless
required by law to keep longer.

---

## 19.4 Required Security Controls for Both IdPs and RPs

Both parties MUST:

- store signing/private keys in HSM or equivalent protection  
- enforce TLS for all VPAT API communication  
- implement rate limiting to prevent enumeration  
- implement monitoring for abnormal validation / issuance patterns  
- provide a clear breach notification procedure  
- isolate VPAT token validation from high-risk backend systems  

---

## 19.5 Data Minimization Principle

VPAT is designed to prevent:
- over-collection,
- cross-platform identity correlation,
- long-term identity tracking.

Therefore:

- IdPs SHOULD store the minimal internal identity attributes required for LoA.  
- RPs SHOULD NOT store VPAT tokens beyond what is required for transaction integrity.  

---

## 19.6 Revocation Record Handling

IdPs MUST maintain a revocation table containing:

- `jti` identifiers of revoked tokens  
- reason for revocation (compromised, fraud, user request, issuer policy)  
- timestamp of revocation  

RPs MUST query revocation status on every high-risk action.

---

## 19.7 Recommended Audit Practices

IdPs SHOULD undergo:

- annual cryptographic key audits  
- annual identity assurance audits  
- automated anomaly detection  
- external penetration testing (recommended)  

RPs SHOULD:

- test validation logic  
- log invalid token attempts  
- notify IdPs of suspected compromised accounts  

---

# 20. Change Log
v1.1 (2025-12-10):  
- Added Recommended Data Storage & Handling Guidelines  
- Added Trust Level Framework (TLF 1.0)
                    
v1.0 (2025-12-09):  
- First full specification.

---

# End of Specification
