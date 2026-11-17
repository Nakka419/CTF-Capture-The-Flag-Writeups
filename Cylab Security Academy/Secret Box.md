This secret box is designed to conceal your secrets. It's perfectly secure—only you can see what's inside. Or can you? Try uncovering the admin's secret.

![Secret Vault](secret%20vault%20hp.png)


**Walkthrough**
- Sign Up: user: test; password:test
![Secret Vault](sign%20up.png)


-  Login with the credentials you just created and on the home page select create new secrets. in the text box enter a single quote. notice in burpsuite the single quote returned a 500 internal server error. which is an indication of a sql injection.
 ![Secret Vault](new%20secret.png) ![Secret Vault](single%20quote.png)




Fingerprint the DBMS by doing string concatenation using the portswigger SQL cheatsheet: https://portswigger.net/web-security/sql-injection/cheat-sheet
- **te'||'st** concantenation worked so its either a Oracle or PostgreSQL.
- I tested the PostgreSQL to extract the database version and it worked so now we know we are dealing with a PostgreSQL database. Payload: '||(SELECT version())||'
  
