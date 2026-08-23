## Write-Up

At first we need to find admin panel. We can scan it or simply check most common path /admin for admin panel. There we find functionality to enter process data and receive it's info. Possibly our input is simply concatenated with some linux bash command. We can try to finish previous command with semicolon and start new one. 

Solution: `; export`
