# TrustID VPAT – Examples

This folder contains reference examples for implementers of the VPAT protocol.

## Files

### issue-request.json
Request sent by a Relying Party (RP) to the Identity Provider (IdP) to issue a VPAT token.

### issue-response.json
Example response containing a short-lived VPAT JWT.

### verify-request.json
Request used by an RP or third party to validate a VPAT token.

### verify-response-valid.json
Successful validation of a token.

### verify-response-revoked.json
Example of a revoked proof/token.

### sample-token.jwt
Raw JWT for testing decoders.

### sample-jwks.json
Public key set (JWKS) used to verify signatures.

---

These examples are non-normative and provided for demonstration only.
