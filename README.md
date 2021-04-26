# Herramientas-CTF
Herramientas útiles para afrontar CTF.

## Reconocimiento:
### NETDISCOVER
Ejemplo:
```
netdiscover 192.168.1.110
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

## Otras Herramientas
### Analizador de Hashes
https://www.tunnelsup.com/hash-analyzer/

### Hash Cracker
https://crackstation.net/
