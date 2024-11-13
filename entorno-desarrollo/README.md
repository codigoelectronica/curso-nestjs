# Entrono de trabajo para NestJS

Para configurar un entorno de trabajo óptimo para NestJS en Visual Studio Code (VSCode), necesitarás realizar algunos ajustes e instalar extensiones específicas que mejorarán tu experiencia de desarrollo. A continuación, te proporciono una guía paso a paso para configurar tu entorno de trabajo.

## Instalar nodejs
Antes de comenzar, asegúrate de que Node.js esté instalado en tu máquina, ya que NestJS funciona con Node.js. Esto también instalará npm, que es el gestor de paquetes necesario para instalar las dependencias de NestJS.

El siguiente enlace: [Cómo instalar y usar NVM para gestionar versiones de Node.js en Ubuntu y Windows](http://codigoelectronica.com/blog/como-instalar-y-usar-nvm-para-gestionar-versiones-de-node.js-en-ubuntu-y-windows) puede instalar la versión de node según su requerimiento

## Instslar nest CLI
El CLI de NestJS te facilita la creación de proyectos, módulos, controladores, servicios y más. Instálalo globalmente en tu máquina:
´´´bash
npm install -g @nestjs/cli
´´´
Para verificar la instalación
´´´bash
nest --version
´´´

## Instalar Visual Studio Code
Si aún no tienes Visual Studio Code instalado, descárgarlo desde su sitio oficial. [https://code.visualstudio.com/](https://code.visualstudio.com/)

### Extencion escenciales para nestjs visual studio code
* [Material icons](https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme) es una extencion que pemite personalizar los iconos y caretas de los proyectos en visual studio code, esto facilita y ayuda visualmente a encontrar o diferenciar el archivo.
* [Thunder client](https://marketplace.visualstudio.com/items?itemName=rangav.vscode-thunder-client) Es un cliente http que permite realizar dichas peticiones desde el visual studio code
* [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) esta extensión asegura que tu código siga las reglas de estilo y mejores prácticas configuradas en el archivo .eslintrc.js del proyecto. Te ayudará a detectar errores de sintaxis y aplicar un código más limpio y consistente.


### Extenciones para navegador
* Json viwer: lo puede buscar para el navegador que trabaje, esta extencion pemite customizar y mejorar visulmente las estructuras JSON en el navegador web.

## Configurar visual studio

### Configurar Material icons
<first-app>/.vscode vamos a crear el siguiente archivo : settings.json donde agregamos la siguiente linea
```json
{
	"material-icon-theme.activeIconPack": "nest"
}
```

### Configurar eslint
Asegúrate de que tienes un archivo de configuración .eslintrc.js en tu proyecto (ya debería estar por defecto si creaste el proyecto con Nest CLI).
Ingresamos a <first-app>/.eslintrc donde realizamos lo siguiente:

Agregamos al final el siguiente contenido

```javascript
"prettier/prettier": [
    "error", {
      "endOfLine": "auto"
    }
]
```
Esto se hace para que en Windows no genere error los saltos de linea