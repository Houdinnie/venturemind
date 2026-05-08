---
name: vault-access
description: VentureMind Zero-Knowledge Document Vault skill — client-side AES-256-GCM encryption, encrypted storage, TEE-based document processing, and access policy enforcement.
metadata:
  author: houdinnie.zo.computer
  version: 1.0
---

# Zero-Knowledge Document Vault Skill

The Vault is VentureMind's secure document storage system. Documents are encrypted client-side before leaving the browser — the server never sees plain text.

## Encryption Flow

```
User File → Browser (AES-256-GCM)
              ↓
        PBKDF2 key derivation
        (100,000 iterations, unique salt)
              ↓
        Encrypted Blob → S3/Vault
              ↓
        Key never stored server-side
        (user holds decryption key)
```

## Supported Document Types

| Type | Examples | Retention | Re-Auth Frequency |
|------|----------|-----------|-------------------|
| PASSPORT | Passport, National ID | Account deletion | 30 days biometric |
| TAX_ID | SSN, EIN, TIN | 7 years | Every access |
| CONTRACT | Operating agreement, LOI | Contract expiry + 3 years | Every access |
| BANK_STATEMENT |Statements, advices | 5 years | Every access |
| CRYPTO_WALLET | Seed phrases, keys | Never delete | Every access (hardware) |

## Access Policy

1. User biometric re-verification required every 30 days
2. TEE (Trusted Execution Environment) used for all document processing
3. Agent never sees raw document — only TEE session output
4. Document checksum verified on every decrypt

## Vault API

```typescript
// Upload document (client-side encrypted)
POST /vault/documents
Body: {
  documentType: DocumentType,
  encryptedChecksum: string,
  vaultAlgorithm: "vault:v1:aes256gcm",
  keySalt: string,
  iv: string,
  ciphertext: string,
  region: string,
  bucket: string
}

// Request document access (triggers TEE session)
POST /vault/documents/:id/access
Body: {
  justification: string,
  requestingAgent: string,
  ttlSeconds: number
}

// TEE session response (agent receives encrypted reference + session token)
Response: {
  documentId: string,
  teeSessionToken: string,
  expiresAt: string,
  allowedActions: ["view", "redact", "ocr"]
}
```

## Integrity Verification

Every document stored has:
- `encryptedChecksum`: SHA-256 of plaintext (for integrity verification)
- `iv`: Unique per document (12-byte nonce)
- `keySalt`: Unique per document

On decrypt: recompute checksum, compare to stored `encryptedChecksum`. Mismatch → CRIMSON trigger.