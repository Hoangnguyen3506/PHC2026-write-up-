## Write-Up

### Objective

Scan the allocated subnet, discover active hosts, identify running services, and obtain all parts of the flag.

### Solution

1. Connect to the service:

```bash
ssh root@<ip-address> -p 2222
```

Password: `68b329da9893e34099`

2. Scan the subnet for available hosts:

```bash
nmap -sn 192.168.0.0/21
```

3. Find the flag on 192.168.0.10 (part 1):

```bash
curl http://192.168.0.10
cybered{3843008d40a4c9888c1fadc3267ffe32}


```

4. Scan 192.168.0.113 over UDP:

```bash
nmap -sU 192.168.0.113 -p- --open -v -T5 --max-retries 1 --min-rate 1000 --max-rate 5000
```

Find port 670 and read the flag:

```bash
nc -u 192.168.0.113 670
flag
```

5. Scan 192.168.0.230:

```bash
nmap -Pn -n -p- --open -v 192.168.0.230
```

Find port 6379 (Redis):

```bash
nc 192.168.0.230 6379
get flag
```

6. Scan 192.168.1.20:

```bash
nmap -Pn -n -p- --open -v 192.168.1.20
```

Find port 10418 and read the 4th part of the flag:

```bash
curl http://192.168.1.20:10418
```

### Note

The flag is collected in parts from different hosts.
