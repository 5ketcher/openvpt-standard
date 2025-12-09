
# TrustID: Verified Person & Age Token Protocol (VPAT)
**Draft Version:** 1.0  
**Status:** Experimental Specification  
**Date:** 2025-12-09  

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

# 3. Core Principles

1. Minimal Disclosure  
2. Cryptographic Integrity  
3. Revocability  
4. Interoperability  
5. Privacy  

---

# 4. Identity Provider Internal Model (Private)

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

# 5. Token Format

## Standard Claims
iss, sub, iat, exp, jti.

## TrustID Claims
real_person, age_bracket, age_13_plus, age_18_plus, trust_level, loa.

## Social Claims
subject_type, platform, handle, profile_url.

## Device Claims
device_certified, device_type.

---

# 6. API Endpoints

### POST /tokens/issue/by-handle  
Issues VPAT token.

### POST /verify-token  
Validates VPAT token.

### GET /.well-known/jwks.json  
Public keys.

---

# 7. Token Lifetime Rules
Short-lived (5 minutes).  
Proof tokens up to 1 year.

---

# 8. Revocation Model  
revoked, expired, device_revoked.

---

# 9. Security Requirements

IdP must rotate keys, secure private keys, enforce TLS.  
RP must validate signature & expiration.

Privacy rules apply.

---

# 10. Threat Model
Mitigates botnets, synthetic identities, deepfakes, underage access, etc.

---

# 11. Protocol Flows

### Verification Flow:
User → RP → IdP → RP (token) → IdP (verify)

### Revocation Flow:
RP → /verify-token → IdP

---

# 12. Interoperability
Compatible with eIDAS2, BankID, national identity systems, FIDO2.

---

# 13. Reference Implementation  
This repository includes example payloads and flows under /examples/.
The full Laravel backend implementation is separate.

---

# 14. Future Extensions
age_over_21, verified_business_account, COSE tokens, device risk scoring.

---

# 15. Change Log
v1.0 (2025-12-09): First full specification.

---

# End of Specification
