---
layout: post
title: MonitorsTwo – HTB
subtitle: Clam AV DC DNS Docker Dotnet Firejail JSON JWT Kerberos Latex Injection LFI
comments: true
---
MonitorsTwo es una emocionante máquina de dificultad fácil que se encuentra en la plataforma de Hack The Box (HTB). En esta desafiante aventura, nos enfrentaremos a una serie de pasos para lograr el acceso a la máquina.

El primer paso consiste en aprovechar una vulnerabilidad en Cacti, una herramienta de monitoreo, para abrirnos paso y obtener acceso a un contenedor. Una vez dentro del contenedor, nuestro objetivo será elevar nuestros privilegios mediante la explotación de un binario SUID. Esto nos permitirá obtener un mayor control sobre el sistema.

Una vez que hayamos logrado elevar nuestros privilegios en el contenedor, nos enfrentaremos al desafío de acceder a la máquina principal. Para lograrlo, realizaremos una minuciosa enumeración de la base de datos MySQL y obtendremos un hash. Utilizando técnicas de cracking, descifraremos el hash y obtendremos acceso a la máquina principal.

Sin embargo, la aventura no termina aquí. Nuestro siguiente objetivo será escalar nuestros privilegios en la máquina principal. Para ello, descubriremos una vulnerabilidad en Docker que nos permitirá ejecutar comandos del contenedor en la máquina principal. Aprovechando esta brecha, lograremos obtener permisos de root gracias a la bash con permisos SUID.

MonitorsTwo es un desafío estimulante que combina habilidades de explotación de vulnerabilidades, enumeración de bases de datos y conocimientos de Docker. A medida que progresamos en esta máquina, aprenderemos valiosas lecciones sobre seguridad informática y ampliaremos nuestra experiencia en el campo del hacking ético.

¡Prepárate para sumergirte en MonitorsTwo y desbloquear las etapas de esta emocionante misión!

## Enumeración
## Escaneo de puertos

Here's a useless table:

| Number | Next number | Previous number |
| :------ |:--- | :--- |
| Five | Six | Four |
| Ten | Eleven | Nine |
| Seven | Eight | Six |
| Two | Three | One |


How about a yummy crepe?

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg)

It can also be centered!

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg){: .mx-auto.d-block :}

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
