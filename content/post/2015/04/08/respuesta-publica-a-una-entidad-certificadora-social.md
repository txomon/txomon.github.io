---
categories:
- Tecnicismos
- Politics
date: "2015-04-08T16:52:01+01:00"
tags:
- respuesta publica
- GPG
- CA social
- DNIe
- Podemos
title: Respuesta pública a una CA social (para Podemos)
---
A cuenta de la dimisión del I+D de Podemos a cuenta del censo, [Jivago](http://www.jivablog.com/) ha salido con una idea sobre tener una CA social. Expongo mi razonamiento sobre porque no es posible para una organización política como Podemos y sus objetivos.

El tema de un identificador electrónico certificado solo se consigue a través de dos maneras, con una CA oficial y centralizada o una por confianza y distribuida.

Tenemos ambos métodos disponibles. El software del DNI-e es una mierda (en eso estamos todos de acuerdo), pero se pueden mejorar porque usan estándares abiertos. Desconozco el coste del desarrollo, pero con un equipo de 3 personas con dedicación completa en 2 meses sacas seguro una librería que maneje bien el DNIe, y en otros tantos meses puedes hacer aplicaciones usando esa librería. Pero en definitiva se puede hacer.

Por otro lado, la CA distribuida es un tostón de atar. Ya existe, y se llama GPG, con sus signing parties, etc. Donde las personas acreditan la identidad en persona, firmando la clave pública de la persona y subiéndo la firma a servidores de backup. El problema es que hay que decidir cuando un número de firmas es aceptable, así como la validez de las mismas etc.

Por esa misma razón, una CA distribuida no es posible para Podemos, una persona debería poder participar sin necesidad de enseñar su DNI a nadie.

Sin embargo, si que se podría hacer un híbrido, donde sea posible para la acreditación de personas tanto por DNIe como físicamente en el lugar. Como esto no va a ningún sitio, solo decir que es posible conseguir que este sistema funcione, garantizando la accesibilidad a los que no tienen DNIe o no tienen los recursos, y garantizando la privacidad a los que no queremos enseñar nuestro DNI a otros.

En definitiva, yo creo que esto da cabida a un proyecto, es posible, y es interesante, pero por desgracia no tengo suficiente tiempo.
