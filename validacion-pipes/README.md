# Validación y Pipes

## Uso de DTOs (Data Transfer Objects).
En NestJS, los DTOs (Data Transfer Objects) se usan para definir la estructura de datos que se enviará y recibirá en las solicitudes HTTP. Los DTOs ayudan a asegurar que los datos cumplan con ciertas reglas antes de procesarlos en los controladores y servicios, y suelen implementarse como clases de TypeScript.

### Crear un DTO Básico

El siguiete ejemplo se crea una clase DTO basica para un usuario

```typescript
// src/users/dto/create-user.dto.ts
export class CreateUserDto {
  username: string;
  email: string;
  password: string;
}
```

Para usarla en el controlador 
```typescript
// src/users/users.controller.ts
import { Controller, Post, Body } from '@nestjs/common';
import { CreateUserDto } from './dto/create-user.dto';

@Controller('users')
export class UsersController {

  @Post()
  async createUser(@Body() createUserDto: CreateUserDto) {
    return `Usuario creado: ${JSON.stringify(createUserDto)}`;
  }
}
```

## Validación de datos con class-validator y class-transformer.
Un DTO en NestJS generalmente se define como una clase, utilizando decoradores de class-validator y class-transformer para validar y transformar datos.

Ejemplo de DTO:
Imaginemos que tienes una entidad User con propiedades como username, email, y password. Puedes crear un DTO llamado CreateUserDto que defina las propiedades y reglas de validación para los datos de entrada al crear un usuario.

Instala las dependencias necesarias:

```bash
npm install class-validator class-transformer
```
Luego, crea el archivo del DTO:
```typescript
// src/users/dto/create-user.dto.ts
import { IsString, IsEmail, IsNotEmpty, MinLength } from 'class-validator';

export class CreateUserDto {
  @IsString()
  @IsNotEmpty()
  username: string;

  @IsEmail()
  email: string;

  @IsString()
  @MinLength(6)
  password: string;
}
```

En este ejemplo:

* __@IsString()__ valida que el valor sea una cadena.
* __@IsNotEmpty()__ asegura que el campo no esté vacío.
* __@IsEmail()__ valida que el campo contenga una dirección de correo electrónico válida.
* __@MinLength(6)__ especifica que password debe tener al menos 6 caracteres.

Para aplicar el DTO en un controlador, se usa en el parámetro de un método de controlador junto con el decorador @Body(). Esto permite que NestJS valide automáticamente los datos de la solicitud.

```typescript
// src/users/users.controller.ts
import { Controller, Post, Body } from '@nestjs/common';
import { CreateUserDto } from './dto/create-user.dto';
import { UsersService } from './users.service';

@Controller('users')
@UsePipes(new ValidationPipe())
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Post()
  async createUser(@Body() createUserDto: CreateUserDto) {
    return this.usersService.createUser(createUserDto);
  }
}
```

#### Activar la Validación Global en la Aplicación
Para que la validación funcione correctamente en toda la aplicación, activa la validación global en el archivo principal main.ts:

```typescript
// src/main.ts
import { ValidationPipe } from '@nestjs/common';
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // Activar la validación global
  app.useGlobalPipes(new ValidationPipe({
    whitelist: true,
  }));
  
  await app.listen(3000);
}
bootstrap();
```

En este caso, @Body() indica que el CreateUserDto se llenará con el contenido del cuerpo de la solicitud y validará los datos automáticamente. Si los datos no cumplen con las reglas definidas en el DTO, NestJS lanzará un error de validación.


## Pipes: Transformación y validación de datos en rutas.

En NestJS, los Pipes son funciones que se ejecutan antes de que el controlador procese una solicitud. Su función principal es transformar y validar los datos de entrada, asegurando que los controladores solo reciban datos que cumplen con los requisitos especificados. Los pipes permiten, por ejemplo, verificar que los datos enviados por el cliente tengan el formato correcto o convertir valores de texto a números cuando se necesite.

### Funcionalidad de los Pipes
* __Transformación__: Modifican los datos de la solicitud antes de que lleguen al controlador. Por ejemplo, un pipe puede convertir un valor de tipo string en un entero.
* __Validación__: Verifican que los datos cumplen ciertos criterios y lanzan una excepción en caso de que no se cumplan. Si los datos son inválidos, el pipe impide que la solicitud llegue al controlador y retorna un error.

### Tipos de Pipes en NestJS
NestJS incluye algunos pipes ya definidos, y también permite crear pipes personalizados según las necesidades de la aplicación.

Ejemplos de Pipes Integrados en NestJS
* __ParseIntPipe__: Convierte un valor string a entero. Útil cuando los parámetros de URL deben ser enteros.
* __ValidationPipe__: Valida los datos de entrada usando decorators como @IsString(), @IsInt(), entre otros, que se definen en los DTOs.

#### Ejemplo 
Este pipe convierte el parámetro id a un número entero. Si el valor no es un número, lanzará una excepción automáticamente.
```typescript
import { Controller, Get, Param, ParseIntPipe } from '@nestjs/common';

@Controller('users')
export class UsersController {
  @Get(':id')
  getUserById(@Param('id', ParseIntPipe) id: number) {
    return { message: `Usuario con ID ${id}` };
  }
}
```

### Uso de ValidationPipe con DTOs
El ValidationPipe se usa junto con los Data Transfer Objects (DTOs) para validar que los datos recibidos cumplen con ciertas reglas. En este caso, se requiere que el name sea una cadena y que age sea un entero positivo.

Define un DTO para validar los datos:

```typescript
// src/users/dto/create-user.dto.ts
import { IsString, IsInt, Min } from 'class-validator';

export class CreateUserDto {
  @IsString()
  name: string;

  @IsInt()
  @Min(1)
  age: number;
}
```
Configura el controlador para usar ValidationPipe:

```typescript
import { Controller, Post, Body, UsePipes, ValidationPipe } from '@nestjs/common';
import { CreateUserDto } from './dto/create-user.dto';

@Controller('users')
export class UsersController {
  @Post()
  @UsePipes(new ValidationPipe())
  createUser(@Body() createUserDto: CreateUserDto) {
    return {
      message: 'Usuario creado',
      user: createUserDto,
    };
  }
}
```
En este ejemplo:

ValidationPipe valida automáticamente los datos del Body usando las reglas definidas en el DTO.
Si algún campo no cumple con los requisitos (name no es un string o age no es un número positivo), NestJS devolverá un error de validación sin llegar al controlador.

### Creación de un Pipe Personalizado
Si necesitas una transformación o validación específica, puedes crear tu propio pipe:

```typescript
import { PipeTransform, Injectable, BadRequestException } from '@nestjs/common';

@Injectable()
export class UppercasePipe implements PipeTransform {
  transform(value: any) {
    if (typeof value !== 'string') {
      throw new BadRequestException('El valor debe ser un string');
    }
    return value.toUpperCase(); // Convierte el valor a mayúsculas
  }
}
```
Para utilizarlo:

```typescript
import { Controller, Post, Body } from '@nestjs/common';
import { UppercasePipe } from './pipes/uppercase.pipe';

@Controller('items')
export class ItemsController {
  @Post()
  createItem(@Body('name', UppercasePipe) name: string) {
    return { name };
  }
}
```