# TrustID: Verified Person & Age Token Protocol (VPAT)
**Draft Version:** 0.1  
**Status:** Experimental Draft  
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
TrustID is an open protocol for privacy-preserving verification of real human identity and age.  
It is designed for online platforms that need to distinguish verified humans from bots, synthetic identities, deepfake-generated accounts, and underage users.  

The protocol enables Identity Providers (IdPs) to issue **minimal, cryptographically signed proof tokens** containing statements such as:
- `verified_person = true`
- `age_over_18 = true`
- `trust_level = 4`

No personal identity data (name, birthdate, address, national ID) is ever disclosed to relying parties.

---

# 1. Introduction
The TrustID Verified Person & Age Token Protocol (VPAT) defines a standardized way for online platforms (Relying Parties, RPs) to validate that an account is operated by a real human being.

The protocol is built on:
- minimal disclosure principles,  
- strong cryptographic signatures,  
- pseudonymous identifiers,  
- high interoperability across providers.

TrustID is compatible with eIDAS 2.0, EU digital identity wallets, and national identity providers, while exposing only non-sensitive boolean or bracketed claims to relying parties.

---

# 2. Roles in the TrustID Ecosystem

## 2.1 Subject (User)
A natural person whose identity or age has been verified by the IdP.

## 2.2 Identity Provider (IdP)
An entity that:
- verifies the subject using strong identity mechanisms (eIDAS, bank identity, national ID),
- stores internal identity metadata,
- issues signed TrustID proof tokens.

## 2.3 Relying Party (RP)
An online service that:
- receives proof tokens from users,
- validates token signatures,
- interprets claims based on platform policies.

---

# 3. Core Principles

### 3.1 Minimal Disclosure  
The RP receives **only** boolean or categorical claims.  
No legal identity data is shared.

### 3.2 Cryptographic Integrity  
Tokens MUST be signed using JOSE/JWT or COSE structures with strong algorithms (RS256, ES256 or stronger).

### 3.3 Privacy-first Architecture  
The IdP operates as a gatekeeper of identity data.  
RPs never interact with legal identifiers.

### 3.4 Interoperability  
Any compliant IdP may issue tokens; any compliant RP may validate them.

### 3.5 Revocability  
Tokens contain a unique `jti` and may be revoked at any time.

---

# 4. Internal Identity Model (IdP)

The IdP stores identity metadata internally.  
This data is **never** exposed to RPs.

Example internal structure:

```json
{
  "sub": "pseudonymous-user-id-239f2j3f9",
  "eidas_id_hash": "hash_eidas_id",
  "age_bracket": "18+",
  "loa": "eIDAS-high",
  "trust_level": 4,
  "real_person": true,
  "last_verified_at": "2025-12-01T12:34:56Z"
}
