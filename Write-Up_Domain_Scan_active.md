## Write-Up

### Objective

Perform active subdomain enumeration for the domain `company.local` and find a web application containing the flag.

### Solution

1. Connect to the scanner server via SSH:

```bash
ssh scanner@<ip-address> -p 2222
```

Password: `edgvikdvre`

2. Check DNS server availability:

```bash
nslookup company.local 172.20.0.53
```

3. Use a subdomain brute-forcing tool:

```bash
ffuf -w ./SecLists/Discovery/DNS/subdomains-top1million-20000.txt -u http://company.local -H "Host: FUZZ.company.local" -fs 403
```

Or `gobuster`:

```bash
gobuster dns -d company.local -w ./SecLists/Discovery/DNS/subdomains-top1million-20000.txt -t 50 -r 172.20.0.53:53
```

4. Among the discovered subdomains, find `ldaptest.company.local`

5. Check the subdomain's availability:

```bash
curl http://ldaptest.company.local
```

The flag is returned in the response.

### Why It Works

The subdomain exists in DNS but is not published in public sources. Brute-forcing common names causes the DNS server to return existing records, allowing hidden services to be discovered.
