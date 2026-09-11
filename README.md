# Cybersecurity notes

Personal reference notes on cybersecurity fundamentals, methodology, and the tools I use most for bug bounty and pentesting work.

## Contents

- [Fundamentals](#fundamentals)
- [Methodology](#methodology)
- [Tools and networking](#tools-and-networking)
- [Web security](#web-security)
- [Bash scripting](#bash-scripting)
- [Bug bounty report template](#bug-bounty-report-template)

---

## Fundamentals

### CIA triad

The three properties most security controls exist to protect:

| Property | Meaning |
|---|---|
| Confidentiality | Keep data secret |
| Integrity | Data must not be changed |
| Availability | Data and systems must be accessible |

### Threat, vulnerability, and risk

- **Threat** — a possible danger: an attacker, a virus.
- **Vulnerability** — a weakness or hole in a system.
- **Risk** — what you get when a threat meets a vulnerability. Most security work is about reducing risk, not eliminating every threat or vulnerability that exists.

### CVE, severity, and CVSS

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

### Encryption and hashing

Encryption turns readable data (plaintext) into unreadable data (ciphertext) using an algorithm and a key. Decryption reverses it with the correct key.

Symmetric encryption uses one key for both directions — AES and ChaCha20 are the common ones. Asymmetric encryption uses a public key to encrypt or verify and a private key to decrypt or sign — RSA and ECC.

Hashing converts data of any size into a fixed-length value. A hash function should be one-way (you can't get the input back from the hash) and deterministic (same input always produces the same hash). SHA-256, SHA-3, bcrypt, and Argon2 are the common choices.

Passwords should be stored as hashes, never as plaintext, and each one needs a unique random salt before hashing to defeat rainbow table attacks. bcrypt, scrypt, and Argon2 are built for this — a general-purpose hash like SHA-256 isn't a good fit for passwords on its own.

A digital signature proves data came from the claimed sender and wasn't altered in transit: the private key creates the signature, the public key verifies it.

Encryption gets you confidentiality. Hashing gets you integrity, since it lets you detect whether data changed. Digital signatures cover integrity, authenticity, and non-repudiation together.

HTTPS/TLS encrypts traffic between a browser and a site. SSL is the deprecated predecessor — modern secure connections run on TLS.

Mistakes worth watching for: using MD5 or SHA-1 for passwords, storing encryption keys next to the data they encrypt, hardcoding keys in source code, weak or reused passwords, and rolling custom encryption instead of using a trusted library.

### Authentication vs authorization

Authentication answers "who are you" — username and password. Authorization answers "what can you do" — your permissions once you're in. Logging into Gmail is authentication; only being able to read your own emails is authorization.

### Payloads

A payload is the malicious code that runs once a system has been compromised — a reverse shell, ransomware, a backdoor. Metasploit is a common tool for building one.

---

## Methodology

### Hacker types

- **White hat** — ethical hacker, works with permission.
- **Black hat** — criminal hacker.
- **Grey hat** — hacks without permission but reports what they find.

### Team roles

- **Red team** — attacks.
- **Blue team** — defends.
- **Purple team** — does both, usually to get the other two working better together.

### Engagement basics

Every engagement defines a scope (what you're allowed to test) and rules of engagement (the rules you follow while testing).

Testing is usually one of three types:

- **Black box** — no information given.
- **Grey box** — partial information given.
- **White box** — full information given.

Common methodologies for structuring the work: OWASP, PTES, OSSTMM.

### The penetration testing process

1. Reconnaissance — gather information.
2. Scanning — find weaknesses.
3. Gaining access — exploit.
4. Maintaining access.
5. Covering tracks.
6. Reporting — write it up.

### MITRE ATT&CK

A free, public database of real-world attacker techniques. It documents how attackers actually behave, and most large companies and government agencies use it as a reference.

### Cyber kill chain

1. Reconnaissance
2. Weaponization
3. Delivery
4. Exploitation
5. Installation
6. Command and control
7. Actions on objectives

---

## Tools and networking

### Nmap

#### Scan types

| Flag | What it does |
|---|---|
| `-sS` | TCP SYN scan — stealthy, doesn't complete the handshake |
| `-sU` | UDP scan — for ports like DNS and DHCP |
| `-sT` | TCP connect scan — completes the full connection, doesn't need root |
| `-F` | Fast scan — top 100 ports |
| `-sV` | Version detection — identifies service/software versions on open ports |
| `-O` | OS detection |
| `-A` | Aggressive scan — OS detection, version detection, scripts, and traceroute together |

#### Output

| Flag | What it does |
|---|---|
| `-oN` | Save as plain text |
| `-oX` | Save as XML |
| `-oA` | Save in all formats at once |
| `-v` | Verbose — show results as they come in |

#### Timing

`-T0` through `-T5` control scan speed. `-T4` is the usual recommended default.

#### Targets and general options

| Flag | What it does |
|---|---|
| `-iL` | Read targets from a file, e.g. `nmap -iL targets.txt` |
| `-iR` | Scan random targets, e.g. `nmap -iR 100` |
| `--exclude` | Exclude specific hosts, e.g. `--exclude 1.1.1.1` |
| `-sL` | List targets without sending any packets |
| `-sn` | Ping scan — check which hosts are up without scanning ports |
| `-Pn` | Treat every host as online, skipping discovery — useful when a firewall blocks pings |
| `--traceroute` | Trace the network path to the target |
| `-sO` | Scan IP protocols instead of ports |
| `-p` | Scan specific port(s) |
| `-p-` | Scan every port, 1–65535 |
| `--script vuln` | Run NSE scripts to check for known vulnerabilities |

### curl

| Flag | What it does |
|---|---|
| `-I` / `--head` | Fetch headers only |
| `-i` / `--include` | Include headers with the response body |
| `-v` / `--verbose` | Show the full request, response, and handshake |
| `-L` / `--location` | Follow redirects |
| `-X` / `--request` | Set the HTTP method (GET, POST, PUT, DELETE...) |
| `-d` / `--data` | Send POST data, e.g. `-d "param=value"` |
| `-H` / `--header` | Add a custom header, e.g. `-H "Authorization: Bearer <token>"` |
| `-u` / `--user` | Basic auth credentials (`user:password`) |
| `-b` / `--cookie` | Send cookies, as a string or from a file |
| `-c` / `--cookie-jar` | Save received cookies to a file |
| `-o` / `--output` | Write output to a file |
| `-O` / `--remote-name` | Save using the remote file's name |
| `-k` / `--insecure` | Skip TLS certificate checks |
| `-s` / `--silent` | No progress meter or error output |
| `-x` / `--proxy` | Route through a proxy, e.g. `-x 127.0.0.1:8080` |

### HTTP status codes

- 1xx — informational: request received, still processing.
- 2xx — success.
- 3xx — redirection: more action needed to finish the request.
- 4xx — client error: bad request, can't be fulfilled as sent.
- 5xx — server error: the server failed on a request that looked valid.

The ones that come up most often:

| Code | Meaning |
|---|---|
| 200 OK | Request succeeded |
| 201 Created | Succeeded, new resource created (common after POST) |
| 204 No Content | Succeeded, nothing to return |
| 301 Moved Permanently | Resource has a new permanent URL |
| 302 Found | Temporary redirect |
| 304 Not Modified | Cached version is still valid |
| 400 Bad Request | Malformed request |
| 401 Unauthorized | Not authenticated |
| 403 Forbidden | Authenticated but not allowed |
| 404 Not Found | Resource doesn't exist |
| 405 Method Not Allowed | Wrong HTTP method for this endpoint |
| 429 Too Many Requests | Rate limited |
| 500 Internal Server Error | Generic server-side failure |
| 502 Bad Gateway | Upstream server sent an invalid response |
| 503 Service Unavailable | Server temporarily can't handle requests |
| 504 Gateway Timeout | Upstream server didn't respond in time |

### HTTP methods

- `GET` — request data.
- `POST` — submit data for processing.
- `PUT` — replace a resource entirely.
- `PATCH` — apply a partial update.
- `DELETE` — remove a resource.
- `OPTIONS` — describe what's supported on an endpoint.

### Common ports

| Port | Service | Notes |
|---|---|---|
| 21 | FTP | unencrypted |
| 22 | SSH | encrypted remote access |
| 23 | Telnet | unencrypted |
| 25 | SMTP | sending mail |
| 53 | DNS | name resolution |
| 69 | TFTP | UDP, no auth |
| 80 | HTTP | unencrypted web |
| 88 | Kerberos | Active Directory auth |
| 110 | POP3 | retrieving email |
| 123 | NTP | clock sync |
| 139 | NetBIOS | older Windows session management |
| 143 | IMAP | email access |
| 161 | SNMP | monitoring network devices |
| 389 | LDAP | directory lookups |
| 443 | HTTPS | encrypted web |
| 445 | SMB | file sharing |
| 636 | LDAPS | LDAP over TLS |
| 1433 | MSSQL | |
| 1521 | Oracle DB | |
| 3306 | MySQL | |
| 3389 | RDP | Windows remote desktop |
| 5432 | PostgreSQL | |
| 6379 | Redis | |
| 8080 | HTTP-Proxy / Alt-HTTP | common alt web port |
| 8443 | HTTPS-Alt | common alt secure port |

### Linux commands

`ls`, `ls -la`, `pwd`, `cd`, `mkdir`, `rmdir`, `touch`, `cp`, `mv`, `rm`, `rm -rf`, `cat`, `less`, `head`, `tail`, `tail -f`, `nano`, `grep`, `find`, `chmod`, `chown`, `ps aux`, `top`, `kill`, `df -h`, `du -sh`, `tar`, `curl`, `wget`, `sudo`, `clear` — the usual set for navigating, editing, and inspecting a system during an engagement.

### OSINT and recon tools

| Tool | Purpose |
|---|---|
| subfinder | Passive subdomain enumeration |
| assetfinder | Find related domains and subdomains |
| amass | Attack surface mapping and asset discovery |
| httpx | Probe live hosts over HTTP |
| naabu | Fast port scanner |
| dnsx | DNS queries |
| waybackurls | Pull known URLs from the Wayback Machine |
| gau | Pull known URLs from AlienVault, Wayback Machine, and Common Crawl |
| katana | Crawling and spidering |
| ffuf | Web fuzzing |
| gobuster | Brute-force URIs, DNS subdomains, vhosts |
| nuclei | Vulnerability scanning with YAML templates |
| dirsearch | Web path scanning/brute-forcing |
| wpscan | WordPress vulnerability scanner |
| arjun | Find hidden GET/POST parameters |

---

## Web security

### OWASP Top 10

| # | Risk | Description |
|---|---|---|
| A01 | Broken Access Control | Unauthorized users reaching resources or actions they shouldn't |
| A02 | Cryptographic Failures | Sensitive data exposed through missing or weak encryption |
| A03 | Injection | Untrusted input executed as commands — SQLi, command injection, LDAP injection |
| A04 | Insecure Design | Flaws baked into the architecture before any code is written |
| A05 | Security Misconfiguration | Default passwords, exposed debug logs, unpatched services |
| A06 | Vulnerable and Outdated Components | Libraries or frameworks with known CVEs still in use |
| A07 | Identification and Authentication Failures | Weak passwords, no brute-force protection, broken sessions |
| A08 | Software and Data Integrity Failures | Unsigned plugins, untrusted auto-updates, insecure deserialization |
| A09 | Security Logging and Monitoring Failures | No breach alerts, no log retention, slow response |
| A10 | Server-Side Request Forgery (SSRF) | Tricking a server into fetching internal resources |

---

## Bash scripting

Quick syntax reference for scripting during engagements.

### Basics

- `chmod +x file.sh` makes a script executable.
- `#!/bin/bash` is the shebang — it tells the shell which interpreter to use.
- `echo` prints to the screen.
- `variable_name=value` defines a variable — no spaces around the `=`.
- `unset var_name` deletes a variable.
- `var_name=$(date)` or `` var_name=`date` `` captures a command's output into a variable.
- `$0` is the script name, `$1`/`$2` are positional arguments, `$#` is the argument count, `$@` is the full argument list.

### Comparisons

| Operator | Meaning |
|---|---|
| `-eq` / `==` | equal |
| `-ne` / `!=` | not equal |
| `-gt` | greater than |
| `-ge` | greater than or equal |
| `-lt` | less than |
| `-le` | less than or equal |
| `&&` / `-a` | AND |
| `\|\|` / `-o` | OR |

### Control structures

```bash
if [ <test> ]; then
    <commands>
elif [ <test> ]; then
    <commands>
else
    <commands>
fi
```

```bash
for i in 1 2 3; do echo "$i"; done
for i in $(seq 1 10); do echo "$i"; done
for i in {1..10}; do echo "$i"; done
```

```bash
n=1
while [ $n -lt 10 ]; do
    echo "Hello $n"
    ((n++))
done
```

### Functions, arrays, and redirection

```bash
function_name() { <commands>; }
function_name  # call it
```

Arrays:

```bash
arr=("a" "b" "c")
echo ${arr[0]}    # first element
echo ${#arr[@]}   # element count
```

`echo $?` returns the exit code of the last command — 0 means success.

Redirection:

```bash
echo "Hello" > file.txt    # overwrite
echo "Hello" >> file.txt   # append
cat < file.txt              # read as input
```

---

## Bug bounty report template

```
Title:
Severity: Low / Medium / High / Critical
Summary:
Endpoint:
Steps to Reproduce:
POC:
Impact:
Remediation:
```
