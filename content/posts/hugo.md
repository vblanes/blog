---
title: "Migración del blog a Hugo y como publicarlo en github pages"
date: 2022-01-16T17:08:44+01:00
draft: true
tags: ['hugo', 'markdown', 'blog']
---

# Introdución

En este post voy a cubrir mi expericia utilizando Hugo (https://gohugo.io/) y a incluir los pasos básicos para crear un sitio web y publicarlo en Github pages. Hugo es un framework para creación de páginas de web estáticas. Es decir, de entrada parece una opción genial para crear un blog. Yo lo conocía desde hacía tiempo, pero no sabía si valía la pena aprender a utilizarlo hasta que vi que Jacob Kaplan-Moss, uno de los creadors del framework web Django, lo utilizaba para su propio blog (https://jacobian.org/).

Siempre quise tener un blog donde hablar sobre tecnología, tutoriales y trucos pero nunca me habían convencido los blogs basados en wordpress, siempre pensé que siendo yo programador, e ingeniero informático, lo más __cool__ era que me hiciera mi propio blog y lo auto-hosteara. Mi primer instinto fue crear utilizando Django, principalmente porque la única herramienta de trabajo que he manejado para hacer programación web, pero a los tres primeros minutos de tener esta idea uno se da cuenta de que un framework tan grande y que incluye tantas cosas no es adecuado para un blog con muy pocas funcionalidades, que podría ser perfectamente un sitio web estático.

La segunda versión del blog era un compendio de ficheros html y la librería bootstrap para la maquetación de las páginas. Su simplicidad me gustaba mucho, ya que no usaba más recursos de los que necesitaba y me permitía una flexibilidad absoluta. No obstante habían algunos inconvenientes. La primera cosa que me molestaba era que si quería mantener una barra superior común a todas las páginas, debía incorportar el mismo trozo de html una y otra vez. Otra de las cosas que se me hacían muy pesadas era la maquetación de las páginas, perdía mucho tiempo picando las étiquetas para maquetar, que si ```<p>```, que si ```<b>```, ahora una ```<table>```. Es por eso que decidí probar Hugo migrando el poco contenido que sí tenía el blog.

**DISCLAIRMER**: __No soy experto en Hugo, de hecho todavía estoy empezando a usarlo, no obstante para mi ha sido tan sencillo montar un blog sencillo y con buena pinta que no puedo resistirme a recomendarlo. Y bueno, escribir posts utilizando markdown en lugar de html **muchísimo** más rápido (y más satisfactorio)__

# Tutorial

Odio **muchísimo** cuando buscas algo sobre una tecnología y solo encuentras el mismo tutorial básico una y otra vez. Y sí, yo también lo estoy haciendo aquí, no obstante diré en mi defensa que este está en castellano (jeje) y además aprovecharé para comentar algunas consideraciónes y explicar que ~~carajo~~ hace cada uno de los comandos.

## Instalación

Hugo puede instalarse a través de la terminal, usando el systema de paquetes, en cualquier distribución linux. En mi caso que utilizo manjaro sería algo como ```sudo pacman -Sy hugo```. Con Ubuntu u otros sistemas basados en debian debería ser ```sudo apt install hugo```. Para más información en otras plataformas consultado el tutorial oficial: https://gohugo.io/getting-started/quick-start/#step-1-install-hugo.

## Creación del sitio

El primer paso es colocarse en la carpeta donde uno quiere crear su proyecto, en mi caso, tengo los proyectos personales en una carpeta llamada "projects", que cuelga de mi home, así que en una terminal recien abierta haré ```cd projects```. 

```bash
hugo new site misitio
```

Básicamente llamamos al programa ```hugo``` y con el comando ```new``` le indicamos que queremos un elemento nuevo, que ente caso es un sitio web ```site``` y por último, el argumento será el **nombre** que queramos ponerle a nuestro proyecto. Veremos que una carpeta con el nombre del proyecto ha aparecido, y nos vomemos a su interior. El contenido de la carpeta de proyecto será algo similar a esto:

![Imagen de la raíz](/img/posts/hugo/raiz.webp)

Y nos referiremos a esta carpeta como **raíz**...

## Instalar un tema

El primer paso para añadir contenido en nuestra página es seleccionar un tema. Un tema no es más que un conjunto de ficheros que configuran Hugo para que la página web que sea generada al final del proceso tengo un aspecto concreto. El mismo concepto que una plantilla de wordpress. La página de Hugo ofrece una lista de entre los que elegir: https://themes.gohugo.io/. Estoy seguro de que existen muchos más que no están indexados en la página oficial. También está la opción de crear tu propio tema si tienes el tiempo y los conocimientos necesarios... ¯\\_(ツ)\_/¯. Para este blog no quería complicarme mucho y elegí la plantilla "minimal".

El primer paso es iniciar un repositorio git en nuestra carpeta mediante el comando ```git init```. Además de permitirnos mantener el control de versiones de nuestro blog será **necesario** para colgar nuestro blog en github pagers. Una vez hecho esto, debemos descargar la plantilla del github del creador, pero hay que añadirla como un módulo git separado, esto es para mantener la independencia entre el código del creador de la plantilla y nuestro proyecto, ya que no tiene mucho sentido replicar sus ficheros en nuestro proyecto. Para ello ejecutamos:

```bash
git submodule add https://github.com/calintat/minimal.git themes/minimal
git submodule init
git submodule update
```

Estos comandos clonan los contenidos del repositorio en el interior de la carpeta **themes** y buscan actualizaciones. Una vez hecho esto iremos a la carpeta del tema y copiaremos el fichero **config.toml** en la carpeta **raíz**. Podemos hacerlo desde la consola utilizando el comando: ```cp themes/minimal/exampleSite/config.toml . ```

**Importante:** Este fichero debe sustituir el que existia en la raíz con el mismo nombre.

**Importante:** Se recomienda añadir un fichero **.gitignore** tal cual se crea el repositorio en la raíz, para no incluir ficheros indeseados al repositorio de github, mi sugerencia: https://www.toptal.com/developers/gitignore/api/hugo

## Editar el fichero config.toml

Debemos editar el fichero de configuración, en el que añadiremos el nombre de nuestro blog, el subtítulo, nuestro nombre y algunos detalles más como en este caso los elementos que aparecen en la barra superior del blog.

```toml
baseURL = "https://vblanes.github.io/"
languageCode = "es"
title = "S.S. Aleph"
theme = "minimal"
publishDir = "docs"

[params]
    author = "Vicent Blanes"
    description = "Programación, Ciencia y Linux"
    githubUsername = "vblanes"
    accent = "blue"
    showBorder = true
    backgroundColor = "#f5f0f0"
    font = "Raleway" # should match the name on Google Fonts!
    highlight = true
    highlightStyle = "default"
    highlightLanguages = ["go", "haskell", "kotlin", "scala", "swift", "python"]
```
De este trozo de fichero cabe destacar varias cosas:

1. Utilizamos como baseURL, la dirección que tendrá nuestra github page, si en este paso no estamos seguros siempre podemos modificarlo después.
2. Deberemos el parametro ```publishDir = "docs"``` para que posteriormente el blog funcione en github pages.
3. He cambiado minimamemte algunas cosas, como el color del accento que era rojo de base, o el color del fondo, que he cambiado de "white" a una tonalidad más crudo.

En la segunda parte del fichero unicamente edité los campos para poner mis redes y customicé ligeramente las secciones por defecto

```toml
[[menu.main]]
    url = "/"
    name = "Home"
    weight = 1

[[menu.main]]
    url = "/about/"
    name = "About"
    weight = 2

[[menu.main]]
    url = "/posts/"
    name = "Posts"
    weight = 3

[[menu.main]]
    url = "/links"
    name = "Links"
    weight = 4

# Social icons to be shown on the right-hand side of the navigation bar.
# The "name" field should match the name of the icon in Font Awesome.
# The list of available icons can be found at http://fontawesome.io/icons.

[[menu.icon]]
    url = "mailto:viblasel @ gmail.com"
    name = "fas fa-envelope"
    weight = 1

[[menu.icon]]
    url = "https://github.com/vblanes/"
    name = "fab fa-github"
    weight = 2

[[menu.icon]]
    url = "https://twitter.com/vicent_bdslab/"
    name = "fab fa-twitter"
    weight = 3

```
## Añadir contenido

Para crear un post, o realmente cualquier otra página como la de **about**, podemos utilizar el comando ```hugo new posts/mipost.md``` o ```hugo new about.md```. Este comando se limita a crear un fichero markdown con un poco de metainformación en la parte superior. Todos los contenidos de hugo son creado en las carpeta **content** que se encuentra en **la raíz**.

A partir de aquí nos pondremos a escribir el contenido que deseemos utilizando la sintaxis de markdown, por ejemplo, la página de about está escrita de la siguiente manera:

```markdown
---
title: "About"
date: 2022-01-15T18:39:59+01:00
draft: true
---

# Vicent Blanes-Selva

Estudiante de doctorando en tecnologías de la salud y el bienestar en la Universitat Politècnica de València, especializado en Machine Learning aplicado a medicina y programador, sobretodo programador.

Mi linea de investigación versa sobre la construcción de sistemas de ayuda a la decisión (CDSS), en el ámbito médico, sobretodo con el foco sobre los cuidados paliativos y la detección precoz de pacientes con necesidad de estos.

Puedes enviarme un correo utilizando el icono de la barra superior. 🇬🇧🇺🇸 **This website is meant to be in Spanish, but you can contact me in English too**

![A picture of me!](/img/about/me.webp)

# Publicaciones

Estas son algunas de las publicaciones con relación directa a mi linea de investigación


* [Design of 1-year mortality forecast at hospital admission: A machine learning approach](https://journals.sagepub.com/doi/full/10.1177/1460458220987580)
* [Responsive and Minimalist App Based on Explainable AI to Assess Palliative Care Needs during Bedside Consultations on Older Patients](https://www.mdpi.com/2071-1050/13/17/9844)
* [(Congreso) Machine Learning-Based Identification of Obesity from Positive and Unlabelled Electronic Health Records](https://ebooks.iospress.nl/volumearticle/54286)


# Publicaciones como colaborador
* [Deep ensemble multitask classification of emergency medical call incidents combining multimodal data improves emergency medical dispatch](https://www.sciencedirect.com/science/article/pii/S0933365721000816)
* [A User-Centered Chatbot (Wakamola) to Collect Linked Data in Population Networks to Support Studies of Overweight and Obesity Causes: Design and Pilot Study](https://medinform.jmir.org/2021/4/e17503)
* [How the Wakamola chatbot studied a university community’s lifestyle during the COVID-19 confinement](https://journals.sagepub.com/doi/full/10.1177/14604582211017944)
* [A user-centered chatbot to identify and interconnect individual, social and environmental risk factors related to overweight and obesity](https://www.tandfonline.com/doi/abs/10.1080/17538157.2021.1923501)
```

Hay títulos con \#, hay negritras rodeando el texto de doble asterisco (\*), hay una imagen utilizando la contrucción ```![texto alternativo](link_imagen)```. Y hay diversion enlaces, creados de manera muy similar a las imágenes ```[Texto del enlace](enlace)```. Las imágenes están alojadas en la carpeta static, donde tengo una subcarpeta llamada **img** y luego, dentro de esta, una subcarpeta para cada publicación. Es mi manera de organizar las cosas, el blog seguiria funcionando aunque estuvieran todas las imágenes directamente en **/static** (solo cambiaría su ruta para incluirlas).

![captura de static](/img/posts/hugo/static.webp)

### Un poco más avanzado

La forma presentada de colocar imágenes es la más basica, aunque creo que serivará al 99% de la gente que quiera crear su blog, ya que con las conexiones modernas a duras penas existen problemas de rendimiento en páginas estáticas. No obstante, en la página oficial de Hugo y como parte de sus tutorial enseñan como procesar y editar las imágenes de manera más eficiente (https://gohugo.io/content-management/image-processing/). Mi única medida a este respecto es transforma las imágenes al formato webp, que es mucho más liviano.

## Servidor local

Podemos poner en marcha el servidor de Hugo para ver nuestra web en local, para ello utilizamos el comando ```hugo server -D```. Mientras el servidor este en marcha, podemos editar nuestro blog y los cambios serán recargados en el momento. Es una herramienta muy buena para ir viendo como van quedando nuestros posts a medidas que los escribimos.

Si utilizamos el comando ```hugo -D``` exportamos los ficheros html a la carpeta **public** de la raíz, es por ello que normalmente esta carpeta suele incluirse en el fichero .gitignore.

## Colocar nuestra página/blog en github pages

Fuente: https://gohugo.io/hosting-and-deployment/hosting-on-github/

Antes de empezar, necesitamos tener una cuenta en github. Si no la tienes, háztela, venga te espero. Ahora necesitamos tener nuestro nustros ficheros en un repositorio. Un detalle muy importante es que, las github pages tienen el siguiente esquema: ```<nombre de usuario>.github.io/<nombre del repositorio>```, es decir, que si mi nombre de usuario es vblanes y mi repositorio con el blog en hugo se llama "blog", la url base sería **vblanes.github.io/blog/**. Si has prestado atención, verás que la url base de la página que estás viendo no tiene el nombre del blog. Esto se consigue cuando el nombre de tu repositorio es ```<nombre de usuario>.github.io``` o en mi caso **vblanes.github.io**, mira, échale un vistazo: [link al repositorio de este blog](https://github.com/vblanes/vblanes.github.io).

Una vez tengamos nuestro código en su repositorio de github, tengamos claro el nombre de este repositorio y por tanto su url base, debemos acordarnos de modificar el parámetro **baseURL** que mencionamos hace algunos apartados. Por último, solo necesitaríamos añadir un fichero a nuestro proyecto, que le dirá a github que con cada push que hagamos, queremos que reconstruya el blog y depsliegue nuestro blog con la última versión.

Para ello debemos crear una carpeta llamada **.github** en la raíz de nuestro proyecto, y asimismo dentro de esta, una carpeta llamada workflows. Dentro de esta última carpeta añadiremos un fichero yaml, cuyo nombre no importa demasiado pero que yo he llamado **gh-pages.yml**, con el siguiente contenido:

```yaml
name: github pages

on:
  push:
    branches:
      - main  # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          # extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.ACTIONS_DEPLOY_KEY }}
          publish_dir: ./public
```

Solo falta un detalle para que esto funciona, y es que debemos tener un token personal para acceder a github. Desde hace unos meses es obligatorio, pero si no lo tienes puedes mirar como conseguir uno en el siguiente enlace: https://docs.github.com/es/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token. Debemos accerder a nuestro repositorio y en **Settings > Secrets** lo añadiremos, en mi caso lo he añadido bajo el nombre **ACTIONS_DEPLOY_KEY** y se utiliza en la penúltima linea del fichero que acabamos de crear.

Con todo esto solo cabe asegurar de que la configuración de las github pages es correcta, esto se puede consultar en **Settings > Pages**. Donde deberíais tener algo similar a:

![captura de las pages](/img/posts/hugo/githubpages.webp)

Ahora solo quedaría utilizar el comando ```hugo -D``` para generar nuestro sitio, y hacer commit y push de nuestros cambios, y se iniciará la acción para el despliegue que en pocos segundos hará que nuestro blog esté visible. **Voilá**.


# Problemas pendientes

A pesar de que poner la parte básica en funcionamiento ha sido relativamente fácil, tengo todavía algunos problemillas que no he sabido como solucionar en Hugo y que dejo aquí a continuación para editarlos cuando encuentre una solución (o si alguien me lee y sabe la respuesta 👀)

1. No sé como poner contenido en dos columnas. En el blog anterior tenía en mi sección de about dos columnas, en una estaba mi fotografía y en la otra mi descripción. En este momento no sé como se puede cambiar esa maquetación, aunque es cuestión de tiempo que lea más y vaya entiendo mejor todo el stack. **Estúpida y sensual web, como te odio**.

2. En Firefox los bloques de código aparecen centrados y mal identidas, al contrario que chromium. Además el esquema de colores para algunos lenguajes es **fatal**. Entiendo que para ello deberé editar los CSS del tema, pero no he tenido tanto tiempo... todavía.

    * EDITADO: Junto al problema original, resulta que los texto de todas las entradas también aparecen centrados en Firefox. Esto se debe a la propiedad align que está en toda la etiqueta **main**, perece que por algún motivo en navegadores tipo chromium se renderiza como left mientras que en Firefox como center. La solución es añadir esta segunda linea de CSS al fichero **main.css** del tema.

    ```css
    .content {
        padding-top: 20px;
        text-align: left;
    }
    ```