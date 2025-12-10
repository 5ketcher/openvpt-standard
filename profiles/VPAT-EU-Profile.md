# VPAT-EU-Profile (v1.0 – Draft)

**Status:** Proposed Profile for EU-compatible deployments  
**Author:** Vojtěch Sejkora  
**Date:** 2025-12-10  

---

# 1. Purpose

This profile defines the rules, assurance levels, and allowed Identity Providers (IdPs) for deployments of the TrustID VPAT standard within the European Union.

This profile aligns with:

- eIDAS 2.0
- EU Digital Identity Wallet (EUDI)
- regulated KYC/AML identity providers
- EU cybersecurity and privacy directives

---

# 2. Allowed Identity Providers

An IdP MUST fall into one of the following categories:

- EU Member State eIDAS-notified Identity Provider
- EU Digital Wallet Provider
- Licensed KYC/AML provider operating under EU regulation
- EU-based financial institution (bank)
- EU telecom operator complying with SIM registration requirements

Unregulated providers MAY NOT issue VPAT tokens under this profile.

---

# 3. Required Assurance Levels

The token MUST contain:

- assurance_level = "substantial" or "high"

Tokens with "low" MUST be rejected by Relying Parties.

---

# 4. Required Token Claims

The following claims MUST exist in all VPAT tokens issued under this profile:

- real_person = true
- age_bracket or age_over_*
- issuer_country must be an EU member state
- policy_profile = "vpat-eu-1.0"

---

# 5. Cryptographic Requirements

The IdP MUST:

- sign using ES256, ES384, or RS256 with key length ≥ 2048
- rotate keys at least every 180 days
- publish a JWKS endpoint accessible over HTTPS

---

# 6. Revocation Requirements

The IdP MUST:

- support token revocation lookup
- maintain account/device revocation lists
- immediately revoke tokens upon account compromise

---

# 7. Relying Party Requirements

An RP accepting this profile MUST:

- reject tokens that do not contain policy_profile = "vpat-eu-1.0"
- reject issuers outside the EU
- verify LoA and issuer metadata
- enforce minimum age verification where required

---

# 8. Versioning

This profile is identified by:

```
policy_profile = "vpat-eu-1.0"
```

# End of VPAT-EU-Profile
