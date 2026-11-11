# theHarvester OSINT Scan

This folder contains command screenshots and captured output from using
[theHarvester](https://github.com/laramies/theHarvester) 4.8.2 to collect
publicly available information about `microsoft.com`.

## Commands

### Broad source scan

```bash
theHarvester -d microsoft.com -l 50 -b all
```

![Broad theHarvester scan](Command%20screenshots/theharvester%20all.png)

### Baidu-only scan

```bash
theHarvester -d microsoft.com -l 1000 -b baidu
```

![Baidu theHarvester scan](Command%20screenshots/theHarvester%20baidu.png)

## Captured output

| Scan | Output |
| --- | --- |
| All configured sources | [`theHarvester all.txt`](Output%20files/theHarvester%20all.txt) |
| Baidu | [`theHarvester baidu.txt`](Output%20files/theHarvester%20baidu.txt) |

## Results summary

| Scan | Findings recorded |
| --- | --- |
| All sources | 818 hosts, no IPs, emails, or people |
| Baidu | 24 hosts, 2 email strings, no IPs or people |

The broad scan also recorded missing API-key warnings for several providers,
network disconnections, and errors while attempting API-endpoint and
SecurityScorecard scans. These messages are preserved in the full output file.

## Requirements

- Kali Linux or another Linux environment
- theHarvester 4.8.2
- Network access to the selected search sources
- API keys configured in `/etc/theHarvester/api-keys.yaml` when required by a source

## Responsible use

Only run theHarvester against domains and systems that you own or have
permission to assess. Treat discovered hostnames, email strings, and other
results as unverified reconnaissance data and handle them appropriately.
