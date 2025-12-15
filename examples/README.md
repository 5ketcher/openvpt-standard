# OpenVPT — Examples

This folder contains reference examples for implementers of the **OpenVPT
(Open Verified Person Token)** standard.

## Files

### issue-request.json
Request sent by a Relying Party (RP) to the Identity Provider (IdP) to issue a
**VPT (Verified Person & Age Token)**.

### issue-response.json
Example response containing a short-lived **VPT JWT**.

### verify-request.json
Request used by an RP or third party to validate a VPT token.

### verify-response-valid.json
Successful validation of a token.

### verify-response-revoked.json
Example of a revoked token.

### sample-token.jwt
Raw JWT for testing decoders and integration.

### sample-jwks.json
Public key set (JWKS) used to verify token signatures.

---

These examples are **non-normative** and provided for demonstration and
integration testing purposes only.
