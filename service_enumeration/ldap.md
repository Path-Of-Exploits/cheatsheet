## LDAP

```bash
$ nmap -p 389 --script ldap-search $TARGET
$ nmap --script ldap-* -p 389 $TARGET
$ nmap --script "ldap* and not brute" -p 389 $TARGET
$ nmap --script ldap-brute --script-args ldap.base='"cn=users,dc=example,dc=com"' -p 389 $TARGET

$ ldapsearch -x -h $TARGET -b "<base-dn>" "(objectclass=*)"
$ ldapsearch -x -H ldap://$TARGET -D "<bind-dn>" -w "<password>" -b "<base-dn>" "<filter>" <attributes>
```

| Input  | Description                                                                                                                                                                                                                                  |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *      | An asterisk * can match any number of characters.                                                                                                                                                                                            |
| ( )    | Parentheses ( ) can group expressions.                                                                                                                                                                                                       |
| &#124; | A vertical bar &#124; can perform logical OR.                                                                                                                                                                                                |
| &      | An ampersand & can perform logical AND.                                                                                                                                                                                                      |
| (cn=*) | Input values that try to bypass authentication or authorization checks by injecting conditions that always evaluate to true can be used. For example, (cn=*) or (objectClass=*) can be used as input values for username or password fields. |

