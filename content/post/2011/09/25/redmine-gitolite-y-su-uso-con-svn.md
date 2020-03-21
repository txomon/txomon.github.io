---
categories:
- Hallazgos
- Programación
date: "2011-09-25T01:25:37+02:00"
tags:
- avances del dia
- git
- gitolite
- NQaS
- perl
- redmine
- svn
title: Redmine, Gitolite y su uso con SVN
---

Me he quedado atascado en la instalación de Redmine y de Gitolite. He dejado de leer los docs de Redmine en cuanto he visto que necesitaba instalar Gitolite primero. Como la versión que tengo en mi ordenador de Gitolite tiene ya más de 1 año, me he decidido a leerme todos los docs (por cierto muy completos), y he encontrado esto:


http://sitaramc.github.com/gitolite/doc/gitolite.rc.html#SVNSERVE


El tag SVNSERVE no funciona, pero si buscais SVNSERVE os lleva. Esto me ha encendido un foco en la cabeza... PUEDO UTILIZAR GITOLITE PARA EL $%!"·$% ADMINISTRADOR DE SVN!!


Y me he vuelto loco con la idea. De momento, estoy haciendo un adm en python, con una manera de código muy chunga pero que he conseguido alguna funcionalidad. La cosa es que después de ver esto, se me ha abierto una luz en el cielo.


Y me he puesto en contacto con Sitaram para ver si había algo que hacer, o al menos para saber el soporte que daba a svn, y me ha contestado que lo único que daba era un soporte de autenticación, se limita a decir que ese usuario es el que se ha logueado.


Y eso me ha apagado el foco.


Eso significa que para utilizar gitolite, voy a tener que hacer módulos que se integren, evidentemente en perl, con el código de este. Por ello, voy a volver a insistir a Alberto (al pobre lo tengo crucificado con este tema) sobre pasarnos a Git. Las razones son estas:



* Tal y como funciona subversión, solo puedes limitar el acceso al repositorio. Lo que hagan dentro es casi imposible, ya que las estructuras de carpetas dentro del repositorio siguen un orden que lo pone el usuario.

* Tendría que cambiar todo el diseño que tengo hasta el momento de los repositorios de svn para que se adapte a las nuevas necesidades, y  a este paso, podrá ser mi proyecto de fin de carrera (por el tiempo que me va a llevar).

* Con gitolite puedes controlar desde que rutas escriben los usuarios hasta que usuarios pueden hacer tags de versiones etc.

* Puedes delegar, hacer repositorios comodines (ver los docs de Sitaram http://sitaramc.github.com/gitolite/doc/wildcard-repositories.html)

* Con Git puedes trabajar en casa o en tu pueblo sin necesidad de internet.

* Con Git, un merge no es una pesadilla, y además está preparado para poder hacer todo lo que te imagines con el repositorio.


En definitiva, es un sistema completo, que tiene actualizaciones regulares, soporte especializado, y totalmente moldeable.

Alberto me va a mandar al cuerno, pero es que ahorraría un trabajo de la leche. Ademas de que aprender a utilizar Git es facilísimo y está lleno de utilidades (como encontrar archivos a lo largo de su historia etc.)
