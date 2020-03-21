---
categories:
- Programación
- Tecnicismos
date: "2012-12-29T18:33:25+01:00"
title: Backport de mod_authnz_ldap de apache 2.4 a 2.2
---
Lo primero decir que ha costado dios y ayuda.

Como pro-tip, la siguiente vez que queráis compilar algo, mirad si tiene algo especial en el linkado. este modulo necesita -lldap y -llber -llber (2 veces o así lo dice apr-config --with-ldap).

Otro inconveniente que me he encontrado es que han hecho un cambio brutal en los modulos auth, básicamente porque ya distinguen entre authn y authz (authentication y authorization respectivamente). Y eso en el código se nota demasiado.

No tengo tiempo para documentar el proceso, pero puedo decir pestes de todo tipo sobre ubuntu. Odio que no tengan una rama experimental.
