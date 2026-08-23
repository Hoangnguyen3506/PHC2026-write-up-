## Write Up

There is order view page after order is succesfully made. 

We can see `/receipt.php?orderID=2` in the url. 

`orderID` contains IDOR vulnerability. 

We can iterate over it and find flag with `orderID=1`.