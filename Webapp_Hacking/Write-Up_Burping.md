## Write-Up

Phrases on the website provide hints on what needs to be done. The tasks are presented one by one and all requirements must be met:

- Set the POST method
- Set the header `X-Cyber-Ed: hacker`
- Request the resource `/robots.txt`
- Set the GET parameter `cyber-ed=hacker`
- Set the header Content-Type: `application/x-www-form-urlencoded`
- Set the cookie `cyber-ed`
- Set the header `User-Agent` with the substring `YaBrowser`

Command to solve the task: 

`curl -X POST -H 'User-Agent: YaBrowser' -H 'Content-Type: application/x-www-form-urlencoded ' -H 'X-Cyber-Ed: hacker' -b 'cyber-ed=1' 'http://.../robots.txt?cyber-ed=hacker'`