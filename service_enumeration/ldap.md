## LDAP

```bash
nmap -p 389 --script ldap-search <target-ip>
nmap --script ldap-* -p 389 <target-ip>
nmap --script "ldap* and not brute" -p 389 <target-ip>
nmap --script ldap-brute --script-args ldap.base='"cn=users,dc=example,dc=com"' -p 389 <target-ip>

ldapsearch -x -h <ldap-server> -b "<base-dn>" "(objectclass=*)"
ldapsearch -x -H ldap://<ldap-server> -D "<bind-dn>" -w "<password>" -b "<base-dn>" "<filter>" <attributes>
```
