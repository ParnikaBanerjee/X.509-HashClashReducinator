# X.509-HashClashReducinator- Interactive Demo

A single-file, dependency-free HTML explainer covering three mechanisms that prevent X.509 certificate hash collisions: **serial number entropy**, **signature algorithm binding**, and **strict DER encoding**. Everything runs in the browser with no build step.

---

## Usage

Open `x509demo.html` in any modern browser. No server required.

---

## Structure

The page is divided into three sections, each with its own interactive demos.

### Section 1 — Serial Number Entropy (Demos 1–7)

Covers why weak serial number generation enables chosen-prefix collision attacks, with four selectable schemes:

| Scheme | Entropy | Risk |
|---|---|---|
| Sequential counter | < 1 bit | Critical |
| Unix timestamp | ~5 bits | Critical |
| Weak random (20-bit) | 20 bits | High |
| Cryptographic CSPRNG (128-bit) | 128 bits | Safe |

| Demo | What it shows |
|---|---|
| 1 — Predictability | Live cert stream; attacker predictions shown in real time |
| 2 — Collision heatmap | 2 000 certs plotted across address space; collisions turn red |
| 3 — Birthday paradox | Trials until first collision vs theoretical 50% threshold |
| 4 — Collision probability | P(collision) sigmoid curves for all four schemes, log-scaled |
| 5 — Attacker work factor | Expected brute-force time at 10⁹ guesses/sec; issuance rate slider |
| 6 — Entropy calculator | Bit-width slider with live address space, threshold, and brute-force readouts |
| 7 — Distribution uniformity | Bucket histogram + χ² uniformity score for 4 000 issuances |

### Section 2 — Signature Algorithm Binding (Demos 8–10)

Covers RFC 5280 §4.1.1.2: the requirement that `tbsCertificate.signature` (inner) and `Certificate.signatureAlgorithm` (outer) carry identical OIDs, preventing algorithm substitution attacks.

| Demo | What it shows |
|---|---|
| 8 — Certificate structure viewer | ASN.1-style cert with both algorithm fields annotated; OID binding verdict |
| 9 — Verifier check | Strict RFC 5280 verifier vs lax verifier, side by side |
| 10 — Algorithm substitution timeline | Animated sequence diagram of the full attack and where it is stopped |

Three modes: **Bound** (secure), **Mismatch** (RFC violation), **Substitution attack** (outer OID swapped post-signing).

### Section 3 — Strict DER Encoding (Demos 11–13)

Covers why DER's canonical encoding rules prevent semantic collisions — two different byte strings that parse as the same certificate but hash differently, breaking CT logs, OCSP, and pinning.

| Demo | What it shows |
|---|---|
| 11 — TLV byte-level inspector | Raw Tag-Length-Value bytes annotated byte by byte; violations flagged |
| 12 — Canonical equivalence checker | DER vs BER variant compared: logical match, byte mismatch, digest divergence |
| 13 — Hash divergence under malleability | Visual digest comparison; shows why lax parsers break fingerprint checks |

Five encoding modes: **Strict DER**, **BER indefinite length**, **BER non-canonical BOOLEAN**, **BER padded INTEGER**, **Malleability attack**.

---

## Key references

- RFC 5280 — Internet X.509 PKI Certificate and CRL Profile
- CA/Browser Forum Baseline Requirements §7.1 (serial number entropy)
- Sotirov et al. 2008 — MD5 rogue CA attack (motivated §1 and §2)
- SHAttered (2017) — SHA-1 chosen-prefix collision

---

## Files

```
x509demo.html   single self-contained file, ~1 870 lines
README.md       this file
```
