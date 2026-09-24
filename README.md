# OSINT Project — Passive Attack Surface Assessment (Redacted)

> **Disclaimer:** This repository contains a sanitized, redacted summary of a passive OSINT assessment. All sensitive identifiers — domains, subdomains, IP addresses, ASNs, vendor names, and screenshots — have been removed or replaced. No active scanning, brute-forcing, exploitation, denial-of-service, or authentication attempts were performed against any live system. This is not a formal responsible-disclosure submission unless explicitly authorized by the asset owner.

---

## Authorization

This assessment was conducted only after obtaining explicit written authorization from the asset owner.

- **Authorized by:** [Name, Role]
- **Authorization date:** [YYYY-MM-DD]
- **Authorization reference:** [Ticket / Contract / Email ID]
- **Target scope:** [REDACTED]
- **Rules of engagement:**
  - Passive OSINT only
  - No active network scanning
  - No directory brute-forcing
  - No credential or login attempts
  - No exploitation or payload delivery
  - No interaction beyond a single standard HTTP GET where necessary
- **Data handling:**
  - Raw evidence stored privately
  - Public repository contains only redacted data
  - Sensitive findings reported privately to the asset owner before any publication

If you do not have written authorization, do not replicate this work against third-party systems.

---

## Executive Summary

A passive OSINT assessment was performed to map the external attack surface of a redacted telecom provider in Pakistan. Only publicly available, passive data sources were used.

### Key numbers

- **172** unique subdomains enumerated from public sources
- **8** IPs independently confirmed as target-owned via RIPE and passive scan data
- **9** findings documented:
  - **1 High**
  - **3 Medium**
  - **2 Low**
  - **3 Informational**
- **0 Critical** findings identified from passive sources alone

### Highest-priority finding

An unauthenticated, fingerprintable enterprise VPN gateway was identified on the target’s own network. The product family was confirmed through two independent passive methods.

### Positive controls observed

- A CDN/WAF provider blocked automated probing on several endpoints
- A second WAF actively rejected unrecognized requests
- No live credentials were found in public code repositories

---

## Scope & Methodology

### In scope

- WHOIS / RDAP
- Certificate Transparency logs
- Public DNS resolution
- Historical DNS and Wayback Machine data
- Passive internet-wide scan databases
- Public code repository search

### Out of scope

- Active network scanning (e.g., Nmap)
- Directory brute-forcing
- Credential or login attempts
- Exploitation
- Any interaction beyond a single standard HTTP GET request

### Tooling

| Phase | Tools |
|---|---|
| Subdomain enumeration | subfinder, crt.sh |
| DNS resolution | dnsx |
| ASN / IP ownership | RIPEStat API, WHOIS |
| HTTP fingerprinting | httpx |
| Passive scan data | Censys, Shodan |
| Historical data | Wayback Machine, waybackurls, gau |
| Code / secret exposure | GitHub code search |

### Workflow

1. Enumerate subdomains across multiple independent sources
2. Merge and de-duplicate into a master list
3. Resolve live hosts and collect IP addresses
4. Verify ASN/IP ownership before attributing infrastructure to the target
5. Fingerprint priority HTTP services
6. Manually verify notable findings with a single request
7. Enrich confirmed target-owned IPs with passive scan data
8. Review historical URLs and public code repositories for forgotten endpoints or leaked secrets

---

## Findings Summary (Sanitized)

