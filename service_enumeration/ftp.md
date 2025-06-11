## FTP

```bash
$ nmap -sC -sV -p 21 $TARGET
$ telnet 10.129.14.136 21
$ nc -nv 10.129.14.136 21
$ ftp $TARGET
$ ftp -n $TARGET
$ openssl s_client -connect $TARGET:21 -starttls ftp
# Download remote files
$ wget -r ftp://username:password@ip/directoryname
$ wget -m --no-passive ftp://anonymous:anonymous@$TARGET
# FTP bounce back attack
$ nmap -Pn -v -n -p80 -b anonymous:password@$TARGET $INTERNAL_ADDR
```

_If there is any connexion issue, switch to passive mod_

ls -R

| Command     | Description                                                      |
| ----------- | ---------------------------------------------------------------- |
| ls          | Lists the files and folders in the remote directory.             |
| ls -R       | Lists the files and folders in the remote directory (recursive). |
| pwd         | Displays the current directory on the remote server.             |
| cd          | Changes the current directory on the remote server.              |
| lcd         | Changes the local directory where files will be downloaded.      |
| get         | Downloads a single file from the server to the local machine.    |
| mget        | Downloads multiple files at once.                                |
| put         | Uploads a file from the local machine to the server.             |
| mput        | Uploads multiple files at once.                                  |
| bye or quit | Ends the FTP session.                                            |
| passive     | Switch to passive mod                                            |

