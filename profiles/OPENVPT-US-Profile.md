### OpenVPT-US-Profile (v1.0)
**Status:** Regional Profile — United States  
**Author:** Vojtěch Sejkora  
**Date:** 2025-12-10  

---

# 1. Purpose

This profile defines rules for **OpenVPT (Open Verified Person Token)** deployments
in the United States, reflecting the decentralized structure of U.S. identity
verification systems.

This profile explicitly aligns with:

- **NIST SP 800-63A (Identity Assurance)**  
- **NIST SP 800-63B (Authentication Assurance)**  
- **NIST SP 800-63C (Federation Assurance)**  

---

# 2. Allowed Identity Providers

Identity Providers (IdPs) issuing **VPT tokens** under this profile MUST belong to
one of the following categories:

- U.S. state-issued identity systems (e.g. DMV, state ID programs)  
- NIST-compliant identity providers  
- U.S.-regulated KYC/AML providers  
- Banks and regulated financial institutions  
- Mobile carriers with validated subscriber identity  
- Government contractors certified under NIST SP 800-63  

IdPs MUST disclose identity verification methods and comply with applicable
federal and state regulations.

---

# 3. Required Assurance Levels (IAL)

The `assurance_level` claim MUST follow **NIST SP 800-63A Identity Assurance Levels (IAL)**.

Permitted values:
- `"ial2"`  
- `"ial3"`  

Tokens with `"ial1"` MUST be rejected for real-person verification use cases.

---

# 4. Authentication Assurance Requirements (AAL)

The authentication event used during identity verification MUST comply with
**NIST SP 800-63B** requirements:

- For **IAL2** identity proofing: **AAL2** is RECOMMENDED  
- For **IAL3** identity proofing: **AAL3** is REQUIRED  

Acceptable AAL2 methods include multi-factor authentication using:
- possession + knowledge, or  
- possession + biometrics  

---

# 5. Federation Assurance Requirements (FAL)

VPT token issuance under this profile aligns with:

- **NIST SP 800-63C FAL1** — signed assertions (minimum requirement)

Higher federation assurance levels (**FAL2**, **FAL3**) MAY be used for deployments
requiring encrypted assertions or hardware-backed key material.

---

# 6. Required Claims

VPT tokens issued under this profile MUST include:

- `real_person = true`  
- `issuer_country = "US"`  
- `policy_profile = "openvpt-us-1.0"`  
- `assurance_level` with a valid IAL value  

Age verification MUST comply with applicable U.S. regulations, including
**COPPA**, relevant state laws, and sector-specific requirements
(e.g. California consumer protection and privacy regulations).

---

# 7. Cryptographic Requirements

Identity Providers issuing tokens under this profile MUST ensure:

- **ES256** or **ES384** signing algorithms are RECOMMENDED  
- **RS256** (≥2048-bit keys) MAY be used  
- signing key rotation occurs at least every **180 days**  
- the JWKS endpoint is accessible exclusively over **HTTPS**  

---

# 8. Revocation Requirements

Identity Providers MUST follow **NIST-recommended revocation and incident
response practices**, including:

- token revocation mechanisms  
- credential compromise handling  
- fraud reporting channels  
- continuous monitoring and response  

---

# 9. Relying Party Requirements

Relying Parties (RPs) accepting this profile MUST:

- enforce U.S.-specific age and consumer protection rules  
- validate issuer metadata  
- validate declared IAL, AAL, and FAL levels where applicable  
- comply with applicable federal and state privacy laws  
- reject tokens not matching `policy_profile = "openvpt-us-1.0"`  

---

# 10. Versioning

This profile is uniquely identified by:

```
policy_profile = "openvpt-us-1.0"
```

---

# End of OpenVPT-US-Profile
