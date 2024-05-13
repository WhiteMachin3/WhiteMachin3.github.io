---
layout: post
title: El gato debió pisar más teclas
subtitle: Cuando tu aporreo de teclado condena el servidor 
cover-img: /assets/img/background1.jpg
thumbnail-img: /assets/img/gato.jpg
share-img: /assets/img/gato.jpg
tags: [writeup ,web]
author: MachinE
---

Este articulo no tiene gran densidad técnica, ni pretende enseñar metodología, está escrito por tres motivos:

- Cuando creas que has fuzzeado suficiente, sigue fuzzeando
- Cuando creas que hay fallos que solo pasan en maquinas de práctica, te equivocas
- Echarnos las risas, que siempre viene bien

Poniendo un poco de contexto, en una reciente auditoria había dos dominios en el scope: **example.com** y **shop.example.com**, ambos con CMS muy comúnes (Wordpress y Prestashop respectivamente). Como todos sabemos, suele ser una perdida de tiempo intentar encontrar fallos en los plugins de Wordpress en caja negra, ya que el propio mercado de plugins está saturado de analistas. La situación es parecida en Prestashop, aunque da un poco más de libertad por no ser tan utilizado y tener mayor número de plugins de pago privados. Que no significa imposible, en el próximo artículo subiré uno de mis descubrimientos 😁

Volviendo al tema, la aplicación tenía algún síntoma de mala gestión con el servidor backend: cabeceras en la respuesta, archivos php.ini/web.config accesibles y **Directory Listing**. Con el Directory Listing (activado en varios de los directorios de la web) explorar sobre las carpetas de los plugins, de descargas y similar se hacía mucho más sencillo. En este punto, había bastante información sensible que podía ser reportada como tal. Pero no ibamos a quedarnos en una low hanging fruit, ¿no?

Teniendo el Directory Listing activado, fuzzear directorios es crucial para poder avanzar. Por lo que, wordlist en mano, enciendo Wfuzz y me piro a por un café.

[SecLists](https://github.com/danielmiessler/SecLists)

~~~
$ wfuzz --hc 404,301 -w wordlist.txt https://example.com/FUZZ/
~~~

Cuando vuelvo del café, me encuentro con un resultado que llama mi atención más que el resto (los valores numéricos me los he inventado fuertemente):

~~~
==================================================================
ID      Response   Lines      Word         Chars          Request
==================================================================

00013:  C=200    14 L       29 W         263 Ch        "asdf"
~~~

Y, para mi sorpresa, dentro tenemos lo siguiente:

![Directory](/assets/img/directoryasdf.png){: .mx-auto.d-block :}

Pesa 802M, y contiene un backup completo de la web en **shop.example.com**. Las cosas han escalado bastante rápido, pero no acaba aqui. Tenemos el secret de la aplicación, la clave del TPV virtual, credenciales de la base de datos y todo lo demás que uno quiera rebuscar sobre el backup de la aplicación. Pero hay un problema: la base de datos no está expuesta al exterior, por lo que se acabó la fiesta 😣

![Gif](https://media.tenor.com/A5cNMzE25REAAAAM/walter-white-breaking-bad.gif){: .mx-auto.d-block :}

No estoy familiarizado con la idea de abandonar a las puertas, asi que después de decenas de pruebas, se me ilumina la bombillita y pienso: ¿Habrán reutilizado contraseñas entre proyectos? Utilizando **wpscan** para enumerar usuarios de Wordpress, obtengo 4 usuarios válidos de **example.com**, y teniendo acceso a **/wp-login.php** comienzo a probar las contraseñas que he recopilado del backup. Y bingo: una de las contraseñas la han reutilizado en el acceso de un administrador.

![Gif2](https://i.gifer.com/origin/85/857ba75399923157e2c729f81c94c76c_w200.gif){: .mx-auto.d-block :}

Ahora los pasos son muy straightforward: subir un plugin malicioso para Wordpress comprimido en un zip y ejectutar una webshell. Efectivamente funcionó, no voy a poner más evidencias de esta parte porque no hay necesidad, pero todos los procesos se estaban ejecutando con el mismo usuario con el que habia aterrizado con la webshell. No lo rooteamos al 100%, pero servidor pwned 😁
