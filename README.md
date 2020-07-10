URL Source

https://community.ui.com/questions/FreeRadius-MySQL-MAC-Authentication-Bypass-/fcf8ebf2-de31-4fba-b331-d968f1f14088

http://wiki.freeradius.org/Mac-Auth

https://community.arubanetworks.com/t5/Security/MAC-auth-via-RADIUS-need-to-set-username-in-return-attributes/td-p/24630

https://carloalbertoscola.it/2019/network/security/linux/freeradius-3-setup-mysql-eap-ttls/

Figured out a way using SQL xlat. The FreeRadius wiki has an extra " that was causing the sql xlat to fail. Here is the solution:

Go to the FreeRadius wiki. You can skip the sections labeled 'raddb/modules/file' and 'raddb/authorized_macs'. When you get to the final section 'raddb/sites-available/default post-auth{}' use the following code instead:
```
if("%{sql:SELECT COUNT(macaddr) FROM radmacauth WHERE macaddr ='%{User-Name}'}" > 0){
  ok
} 
else{
  reject
}
```
You will need to create a table named 'radmacauth' with a field named 'macaddr' in your radius database.