| # | Finding | Risk | Business Impact | Recommended Action |
|---|---|---|---|---|
| 1 | Unauthenticated enterprise VPN gateway fingerprint | High | Exposes the remote-access VPN product family on the corporate front door, allowing attackers to target known CVEs and the staff/partner entry point | Verify firmware against vendor advisories; restrict or disable unauthenticated install/WebLaunch paths |
| 2 | Source-code analysis platform behind CDN, origin unresponsive | Medium | If restored without access control, could expose internal source code, embedded credentials, and API keys | Confirm whether it should be internet-facing; enforce SSO before exposure |
| 3 | Abandoned IIS deployment serving default page | Medium | Indicates incomplete or abandoned deployment with no active oversight; discloses server software details | Decommission the host or redeploy the intended application; remove public DNS if unused |
| 4 | Third-party GPS/fleet tracking portal | Medium | Third-party platform could compromise the target’s instance, risking live employee and vehicle location data; exposed RPC portmapper widens attack surface | Review vendor security contractually; enforce stronger password policy; restrict exposed RPC service |
| 5 | Shared/legacy mail infrastructure | Low | Shared infrastructure widens the pool of parties who could affect security posture; open SMTP should be checked for relay misconfiguration | Confirm ownership and relay configuration internally |
| 6 | Dangling redirect to non-resolving beta host | Low | Latent subdomain takeover risk if the hostname is later provisioned on attacker-claimable infrastructure | Remove the redirect or re-provision the DNS record under target control |
| 7 | Historical biometric verification portal | Info | No live exposure, but confirms naming convention and functional purpose of a sensitive system class is publicly discoverable | Awareness only; no action required from passive findings |
| 8 | WAF blocking automated requests | Info | Documents a working perimeter control rather than an exposure | Continue monitoring |
| 9 | Public code repository secret search | Info | No live, exploitable credentials found; one integration correctly uses environment variables; one placeholder key only | Continue secret scanning and developer education |

---

## Risk Scoring Methodology

Each finding was rated by likelihood × impact: how easily the exposure can be found and leveraged using public tools, weighed against the business function or data behind it.

| Rating | Criteria |
|---|---|
| Critical | Internet-facing service with a publicly known, exploitable CVE; no authentication; or sensitive data exposed publicly |
| High | Internet-facing internal-looking system confirmed via passive fingerprinting, with outdated or disclosed software version |
| Medium | Subdomain takeover risk, verbose error/tech-stack disclosure, unresponsive origin behind WAF/CDN, or third-party supply-chain dependency |
| Low | Cosmetic or low-likelihood issues: shared/legacy infrastructure, expiring certificates, missing security headers on non-critical pages |
| Info | No exploitable exposure identified; included for completeness or to document a working security control |

---

## Prioritized Remediation

1. **VPN gateway:** Verify current firmware against vendor security advisories; restrict unauthenticated access to install/WebLaunch paths where not required.
2. **Source-code analysis platform:** Confirm whether the origin is meant to be internet-facing; if so, restore behind authentication/SSO before it becomes reachable again.
3. **Abandoned IIS deployment:** Decommission the host or redeploy the intended application; remove the public DNS record if the asset is no longer in use.
4. **Third-party tracking platform:** Review vendor security posture contractually; enforce stronger password policy; restrict the exposed portmapper service.
5. **Dangling redirect:** Remove the redirect or re-provision the DNS record under target control to eliminate the takeover window.
6. **Shared SMTP infrastructure:** Confirm ownership and relay configuration internally; verify it is not an open relay.

---

## Redaction Notes

This public version has been sanitized:

- Real domains replaced with `redacted.example`
- Subdomains replaced with role labels such as `vpn.redacted.example`
- Real IPs replaced with RFC 5737 documentation ranges
- ASNs replaced with `AS64500`
- Vendor names genericized where they could identify the target
- Screenshots cropped, blurred, or omitted
- Raw evidence stored privately and not committed to this repository
- PDF metadata stripped before publishing

---

## Ethics & Legal

- Conducted under explicit written authorization.
- Passive OSINT only.
- No active scanning, exploitation, or authentication attempts.
- Findings reported privately to the asset owner before publication.
- This repository is for educational and defensive purposes only.
- Do not use these techniques against systems you do not own or have written permission to assess.

---

## Lessons Learned

- Passive OSINT alone can map a large external attack surface.
- Verify ASN/IP ownership before attributing infrastructure to a target.
- Positive controls matter: document what blocked you, not just what failed.
- Dangling DNS records and abandoned deployments are common and easy to miss.
- Public code repositories should be checked for secrets, even when the result is clean.

---

## Repository Structure

```text
OSINT-project/
├── README.md
├── DISCLAIMER.md
├── methodology.md
├── findings-sanitized.md
├── risk-rubric.md
├── scripts/
│   ├── subdomain_enum.sh
│   ├── dns_resolve.sh
│   └── http_fingerprint.sh
├── sample-output/
│   └── fake-example-output.txt
├── images/
│   └── redacted-diagrams/
└── .gitignore

##Disclaimer

This project is not affiliated with, endorsed by, or sponsored by any organization mentioned in the private raw report. All information published here is redacted and intended for defensive security education only. No warranty is provided. Use responsibly and only with proper authorization.


























