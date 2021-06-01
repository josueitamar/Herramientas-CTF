# Herramientas-CTF
Herramientas útiles para afrontar CTF.

## Ocultar IP
### KALITORIFY
https://www.youtube.com/watch?v=bC6pL6mEnuo


Verificar IP pública a la cual estamos conectados:
```
wget -qO- http://ipecho.net/plain | xargs echo
```

## Reconocimiento:
### NETDISCOVER
Ejemplos:
```
netdiscover -i eth0 //Para redes comunes en una interface de red específica.
netdiscover -r 192.168.1.0/24 //Para escanear un rango dentro de una red específica (CIDR).
```
### NMAP
Ejemplos:

Detección de servicios standard
```
nmap -sV 192.168.1.110
```
Detección de servicios con vulnerabilidades
```
nmap -sV -vv --script vuln 192.168.1.110
```
Enumeración de directorios SAMBA
```
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 192.168.1.110
```
### Enumeración de subdominios
Web
```
https://www.nmmapper.com/sys/tools/subdomainfinder/
```
Herramientas incluidas en Kali/ParrotOS
```
sublist3r -d dominio.com
```
### WHOIS
Web
```
https://who.is/whois
```
Herramientas incluidas en Kali/ParrotOS
```
whois dominio.com
```
### GOBUSTER
Detección de directorios y archivos con la ayuda de un diccionario
```
gobuster dir -u http://192.168.1.110/ -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -x php,html,txt,old,bak,bin
```
### PYTHON3
Ejemplos:

Inicar WebServer
```
python3 -m http.server 12345
```

## Otras Herramientas
### Analizador de Hashes
https://www.tunnelsup.com/hash-analyzer/

### Hash Cracker
https://crackstation.net/
