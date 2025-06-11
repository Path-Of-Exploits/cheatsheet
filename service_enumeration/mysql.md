## MySQL

```bash
$ nmap $TARGET -sV -sC -p3306 --script mysql*
$ mysql -u $USER -h $TARGET
$ mysql -u $USER -p$PASSWORD -h $TARGET
$ mysql -u $USER -p $PASSWORD -h $MYSQLSERVERNAME -e 'SELECT * FROM foo...' $DBNAME
```


| Command                     | Description                                                                                      |
| --------------------------- | ------------------------------------------------------------------------------------------------ |
| mysql -u  -p -h             | Connect to the MySQL server. There should not be a space between the '-p' flag and the password. |
| SHOW databases;             | Show all databases.                                                                              |
| use ;                       | Select one of the existing databases.                                                            |
| SHOW tables;                | Show all available tables in the selected database.                                              |
| SHOW columns FROM ;         | Show all columns in the selected table.                                                          |
| SELECT * FROM ;             | Show everything in the desired table.                                                            |
| select * FROM WHERE  = ""; | Search for needed string in the desired table.                                                    |
