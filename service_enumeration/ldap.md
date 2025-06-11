## LDAP

```bash
$ nmap -p 389 --script ldap-search $TARGET
$ nmap --script ldap-* -p 389 $TARGET
$ nmap --script "ldap* and not brute" -p 389 $TARGET
$ nmap --script ldap-brute --script-args ldap.base='"cn=users,dc=example,dc=com"' -p 389 $TARGET

$ ldapsearch -x -h $TARGET -b "<base-dn>" "(objectclass=*)"
$ ldapsearch -x -H ldap://$TARGET -D "<bind-dn>" -w "<password>" -b "<base-dn>" "<filter>" <attributes>
```
