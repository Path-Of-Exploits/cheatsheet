## NFS

```bash
$ nmap $TARGET -p111,2049 -sV -sC
$ nmap --script "nfs*" $TARGET -sV -p111,2049
$ showmount -e $TARGET
# Mount NFS, (exegol --privileged)
$ mkdir target-NFS
$ sudo mount -t nfs $TARGET:/ ./target-NFS/ -o nolock
$ sudo umount ./target-NFS
```