# SKYTOWER
Desarrollo del CTF SKYTOWER

Download de la VM: https://www.vulnhub.com/entry/skytower-1,96/

## 2. Escano de Puertos

```
nmap -n -P0 -p- -sC -sV -O -T5 -oA full 10.10.10.134
Nmap scan report for 10.10.10.134
Host is up (0.00048s latency).
Not shown: 65532 closed ports
PORT     STATE    SERVICE    VERSION
22/tcp   filtered ssh
80/tcp   open     http       Apache httpd 2.2.22 ((Debian))
|_http-server-header: Apache/2.2.22 (Debian)
|_http-title: Site doesn't have a title (text/html).
3128/tcp open     http-proxy Squid http proxy 3.1.20
| http-open-proxy: Potentially OPEN proxy.
|_Methods supported: GET HEAD
|_http-server-header: squid/3.1.20
|_http-title: ERROR: The requested URL could not be retrieved
MAC Address: 00:0C:29:0D:B7:67 (VMware)
Device type: general purpose
Running: Linux 3.X
OS CPE: cpe:/o:linux:linux_kernel:3
OS details: Linux 3.2 - 3.10, Linux 3.2 - 3.16
Network Distance: 1 hop
```

- Llama la atención el puerto TCP/3128 que es un PROXY. Podemos utilizar el proxy para escanear puertos.

```
Primero debemos editar el archivo /etc/proxychains.conf y añadir el PROXY.
root@kali:~# vim /etc/proxychains.conf 
http 10.10.10.134 3128

root@kali:~# proxychains nmap -n -F -sT 10.10.10.134
ProxyChains-3.1 (http://proxychains.sf.net)
Starting Nmap 7.91 ( https://nmap.org ) at 2021-03-10 17:34 EST
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:8888-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:8080-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:111-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:22-<><>-OK
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:5900-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:21-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:443-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:139-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:110-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:3389-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:25-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:53-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:995-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:80-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:143-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:445-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:113-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:135-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:23-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:554-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:3306-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:993-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:1723-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:1720-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:1025-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:199-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:587-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:49152-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:543-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:37-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:8009-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:9999-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:5101-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:2049-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:1900-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:106-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:5009-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:2000-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:514-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:548-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:873-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:8081-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:4899-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:1029-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:88-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:515-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:1026-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:544-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:990-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:646-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:5800-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:5060-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:8000-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:6646-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:427-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:49157-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:6001-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:5190-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:81-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:179-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:5357-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:2717-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:26-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:9-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:1028-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:7070-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:7-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:8443-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:49155-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:32768-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:1110-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:5432-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:631-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:49154-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:5666-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:119-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:465-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:9100-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:5631-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:389-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:2121-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:1755-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:49156-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:3986-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:144-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:2001-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:1027-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:3000-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:1433-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:444-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:5051-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:5000-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:13-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:513-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:49153-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:6000-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:8008-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:79-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:3128-<--denied
|S-chain|-<>-10.10.10.134:3128-<><>-10.10.10.134:10000-<--denied
Nmap scan report for 10.10.10.134
Host is up (0.0022s latency).
Not shown: 99 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
MAC Address: 00:0C:29:0D:B7:67 (VMware)
```
<img src="https://github.com/El-Palomo/SKYTOWER/blob/main/skytower1.jpg" width=80% />


- A través del PROXY podemos acceder al puerto TCP/22 del servicio SSH.

## 3. Enumeración

> La enumetación de GOBUSTER no arroja nada importante ni escanenandolo de manera directa ni a través del PROXY. 

<img src="https://github.com/El-Palomo/SKYTOWER/blob/main/skytower2.jpg" width=80% />

```
root@kali:~/SKYTOWER/autorecon/10.10.10.134/scans# gobuster dir -u http://10.10.10.134 -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt -p http://10.10.10.134:3128
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.10.134
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-1.0.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] Proxy:          http://10.10.10.134:3128
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2021/03/10 17:43:16 Starting gobuster
===============================================================
/index (Status: 200)
/background (Status: 200)
===============================================================
2021/03/10 17:43:57 Finished
===============================================================
root@kali:~/SKYTOWER/autorecon/10.10.10.134/scans# gobuster dir -u http://10.10.10.134 -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.10.134
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-1.0.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2021/03/10 17:44:10 Starting gobuster
===============================================================
/background (Status: 200)
/index (Status: 200)
```

## 4. Identificación de Vulnerabilidades

### 4.1. Inyección SQL

- Al parecer no hay nada importante en el proceso de enumeración sobre la página web.
- El puerto TCP/22 del servicio SSH esta abierto a través del PROXY. Una opción sería ejecutar un ataque de diccionario.
- Veamos que tiene el portal web. Lo primero que intentamos es una inyección SQL, al parecer si hay SQLi.

<img src="https://github.com/El-Palomo/SKYTOWER/blob/main/skytower3.jpg" width=80% />

- Llama la atención el mensaje de error que dice que hay un error cerca de 11. Nunca escribimos 11.

<img src="https://github.com/El-Palomo/SKYTOWER/blob/main/skytower4.jpg" width=80% />

- Lo que ocurre es que el sistema está filtrado palabras. Eliminar la palabra "OR", el igual ("=") y los guiones de comentarios ("-- "). 

<img src="https://github.com/El-Palomo/SKYTOWER/blob/main/skytower5.jpg" width=80% />

<img src="https://github.com/El-Palomo/SKYTOWER/blob/main/skytower6.jpg" width=80% />

- Debemos buscar algun reemplazo para las palabras filtradas. Es un tema de ensayo/error. 
- Reemplazamos así: OR por || , = por LIKE y "-- " por #. 

<img src="https://github.com/El-Palomo/SKYTOWER/blob/main/skytower7.jpg" width=80% />

<img src="https://github.com/El-Palomo/SKYTOWER/blob/main/skytower8.jpg" width=80% />

### 4.2. Acceso a través de SSH

- El mensaje obtenido indica lo siguiente: Todos los usuarios deben iniciar sesión a través de SSH y nos dan una cuenta. Probemos el acceso SSH a través del PROXY.

<img src="https://github.com/El-Palomo/SKYTOWER/blob/main/skytower9.jpg" width=80% />

Importante: Debemos añadir al final "/bin/sh" sino la conexión se cierra de inmediato.


## 5. Elevar Privilegios





