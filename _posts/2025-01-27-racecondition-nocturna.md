---
layout: post
title: Se creativo, te hará diferente
subtitle: Explotando Race Conditions de madrugada
cover-img: /assets/img/background1.jpg
thumbnail-img: /assets/img/bolt.jpg
share-img: /assets/img/bolt.jpg
tags: [writeup ,web]
author: MachinE
---

ESPAÑOL

Esta es la historia de mi último reporte a Miele, la cual además de graciosa, me ha enseñado bastante.

Como alguno sabréis, estoy estudiando este año en Polonia, viviendo en una residencia de estudiantes. Lo que implica que estoy cerca de software nuevo, nada más que decir. Tenemos una lavandería dentro del edificio la cual es gestionable desde una App móvil y desde la Web. Tras simplemente observar su funcionamiento, busco información sobre Miele para conocer si están en algún programa de Bug Bounty o tienen su propio Vulnerability Disclosure Program. Bingo, Miele tiene un [VDP](https://www.miele.com/en/com/cybersecurity-5047.htm)

Que más puedes pedir? Tienes permiso de la empresa y tienes el reto de decir que has hackeado tu lavanderia. Nos ponemos manos a la obra a mapear toda la arquitectura de la aplicación: enumeración de todas las funcionalidades, investigación de todo el comportamiento, búsqueda de la documentación de la API, análisis de la aplicación para administradores e incluso leerme el manual de montaje de la propia lavadora (si, ahora soy experto en el cableado y en las tomas de agua)

Con este primer acercamiento, anterior a cualquier intento de ataque complejo, me encuentro con la documentación de la API y algunos errores no manejados que me dieron más información de rutas, librerias y el stack en su conjunto. Reportable? Supongo, pero no soy esa clase de analista.

Tras algunas horas peleandome contra la aplicación, la cual era más robusta de lo que parecía de primeras, me encuentro con dos XSS stored, uno de ellos blind. Como estar dedicando el dia entero durante examenes al VDP no era la mejor idea, realizo el reporte y se lo envío. Al dia siguiente, el departamento de Product Security de Miele ya me habia dado respuesta y, al segundo dia, ya estaba solucionado. Gratamente sorprendido con su profesionalidad, ya que como muchos sabéis de primera mano no suele ser así en la mayoría de empresas, me añaden al Hall of Fame y me tratan de forma muy positiva.

![hof](/assets/img/hof.PNG){: .mx-auto.d-block :}

Viendo esto, no podía dejar ir la oportunidad de encontrar algo más bonito, más técnico. Por suerte, llevaba un tiempo investigando y jugando con las Race Conditions y todo lo que tuviera que ver con los tiempos de respuesta gracias a James Kettle, el director de investigación de Portswigger. Especialmente en ["Listen to the whispers: web timing attacks that actually work"](https://portswigger.net/research/listen-to-the-whispers-web-timing-attacks-that-actually-work) y en ["Smashing the state machine: the true potential of web race conditions"](https://portswigger.net/research/smashing-the-state-machine). 

En que momento se me ocurre que es vulnerable? Bueno, tuve un sueño en el que el proceso de pago se podia atacar de esta forma, me desperté a las 4am y lo escribí en un papel. Tan sencillo como eso, me ha pasado multitud de veces y nunca me ha fallado el instinto. Mi demonio de la parálisis del sueño debe de alucinar conmigo.

![donald](https://media3.giphy.com/media/mkhMTALnrYRLnuoe5P/giphy_s.gif?cid=6c09b952cfqjkwmm9kuyztgm5vyjhe3y2wbegj49uoebqmx4&ep=v1_gifs_search&rid=giphy_s.gif&ct=g){: .mx-auto.d-block :}

Vale, tenemos la idea del ataque y la corazonada de que es vulnerable. El ataque se basa en activar multitud de lavadoras disponiendo únicamente de saldo para una, atacando la ventana de tiempo entre comprobaciones de saldo. Básicamente: pago por una, activo todas.

Para el Proof of Concept, tenía que hacer algo creativo y diferente, ya que es mi parte favorita del proceso: no me tires un análisis automático y me leas la teoría, explótalo tu mismo y demuestra que es verdad. Estamos interactuando con algún elemento físico, no? Vamos a ir a la lavandería para hacerlo ahi mismo.

Un martes a las 3 de la mañana, me planto con un pijama de cuadros, bata negra y chanclas a grabarme un video con el portatil en mano lanzando el ataque en directo. El resto es historia: el ataque funciona como habia ideado y se confirma la Race Condition, el saldo de mi cuenta en la app está en negativo y yo tengo una sonrisa en la cara. A saber que estarían pensando mis dos amigos grabando y los de seguridad.

![wash1](/assets/img/wash1.jpeg){: .mx-auto.d-block :}

![wash2](/assets/img/wash2.jpeg){: .mx-auto.d-block :}

Cual es el impacto potencial de esto? Activar simultaneamente todas las lavadoras que maneje la API, que son miles (para comenzar el programa de lavado requiere interacción física, pero quedaría pagado el servicio).
Miele tarda algunas semanas en parchear este problema, con un trato ejemplar por lo cual estoy muy agradecido.

**Conclusiones**

- Esta vulnerabilidad es "rara", no es algo que un escaner te vaya a testear de forma automática y en pocos casos un actor malicioso se deje la pasta probando.
- Hacer PoCs es un arte y, a mi criterio, lo más interesante de un reporte.
- Practica en los VPDs, suelen estar menos explorados.
- Se creativo y no tengas miedo, el departamento de Product Security de una empresa de 5.000 millones me ha visto en pijama y chanclas.


-------------------------------

ENGLISH

This is the story of my latest report to Miele, which, besides being amusing, has taught me a lot.

As some of you may know, I am studying this year in Poland, living in a student dormitory. This means I am close to new software—enough said. We have a laundry room inside the building, which can be managed through a mobile app and a web platform. After simply observing how it worked, I looked up information about Miele to see if they had a Bug Bounty program or a Vulnerability Disclosure Program. Bingo, Miele has a [VDP](https://www.miele.com/en/com/cybersecurity-5047.htm).

What more could you ask for? You have the company's permission, and you get the challenge of saying you've hacked your own laundry system. So, let's get to work mapping out the entire application architecture: enumerating all functionalities, analyzing behavior, searching for API documentation, investigating the admin interface, and even reading the washing machine's assembly manual (yes, I am now an expert in wiring and water connections).

With this initial approach—before attempting any complex attack—I found the API documentation and some unhandled errors that revealed additional information about routes, libraries, and the overall stack. Reportable? Maybe, but I am not that kind of analyst.

After spending a few hours struggling with the application—which was more robust than it initially seemed—I discovered two stored XSS vulnerabilities, one of them blind. Since dedicating an entire day to the VDP during exams wasn't the best idea, I submitted the report and sent it over. The next day, Miele’s Product Security team responded, and by the second day, it was already fixed. I was pleasantly surprised by their professionalism, as many of you know firsthand that this is not common in most companies. They added me to their Hall of Fame and treated me very well.

![hof](/assets/img/hof.PNG){: .mx-auto.d-block :}

Seeing this, I couldn't pass up the opportunity to find something more interesting, something more technical. Fortunately, I had been researching and experimenting with Race Conditions and response timing attacks for some time, thanks to James Kettle, the Director of Research at PortSwigger. Particularly in ["Listen to the whispers: web timing attacks that actually work"](https://portswigger.net/research/listen-to-the-whispers-web-timing-attacks-that-actually-work) and ["Smashing the state machine: the true potential of web race conditions"](https://portswigger.net/research/smashing-the-state-machine).

When did I realize it might be vulnerable? Well, I had a dream that the payment process could be attacked this way. I woke up at 4 AM and wrote it down on a piece of paper. As simple as that. This has happened to me many times, and my instinct has never failed me. My sleep paralysis demon must be fascinated by me.

![donald](https://media3.giphy.com/media/mkhMTALnrYRLnuoe5P/giphy_s.gif?cid=6c09b952cfqjkwmm9kuyztgm5vyjhe3y2wbegj49uoebqmx4&ep=v1_gifs_search&rid=giphy_s.gif&ct=g){: .mx-auto.d-block :}

Alright, we have the attack idea and a gut feeling that it is vulnerable. The attack is based on activating multiple washing machines while only having enough balance for one, exploiting the time window between balance checks. Basically: I pay for one, but I activate them all.

For the Proof of Concept, I had to do something creative and different—this is my favorite part of the process. Don't just run an automated scan and read the theory; exploit it yourself and prove it's real. We’re interacting with a physical element, right? Let's go to the laundry room and do it on-site.

On a Tuesday at 3 AM, dressed in plaid pajamas, a black robe, and slippers, I recorded myself with a laptop in hand, launching the attack live. The rest is history: the attack worked exactly as I had envisioned, confirming the Race Condition. My app balance was negative, and I had a big smile on my face. Who knows what my two friends filming and the security guards were thinking.

![wash1](/assets/img/wash1.jpeg){: .mx-auto.d-block :}

![wash2](/assets/img/wash2.jpeg){: .mx-auto.d-block :}

What is the potential impact of this? Activating all the washing machines managed by the API simultaneously—thousands of them (starting a wash cycle still requires physical interaction, but the service would already be paid for). Miele took a few weeks to patch this issue, handling it in an exemplary manner, for which I am very grateful.

**Conclusions**

- This vulnerability is "rare"; an automated scanner won’t easily detect it, and a malicious actor is unlikely to spend money testing it.
- PoCs are an art, and in my opinion, the most interesting part of a report.
- Practice in VDPs—they are often less explored.
- Be creative and don’t be afraid. A $5 billion company’s Product Security team has seen me in pajamas and slippers.

