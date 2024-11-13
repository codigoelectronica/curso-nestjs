# Fundamentos de NestJS
Antes de iniciar con este contenido recomiento revisar lo siginete:
* __Instalacion entorno__: Istalaremos NodeJS y Visual Studio para trabajar con NestJS.
* __Instslacion de NestJS__: Realizamos la instalacion y creacion del primer proyecto.

## Estructura de un proyecto NestJS
Una vez que el proyecto esté creado, se generará la siguiente estructura básica la cual vamos a explicar:

### src
Contiene el código fuente de la aplicación, internamente crea la siguientes estructura de archivos.
```
src/
 ├── app.controller.ts
 ├── app.module.ts
 ├── app.service.ts
 └── main.ts
```
Donde: 
* __app.controller.ts__: Este archivo contiene el controlador principal de la aplicación. Los controladores son responsables de gestionar las solicitudes HTTP entrantes, determinar qué servicios deben llamarse y qué respuesta se debe devolver.
* __app.service.ts__: Contiene la lógica de negocio de la aplicación. Los servicios son responsables de realizar las operaciones más complejas, como acceder a bases de datos, realizar cálculos o interactuar con otros sistemas. No manejan directamente las solicitudes HTTP, sino que proveen funcionalidad a los controladores que los usan.
* __app.module.ts__: Este es el módulo raíz de la aplicación. Un módulo en NestJS es una clase que agrupa los controladores, servicios y otros módulos de la aplicación. En AppModule, importamos y configuramos otros módulos que forman parte de la aplicación.
* __main.ts__: Es el punto de entrada principal de la aplicación NestJS. Aquí se inicializa la aplicación y se configuran los parámetros iniciales, como la habilitación de CORS, el uso de pipes, interceptores, y más.


### Archivos de configuracion

#### eslintrc.js
Archivo de configuración para ESLint, una herramienta de linting que se utiliza para analizar y encontrar problemas en el código JavaScript o TypeScript. ESLint ayuda a mantener un estilo de código consistente y detectar errores potenciales o malas prácticas en tu proyecto.

#### nest-cli.json
Este archivo es la configuración del CLI de NestJS. Define cómo el CLI de NestJS debe interactuar con tu proyecto, y contiene configuraciones específicas para la estructura del proyecto.

Algunas propiedades importantes son:

* __entryFile__: Especifica el archivo de entrada principal, normalmente main.ts.
* __sourceRoot__: Indica la carpeta principal del código fuente, que generalmente es src.
* __compilerOptions__: Configura opciones para la compilación, como si quieres eliminar comentarios o el formato de salida del compilador.

#### node_modules
Esta carpeta contiene todos los módulos y dependencias que instalas con npm o yarn. Aquí se almacenan las bibliotecas que tu proyecto necesita para funcionar, incluyendo NestJS y cualquier otra dependencia que hayas especificado en el archivo package.json.

#### package.json

Este archivo contiene la información y la configuración del proyecto. Incluye el nombre, versión, dependencias, scripts de ejecución y más.

Algunas propiedades importantes:
* __name__: El nombre del proyecto.
* __version__: La versión actual del proyecto.
* __dependencies__: Las dependencias que tu aplicación necesita para funcionar (como @nestjs/core, @nestjs/common, etc.).
* __devDependencies__: Las dependencias necesarias solo en el entorno de desarrollo (como herramientas de testing o linters).
* __scripts__: Comandos que puedes ejecutar desde la terminal, como npm run start.

#### package-lock.json
Este archivo asegura que todos los desarrolladores y entornos de tu proyecto utilicen las mismas versiones exactas de las dependencias instaladas. Es un archivo generado automáticamente por npm cuando instalas paquetes y sirve para hacer el proyecto más predecible.

Su función es bloquear las versiones exactas de las dependencias instaladas, lo que garantiza que las mismas versiones se utilicen en diferentes instalaciones.

#### README.md
Este archivo está escrito en formato Markdown y contiene información importante sobre el proyecto.

#### tsconfig.json
Este archivo contiene la configuración principal de TypeScript para tu proyecto. Define las reglas sobre cómo se debe compilar el código TypeScript.

