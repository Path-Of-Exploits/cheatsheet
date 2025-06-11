## IMAP & POP3

### IMAP
```bash
nmap -p 143,993 --script imap-capabilities <target>
hydra -L usernames.txt -P passwords.txt -s 143 -f <target> imap

telnet <server_ip> 143
openssl s_client -connect <server_ip>:993

a login username password
a list "" "*"
a select inbox
a fetch 1:* (FLAGS BODY[HEADER])
a fetch <message_id> body[text]
a logout
```

### POP3
```bash
nmap -p 110,995 --script pop3-capabilities <target>
hydra -L usernames.txt -P passwords.txt -s 110 -f <target> pop3

telnet <server_ip> 110
openssl s_client -connect <server_ip>:995

USER username
PASS password
LIST
RETR <message_id>
DELE <message_id>
QUIT
```
