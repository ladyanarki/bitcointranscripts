---
title: Bellare Neven
transcript_by: Bryan Bishop
translation_by: Blue Moon
categories: ['core-dev-tech']
tags: ['multisig']
date: 2018-03-05
aliases: ['/es/bitcoin-core-dev-tech/2018-03-05-bellare-neven/']
---

Ver también <http://diyhpl.us/wiki/transcripts/bitcoin-core-dev-tech/2017-09-06-signature-aggregation/>

# Bellare-Neven

Se ha publicado, ha existido durante una década, y es ampliamente citado. En Bellare-Neven, es en sí mismo, es un esquema multi-firma que significa múltiples pubkeys y un mensaje. Debe tratar las autorizaciones individuales para gastar entradas, como mensajes individuales. Lo que necesitamos es un esquema de firma agregada interactiva. El artículo de Bellare-Neven sugiere una forma trivial de construir un esquema de firma agregada a partir de un esquema multisig donde interactivamente todos firman el mensaje de todos. Encontramos una vulnerabilidad en este esquema, es muy sutil.

A Russell O'Connor se le ocurrió este ataque. Digamos que tienes dos salidas con la misma clave, ya es un problema, pero no podemos evitarlo. Quieres participar en el coinjoin conmigo en el que solo intentas gastar una de ellas y no la otra. Así que tenemos tres UTXOs, y también hablaremos de los mensajes para cada uno de esos UTXOs, mensajes que autorizan el gasto de esas salidas. Supongamos que son cosas de una sola entrada. Y estamos tratando de hacer un coinjoin para dos de los mensajes, y estoy tratando de robar esta salida de usted. Se explica en el papel musig en realidad. En resumen, voy a participar en el protocolo de multifirma, por lo que el Bellare-neven levantado a un protocolo de firma agregada interactiva donde prented mi clave, donde pretendo tener su clave y su mensaje, y voy a comer el nonce que usted viene con. Simplemente repito esto, actúo como un espejo, tú dices que tu clave es esto, y yo digo que mi clave dice esto, y tú dices tu nonce y yo digo un nonce; hay una versión más avanzada donde puedo ocultarlo realmente. Asumo que no validas el mensaje que estoy tratando de firmar, ese es el punto clave. En el esquema de firma agregada, cada uno tiene su propio mensaje, y no debería importarte qué mensaje está intentando firmar alguien; debería ser imposible que alguien firme con la otra clave..... los mensajes serán los mismos, los dos hashes serán los mismos, entre lo que crees que voy a firmar y lo que realmente estoy firmando-- no estoy diciendo cuál de los dos somos. Con el índice, es seguro. Sin el índice, es inseguro. Bellare-neven ha iactualmente especificado esta manera- en el hash es la lista de todas las claves, y luego su clave a sí mismo, pero no dice hwat índice en que la lista es. El problema es que si esas claves son las mismas, puedo usar tu hash para la primera, y repetirlo, y si p1 y p2 son iguales, si estas dos cosas son iguales, mientras que si estoy firmando p1 y p2, y ... esto no funciona. Vamos a firmar una multifirma con 2 veces el valor de la clave.. si no estoy usando el índice, entonces las dos cosas son exactamente el mismo hash, y como resultado su firma parcial y doble, y ahora es una firma válida para ambos. Así que la solución es usar un índice, y no las claves públicas. Tienes que obligar a la gente a poner los índices.

La ventaja de no tener índices es que, estás ordenando, tu ordenación local. Sólo dices, esto, haces una ordenación lexicográfica en las pubkeys, pero ves esto como la serialización de tus claves, no necesitas tener una idea del ordenamiento de las claves, y en el papel de Bellare-Neven... todos los índices son índices locales que no tienen que corresponder a los índices de las otras partes, y esto resulta en los ataques de Russell. De todos modos, es una ventaja muy teórica tener sólo índices locales... Me gustaría mucho ver una prueba de que esta construcción realmente resuelve este problema. Este aspecto de convertir un esquema multisig en un esquema de firma agregada interactiva no viene con una prueba en el documento.

También se podría arreglar diciendo que nunca se permita la reutilización de una firma parcial si alguno de los otros participantes afirma tener su pubkey. Creo que esto también es una solución suficiente. Pero no confío en que las implementaciones lo hagan correctamente.

Digamos que tienes tuplas de claves y mensajes. Ordenas lexicográficamente esas tuplas. Y luego digamos que L es el hash de todo eso. Y entonces usas la ecuación Bellare-Neven se convierte... y ahora ni siquiera necesitas .... esto nunca separa el mensaje de la pubkey. Es un esquema de firma, sólo que lista de pares de mensajes de la pubclave en lugar de conjuntos de pares de mensajes de la pubclave. Los conjuntos son mucho más difíciles que las listas.

Busco a alguien que pueda demostrarlo, porque yo no puedo.
