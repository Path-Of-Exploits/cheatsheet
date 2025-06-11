## SMB

```bash
$ nmap $TARGET -sV -sC -p139,445
$ smbclient -N -L //$TARGET
$ smbmap -H $TARGET
$ smbmap -H $TARGET -r share_name
$ enum4linux-ng.py  -A $TARGET
# If user has write permissions
$ smbmap -H $TARGET --download "share_name\test.txt"
$ smbmap -H $TARGET --upload test.txt "share_name\test.txt"
```

Using nxc 
```bash
$ nxc smb $TARGET -u '' -p '' --shares
$ nxc smb $TARGET -u '' -p '' -M spider_plus
$ nxc smb $TARGET -u '' -p '' -M spider_plus -o DOWNLOAD_FLAG=True
$ nxc smb $TARGET -u '' -p '' -M spider_plus -o DOWNLOAD_FLAG=True LIMIT_SIZE=500000
```