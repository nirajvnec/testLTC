
The path where you want to store sqlnet.ora (e.g., C:\Program Files (x86)\Oracle\clients\19c_64\network\admin).

Modify the sqlnet.ora file to include Kerberos:

SQLNET.AUTHENTICATION_SERVICES = (KERBEROS5)
