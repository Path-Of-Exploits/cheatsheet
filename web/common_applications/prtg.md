## PRTG

- Check CVEs
- Check default credentials : prtgadmin:prtgadmin,Password123

```bash
$ curl -s http://$TARGET:8080/index.htm -A "Mozilla/5.0 (compatible;  MSIE 7.01; Windows NT 5.0)" | grep version
```

[PRTG < 18.2.39 Command Injection Vulnerability](https://codewatch.org/2018/06/25/prtg-18-2-39-command-injection-vulnerability/)

Abusing Notification settings : 
https://academy.hackthebox.com/module/113/section/1094