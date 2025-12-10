# TrustID — Open Standard for Verified Human & Age Tokens (VPAT)

**Author:** Vojtěch Sejkora  
**Status:** Experimental Open Standard  
**Current Specification:** VPAT 1.0 Draft  
**License:** Apache License 2.0  

---

# 📌 Overview

**TrustID** is an open, privacy-preserving protocol that allows online platforms to verify that an account belongs to a **real human being**, and optionally that the user is **above a required age threshold** (13+, 15+, 18+, 21+).

TrustID provides **cryptographically signed proof tokens** issued by trusted Identity Providers (IdPs).  
These tokens contain *minimal claims only*, such as:

```json
{
  "verified_person": true,
  "age_over_18": true,
  "trust_level": 4
}
```

No personal identity information is ever shared with platforms:
- no name  
- no birthdate  
- no address  
- no national ID  
- no document numbers  

TrustID is designed to strengthen digital trust while preserving individual privacy.

TrustID is a neutral protocol: trust decisions and accepted issuers are always determined by each platform's or region's Trust Policy.

---

# 📄 Abbreviations

| Abbreviation | Meaning |
|--------------|---------|
| VPAT | Verified Person & Age Token |
| IdP | Identity Provider |
| RP | Relying Party |
| JWT | JSON Web Token |
| JWKS | JSON Web Key Set |
| JOSE | JSON Object Signing & Encryption |
| KID | Key Identifier |
| JTI | Token Identifier |
| LOA | Level of Assurance |
| eIDAS | EU Digital Identity Framework |
| PII | Personally Identifiable Information |
| HSM | Hardware Security Module |

---
# 🎯 Purpose & Motivation

The internet increasingly suffers from:
- automated bot accounts  
- fake and synthetic identities  
- deepfake-based impersonation  
- underage access to restricted services  
- mass spam and coordinated manipulation  

Existing verification methods often require:
- uploading sensitive documents  
- revealing personal identity  
- trusting unregulated services with private data  

**TrustID solves this by defining a standardized, minimal, cryptographically verifiable token** that proves only what is necessary:

- "This is a real human."  
- "This user is at least 18 years old."  
- "This identity was verified using a high-assurance method."

Nothing more.

---

# 🧩 Core Features

- ✔ **Privacy-first identity verification**  
- ✔ **Boolean and bracket-style age claims** (13+, 15+, 18+, 21+)  
- ✔ **Cryptographically verifiable JWT-based tokens**  
- ✔ **No personal identity exposure**  
- ✔ **Compatible with eIDAS 2.0 & national eID systems**  
- ✔ **Lightweight and easy to integrate for platforms**  
- ✔ **Revocation and trust-level model**  

---

# 🏗 Architecture Summary

TrustID defines three ecosystem roles:

### 1. Identity Provider (IdP)
Verifies the subject using approved identity methods and issues signed tokens.

### 2. Subject (User)
A natural person who wishes to prove that they are a verified human.

### 3. Relying Party (RP)
A platform or service that accepts TrustID tokens to verify account authenticity.

---

# 🌐 Trust Policy Layer & Global Deployment

TrustID separates the technical protocol from trust decisions.

The protocol defines how tokens are issued, structured and verified.
However, TrustID does **not** determine which issuers or countries are
trusted. This is handled by a **Trust Policy Layer**, which each platform,
region or regulatory ecosystem may define independently.

A Trust Policy specifies:
- which Identity Providers (IdPs) are accepted,
- which countries or trust schemes qualify,
- required Levels of Assurance (LoA),
- rules for high-risk features (e.g. political advertising, monetisation),
- how issuers are audited, onboarded, suspended or removed.

This keeps VPAT globally neutral while enabling strict governance where required.

---

# 🧩 Policy Profiles

Trust Policies may define standardized **Profiles**, representing
different assurance and governance requirements.

Examples:
- `vpat-eu-1.0` — EU profile based on eIDAS and regulated KYC providers
- `vpat-global-0.1` — minimal worldwide interoperability profile
- `vpat-sandbox` — development/testing profile

Tokens include a `policy_profile` claim so Relying Parties can apply
rules appropriate to their jurisdiction or risk model.

A reference EU profile will be available in:
👉 /profiles/VPAT-EU-Profile.md

---

# 📘 Specification

The complete specification for the TrustID Verified Person & Age Token Protocol (VPAT) is here:

👉 **[VPAT-SPEC.md](./VPAT-SPEC.md)**  
Old draft: **[trustid-spec-0.1.md](./trustid-spec-0.1.md)**


This document includes:
- internal IdP identity model  
- exact token structure  
- all required and optional claims  
- example tokens  
- cryptographic requirements  
- revocation mechanisms  
- security & privacy considerations  
- trust policy & profile definitions  
- issuer metadata requirements. 

---

# 📁 Reference Examples
Example payloads, JWTs, and JWKS sets can be found in:

👉 /examples/

This includes:

- issue-request.json  
- issue-response.json  
- verify-request.json  
- verify-response-valid.json  
- verify-response-revoked.json  
- sample-token.jwt  
- sample-jwks.json  

---

# 🛡 Handling Unknown, Untrusted, or Hostile Issuers

