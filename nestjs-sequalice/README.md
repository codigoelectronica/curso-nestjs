# NestJs sequalice 

[https://docs.nestjs.com/recipes/sql-sequelize](SQL (Sequelize))

https://github.com/kentloog/nestjs-sequelize-typescript

Sequelize es un Mapeador Relacional de Objetos (ORM) popular escrito en JavaScript básico, pero existe un contenedor TypeScript sequelize-typescript que proporciona un conjunto de decoradores y otros extras para el sequelize base.

Vamos a crear una api para buscar y consultar los paices e información basica de estos

## Crear el proyecto

Creamos el proyecto

```bash
nest new countries
```

Procedemos a instalar las dependencias necesarias

## Instala las dependencias necesarias
Ejecuta los siguientes comandos en tu proyecto de NestJS para instalar @nestjs/sequelize, sequelize, sequelize-typescript y el cliente de PostgreSQL.

```bash
npm install @nestjs/sequelize sequelize sequelize-typescript pg pg-hstore
```

Adicionalmente, instalaremos el siguiente paquete

```bash
npm install --save-dev @types/sequelize
```
```bash
npm install --save-dev sequelize-cli
```

## configurar sequalice

npx sequelize-cli init