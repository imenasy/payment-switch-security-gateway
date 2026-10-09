# Standard Operating Procedure (SOP): Payment HSM Key Management & Rotation

## 1. Regulatory Context
Aligned with National Bank of Rwanda (BNR) Cybersecurity Directives and PCI-DSS v4.0 Requirement 3 (Protect Account Data).

## 2. Cryptographic Architecture
The Cooperative Bank and District SACCO Interoperability Engine utilizes Hardware Security Modules (Thales payShield 10K / Entrust nShield) to manage PIN encryption, CVV generation, and ISO 8583 message payload authentication.

## 3. Key Ceremony & Custodianship Protocol
- **Zone Master Keys (ZMK):** Generated under strict split-knowledge dual control using 3 separate physical smart cards held by independent custodians (Senior Manager Cybersecurity, Head of IT, Chief Risk Officer).
- **Zone PIN Keys (ZPK) & Terminal Master Keys (TMK):** Rotated automatically every 90 days via mTLS-secured key distribution.
- **Data Encryption Keys (DEK):** Rotated annually. Master database keys re-wrapped without plain-text export.

## 4. Key Decommissioning & Revocation Workflow
1. Declare revocation event in writing to the Head of IT.
2. Zeroize compromised key slot on HSM cluster across active and disaster recovery (DR) data centers simultaneously.
3. Verify backup key ring synchronization.
4. Document the key destruction certificate with timestamps and witness signatures.
