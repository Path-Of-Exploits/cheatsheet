## Windows
```powershell
Set-WinUserLanguageList -LanguageList fr-FR
```


## Linux
```bash
setxkbmap fr # Nécessite le packet x11-xkb-utils
```
### Fichiers de conf (à checker)
```bash
sudo vim /etc/default/keyboard
XKBMODEL="pc105"
XKBLAYOUT="fr"
XKBVARIANT="azerty"
sudo setupcon
```