Algunas propiedades importantes:
* __compilerOptions__: Define opciones de compilación, como la versión de ECMAScript a usar, los módulos, y si debe generar mapas de origen (source maps).
* __include__ y __exclude__: Define qué archivos se deben incluir o excluir del proceso de compilación.

#### prettierrc
El archivo de configuración para Prettier, una herramienta de formateo de código. Prettier se encarga de darle un formato consistente al código, asegurando que siga un estilo predefinido, lo que facilita la lectura y el mantenimiento. A diferencia de ESLint, que verifica reglas de calidad de código y buenas prácticas, Prettier se enfoca únicamente en el formato (espacios, comas, llaves, etc.).


## Primer modulo, controlador y servicio.
A continuación, realizaremos un ejercicio en donde crearemos un modulo, controlador y servicios y explicaremos como se relacionan entre sí.

### Módulos: Creación y organización
Un módulo en NestJS es una clase que agrupa los controladores, servicios y otros módulos de la aplicación.
Para crear un modulo podemos usar la terminal de comandos y la cli de nest, veamos un ejemplo:
```bash
nest generate module blog
```
Esto creara la siguiente estructura:
```bash
C:\FIRST-APP\SRC
│   app.controller.spec.ts
│   app.controller.ts
│   app.module.ts
│   app.service.ts
│   main.ts
│
└───blog
        blog.module.ts
```
Creara la carpera blog y dentro el modulo blog.module.ts

El archivo blog.module.ts contiene la clase BlogModule con la que ya podemos trabajar
```typescript
import { Module } from '@nestjs/common';

@Module({})
export class BlogModule {}
```

Ahora al abrir app.module.ts nos vamos a encontrar con lo siguinete:
```typescript
...
import { BlogModule } from './blog/blog.module';

@Module({
  imports: [BlogModule],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```
Agrega el modulo blog.module.ts automativamente a los imports para que ya podamos usar el modulo blog

### Controladores: Creación y organización
Los controladores son responsables de gestionar las solicitudes HTTP entrantes, determinar qué servicios deben llamarse y qué respuesta se debe devolver.
Para crear un controlador podemos usar la terminal de comandos y la CLI de nest, veamos un ejemplo:
```bash
nest generate controller blog
```
```bash
C:\FIRST-APP\SRC
| .....
│
└───blog
        blog.controller.spec.ts
        blog.controller.ts
        blog.module.ts
```
Creara la carpera blog y dentro el controlador blog.controller.ts con su archivo de prueba.

El archivo blog.controller.ts contiene la clase BlogController con la que ya podemos trabajar a crear las peticiones http.
```typescript
import { Controller } from '@nestjs/common';

@Controller('blog')
export class BlogController {}

```
Ahora al abrir blog.module.ts nos vamos a encontrar con lo siguinete:
```typescript
...
import { Module } from '@nestjs/common';
import { BlogController } from './blog.controller';

@Module({
  controllers: [BlogController]
})
export class BlogModule {}

```
Agrega el modulo blog.controller.ts automaticamente a los controllers para que ya podamos usar el controlador blog

### Servicios: Creación y organización
Los servicios son responsables de realizar las operaciones más complejas, como acceder a bases de datos, realizar cálculos o interactuar con otros sistemas. No manejan directamente las solicitudes HTTP, sino que proveen funcionalidad a los controladores que los usan.

Para crear un servivio podemos usar la terminal de comandos y la CLI de nest, veamos un ejemplo:
```bash
nest generate service blog
```
```bash
C:\FIRST-APP\SRC
| .....
│
└───blog
        .......
        blog.service.spec.ts
        blog.service.ts
```
Creara la carpera blog y dentro el servicio blog.service.ts con su archivo de prueba.

El archivo blog.service.ts contiene la clase BlogService con la que ya podemos trabajar.
```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class BlogService {}
```
Ahora al abrir blog.module.ts nos vamos a encontrar con lo siguinete:
```typescript
.....
import { BlogService } from './blog.service';

@Module({
  controllers: [BlogController]
  providers: [BlogService]
})
export class BlogModule {}

```
Agrega el modulo blog.service.ts automaticamente a los providers para que ya podamos usar el controlador blog

Por ultimo para usar los service lo inyectamos en el contolador de la siguente forma
```typescript
import { BlogService } from './blog.service';

@Controller('blog')
export class BlogController {

    constructor(private blogService: BlogService) {   
    }
}
```