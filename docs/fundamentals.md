# Fundamentals

## CIA triad

The three properties most security controls exist to protect:

| Property | Meaning |
|---|---|
| Confidentiality | Keep data secret |
| Integrity | Data must not be changed |
| Availability | Data and systems must be accessible |

## Threat, vulnerability, and risk

- **Threat** — a possible danger: an attacker, a virus.
- **Vulnerability** — a weakness or hole in a system.
- **Risk** — what you get when a threat meets a vulnerability. Most security work is about reducing risk, not eliminating every threat or vulnerability that exists.

## CVE, severity, and CVSS

- **CVE** — a unique ID assigned to a vulnerability, e.g. `CVE-2024-1234`.
- **Severity** — Low, Medium, High, or Critical.
- **CVSS** — a 0–10 score, where 10 is the most dangerous.

CVSS scores are built from a fixed set of metrics:

| Metric | Values |
|---|---|
| Attack Vector (AV) | Network / Adjacent / Local / Physical |
| Attack Complexity (AC) | Low / High |
| Privileges Required (PR) | None / Low / High |
| User Interaction (UI) | None / Required |
| Scope (S) | Unchanged / Changed |
| Confidentiality (C) | None / Low / High |
| Integrity (I) | None / Low / High |
| Availability (A) | None / Low / High |

## Encryption and hashing

Encryption turns readable data (plaintext) into unreadable data (ciphertext) using an algorithm and a key. Decryption reverses it with the correct key.

Symmetric encryption uses one key for both directions — AES and ChaCha20 are the common ones. Asymmetric encryption uses a public key to encrypt or verify and a private key to decrypt or sign — RSA and ECC.

Hashing converts data of any size into a fixed-length value. A hash function should be one-way (you can't get the input back from the hash) and deterministic (same input always produces the same hash). SHA-256, SHA-3, bcrypt, and Argon2 are the common choices.

Passwords should be stored as hashes, never as plaintext, and each one needs a unique random salt before hashing to defeat rainbow table attacks. bcrypt, scrypt, and Argon2 are built for this — a general-purpose hash like SHA-256 isn't a good fit for passwords on its own.

A digital signature proves data came from the claimed sender and wasn't altered in transit: the private key creates the signature, the public key verifies it.

Encryption gets you confidentiality. Hashing gets you integrity, since it lets you detect whether data changed. Digital signatures cover integrity, authenticity, and non-repudiation together.

HTTPS/TLS encrypts traffic between a browser and a site. SSL is the deprecated predecessor — modern secure connections run on TLS.

Mistakes worth watching for: using MD5 or SHA-1 for passwords, storing encryption keys next to the data they encrypt, hardcoding keys in source code, weak or reused passwords, and rolling custom encryption instead of using a trusted library.

## Authentication vs authorization

Authentication answers "who are you" — username and password. Authorization answers "what can you do" — your permissions once you're in. Logging into Gmail is authentication; only being able to read your own emails is authorization.

## Payloads

A payload is the malicious code that runs once a system has been compromised — a reverse shell, ransomware, a backdoor. Metasploit is a common tool for building one.
