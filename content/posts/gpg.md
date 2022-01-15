---
title: "Pass y GPG para compartir contraseñas"
date: 2021-04-24T20:20:07+01:00
draft: true
---

# Introducción

Pass (https://www.passwordstore.org/) es una utilidad de código libre para la gestión de contraseñas de manera sencilla. Pass sigue la filosofía unix, que es hacer una única cosa de la mejor manera posible. Pass es una utilidad escrita en Bash, el código contiene bashisms y por lo tanto no es compatible con otros shells como zsh, dash o fish, no obstante la inmensa mayoría de las distribuciones linux utilizan bash por defecto. La seguridad de pass utiliza como base el programa GNU Privacy Guard, conocido por su acrónimo GPG. A pesar de que Pass es muy sencillo y está pensado para un uso personal, se puede utilizar para compartir contraseñas en un equipo sin necesidad de usar una contraseña maestra común para todos. De hecho, es el método que utilizamos en mi equipo para la administración de sistemas. La siguiente guía está basada en un [post de medium](https://medium.com/@davidpiegza/using-pass-in-a-team-1aa7adf36592), no obstante, por descuido o por estar enfocado a un público más experto, la guía ignora algunos pasos esenciales con GPG antes de poder utilizar Pass.

# Construir un par de claves pública/privada con GPG

*Referencia:* [Manual GNU](https://www.gnupg.org/gph/en/manual/c14.html)

 La idea de este paso es crear un par de claves, una pública y una privada. GPG requerirá un nombre, un correo (muy importante ponerlo correctamente) y una contraseña. Se pueden modificar las opciones para la creación del keypair, no obstante se recomienda la creación por defecto. Para más detalles sobre el algoritmo utilizado puede consultarse el link en la referencia.

La contraseña que GPG pide es para encriptar de manera simétrica la clave privada y que no exista como un fichero de texto plano en nuestro sistema. Aunque todavía no hemos llegado a este punto, es importante saber que Pass encriptará nuestras contraseñas con nuestras claves públicas. De esa manera solo se podrán desencriptar con nuestra clave privada, que solo nosotros deberemos conocer. 

![](/img/posts/gpg/gpg1.webp)

# Compartir / Recibir las claves públicas del resto de miembros del equipo

Para que las contraseñas que creemos puedan ser desencriptadas por otra gente necesitamos su clave pública. GPG ofrece ambas opciones de exportar e importar de manera sencilla. 

![](/img/posts/gpg/gpg2.webp)

__NOTA: Los cuadrados blancos tapan datos personales, en la ejecución normal aparecerá el nombre y correo de la persona propietaria de la clave__

Acto seguido enviaremos este fichero a nuestros colaboradores. Ellos tendrán que importar la clave y firmarla.

![](/img/posts/gpg/gpg3.webp)

 El proceso de firma se ilustra con la siguiente imagen. Es muy importante comprobar que el fingerprint coincide con el proporcionado. Con el comando `gpg --list-keys –fingerprint`.

Podemos consultar todos los fingerprints de las claves públicas almacenadas, incluida la nuestra, y trasmitirlo a nuestros colaboradores. 

![](/img/posts/gpg/gpg4.webp)

# Crear el almacenamiento de contraseñas

Antes de empezar a usar pass, es importa señalar dos aspectos fundamentales de su funcionamiento:

1. La idea detrás de pass es guardar cada contraseña en un fichero de texto encriptado, cuyo nombre es la entidad o sitio web al que pertenece. Esto hace posible compartir contraseñas enviando simplemente los ficheros.

2. pass tiene buena integración con git, lo que permite mantener nuestras contraseñas en un repositorio remoto, esto tiene muchas ventajas como por ejemplo mantenerlas si el sistema queda inaccesible o compartirlas y sincronizarlas de manera muy fácil con nuestros colaboradores. Esta segunda opción será la que escojamos.

El comando `pass init correo_public_key` inicializa el repositorio de contraseñas en una carpeta oculta (empieza por punto) en nuestro directorio home, con opciones extra esta carpeta se puede colocar en cualquier otro path.

![](/img/posts/gpg/gpg5.webp#smallfigure)

Junto con esta carpeta se nos habrá creado un fichero llamado .gpg-id, este fichero sirve para indicar que claves públicas, usando su correo, se utilizan para encriptar las contraseñas.

![](/img/posts/gpg/gpg6.webp#smallfigure)

Es importante destacar que .password-store también acepta la creación de subcarpetas, y cada una de estas carpetas tiene su .gpg-id independiente, de modo que podemos utilizar una de estas subcarpetas como “carpeta compartida” con nuestros colaboradores. 

# Carpeta compartida

![](/img/posts/gpg/gpg7.webp#smallfigure)

En este caso se ha creado la carpeta acme, que aparece al hacer ls junto con la que ya tenía creada de antemano. Esta carpeta tiene en su interior el fichero .gpg-id. De haber utilizado el comando: `git init –p nombre_carpeta correo` pass hubiera creado lo mismo, más un repositorio git en el interior. Este repositorio se puede crear a posteriori produciendo exactamente el mismo resultado.

# Crear contraseña

Podemos empezar a añadir contraseñas, podemos usar nuestra carpeta raiz (.password_store) o nuestra subcarpeta compartida mediante el comando `pass insert`.

![](/img/posts/gpg/gpg8.webp#smallfigure)

También existe la opción de generar una contraseña aleatoria de N digitos con `pass generate`.

![](/img/posts/gpg/gpg9.webp)

# Añadir colaboradores y re-encriptar

Para ello únicamente tenemos que abrir el fichero .gpg-id con nuestro editor favorito y añadir los correos de nuestros colaboradores, uno por linea.

![](/img/posts/gpg/gpg10.webp)

Cuando se realizan cambios en el fichero .gpg-id de la carpeta a compartir, es necesario ejecutar la siguiente orden para reencriptar los ficheros con las diferentes claves.

![](/img/posts/gpg/gpg11.webp)

El comando entre $() simplemente muestra el contenido del fichero donde están los correos de los colaboradores para que el comando pass init –p lo utilice como argumento.

# Otros comandos

Además de insert y generate pass tiene otros comandos para editar, borrar, buscar, mostrar, copiar y mover contraseñas. Todos estos comandos se pueden consultar en el manual (man pass)

![](/img/posts/gpg/gpg12.webp)

# Otras consideraciones

 Pass es un programa libre, util y muy sencillo, que permite compartir y guardar nuestras contraseñas en simples ficheros de texto encriptados. Este funcionamiento hace que tenga una infinidad de ventajas respecto a sus competidores, como la transparencia en el manejo de contraseñas, mantener la información en tus sistemas, la mínima instalación, la facilidad para compartir, etc...

No obstante, el planteamiento tiene puntos débiles o puntos que requieren un cuidado especial que quizás en otro tipo de software no se necesitarían:

* Cada usuario tiene que hacerse cargo de su clave privada y tener un backup por si su sistema fallara poder recuperarla

* Pass no tiene multiusuario, de modo que todos los usuarios verían todas las contraseñas. Esto puede mitigarse creando diversas subcarpetas con distintos usuarios simulando grupos de permisos.

* Aunque pass se encarga de crear un commit cada vez que se produce un cambio en una carpeta que contiene un repositorio, es importante hacer un push en repositorios compartidos, de la misma manera que se necesota hacer un pull para estar al día. Hay que hacerlo manualmente o con un script
