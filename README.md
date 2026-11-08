# zk-password-manager

![Status](https://img.shields.io/badge/status-M0%20design-E3B341?style=flat-square)
![Security](https://img.shields.io/badge/model-zero--knowledge-2EA44F?style=flat-square)
![Crypto](https://img.shields.io/badge/crypto-Argon2id%20%C2%B7%20AES--256--GCM-1F6FEB?style=flat-square)
[![License: MIT](https://img.shields.io/badge/license-MIT-yellow?style=flat-square)](LICENSE)

A **zero-knowledge password manager**, web vault plus browser extension, built from scratch to learn and demonstrate **applied cryptography** and **secure software design**. Inspired by KeePass, Bitwarden and Dashlane.

> **Educational project, not audited.** Do not store real passwords before v1.0 and an independent review.

## Status

**Current milestone: M0, design.** Specifications, security design and threat model come first; code starts at M1. Progress is tracked in [ROADMAP.md](ROADMAP.md).

## Planned features

| Feature | Milestone |
|---|---|
| Client-side encrypted vault (logins, secure notes) | M1 to M3 |
| Password and passphrase generator with entropy display | M3 |
| Browser extension with origin-matched autofill | M4 |
| Auto-lock and clipboard clearing | M3, M4 |
| Optional TOTP second factor, breach check (Have I Been Pwned k-anonymity) | M5 |
| Encrypted export, KeePass import | M5, M6 |

## Security model in one picture

```mermaid
flowchart LR
    MP["Master password"] --> A["Argon2id"] --> MK["Master key"]
    MK -->|"HKDF"| EK["Encryption key"]
    MK -->|"HKDF"| AK["Auth key"]
    EK --> VK["Vault key"] --> IT["Encrypted items"]
    AK --> S["Server: hash only"]
```

The server only ever stores **ciphertext** and a **hash** of a derived key. Full details in [docs/SECURITY_DESIGN.md](docs/SECURITY_DESIGN.md).

## Architecture

```mermaid
flowchart LR
    X["Browser extension<br/>Manifest V3"] --> C["Crypto core<br/>TypeScript, WebCrypto, Argon2 WASM"]
    W["Web vault<br/>React"] --> C
    C -->|"HTTPS, ciphertext only"| API["API<br/>FastAPI"]
    API --> DB[("PostgreSQL")]
```

| Layer | Technology |
|---|---|
| Crypto core | TypeScript, WebCrypto API, Argon2id (WASM) |
| Web vault | React, Vite |
| Browser extension | Manifest V3, TypeScript |
| API | Python, FastAPI |
| Database | PostgreSQL |
| CI | GitHub Actions: tests, CodeQL, Gitleaks, Dependabot |

## Documentation

| Document | Content |
|---|---|
| [Specifications (cahier des charges)](docs/SPECIFICATIONS.md) | Goals, scope, use cases, functional and non-functional requirements, acceptance criteria |
| [Security design](docs/SECURITY_DESIGN.md) | Key hierarchy, cryptographic choices, flows, extension and server hardening |
| [Threat model](docs/THREAT_MODEL.md) | Assets, trust boundaries, adversaries, STRIDE analysis, residual risks |
| [Roadmap](ROADMAP.md) | Milestones M0 to M6 |

---

**Author:** Emmanuel Tankam Talla, cybersecurity engineering student (JUNIA ISEN). Licensed under MIT.
