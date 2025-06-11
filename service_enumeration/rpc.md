## RPC

```bash
$ rpcclient -U "" $TARGET
$ for i in $(seq 500 1100);do rpcclient -N -U "" $TARGET -c "queryuser 0x$(printf '%x\n' $i)" | grep "User Name\|user_rid\|group_rid" && echo "";done
$ samrdump.py $TARGET
$ rpcdump.py $TARGET
$ enum4linux-ng.py $TARGET -A
```


| Query           | Description                                                        |
| --------------- | ------------------------------------------------------------------ |
| srvinfo         | Server information.                                                |
| enumdomains     | Enumerate all domains that are deployed in the network.            |
| querydominfo    | Provides domain, server, and user information of deployed domains. |
| netshareenumall | Enumerates all available shares.                                   |
| netsharegetinfo | Provides information about a specific share.                       |
| enumdomusers    | Enumerates all domain users.                                       |
| queryuser       | Provides information about a specific user.                        |