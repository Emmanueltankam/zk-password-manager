# Specifications (cahier des charges)

| | |
|---|---|
| **Project** | zk-password-manager |
| **Author** | Emmanuel Tankam Talla |
| **Version** | 0.1 (design phase) |
| **Status** | Draft, updated at every milestone |

---

## 1. Context and goals

Password reuse and weak passwords remain one of the first causes of account compromise. Password managers solve this, but they concentrate every secret in one place: their design has to assume that **the server, the network and even the browser page can be hostile**.

This project builds a password manager from scratch to **learn and demonstrate applied cryptography and secure software design**, with three goals:

1. **Zero-knowledge**: the server never sees a master password, a key or a plaintext secret.
2. **Usable**: a web vault plus a browser extension that fills credentials, like Bitwarden or Dashlane.
3. **Security by design**: requirements, threat model and tests come before features.

## 2. Scope

| In scope (v1.0) | Out of scope (v1.0) |
|---|---|
| Single-user personal vault | Organisations, item sharing between users |
| Web vault (desktop browsers) | Native mobile apps |
| Browser extension (Chromium and Firefox, Manifest V3) | Passkeys / WebAuthn credential storage |
| Logins, secure notes, password generator | Credit cards and identity autofill |
| Optional TOTP second factor for the account | Account recovery without the master password |
| Encrypted export / import (JSON, KeePass CSV import) | Self-hosted sync between several servers |

## 3. Users and main use cases

**Persona**: a privacy-conscious individual who wants one strong master password instead of dozens of reused ones.

| ID | Use case |
|---|---|
| UC-01 | Create an account and a vault protected by a master password |
| UC-02 | Unlock the vault, browse and search entries |
| UC-03 | Add, edit and delete a login or a secure note |
| UC-04 | Generate a strong password or passphrase |
| UC-05 | Fill a login form from the browser extension on the right website only |
| UC-06 | Lock the vault manually or automatically after inactivity |
| UC-07 | Change the master password without re-encrypting every entry |
| UC-08 | Export an encrypted backup and import from KeePass |

## 4. Functional requirements

Priorities follow MoSCoW: **M**ust, **S**hould, **C**ould.

| ID | Requirement | Priority |
|---|---|---|
| FR-01 | Account registration with email and master password; the master password never leaves the client | M |
| FR-02 | Login with a key derived on the client; the server stores only a hash of that derived key | M |
| FR-03 | Vault unlock derives the encryption key locally (Argon2id) | M |
| FR-04 | Create, read, update, delete vault items (login, secure note) | M |
| FR-05 | Every item is encrypted on the client with AES-256-GCM before upload | M |
| FR-06 | Search and filter on decrypted data, in memory only | M |
| FR-07 | Password generator: length, character sets, passphrase mode, entropy shown | M |
| FR-08 | Automatic lock after configurable inactivity, and manual lock | M |
| FR-09 | Browser extension: popup to search entries and copy credentials | M |
| FR-10 | Browser extension: fill credentials only when the page origin matches the saved URL | M |
| FR-11 | Clipboard cleared automatically after 30 seconds | S |
| FR-12 | Master password change by re-wrapping the vault key only | S |
| FR-13 | Optional TOTP second factor on the account | S |
| FR-14 | Breach check of a password through the k-anonymity range API of Have I Been Pwned | S |
| FR-15 | Encrypted JSON export and import; import from KeePass CSV | S |
| FR-16 | Password health report: reused, weak and old passwords | C |
| FR-17 | Account activity log (logins, new devices) | C |

## 5. Non-functional requirements

| ID | Category | Requirement |
|---|---|---|
| NFR-01 | Security | Target **OWASP ASVS level 2** for the API and the web vault |
| NFR-02 | Security | Cryptography only from vetted primitives (WebCrypto, Argon2id); no home-made algorithm |
| NFR-03 | Security | Strict Content Security Policy, no inline scripts, no `eval` in the web vault and the extension |
| NFR-04 | Security | TLS everywhere, HSTS, secure cookies; rate limiting and lockout on authentication endpoints |
| NFR-05 | Security | Secrets never written to logs; automated secret scanning in CI |
| NFR-06 | Performance | Vault unlock in under 1.5 s on a mid-range laptop with the chosen Argon2id parameters |
| NFR-07 | Reliability | No data loss on concurrent edits: item versioning with conflict detection |
| NFR-08 | Usability | Main flows usable with the keyboard only; clear error messages that never leak secrets |
| NFR-09 | Maintainability | Unit tests on the crypto core with published test vectors; CI blocks merges on failure |
| NFR-10 | Privacy | Minimal personal data (email only), no third-party tracking, GDPR-friendly account deletion |

## 6. Constraints

- **Languages**: TypeScript for the crypto core, web vault and extension; Python (FastAPI) for the API.
- **Database**: PostgreSQL.
- **Hosting**: Docker Compose for local development; deployable in my home lab.
- **Budget**: open-source tools only.

## 7. Deliverables

| Deliverable | Milestone |
|---|---|
| Specifications, security design, threat model, roadmap | M0 |
| Crypto core library with tests and test vectors | M1 |
| REST API with automated tests | M2 |
| Web vault | M3 |
| Browser extension | M4 |
| Hardening report, updated threat model | M5 |
| v1.0 release, user and deployment documentation, demo | M6 |

## 8. Acceptance criteria for v1.0

- A database dump of the server reveals **no master password, no key and no plaintext item**.
- The extension **refuses to fill** a look-alike domain (for example `examp1e.com` for `example.com`).
- All functional requirements marked **M** are implemented and covered by tests.
- CI is green: tests, linting, dependency audit, static analysis and secret scanning.
- The threat model has been reviewed against the final implementation.

See [SECURITY_DESIGN.md](SECURITY_DESIGN.md), [THREAT_MODEL.md](THREAT_MODEL.md) and [ROADMAP.md](../ROADMAP.md).
