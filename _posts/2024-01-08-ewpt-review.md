---
layout: post
title: eWPT Review
subtitle: Impresiones y consejos 
cover-img: /assets/img/background1.jpg
thumbnail-img: /assets/img/eWPT2.png
share-img: /assets/img/ewpt.png
tags: [reviews, web]
author: MachinE
---
## Introducción

El pasado 30 de Diciembre me presenté al examen eWPTv2, una certificación de Pentesting Web que oferta INE, lo que antes era eLearnSecurity.

Este post tiene como intención resolver algunas dudas sobre el examen y su contenido, así como algun consejo para poder superarlo de forma satisfactoria (sin spoilers)

Si tu intención es sacar la certificación y quieres tener una referencia adicional, sigue leyendo.

## Sobre mi

Tengo un año de experiencia trabajando en Ciberseguridad, el apartado web es lo que más me ha llamado la atención dentro del pentesting y he realizado auditorias reales en aplicaciones web mas allá de mi experiencia con CTFs y similares. 

En Marzo aprobé el eJPTv2.

Tengo algo de rodaje? No lo suficiente, pero el trabajo diario es algo que se nota mucho durante el examen. 

## Como prepararse

En esta parte poco puedo recomendar con certeza. El curso que tiene INE para el eWPT por lo que he leido no es de la mejor calidad (al menos en su versión anterior, desconozco si la actual cumple mejor con las expectativas), pero puedo asegurar que para el examen ayuda, y mucho. 

Hay un montón de secciones durante el examen que son técnicas y formas concretas de hacer las cosas que se dan durante su curso, y no haberlo hecho conlleva dejarte un tiempo en adaptar tu conocimiento a la prueba. 

Entonces la pregunta es: ¿Es necesario? No, pero ayuda mucho.

Recomendación personal para prepararse de forma gratuita sin duda PortSwigger (las secciones de Client-Side) y HackTheBox/Vulnhub, cubren un montón, por no decir todo el contenido de la certificación.

## Sobre el examen

Recientemente INE ha actualizado la certificación a su versión 2. 

Eso conlleva un cambio en el contenido y en el formato, asi que la mayoría de reviews que puedes leer ya no aplican al 100%. 

La versión anterior estaba basada en un entorno al que te conectabas por VPN con tu equipo, y tenias 7 dias de entorno, 7 dias para realizar un informe lo más profesional posible, y otros 7 dias de retake en caso de fallar. 

¿Que ha cambiado? Pues todo. El examen ahora son 10 horas, el equipo te lo proporcionan mediante navegador (Apache Guacamole) y tienes 40 preguntas que responder. Mismo formato que la eJPTv2. 

Opino que son más accesibles y que ahorran muchos problemas que pueden surgir de que cada persona use su equipo, las wordlists y las herramientas que necesitas están indicadas en las guias del examen, asi que si algo no está ahi, te estás yendo más lejos de la cuenta.

## Mi experiencia durante el examen

Estudiar y trabajar no es fácil de compaginar con lo que llaman tiempo libre, asi que me dispuse a comenzar el examen el sábado 30 de Diciembre, una hora después de levantarme. 
Desayunad fuerte porque la hora de la comida no la vais a disfrutar mucho. 

Aunque habia hecho un par de auditorias recientemente, no me habia preparado nada para el examen: ninguna maquina, ni laboratorio, ni siquiera una cheatsheet de la que tirar. Tampoco tenía nada instalado donde tomar notas organizadas asi que tiré de VSCode y un documento de texto en blanco. 

No miento si digo que lo comencé con una sonrisa bastante confiado, y que tras un par de horas me fui poniendo cada vez más tenso. Habia una gran cantidad de apartados y teoría propia de la cert que no controlaba suficiente.

Finalmente tras 7h20m de las 10 horas que habia de limite lo entregué y al minuto obtuve la nota, 88%. 
Que pensarás, ¿Que gran nota no? Estuve a muy poco de suspender, el formato del tipo test obliga a tener un 70% en el examen y a tener un 70% EN CADA APARTADO, asi que si en una sección de 6 preguntas fallas 2 te vas para casa.

## Consejos para el examen

Esta sección creo que sea la más importante de la review. No quiero hacer más spoilers de la cuenta, asi que vamos allá.

- Lee todo el test antes de empezar:

Y durante el examen, y al final. Las preguntas del test dan MUCHA información, y cuando digo mucha es mucha. En muchas ocasiones donde te atasques, vuelve a leer las preguntas.
Es un tipo test, la respuesta está entre las cuatro opciones, eso acota mucho lo que estás buscando.

- Respeta tus tiempos y tu cabeza:

Se realista sobre cuando necesitas descansos, sobre cuando tienes que ir a comer algo. Escucha a tu cabeza aunque sea dificil en ese momento.
Levanta el culo de la silla y date una vuelta, todas las veces que lo hice durante el examen fui capaz de salir de un bloqueo mental y terminar X sección.

- Organizate y no tengas prisa:

Antes de comenzar, prepara todas las herramientas que tengas para tomar notas, yo recomiendo utilizar Obsidian por el canvas.
También te vale hacerte el esquema en un cuaderno de matemáticas que tengas desde la ESO, pero vete cuadrandote el entorno.

- No te vayas por las ramas:

El examen va muy a tiro fijo en las vulnerabilidades, y si bien hay que encadenar diversas vulnerabilidades en alguna sección, por lo general son cosas muy concretas las que tienes que buscar.
Tienes un Linux con herramientas limitadas, con las wordlists justas y no tienes internet, el exploit que buscas no ha salido hace 4 dias.

- No hay atajos:

Algo que experimenté en el eJPTv2 es que si pawneabas la máquina, tenías el resto de respuestas relacionadas con ese equipo resueltas. Bastaba con indagar un poquito como root para ver usuarios, credenciales, versiones y lo que quieras. En este no, cada servicio por lo general va totalmente aislado aun estando en la misma ip (van dockerizados).

- No hace falta hacerlo para contestarlo:

Esto es algo que me hace bastante gracia pero es real, durante el examen habrá situaciones donde estés seguro de que la vulnerabilidad es una en concreto, que tengas el PoC en mano y aun así no seas capaz de explotarla. Vuelve a leer las preguntas, lo más seguro es que no te pida una flag si no indicar como lo explotarias. Esto es enserio.

- No saltes de arbol en arbol:

Intenta centrarte en algo y sigue hasta que lo termines o te atasques por completo, es un entorno bastante grande y tener todos los apartados en la cabeza simultaneamente es un problema

Espero haber aclarado alguna duda, Happy Hacking ;)
