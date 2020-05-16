# Demos

A la hora de probar los casos de demo en local podemos hacer uso del paquete [http-server](https://www.npmjs.com/package/http-server) de npm. Podemos instalarlo de forma global en nuestra máquina utilizando npm.

```bash
npm i -g http-server
```

Una vez instalado nos podemos situar sobre la carpeta `demos` y ejecutar:

```bash
http-server .
```

Ahora deberíamos tener un servidor arrancado en [http://localhost:8080/](http://localhost:8080/) que sirve nuestra carpeta `demos`.