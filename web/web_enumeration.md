## Information Gathering - Web Edition

### Whois
```bash
whois facebook.com
```

### Fingerprint
```bash
wafw00f inlanefreight.com
```

### Crawling
```bash
/sitemap.xml
/.git
/robots.txt
/.well-known
python3 ReconSpider.py http://inlanefreight.com
python3 finalrecon.py --headers --whois --url http://inlanefreight.com
```

### DNS
```bash
dig domain.com
dig domain.com A
dig domain.com AAAA
dig domain.com MX
dig domain.com NS
dig domain.com TXT
dig domain.com CNAME
dig domain.com SOA
dig @1.1.1.1 domain.com
dig +trace domain.com
dig -x 192.168.1.1
dig +short domain.com
dig +noall +answer domain.com
dig domain.com ANY

# Bruteforcing
dnsenum --enum inlanefreight.com -f /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -r
# Zone Transfert
dig axfr @nsztm1.digi.ninja zonetransfer.me
# VHOST
gobuster vhost -u http://<target_IP_address> -w <wordlist_file> --append-domain
ffuf -fs <default_size> -c -w `fzf-wordlists` -H 'Host: FUZZ.machine.org' -u "http://$TARGET/"
ffuf -ac -c -w `fzf-wordlists` -H 'Host: FUZZ.machine.org' -u "http://$TARGET/"
# Enum from certificates
curl -s "https://crt.sh/?q=domain.com&output=json" | jq -r '.[] | select(.name_value | contains("dev")) | .name_value' | sort -u
curl -I inlanefreight.com
```