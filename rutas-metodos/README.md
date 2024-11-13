# Rutas y métodos HTTP
En NestJS, las rutas y métodos HTTP son esenciales para definir cómo los clientes interactúan con el servidor. NestJS se basa en decoradores que facilitan la declaración de estas rutas y métodos HTTP dentro de controladores.

## Métodos HTTP en NestJS
NestJS utiliza los métodos HTTP comunes como:

* __GET__: Para obtener datos.
* __POST__: Para crear nuevos recursos.
* __PUT__: Para actualizar completamente un recurso.
* __PATCH__: Para actualizar parcialmente un recurso.
* __DELETE__: Para eliminar un recurso.
Estos métodos se definen con decoradores dentro de los controladores:

## Decoradores de Métodos HTTP
* __@Get()__: Define una ruta que maneja solicitudes HTTP GET.
* __@Post()__: Define una ruta que maneja solicitudes HTTP POST.
* __@Put()__: Define una ruta que maneja solicitudes HTTP PUT.
* __@Patch()__: Define una ruta que maneja solicitudes HTTP PATCH.
* __@Delete()__: Define una ruta que maneja solicitudes HTTP DELETE.
Cada uno de estos decoradores puede opcionalmente aceptar un parámetro de ruta para especificar la URL asociada.

## Ejemplo de controlador con todos los métodos HTTP
Los controladores son las clases que gestionan las solicitudes HTTP entrantes y devuelven la respuesta correspondiente al cliente. Cada ruta y método HTTP se define en los controladores utilizando decoradores específicos como @Get(), @Post(), @Put(), entre otros.
Crearemos un controlador de productos en donde crearemos los distintos metodos http.

El siguiente es un ejercicio realizado en `user.controller.ts` donde se crearan las distintas peticiones http y se realozara la explicación:

Para usar las peticiones, devemos importatlas, las realizamos con la siguiete linea
```typescript
import { Controller, Get, Post, Put, Delete } from '@nestjs/common';
```
Nota: todas las respuestas que daremos a continuacion son en texto plano

### Peticion get
`@Get` Define una ruta que maneja solicitudes HTTP GET.

```typescript
@Get()
getAllUser() {
    return 'Obtener todos los usuarios';
}
```

### Peticion post
@Post(): Define una ruta que maneja solicitudes HTTP POST.
```typescript
@Post()
createUser() {
    return 'Crear un nuevo usuario';
}
```
### Peticion put
@Put(): Define una ruta que maneja solicitudes HTTP PUT.
```typescript
 @Put()
  updateUser() {
    return 'Actualizar un usuario';
  }
```
### Peticion Patch
@Patch(): Define una ruta que maneja solicitudes HTTP PATCH.
```typescript
 @Patch()
  partialUpdateProduct() {
    return 'Actualizar parcialmente un producto';
  }
```
### Peticion delete
@Delete(): Define una ruta que maneja solicitudes HTTP DELETE.
```typescript
@Delete()
  deleteUser() {
    return 'Eliminar un usuario';
  }
```

## Peticiones http

### Parámetros en las Rutas
NestJS permite definir rutas dinámicas usando parámetros de ruta. Los parámetros de ruta se definen en la URL con el símbolo : y se extraen dentro del método del controlador con el decorador @Param().
```typescript
@Controller('orders')
export class OrdersController {
  @Get(':id')
  getOrderById(@Param('id') id: string) {
    return `Obtener la orden con ID: ${id}`;
  }
}
```
Aquí, la ruta GET /orders/:id extrae el valor de id y lo pasa como argumento al método getOrderById.

### Cuerpos de Solicitud (Request Body)
Cuando manejas solicitudes POST o PUT que envían datos en el cuerpo (como crear o actualizar recursos), puedes usar el decorador @Body() para acceder a esos datos.

```typescript
@Controller('users')
export class UsersController {
  @Post()
  createUser(@Body() body: any) {
    return `Usuario creado: ${JSON.stringify(body)}`;
  }

  @Put(':id')
  updateUser(@Param('id') id: string, @Body() body: any) {
    return `Usuario con ID ${id} actualizado con: ${JSON.stringify(body)}`;
  }
}
```
### Query Parameters
Los query parameters son parámetros que se envían en la URL después del símbolo ?. Para extraerlos en NestJS, se utiliza el decorador @Query().

```typescript
@Controller('search')
export class SearchController {
  @Get()
  search(@Query('term') term: string) {
    return `Buscando por: ${term}`;
  }
}
```
Si realizas una solicitud a GET /search?term=nestjs, el valor de term será extraído por el método search.

## Respuesta peticiones http
En NestJS, un controlador puede devolver distintos tipos de respuestas dependiendo de la naturaleza de la solicitud y del tipo de dato que se espera. Las respuestas pueden ser simples como cadenas de texto o más complejas como objetos JSON, archivos, o respuestas con estados HTTP personalizados.

A continuación te muestro algunos de los tipos de respuesta más comunes que un controlador puede devolver en NestJS, junto con ejemplos.

### Respuesta de Texto Plano
Puedes devolver una cadena de texto directamente como respuesta.

