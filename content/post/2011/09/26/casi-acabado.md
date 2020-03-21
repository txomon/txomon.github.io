---
categories:
- Off-topic
- Programación
- Tecnicismos
date: "2011-09-26T23:43:48+02:00"
tags:
- avances del dia
- git
- NQaS
- resumen
- svn
title: Casi acabado
---

Bueno, pues parece que ya casi he acabado el sistema de administración del control de versiones.


Es muy muy malo, pero es una forma eficaz de poder empezar a trabajar, ya que hay otras prioridades.


Lo que hace este script en python, es primero comprobar todos los grupos que existen en: `/etc/groups` y mirar cuales tienes svn en su nombre, y empiezan a partir del 9999  (el gid). Despues mira los directorios en la raiz que se halla especificado (mejor que sea en / y no por ahí suelto, por que los grupos se crean con los path completos.


Después de encontrar las diferencias, se dispone a corregir. Como es mas fácil crear una carpeta que un grupo, pues mira todas las carpetas que hay, y crea los grupos correspondientes. Si luego ve que hay un grupo que sobra, pregunta para borrarlo.


Una vez visto que es consistente, se dispone a poner los permisos a todas las carpetas. Como no me quería andar con milongas, he hecho que cada vez que se ejecute, ponga en orden todas los `chown` de las carpetas y que cambie los permisos a g+s de tal manera que todo aquel que haga algo en el repositorio, imaginando que la prevención del umask ha sido salteada, pueda ser arreglada para el resto. Eso me falta por comprobar.


Saber si de verdad funciona todo este garigay. Por que en verdad os digo que no hay nadie con más ganas de acabar de programar algo para un control de versiones que no me gusta.


Por cierto, los repositorios se crean con `svn-admin create ` y el servidor es `svnserve -t ` o eso espero que sea.


De todos modos, me voy a sobar que en 6 horas me levanto.


Resumen del día más o menos hecho.


Bueno, en verdad falta añadir que he estado haciendo problemas de TC y que son horribles y tengo mogollón de dudas, por lo que mañana intentaré encontrar a Luis y pedirle ayuda (Luis: un tio muy majo que da una asignatura que te deja majo, pero interesante eso si)
