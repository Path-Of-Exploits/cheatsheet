## IIS

- Check ffor IIS versions (8.3 and pray)

```bash
$ nmap -p- -sV -sC --open $TARGET
$ java -jar iis_shortname_scanner.jar 0 5 http://$TARGET/
# Or in python https://github.com/lijiejie/IIS_shortname_Scanner
$ python3 iis_shortname_Scan.py $TARGET
```

Crafting custom wordlists depends of what we found :
```bash
$ grep -E '^first_letter_of_file' /opt/seclists/Discovery/Web-Content/raft-large-files-lowercase.txt > wordlists.txt
```

Fuzzing usint the custom wordlists : 
```bash
$ ffuf -c -w list.txt -u "http://$TARGET/FUZZ"
```