Ejemplo:
```typescript
@Controller('text')
export class TextController {
  @Get()
  getText(): string {
    return 'Esta es una respuesta de texto plano';
  }
}
```
Cuando haces una solicitud GET a /text, el servidor responderá con el texto "Esta es una respuesta de texto plano".

###  Respuesta JSON (Objeto o Array)
NestJS automáticamente serializa los objetos y arrays en formato JSON cuando se devuelven desde un controlador.

Ejemplo:
```typescript
@Controller('json')
export class JsonController {
  @Get()
  getJson(): any {
    return { mensaje: 'Respuesta en formato JSON', status: 'ok' };
  }
}
```
La respuesta será:


```json
{
  "mensaje": "Respuesta en formato JSON",
  "status": "ok"
}
```
###  Respuesta con Estado HTTP Personalizado
Puedes usar el decorador @HttpCode() para devolver un estado HTTP específico. Por defecto, las respuestas GET devuelven el estado 200 OK, pero puedes modificarlo si es necesario.

Ejemplo:
```typescript
@Controller('status')
export class StatusController {
  @Post()
  @HttpCode(201)
  create(): string {
    return 'Recurso creado';
  }
}
```
Aquí, el método POST /status devolverá un estado HTTP 201 Created.

###  Respuesta de Redirección
Si deseas redirigir una solicitud a otra URL, puedes usar el decorador @Redirect().

Ejemplo:
```typescript
@Controller('redirect')
export class RedirectController {
  @Get()
  @Redirect('https://nestjs.com', 302)
  redirectToNestJs() {
    // Puedes realizar lógica adicional antes de redirigir si es necesario
  }
}
```
Este controlador redirigirá la solicitud a la página oficial de NestJS con un estado 302 Found.

###  Respuesta con Encabezados HTTP Personalizados
Puedes modificar los encabezados de respuesta usando el objeto @Res() o @Header().

Ejemplo usando @Res():
```typescript
import { Controller, Get, Res } from '@nestjs/common';
import { Response } from 'express';

@Controller('headers')
export class HeadersController {
  @Get()
  setCustomHeaders(@Res() res: Response) {
    res.set('X-Custom-Header', 'MiEncabezadoPersonalizado');
    res.status(200).send('Encabezados personalizados configurados');
  }
}
```
Este ejemplo muestra cómo establecer un encabezado HTTP personalizado en la respuesta.

Ejemplo usando @Header():
```typescript
@Controller('header')
export class HeaderController {
  @Get()
  @Header('X-Powered-By', 'NestJS')
  getWithHeader() {
    return 'Encabezado añadido a la respuesta';
  }
}
```
Esto añade el encabezado X-Powered-By: NestJS a la respuesta.

###  Envío de Archivos
Puedes devolver un archivo como respuesta utilizando el objeto @Res() de express o @SendFile() si estás trabajando con NestJS.

Ejemplo:
```typescript
import { Controller, Get, Res } from '@nestjs/common';
import { Response } from 'express';
import * as path from 'path';

@Controller('file')
export class FileController {
  @Get()
  sendFile(@Res() res: Response) {
    const filePath = path.join(__dirname, '..', 'assets', 'example.pdf');
    res.sendFile(filePath);
  }
}
```
Este controlador envía un archivo PDF como respuesta a una solicitud GET.

###  Stream de Archivos
En casos donde quieres enviar un stream de datos (por ejemplo, cuando descargas un archivo de gran tamaño), puedes usar un stream.

Ejemplo:
```typescript
import { Controller, Get, Res } from '@nestjs/common';
import { createReadStream } from 'fs';
import { Response } from 'express';
import * as path from 'path';

@Controller('stream')
export class StreamController {
  @Get()
  streamFile(@Res() res: Response) {
    const filePath = path.join(__dirname, '..', 'assets', 'example.pdf');
    const fileStream = createReadStream(filePath);
    fileStream.pipe(res);
  }
}
```
Este ejemplo envía un archivo como stream de datos, útil cuando se manejan archivos de gran tamaño.

###  Manejo de Errores (Respuesta con Código de Error)
Puedes devolver respuestas con estados de error, como 404 Not Found, 500 Internal Server Error, entre otros, utilizando @HttpException.

Ejemplo:
```typescript
import { Controller, Get, Param, NotFoundException } from '@nestjs/common';

@Controller('error')
export class ErrorController {
  @Get(':id')
  getItem(@Param('id') id: string) {
    if (id !== '1') {
      throw new NotFoundException('Item no encontrado');
    }
    return 'Item encontrado';
  }
}
```
Si la solicitud se realiza con un id distinto a 1, el servidor devolverá una respuesta de error 404 Not Found.

###  Respuestas Vacías
Si no necesitas devolver ningún dato en la respuesta (por ejemplo, cuando simplemente confirmas una acción sin cuerpo de respuesta), puedes devolver una respuesta vacía con un estado HTTP específico.

Ejemplo:

```typescript
import { Controller, Delete, HttpCode } from '@nestjs/common';

@Controller('empty')
export class EmptyController {
  @Delete(':id')
  @HttpCode(204) // Estado "No Content"
  deleteItem() {
    // No devuelve contenido
  }
}
```
Este controlador devuelve el código 204 No Content, lo cual indica que la operación fue exitosa pero no hay contenido en la respuesta.