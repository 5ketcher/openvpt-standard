# OpenVPT — Open Verified Person & Age Token Standard

**Author:** Vojtěch Sejkora  
**Status:** Public Working Draft  
**Current Specification:** OpenVPT Core 1.1 (Draft)  
**License:** Apache License 2.0  

> Note: This project was previously published under a different working name.
> It has been renamed to **OpenVPT** for clarity and trademark safety.

---

# 📌 Overview

**OpenVPT** (Open Verified Person Token) is an open, privacy-preserving standard that enables online platforms to verify that an account belongs to a **real human being**, and optionally that the user belongs to a required **age group** (13+, 15+, 18+, 21+) — **without revealing identity data**.

OpenVPT defines the **Verified Person & Age Token (VPT)**:  
a cryptographically signed proof issued by trusted Identity Providers (IdPs).

Example minimal claims:

```json
{
  "verified_person": true,
  "age_over_18": true,
  "trust_level": 4
}
```

No personal identity information is ever shared with platforms:
- no name  
- no date of birth  
- no address  
- no national identifier  
- no document numbers  

OpenVPT strengthens digital trust while preserving individual privacy.

OpenVPT is a **neutral protocol standard**:  
trust decisions and accepted issuers are determined by each platform’s or region’s **Trust Policy**, not by the protocol itself.

---

# 📄 Abbreviations

| Abbreviation | Meaning |
|--------------|---------|
| OpenVPT | Open Verified Person Token |
| VPT | Verified Person & Age Token |
| IdP | Identity Provider |
| RP | Relying Party |
| JWT | JSON Web Token |
| JWKS | JSON Web Key Set |
| JOSE | JSON Object Signing & Encryption |
| KID | Key Identifier |
| JTI | Token Identifier |
| LoA | Level of Assurance |
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

Existing verification approaches often require:
- uploading sensitive documents  
- revealing legal identity  
- trusting centralized, opaque verification vendors  

**OpenVPT addresses this by defining a standardized, minimal, cryptographically verifiable token** that proves only what is necessary:

- “This is a real, verified human.”  
- “This user belongs to the 18+ age group.”  
- “This verification meets a defined assurance level.”  

Nothing more.

---

# 🧩 Core Features

- ✔ **Privacy-first verification model**  
- ✔ **Boolean and bracket-style age claims** (13+, 15+, 18+, 21+)  
- ✔ **Cryptographically verifiable JWT-based tokens**  
- ✔ **No personal identity exposure to platforms**  
- ✔ **Compatible with eIDAS 2.0 and national eID systems**  
- ✔ **Lightweight validation for large platforms**  
- ✔ **Trust-level and revocation support**  

---

# 🏗 Architecture Summary

OpenVPT defines three core ecosystem roles:

### 1. Identity Provider (IdP)
Verifies the subject using approved identity methods and issues signed VPT tokens.

### 2. Subject (User)
A natural person who wishes to prove verified personhood or age group.

### 3. Relying Party (RP)
A platform or service that validates OpenVPT tokens for access control or policy enforcement.

---

# 🌐 Trust Policy Layer & Global Deployment

OpenVPT **separates the technical protocol from trust decisions**.

The protocol defines:
- token structure,
- required claims,
- cryptographic requirements,
- validation rules.

OpenVPT deliberately does **not** mandate:
- which issuers are trusted,
- which countries qualify,
- which assurance levels are sufficient.

These decisions are handled by a **Trust Policy Layer**, defined independently by:
- platforms,
- regions,
- regulators,
- industry alliances.

A Trust Policy may specify:
- accepted Identity Providers,
- required Levels of Assurance,
- rules for high-risk features (e.g. political ads, monetization),
- issuer auditing and suspension criteria.

This design keeps OpenVPT globally neutral while enabling strict local governance.

---

# 🧩 Policy Profiles

Trust Policies may define standardized **Policy Profiles**, representing
different governance and assurance requirements.

Examples:
- `openvpt-eu-1.0` — EU profile aligned with eIDAS 2.0 and regulated KYC providers
- `openvpt-global-0.1` — minimal global interoperability profile
- `openvpt-sandbox` — development and testing profile

Tokens include a `policy_profile` claim so Relying Parties can apply
appropriate rules for their jurisdiction or risk model.

A reference EU profile is available at:
👉 `/profiles/OpenVPT-EU-Profile.md`

---

# 📘 Specification

The complete OpenVPT specification is defined in:

👉 **[OPENVPT-SPEC.md](./OPENVPT-SPEC.md)**  
Legacy draft (historical): **[vpat-spec-0.1.md](./vpat-spec-0.1.md)**

The specification covers:
- identity verification assumptions  
- exact token structure  
- mandatory and optional claims  
- example tokens  
- cryptographic requirements  
- revocation mechanisms  
- security and privacy considerations  
- trust policy and profile definitions  
- issuer metadata requirements  

---

# 📁 Reference Examples

Example payloads, tokens, and key sets are provided in:

👉 `/examples/`

Including:
- issue-request.json  
- issue-response.json  
- verify-request.json  
- verify-response-valid.json  
- verify-response-revoked.json  
- sample-token.jwt  
- sample-jwks.json  

---

# 🛡 Handling Unknown or Untrusted Issuers

OpenVPT assumes a realistic threat model.

Every VPT token includes mandatory issuer metadata:
- `issuer`
- `issuer_country`
- `issuer_type`
- `assurance_level`
- `policy_profile`

This enables platforms to whitelist, downgrade, or reject issuers
according to their Trust Policy.

---

# 🔒 Security Principles

OpenVPT requires:
- strong cryptographic signing (RS256 / ES256 or stronger),
- key rotation via JWKS,
- short token lifetimes,
- revocation support,
- strict validation by Relying Parties.

---

# 🛡 Privacy Principles

OpenVPT enforces:
- data minimization by design,
- strong pseudonymity,
- no exposure of legal identity,
- GDPR-aligned architecture,
- protection against cross-platform correlation.

---

# 🌍 Target Use Cases

- Social networks
- Messaging platforms
- Gaming communities
- Marketplaces and payment services
- Dating apps
- Adult content platforms
- Civic and public services

---

# 🏛 Governance & Evolution

OpenVPT is designed to evolve under transparent governance,
while keeping the core protocol stable.

---

# 🏛 Legal Notice

This repository contains an **open technical standard**, not a product or service.
Reference implementations may exist independently.

Authorship and original design are credited to **Vojtěch Sejkora**.

---

# 📜 License

Licensed under the **Apache License 2.0**.

---

# 👤 Author

**Vojtěch Sejkora**  
Czech Republic, 2025  

---

# 🚀 Roadmap

- OAuth-like issuance flow (OpenVPT Connect)
- COSE / CBOR encoding
- hardware-backed signing
- decentralized multi-IdP federation
- advanced trust-level signalling
- optional W3C VC compatibility
- non-human subject extensions

---

# 📬 Contact

Email: sejkora@trynt.cz  
Web: https://openvtp.dev  

---

# End of README
