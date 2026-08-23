## Write-Up

At first we need to find admin panel. We can scan it or simply check most common path /admin for admin panel. If username is wrong there is error "Wrong username". 

Therefore we can bruteforce username (for example, using burp intruder or any other tool). 

- Correct username is `admin`. 
- After that we can brute force password which is `q1w2e3r4`. 

Then there is OTP page. It can be also brute forced or simply found in html sources of the page. Flag will be displayed in html source of the admin panel.