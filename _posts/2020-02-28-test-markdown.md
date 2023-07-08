---
layout: post
title: MonitorsTwo – HTB
subtitle: Clam AV DC DNS Docker Dotnet Firejail JSON JWT Kerberos Latex Injection LFI
comments: true
---
![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg)

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



Here's a code chunk:

~~~
var foo = function(x) {
  return(x + 5);
}
foo(3)
~~~

And here is the same code with syntax highlighting:

```javascript
var foo = function(x) {
  return(x + 5);
}
foo(3)
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
