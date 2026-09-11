# Tools and networking

## Nmap

### Scan types

| Flag | What it does |
|---|---|
| `-sS` | TCP SYN scan — stealthy, doesn't complete the handshake |
| `-sU` | UDP scan — for ports like DNS and DHCP |
| `-sT` | TCP connect scan — completes the full connection, doesn't need root |
| `-F` | Fast scan — top 100 ports |
| `-sV` | Version detection — identifies service/software versions on open ports |
| `-O` | OS detection |
| `-A` | Aggressive scan — OS detection, version detection, scripts, and traceroute together |

### Output

| Flag | What it does |
|---|---|
| `-oN` | Save as plain text |
| `-oX` | Save as XML |
| `-oA` | Save in all formats at once |
| `-v` | Verbose — show results as they come in |

### Timing

`-T0` through `-T5` control scan speed. `-T4` is the usual recommended default.

### Targets and general options

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

## curl

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

## HTTP status codes

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

## HTTP methods

- `GET` — request data.
- `POST` — submit data for processing.
- `PUT` — replace a resource entirely.
- `PATCH` — apply a partial update.
- `DELETE` — remove a resource.
- `OPTIONS` — describe what's supported on an endpoint.

## Common ports

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

## Linux commands

`ls`, `ls -la`, `pwd`, `cd`, `mkdir`, `rmdir`, `touch`, `cp`, `mv`, `rm`, `rm -rf`, `cat`, `less`, `head`, `tail`, `tail -f`, `nano`, `grep`, `find`, `chmod`, `chown`, `ps aux`, `top`, `kill`, `df -h`, `du -sh`, `tar`, `curl`, `wget`, `sudo`, `clear` — the usual set for navigating, editing, and inspecting a system during an engagement.

## OSINT and recon tools

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