TrustID assumes a realistic threat model: some issuers may lack
transparency, proper auditing, or governance, and may attempt to issue
synthetic or non-verifiable identities.

The protocol does **not** block such issuers at the technical level.
Instead, every VPAT token contains mandatory issuer metadata:

- `issuer`
- `issuer_country`
- `issuer_type`
- `assurance_level`
- `policy_profile`

This enables platforms to:
- whitelist approved issuers,
- reject or downgrade untrusted issuers,
- enforce stricter rules for high-risk features,
- selectively apply local/regional trust policies.

Trust decisions remain separate from the core protocol.

---

# 🔒 Security Principles

TrustID requires:
- strong cryptographic signing (RS256/ES256 or stronger)  
- properly rotated public keys via JWKS  
- avoidance of personal data in tokens  
- token expirations and revocation lists  

Relying Parties MUST reject:
- expired tokens  
- tampered tokens  
- tokens with invalid issuers  
- unknown signatures  

An example JWKS file is available in /examples/sample-jwks.json

Platforms MUST validate issuer metadata according to their configured Trust Policy.

---

# 🛡 Privacy Principles

TrustID enforces:
- data minimization  
- strong pseudonymity  
- no sharing of legal identity  
- GDPR-aligned design  
- protection against cross-platform identity correlation  
- unlinkable subject identifiers (future feature)  
- platforms cannot reconstruct legal identity even with multiple tokens  

Future versions will include:
- per-Relying-Party pseudonymous `sub` values  
- verifiable credentials (W3C VC) integration

---

# 🌍 Target Use Cases

TrustID is suitable for platforms such as:

### Social Networks  
Reduce bot activity and label verified humans.

### Messaging Services  
Prevent anonymous abuse while protecting identity.

### Gaming & Online Communities  
Age-gate restricted content without storing identity documents.

### Payment Services  
Require real-person verification without storing sensitive data.

### Dating Apps  
Prevent synthetic profiles safely and securely.

### Adult Content Platforms  
Legally verify `18+` without exposing personal identity.

### Government & Civic Tech  
Support trust layers for public services while maintaining privacy.

---

# 🏛 Governance & Future Expansion

TrustID may be extended by governance bodies, industry alliances or
public institutions.

Governance areas include:
- defining Trust Policies and Profiles,
- approving or suspending Identity Providers,
- maintaining trust registries,
- specifying minimal Levels of Assurance,
- coordinating audits and transparency requirements.

The core protocol remains stable while allowing new profiles,
cryptographic methods, assurance indicators and ecosystem rules to evolve.

---

# 🏛 Legal Notice

This repository contains an **open technical standard**, not an implementation.
A reference Laravel/PHP implementation was used during development.
It is not included in this repository, but selected payload examples are provided in /examples.
Any system may implement TrustID freely under the conditions of the Apache License 2.0.

All rights to authorship remain with **Vojtěch Sejkora**.

---

## TrustID Specification Protection Notice

The TrustID Protocol, including its architecture, token format, claim
definitions, trust model, security design, and all associated concepts,
is an original work authored by **Vojtěch Sejkora**.

This repository contains the authoritative version of the TrustID
Specification (VPAT — Verified Person & Age Token Protocol).

### Rights and Restrictions

- The TrustID specification MAY be implemented freely in any software,
  commercial or non-commercial, under the terms of the Apache License 2.0.

- The **specification itself**, including its structure, logic, diagrams,
  terminology, and token format, MAY NOT be copied, modified, republished,
  forked, or used to create derivative identity standards without
  explicit written permission from the author.

- The name **"TrustID"**, **"TrustID Protocol"**, and **"VPAT"**
  MAY NOT be used for:
    - derivative or competing identity specifications,
    - alternate versions of the standard,
    - modified or incompatible forks,
  without explicit written permission from the author.

- Implementations MAY state that they are
  **"TrustID-compliant"** or **"TrustID-compatible"**
  only if they follow the official specification contained in this repository.

### Purpose of this Notice

This notice protects the integrity of the TrustID standard and ensures that
the protocol evolves under transparent and consistent governance, while still
remaining fully open for adoption by platforms, companies, and public institutions.

Authorship, conceptual design, and the protocol architecture remain the
intellectual work of **Vojtěch Sejkora (2025)**.

---

# 📜 License

This project is licensed under the **Apache License 2.0**, which:

- allows free use and implementation,  
- requires attribution to the original author,  
- provides patent protection,  
- prevents removal of author credit.  

Full license text: https://www.apache.org/licenses/LICENSE-2.0

---

# 👤 Author

**Vojtěch Sejkora**  
Czech Republic, 2025  

Creator of the TrustID protocol and author of the associated specification.

---

# 🚀 Roadmap

Planned future enhancements of the TrustID standard:

- OAuth-like issuance flow (TrustID Connect)  
- COSE/CBOR mobile-friendly token format  
- hardware-keystore-based signing  
- decentralized multi-IdP federation  
- advanced trust scoring algorithms  
- verification for non-human entities (AI agents with operator identity)  
- per-RP pseudonymous subject identifiers  
- verifiable credential (W3C VC) mappings  

---

# 📬 Contact

Email: sejkora@trynt.cz

---

# End of README
