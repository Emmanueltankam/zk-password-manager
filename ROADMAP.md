# Roadmap

Each task becomes a GitHub issue when its milestone starts. Boxes are checked only when the work is merged.

## M0 — Design
- [x] Specifications (cahier des charges)
- [x] Security design and key hierarchy
- [x] Threat model (STRIDE)
- [ ] Repository tooling: linting, formatting, CI skeleton, secret scanning, Dependabot

## M1 — Crypto core (TypeScript)
- [ ] Argon2id key derivation (WASM) with parameter benchmark
- [ ] HKDF key separation
- [ ] AES-256-GCM encrypt and decrypt with additional authenticated data
- [ ] Vault key wrapping and unwrapping
- [ ] Unit tests with published test vectors

## M2 — API (FastAPI + PostgreSQL)
- [ ] Pre-login, registration and login endpoints
- [ ] Session tokens with rotation
- [ ] Encrypted item CRUD with versioning
- [ ] Rate limiting and lockout
- [ ] API tests and Docker Compose setup

## M3 — Web vault (React)
- [ ] Registration, login and unlock screens
- [ ] Item list, search, create, edit, delete
- [ ] Password generator with entropy display
- [ ] Auto-lock

## M4 — Browser extension (Manifest V3)
- [ ] Popup: unlock, search, copy
- [ ] Origin-matched autofill on user action
- [ ] Clipboard clearing and auto-lock

## M5 — Hardening
- [ ] Optional TOTP second factor
- [ ] Have I Been Pwned k-anonymity check
- [ ] CSP review, CodeQL and Semgrep in CI
- [ ] Threat model review against the implementation

## M6 — Release v1.0
- [ ] User and deployment documentation
- [ ] Demo GIF
- [ ] Tagged release
