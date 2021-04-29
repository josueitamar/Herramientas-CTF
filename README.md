# Herramientas-CTF
Herramientas útiles para afrontar CTF.

## Reconocimiento:
### NETDISCOVER
Ejemplo:
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

## Otras Herramientas
### Analizador de Hashes
https://www.tunnelsup.com/hash-analyzer/

### Hash Cracker
https://crackstation.net/
