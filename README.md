# OSINT Project — Passive Attack Surface Assessment (Lab Scenario)

> **Disclaimer:** This repository is a portfolio write-up of a simulated passive OSINT lab exercise. It does not describe any real organization, system, or live environment. All hostnames, IP addresses, ASNs, findings, and figures are illustrative examples created for educational and defensive security purposes only. No active scanning, brute-forcing, exploitation, denial-of-service, or authentication attempts were performed against any real system.

## Overview

This project demonstrates a structured, passive-only workflow for mapping an external attack surface using publicly available data sources. The goal is to show methodology, tooling, and reporting discipline rather than to disclose findings about any specific target.

## Skills Demonstrated

- Passive reconnaissance and OSINT tradecraft
- Subdomain enumeration and de-duplication across multiple sources
- DNS resolution and live-host identification
- ASN and IP ownership verification
- HTTP fingerprinting and technology profiling
- Passive scan database enrichment
- Historical URL and archive review
- Public code repository secret hunting
- Risk scoring using likelihood × impact
- Technical writing and remediation planning

## Methodology

### In scope
- WHOIS / RDAP
- Certificate Transparency logs
- Public DNS resolution
- Historical DNS and web archives
- Passive internet-wide scan databases
- Public code repository search

### Out of scope
- Active network scanning
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
4. Verify ASN/IP ownership before attributing infrastructure
5. Fingerprint priority HTTP services
6. Manually verify notable observations with a single request
7. Enrich confirmed-owned IPs with passive scan data
8. Review historical URLs and public code repositories for forgotten endpoints or leaked secrets

## Example Findings (Illustrative)

| # | Finding Type | Risk | Impact Theme | Remediation Theme |
|---|---|---|---|---|
| 1 | Internal-looking admin interface reachable externally | High | Exposes a privileged entry point | Restrict access; enforce MFA and network controls |
| 2 | Default web server landing page on a live host | Medium | Signals an abandoned or incomplete deployment | Decommission or redeploy the intended application |
| 3 | Staging environment indexed by search engines | Medium | Could expose pre-production data and behavior | Add auth and `noindex`; remove public DNS if unused |
| 4 | Third-party SaaS integration with weak password policy | Medium | Supply-chain exposure affecting hosted data | Contractual review; enforce stronger auth |
| 5 | Dangling DNS record pointing to a decommissioned host | Low | Latent subdomain takeover risk | Remove or re-provision under owner control |
| 6 | Outdated TLS configuration on a non-critical endpoint | Low | Weakens transport security posture | Modernize cipher suites and protocol versions |
| 7 | Public archive references a retired internal portal | Info | Reveals historical naming conventions | Awareness only |
| 8 | WAF blocking automated requests | Info | Confirms a working perimeter control | Continue monitoring |
| 9 | Secret scan of public repos | Info | No live credentials found | Continue secret scanning and developer education |

*Figures, counts, and findings above are synthetic examples for demonstration only.*

## Risk Scoring Methodology

Each item is rated by likelihood × impact: how easily it could be discovered and leveraged using public tools, weighed against the business function or data behind it.

| Rating | Criteria |
|---|---|
| Critical | Internet-facing service with a publicly known, exploitable CVE; no authentication in front; or sensitive data exposed publicly |
| High | Internet-facing internal-looking system confirmed via passive fingerprinting, with outdated or disclosed software version |
| Medium | Subdomain takeover risk, verbose error/tech-stack disclosure, unresponsive origin behind WAF/CDN, or third-party supply-chain dependency |
| Low | Cosmetic or low-likelihood issues: shared/legacy infrastructure, expiring certificates, missing security headers |
| Info | No exploitable exposure identified; included for completeness or to document a control |

## Prioritized Remediation (Generic)

1. Restrict unauthenticated access to privileged interfaces
2. Remove or decommission abandoned deployments and stale DNS records
3. Enforce authentication and SSO before exposing internal tooling
4. Review third-party platform security posture contractually
5. Eliminate dangling DNS and redirect chains
6. Verify mail and shared infrastructure configurations internally

## Ethics & Legal

- Simulated lab exercise; no real organization was assessed
- Passive OSINT techniques demonstrated for defensive education only
- No active scanning, exploitation, or authentication attempts
- Do not use these techniques against systems you do not own or have written permission to assess

## Lessons Learned

- Passive OSINT alone can map a large external attack surface
- Verify ASN/IP ownership before attributing infrastructure
- Positive controls matter: document what blocked you, not just what failed
- Dangling DNS records and abandoned deployments are common and easy to miss
- Public code repositories should be checked for secrets even when the result is clean

## Repository Structure

```text
OSINT-project/
├── README.md
├── methodology.md
├── risk-rubric.md
├── findings-example.md
├── scripts/
│   ├── subdomain_enum.sh
│   ├── dns_resolve.sh
│   └── http_fingerprint.sh
└── sample-output/
    └── example-output.txt
