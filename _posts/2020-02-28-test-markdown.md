---
layout: post
title: MonitorsTwo – HTB
subtitle: Clam AV DC DNS Docker Dotnet Firejail JSON JWT Kerberos Latex Injection LFI
comments: true
---
![MoTwo](https://hyperbeast.es/wp-content/uploads/2023/05/monitorstwo-htb.jpeg)

MonitorsTwo es una emocionante máquina de dificultad fácil que se encuentra en la plataforma de Hack The Box (HTB). En esta desafiante aventura, nos enfrentaremos a una serie de pasos para lograr el acceso a la máquina.

El primer paso consiste en aprovechar una vulnerabilidad en Cacti, una herramienta de monitoreo, para abrirnos paso y obtener acceso a un contenedor. Una vez dentro del contenedor, nuestro objetivo será elevar nuestros privilegios mediante la explotación de un binario SUID. Esto nos permitirá obtener un mayor control sobre el sistema.

Una vez que hayamos logrado elevar nuestros privilegios en el contenedor, nos enfrentaremos al desafío de acceder a la máquina principal. Para lograrlo, realizaremos una minuciosa enumeración de la base de datos MySQL y obtendremos un hash. Utilizando técnicas de cracking, descifraremos el hash y obtendremos acceso a la máquina principal.

Sin embargo, la aventura no termina aquí. Nuestro siguiente objetivo será escalar nuestros privilegios en la máquina principal. Para ello, descubriremos una vulnerabilidad en Docker que nos permitirá ejecutar comandos del contenedor en la máquina principal. Aprovechando esta brecha, lograremos obtener permisos de root gracias a la bash con permisos SUID.

MonitorsTwo es un desafío estimulante que combina habilidades de explotación de vulnerabilidades, enumeración de bases de datos y conocimientos de Docker. A medida que progresamos en esta máquina, aprenderemos valiosas lecciones sobre seguridad informática y ampliaremos nuestra experiencia en el campo del hacking ético.

¡Prepárate para sumergirte en MonitorsTwo y desbloquear las etapas de esta emocionante misión!

## Enumeración
## Escaneo de puertos
Realizamos un análisis exhaustivo con el objetivo de identificar todos los puertos abiertos en la máquina. Durante este proceso, escaneamos y registramos cada puerto activo, lo que nos permite tener un panorama completo de los servicios y protocolos disponibles en el sistema.

En cuanto a una aplicación específica, un ejemplo podría ser Nmap. Nmap es una herramienta popular y ampliamente utilizada para el escaneo de puertos. Proporciona una amplia gama de opciones y técnicas de escaneo, lo que facilita la detección de puertos abiertos y servicios en una máquina remota.

Es importante destacar que el escaneo de puertos se utiliza con fines legítimos, como auditorías de seguridad o pruebas de penetración autorizadas. Se recomienda siempre obtener el consentimiento del propietario del sistema antes de realizar cualquier escaneo o evaluación de seguridad.

```javascript
❯ sudo nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.129.188.45 -oG allPorts
[sudo] contraseña para mrx: 
Starting Nmap 7.80 ( https://nmap.org ) at 2023-05-02 15:02 CEST
Initiating SYN Stealth Scan at 15:02
Scanning 10.129.188.45 [65535 ports]
Discovered open port 22/tcp on 10.129.188.45
Discovered open port 80/tcp on 10.129.188.45
Completed SYN Stealth Scan at 15:02, 12.44s elapsed (65535 total ports)
Nmap scan report for 10.129.188.45
Host is up, received user-set (0.047s latency).
Scanned at 2023-05-02 15:02:25 CEST for 12s
Not shown: 65533 closed ports
Reason: 65533 resets
PORT   STATE SERVICE REASON
22/tcp open  ssh     syn-ack ttl 63
80/tcp open  http    syn-ack ttl 63

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 12.57 seconds
           Raw packets sent: 66860 (2.942MB) | Rcvd: 65624 (2.625MB)
```
Al visitar la página web, nos encontramos con un panel de inicio de sesión de Cacti. Luego, realizamos un escaneo para descubrir los puertos abiertos en la máquina. Durante este proceso, identificamos dos puertos abiertos: el puerto 22 para SSH y el puerto 80 para la web.

Para llevar a cabo el escaneo, utilizamos los siguientes parámetros:

    -p-: Escaneo de todos los puertos disponibles (65535).
    --open: Muestra únicamente los puertos abiertos.
    -sS: Realiza un escaneo TCP SYN para obtener rápidamente información sobre los puertos abiertos.
    --min-rate 5000: Establecemos una tasa mínima de 5000 paquetes por segundo para acelerar el escaneo.
    -vvv: Modo verbose que muestra la información en cuanto se descubre.
    -n: Deshabilita la resolución de DNS para evitar retrasos innecesarios.
    -Pn: Desactiva el descubrimiento de host mediante ping.
    -oG: Guarda los resultados del escaneo en un archivo de una sola línea, lo cual facilita el uso de comandos como grep, sed, awk, entre otros. Este tipo de archivo es especialmente útil para la herramienta extractPorts, que nos permite copiar directamente los puertos abiertos al portapapeles.

En el segundo escaneo, nos enfocamos en descubrir los servicios y las versiones asociadas a los puertos abiertos. Este análisis nos permitirá obtener información más detallada sobre los servicios disponibles en la máquina.

Con estos datos obtenidos, estamos un paso más cerca de comprender la configuración y las posibles vulnerabilidades de la máquina.

```javascript
❯ nmap -p22,80 -sCV 10.129.188.45 -oN targeted
Starting Nmap 7.80 ( https://nmap.org ) at 2023-05-02 15:05 CEST
Nmap scan report for 10.129.188.45
Host is up (0.051s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Login to Cacti
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 10.23 seconds
```
## Intrusión (Docker)
Si utilizamos la herramienta searchsploit, podemos descubrir que la versión del software que se encuentra en la página web presenta una vulnerabilidad conocida como RCE (Remote Code Execution). Esta vulnerabilidad permite a un atacante ejecutar código de forma remota en el sistema afectado, lo que podría comprometer su seguridad y permitir el acceso no autorizado.

En resumen, la función de searchsploit es buscar y listar exploits conocidos y vulnerabilidades en diferentes software y sistemas. En este caso particular, nos ha permitido identificar que la versión del software en la página web es vulnerable a un RCE. Esta información es valiosa para los profesionales de seguridad informática, ya que les permite estar al tanto de las vulnerabilidades existentes y tomar las medidas necesarias para proteger los sistemas afectados.

```javascript
❯ searchsploit cacti 1.2.22
---------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                        |  Path
---------------------------------------------------------------------------------------------------------------------- ---------------------------------     
Cacti v1.2.22 - Remote Command Execution (RCE)                                                                        | php/webapps/51166.py
---------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```



