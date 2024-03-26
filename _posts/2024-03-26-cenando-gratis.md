---
layout: post
title: Cenando gratis
subtitle: Rompiendo pasarelas de pago 
cover-img: /assets/img/coche1.png
thumbnail-img: /assets/img/pizza.png
share-img: /assets/img/pizza.png
tags: [bugbounty, web]
author: MachinE
---
## Introducción

Hace unos meses, un viernes cualquiera pedimos entre varios amigos la cena a domicilio. 

Aunque ya habia acabado la semana laboral, nunca es mal momento para poner un proxy en tu vida, asi que, después de haber hecho el pedido, tenía claro que encontraría alguna forma de pedir más barato. Así que, con portatil en mano y unas cuantas latas, nos ponemos con ello.

## Manos a la obra

La aplicación era bastante básica, programada con PHP y con las funcionalidades justas para poder realizar pedidos directamente al restaurante. Como el objetivo era realizar algún cambio en el pedido, vamos a olvidarnos de encontrar inyecciones SQL, ejecuciones de código o paneles de admin, tunnelvision hacia el proceso de pago.

Con BurpSuite enchufado, vamos a realizar un pedido válido para que nos lleve hasta el pago final. Una vez aqui, podemos ver la petición relevante:

~~~
POST /endpoint.php HTTP/2
HOST: REDACTED.COM
...

function=pedido&pago=tarjeta&telefono=.....
nombre1=Pizza+Cuatro+Quesos&ref1=H1281&precio1=12.50&unidades1=1
~~~

Esta petición simplificada tiene estos cuatro parametros bastante llamativos. La primera prueba es evidente, modificar el valor del precio. Y para sorpresa de todos los presentes, esto no funcionó. Bastante extrañado, la conclusión es clara: sobran parámetros. La aplicación solo está utilizando el código de referencia para añadir productos, ignorando el nombre y el precio.

Antes de darlo por inexpugnable, la bombilla se enciende y recuerdo un fallo que he visto en múltiples procesos de pago que he auditado. Cantidades negativas.Esto ya me habia funcionado antes, y es algo que los desarrolladores suelen olvidar que puede ocurrir (y ser abusado). Asi que vamos con la prueba de concepto.
Realizando un pedido con dos productos, uno más barato que otro, interceptamos la petición. 

~~~
POST /endpoint.php HTTP/2
HOST: REDACTED.COM
...

function=pedido&pago=tarjeta&telefono=.....
nombre1=Pizza+Cuatro+Quesos&ref1=H1281&precio1=12.50&unidades1=1
nombre2=Pizza+Margarita&ref2=H1210&precio2=10&unidades2=-1
~~~

Modificando la cantidad del segundo producto a ser -1 la respuesta de la aplicación va a ser positiva, nos envia a la pasarela de pago con un precio total de 2.5 euros. La vulnerabilidad está confirmada, ahora solo falta averiguar el "impacto".

## Segundas partes

Varias semanas más tarde, me dispose a comprobar el problema por completo, ya que habia dos cosas que comprobar: si el pedido se habia realizado correctamente y si el producto restado se vería reflejado en el TPV y el ticket. Asi que, con mis mejores intenciones, realice el mismo pedido que dias atras pero esta vez completándolo. Lo pasé a recoger en caso de que hubiese algún problema.

Con esto, pude comprobar ambas incognitas:

- El pedido se realiza a la perfección.
- Se refleja en el ticket el producto eliminado.

![Ticket](/assets/img/ticket1.PNG)

Tras hablar con los empleados, arreglamos el pedido para pagar la cantidad correcta. Al dia siguiente, contacté con los desarrolladores para reportar esta vulnerabilidad y explicar su reproducción.

Aunque la cena no saliese más barata, tiene un sabor especial 😁.
