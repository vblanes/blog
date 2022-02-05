---
title: "Blockchain no vale para nada"
date: 2022-01-21T20:54:51+01:00
draft: true
tags: ["blockchain", "opinión"]
---

# Introducción

Llevo un rato pensando en como responder a este tuit que me ha enviado uno de mis amigos. He pensado en conectarme a mi cuenta de tuiter profesional y citar el tuit diciendo que es una estafa, he pensado en hacerlo desde mi cuenta caótica del día a día. Al final creo que lo más constructivo será que analicemos porque este tuit y el [artículo que enlaza](https://t.co/5SIr7Gk22u) no son más que un conjunto de _buzzwords_ provenientes de una gente que no tiene ni idea y que solo quiere ganar visibilidad y como además esto nos da pistas de una verdad un poco más incómoda y es que... BLOCKCHAIN NO VALE PARA NADA.

![El dichoso tuit](/img/posts/blockchainbad/tuitbcml.webp#smaller)

# ¿Qué es el blockchain?

Ríos de tinta (digital) se han vertido sobre esto ya, así que me limitaré a hacer una explicación simplista y enlazar algún recurso que me parezca interesante para entender la tecnología. Blockchain es como un gran libro de cuentas donde se van apuntando las transacciones que se realizan entre los diferentes nodos de la red. Su principal atributo es que es un sistema **distribuido**. Es decir, este tipo de tecnología guarda ese libro del que hablamos en cada nodo (normalmente de manera parcial), de modo que aunque alguien alterara los valores de su copia en su beneficio, la red y los participantes de esta no aceptarían esos cambios, ya que discreparían con todos los demás. [Video: ¿qué es el Blockchain?](https://www.youtube.com/watch?v=V9Kr2SujqHw)

# Por qué el artículo es mentira

El artículo asume que los datos no pueden compartirse entre hospitales por motivos de privacidad, pero sí subirse a una plataforma/nube/blockchain común, ya que ¿eso asegura su consistencia? No tiene ningún sentido, de hecho sigues teniendo el problema de sacar datos del hospital con el añadido de que el blockchain no es más que una lista enlaza de esas que enseñan en introducción a la programación en primero de carrera, y que, por lo tanto, es tremendamente ineficiente para guardar datos complejos como son los registros de un hospital.

En resumen, sigue siendo terriblemente problemático desde el punto de vista de compartir datos, posiblemente necesitando un millón de diferentes comités éticos, y además tenemos datos en un formato tremendamente complejo que cambian de hospital en hospital en hospital y que a día de hoy es un problema abierto llamado **interoperabilidad** y que tiene algunas soluciones propuestas como: [HL7](https://www.hl7.org/events/ciic.cfm?ref=nav). Para poner la guinda, esta red necesitaría de nodos mineros y sería terriblemente ineficiente para entrenar los modelos.

# Usos del blockchain

## Especulación

Los únicos sistemas en producción y de impacto significativo en el mundo que han producido sistemas en blockchain han sido por su utilidad para la especulación, el engaño y los [esquemas Ponzi](https://es.wikipedia.org/wiki/Esquema_Ponzi). Destacan las criptomonedas, especialmente Bitcoin y Etherium, y el reciente boom (artificial) de los [NFT](https://www.theverge.com/22310188/nft-explainer-what-is-blockchain-crypto-art-faq).

Las criptomonedas pudiera parecer que tienen una base razonable, que la gente pueda tener una divisa que no esté controlada por un banco central. Esto es un sueño húmedo anarquista, además, al ser estas monedas anónimas (o pseudo-anónimas), elimina el elemento orwelliano del control por parte de una entidad superior. No obstante, nos hemos dado de bruces contra la realidad. Y esta realidad no es otra que estas monedas únicamente están sirviendo para especular y para timar a aquellos que quieren subirse al carro. De la especulación no es necesario que de pruebas, el lema de los _criptobros_ es "¿No quieres hacerte rico?". Para verificar que realmente es una comunidad llena de estafadores, solo hace falta contemplar el trabajo de algunas personas como [Coffeezilla](https://www.youtube.com/c/Coffeezilla), que en su canal de youtube se dedica a destapar timos basados en cripto como el de [Save the Kids](https://www.youtube.com/watch?v=7F_RKFQl3pg).

Luego tenemos los NFTs. La mejor definición que he escuchado de NFT es: una cola con plazas limitadas, donde cada hueco tiene asignado uno imagen, un video u otro tipo de _token_. No compras el NFT, no compras los derechos sobre el NFT, simplemente compras un pequeño recibo que dice que esa posición en la cola es tuya. En muchos casos ni siquiera la propia imagen, el tipo de NFT más común, está en el blockchain, solamente un link a otra plataforma como un google drive o dropbox.

Uno de los argumentos más usuales a favor de los NFT es "Puedes comprar el NFT de una espada y usarla en múltiples videojuegos". Desde el punto de vista técnico, es imposible una conversión directa. Desde el punto de vista tecnológico, eso ya podría hacerse con la tecnología actual teniendo superplataformas como steam o PS network. Finalmente, de verdad alguien se cree que las empresas y el capitalismo van a dejar de ganar dinero en microtransacciones y demás. La idea hace agua por todos lados y realmente recomiendo mucho esta maravillosa explicación, en inglés, de toda la movida: [Video: What the hell are NFT's?](https://www.youtube.com/watch?v=XwMjPWOailQ)

Por último, y aparte de todas estas implicaciones éticas, que ya haría apartarse del camino de las criptomonedas y los NFTs a cualquiera que se considerase "buena persona", tenemos la movida del impacto ambiental. En Internet es una discusión muy manida, los detractores del blockchain dicen que consumen muchísima energía, los _criptobros_ dicen que hay otras cosas que también consumen mucha energía como las granjas de servidores o las fábricas de teléfonos móviles. Habría que realizar un estudio a fondo de la situación y sobre todo poner los números en contexto. Podemos empezar por aquí: [What's the Environmental Impact of Cryptocurrency?](https://www.investopedia.com/tech/whats-environmental-impact-cryptocurrency/)

## Posibles para la industria

Ciertamente, mi línea de investigación no está basada en blockchain, por eso he de reconocer que no conozco a fondo todas las aplicaciones que se proponen. Solo hay una cosa segura, no hay ninguna que sea madura y haya tenido un impacto significativo todavía. Aparte de la especulación.

Me gustaría hablar sobre un proyecto, en el que aunque no haya participado directamente, si estoy al corriente. La idea básica y sin entrar en detalles es conectar los sensores de una fábrica farmacéutica a un blockchain. De esta manera cuando los inspectores acceden para auditar las plantas, pueden asegurarse de que las lecturas de los sensores no han sido adulteradas y, por tanto, pueden asegurar que las medicaciones fabricadas cumples con los estándares exigidos. La idea es tremenda y fenomenal, pero se me presentan una serie de problemas/preguntas que soy incapaz de resolver para justificar la tecnología blockchain. Los enumero a continuación.

1. ¿Quién asegura que las lecturas de los sensores sean correctas? Si el software (o firmware) de los sensores no es de código abierto o auditable, podría estar introduciendo valores erróneos en el blockchain. Inalterables, sí, pero erróneos.
2. Como se trataría de un blockchain privado, no tengo muy claro que realmente no sea inalterable, por aquello del [ataque del 51%](https://www.bitcoin.com.mx/que-es-un-51-attack-y-como-podria-afectar-a-bitcoin/)
3. Tomando en cuenta el punto 1, el blockchain no sería necesario si los datos se enviaran directamente a un servidor de una autoridad pública y/o certificadora.
4. Con copias de seguridad y un mantenimiento adecuado del punto 3, no es necesario el blockchain para la duplicidad y seguridad de los datos.

# Conclusiones
Parece que los únicos usos del blockchain en su estado actual pasan por las criptomonedas, las estafas, los NFTs y la especulación en general. Hay muchos investigadores, algunos muy buenos, que están invirtiendo su tiempo y esfuerzo en crear buenas aplicaciones con el uso del blockchain, no obstante a días de hoy no he encontrado ninguna que valga para nada. **Este artículo refleja mi punto de vista en el momento actual, no poseo todas las respuestas, pero estoy abierto a una discusión constructiva vía email**. Enlaces en la parte superior :)