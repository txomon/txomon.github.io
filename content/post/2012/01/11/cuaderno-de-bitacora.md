---
categories:
- Automatización
- Cuaderno de bitácora
- Hallazgos
- Programación
- Tecnicismos
date: "2012-01-11T20:59:13+01:00"
title: cuaderno-de-bitácora
---

He hecho muchas cosas hoy. Empezando ayer a las 23:50 después de llegar de casa de nerea (si tarda 10 mins ¬¬ ella sabe a que me refiero).


Empecé a investigar sobre el [algoritmo aho-corasick](http://en.wikipedia.org/wiki/Aho%E2%80%93Corasick_string_matching_algorithm), que según tenía entendido, era un modelo de Luis para el algoritmo. Aunque por lo que me he enterado más tarde, solo interesa la problemática del acceso a memoria compartida que utiliza por debajo el algoritmo. Por lo tanto, puedo directamente, hacer un testcase que haga eso en el Adviser y a correr. De todos modos, me he estudiado el algoritmo, ya que mi PFC y lo de Luis no es lo mismo, y he ideado unos cuantos métodos para hacer el algoritmo más rápido, aunque aún hay que probar mis varias teorias.


Sea lo que sea, aún tengo que poner las cosas en Github, evidentemente, si hago algo va a ser sobre GPL, por que para mí el mundo libre... Ha sido de gran ayuda.


He pasado toda la mañana haciendo cosas sobre el algoritmo, hasta después de comer. Que por cierto, les he dado la caca con mis ideas en gran medida. Y luego me he puesto a hacer cosas que tenía pendientes, por ejemplo, de la banda. He empezado a crear el repositorio del aho-corasick, pero ya seguiré. Lo cierto es que ahora hecho la vista hacia atrás y no se que he hecho.


Mirando mi explorador, que si que sabe lo que he hecho, he estado hasta las 15:30 buscando cosas sobre aho-corasick, después he actualizado, a raíz de un [código que he encontrado](http://sourceforge.net/projects/multifast/) sobre este algoritmo (en verdad a su licencia adjunta), mis conocimientos sobre [licencias](http://www.gnu.org/licenses/why-not-lgpl.es.html) de lo que haga.


Después, el chrome dice que he estado en IRC, hablando sobre una alternativa a [OpenWRT](http://www.openwrt.org/), [FreeWRT](http://freewrt.org/trac/), despué he asistido a una reunion de ubuntu qa (sí, estoy metido ahí). Y luego por último, me he puesto a mirar cosas sobre automatización, desde [IronAHK](http://www.ironahk.net/), luego Jenkins (mi gran coloso), hasta llegar a la automatización de Instalación de sistemas operativos, entonces, he empezado a caer en el fondo de un pozo que me llevaba a sistemas cada vez mejores hasta tocar fondo, Orchestra.


Por ello, al final, he tenido que mandarle un correo al del CIDIR, Jesus, para comentarle lo de Orchestra y Armando por todo lo que he descubierto.

> Buenas Armando,
>
> Aclaro que no espero respuesta vía correo electrónico.
>
> He asistido via IRC a la reunión de QA de ubuntu, y ha salido el tema de > los tests de las isos, la automatización, y eso me ha recordad algunas > cosas que se y que nos podrían venir bien. Antes de mandarte un mail con > mis ideas, me he puesto a buscar y a refrescar el mundo sobre la distribuci> ón de sistemas operativos, instalación automatizada, y todas esas cosas > de grandes empresas.
>
> Y bueno, también ya de paso, he vuelto a darme con uno de mis grandes > colosos que todavía me quedan por derrotar (hay muchos). Jenkins [1]. Es > una plataforma para lanza pruebas, y que está muy muy especializada. Puede > que cuando se hizo la arquitectura de pruebas no existiera, pero este > programa es una pasada (y como he dicho, un coloso).
>
> Esto lo he ido viendo a lo largo de mi corta vida de informática (4~5 > años) en varios sitios, y funciona genial, al menos si lo sabes configurar,>  por que yo lo he intentado (también decir que no me puse en serio), y > fracase.
>
> Pero visto que me tengo que aprender la arquitectura de pruebas y como > funciona, porque no, aprender a utilizar este programa? Al fin y al cabo, > el Adviser en verdad no necesita toda la test-arch, de hecho, con solo el adviser me basto para hacer pruebas de captura (al menos hasta lo que se yo). Es cierto que necesita el Manager, los demonios y todas esas cosas...
>
> Si puedo conseguir todo de un plumazo con Jenkins, por que no intentarlo? no creo que se tarde más de un día en aprender a dominarlo, y la opción de automatizar pruebas, desde que pongo una nueva versión en el repositorio hasta la ejecución del adviser, con resultados de pruebas y logs es demasiado tentadora para mí. Dicho esto, en mi tiempo libre, estudiaré como hacer esto.
>
> Ya que estaba con esto, pues he seguido mirando, y he hablando con uno de QA de xubuntu, me ha comentado que se estaba rompiendo la cabeza con el jenkins que no lo entendía etc. (Debe ser mayor ya por lo que ha dicho) y eso me ha recordado que en los laboratorios de QA de ubuntu, tienen algún tipo de sistema para automatizar las pruebas de ISO, lo que significa, que tienen la capacidad de hasta instalar sistemas automatizadamente.
>
> Y eso me ha llevado a algo sorprendente. Mucho más allá de la replicación de sistemas, Ubuntu, con su intención de introducirse en el mundo de los servidores, de los de verdad, ha desarrollado el tambien llamado `The Ubuntu Orchestra Project`, que para el común de los mortales se conoce como Orchestra. La carta de presentación[2] no deja lugar a dudas, es un pedazo de proyecto de tres pares. Lo mejor de todo es que está en desarrollo activo, que es software libre, y que me da ideas sobre lo que te he dicho de hacerle la vida más facil a Edu (y al resto de paso también) sobre la instalación de sistemas automatizada.
>
> Pues bueno, esos son mis avances de hoy.
>
> Javier Domingo
>
> [1] Jenkins: https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins
>
> [2] Orchestra: http://blog.dustinkirkland.com/2011/08/formal-introduction-to-ubuntu-orchestra.html

Y bueno, eso ha sido todo por hoy, marcho a donde Nerea,

Agur!
