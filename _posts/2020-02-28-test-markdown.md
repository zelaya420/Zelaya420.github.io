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

And here is the same code yet again but with line numbers:

{% highlight javascript linenos %}
var foo = function(x) {
  return(x + 5);
}
foo(3)
{% endhighlight %}

## Boxes
You can add notification, warning and error boxes like this:

### Notification

{: .box-note}
**Note:** This is a notification box.

### Warning

{: .box-warning}
**Warning:** This is a warning box.

### Error

{: .box-error}
**Error:** This is an error box.
