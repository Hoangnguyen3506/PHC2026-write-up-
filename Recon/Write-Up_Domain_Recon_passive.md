## Write-Up

### Objective

Find a hidden subdomain using only open-source intelligence (OSINT), without interacting with the company's infrastructure.

### Solution

1. Use a passive subdomain enumeration method via Certificate Transparency services (e.g., https://www.virustotal.com/gui/domain/cyber-ed.space/relations)

2. Among the discovered subdomains, find:

```
lvl3passive.cyber-ed.space
```

3. Check the domain's TXT record:

```bash
dig TXT lvl3passive.cyber-ed.space
```

The flag is found in the response.

### Why It Works

Certificate Transparency services publicly log all issued TLS certificates. If a certificate was issued for a subdomain, its name becomes publicly accessible.
