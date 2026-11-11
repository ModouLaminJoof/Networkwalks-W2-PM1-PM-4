# Networkwalks W2 PM1 PM4

## Web Reconnaissance and Information Gathering

This project documents a passive and low-impact reconnaissance assessment of `networkwalks.com` using common Kali Linux tools. The purpose is to identify publicly exposed DNS, domain-registration, HTTP, web-technology, and WAF information for authorized security assessment and learning.

> **Scope and authorization:** Run these commands only against systems you own or have explicit permission to assess. The results below are a point-in-time snapshot and should not be treated as a complete vulnerability assessment.

## Objectives

- Resolve the target domain and identify DNS records.
- Review publicly available WHOIS registration information.
- Inspect HTTP response headers and server behavior.
- Fingerprint web technologies and frameworks.
- Identify whether a web application firewall is present.

## Tools and Commands

| Tool | Command | Purpose |
| --- | --- | --- |
| `nslookup` | `nslookup networkwalks.com` | Resolve the domain through DNS. |
| `dnsrecon` | `dnsrecon -d networkwalks.com` | Enumerate common DNS records and related services. |
| `whois` | `whois networkwalks.com` | Retrieve public domain-registration data. |
| `curl` | `curl -I https://networkwalks.com` | Inspect HTTP response headers. |
| `WhatWeb` | `whatweb networkwalks.com` | Identify web technologies and server metadata. |
| `WAFW00F` | `wafw00f networkwalks.com` | Detect web application firewall technology. |

## Findings Summary

### DNS resolution

- Public DNS resolution returned `192.232.216.135`.
- The resolver used in the capture was Google Public DNS at `8.8.8.8`.

### DNS enumeration

- Authoritative name servers: `ns6135.hostgator.com` and `ns6136.hostgator.com`.
- The SOA record pointed to `ns6135.hostgator.com`.
- The MX record pointed to `mail.networkwalks.com` at `192.232.216.135`.
- SPF and Google site-verification TXT records were present.
- DNSSEC was reported as not configured.
- An autodiscover SRV record was observed through cPanel mail discovery.

### WHOIS

- Registrar: GoDaddy.com, LLC.
- Creation date: `2019-11-06`.
- Registry expiration date: `2027-11-06`.
- The domain used privacy-protected registration details.
- The recorded name servers included HostGator servers and DomainControl servers.
- DNSSEC was reported as unsigned.

### HTTP headers

The HTTPS endpoint returned `HTTP/2 200` and exposed, among other headers:

- `server: Apache`
- `content-type: text/html; charset=UTF-8`
- WordPress REST API links through the `Link` header
- A WordPress-related `__wpdm_client` cookie marked `Secure` and `HttpOnly`
- `referrer-policy: no-referrer-when-downgrade`
- `x-nginx-cache: WordPress`

### Technology fingerprinting

WhatWeb identified:

- WordPress `7.1.4`
- WordPress Download Manager `3.3.76`
- Apache
- Bootstrap `7.1.4`
- jQuery `3.7.1`
- HTML5 and Open Graph metadata
- Site title: **Networkwalks Academy**
- Public contact address: `info@networkwalks.com`

The HTTP endpoint redirected from `http://networkwalks.com` to HTTPS, while the HTTPS endpoint returned `200 OK`.

### WAF detection

WAFW00F reported that `https://networkwalks.com` is behind **ModSecurity (SpiderLabs)** after two requests.

## Evidence

### `nslookup`

![nslookup output](Command%20screenshots/nslookup.png)

[Raw output](Output%20files/nslookup%20results.txt)

### `dnsrecon`

![dnsrecon output](Command%20screenshots/dnsrecon.png)

[Raw output](Output%20files/dnsrecon%20results.txt)

### `whois`

![WHOIS output](Command%20screenshots/whois%20.png)

[Raw output](Output%20files/Whois%20results.txt)

### `curl`

![curl output](Command%20screenshots/curl.png)

[Raw output](Output%20files/curl%20results.txt)

### `WhatWeb`

![WhatWeb output](Command%20screenshots/whatweb.png)

[Raw output](Output%20files/Whatweb%20results.txt)

### `WAFW00F`

![WAFW00F output](Command%20screenshots/watw00f.png)

[Raw output](Output%20files/wafw00f%20results.txt)

## Repository Structure

```text
.
├── Command screenshots/
│   ├── curl.png
│   ├── dnsrecon.png
│   ├── nslookup.png
│   ├── watw00f.png
│   ├── whatweb.png
│   └── whois .png
├── Output files/
│   ├── curl results.txt
│   ├── dnsrecon results.txt
│   ├── nslookup results.txt
│   ├── wafw00f results.txt
│   ├── Whatweb results.txt
│   └── Whois results.txt
└── README.md
```

## Conclusion

The assessment identified a WordPress-based website hosted on Apache, supported by HostGator DNS infrastructure, with publicly discoverable DNS and mail records. The site redirects HTTP traffic to HTTPS and is protected by ModSecurity. Further testing should be performed only with authorization and should include version verification, configuration review, and controlled vulnerability assessment.
