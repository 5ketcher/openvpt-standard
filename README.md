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

# 🏛 Legal Notice

This repository contains an **open technical standard**, not an implementation.
A reference Laravel/PHP implementation was used during development.
It is not included in this repository, but selected payload examples are provided in /examples.
Any system may implement TrustID freely under the conditions of the Apache License 2.0.

All rights to authorship remain with **Vojtěch Sejkora**.

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
