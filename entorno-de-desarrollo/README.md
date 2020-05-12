# Entorno de desarrollo

Para empezar a desarrollar componentes web se necesitan las siguientes herramientas



## Chrome

Es el navegador que más rápido implementa las APIs relacionadas con los componentes web. Se puede descargar desde la [página oficial](https://www.google.com/intl/es_es/chrome/).



## Node 8+

Es un entorno de ejecución JavaScript que se ejecuta sobre el sistema operativo que tengamos instalado. Se puede descargar desde el [centro de descargas](https://nodejs.org/es/download/).

Otra opción para instalar node, más recomendable, es hacer uso de [nvm](https://github.com/nvm-sh/nvm) para descargar la versión que nos interese. Está herramienta nos permite tener varias instalaciones de node en nuestra máquina y cambiar facilmente entre ellas.

```bash
  # Para instalar una versión de node con nvm
  # Primero pedimos una lista de versiones disponibles
  nvm ls-remote --lts
  # Utilizamos el flag  "--lts" para filtar las versiones más estables de node (Long Term Support)
  # Instalamos la versión preferida
  nvm install 12.16.2
  # Luego para empezar a utilizar esta versión
  nvm use 12.16.2
```

Junto con node se instalará en nuestra máquina [npm](https://www.npmjs.com/), un gestor de paquetes JavaScript. Podemos comprobarlo utilizando el siguiente comando:

```bash
  npm -v
```

Si la instalación ha sido exitosa podemos utilizar npm para descargar el paquete `http-server`. Este paquete se utiliza para lanzar un servidor de archivos estáticos desde cualquier carpeta en nuestra máquina. Lo utilizaremos para servir los archivos HTML, CSS y JS.

```bash
  # Instalamos el paquete de forma global utilizando el flag "-g" para que npm lo
  # registre en nuestro path y podamos lanzarlo como un comando por consola
  npm i -g http-server
```



## Editor de texto

El desarrollo web se puede llevar a cabo prácticamente en cualquier editor de texto pero los más recomendados son:

* [Visual Studio Code](https://code.visualstudio.com/) `gratuito`
* [WebStorm](https://www.jetbrains.com/es-es/webstorm/)
* [Sublime](https://www.sublimetext.com/)
* [Atom](https://atom.io/) `gratuito`

