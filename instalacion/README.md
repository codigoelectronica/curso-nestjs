# Instalación de NestJS

## Entrono de trabajo para NestJS
Recomendamos revisar el post creado en Capítulo 2: Entrono de trabajo para NestJS para que podamos continuar con NestJS.

## Instalación de NestJS (CLI y proyecto base)

Para comenzar con NestJS, necesitas tener instalado Node.js. Luego, instalarás el CLI de NestJS, que te ayudará a generar proyectos y componentes de forma sencilla.

Primer verificaremos si tenemos instalado nodejs con el siguiente comando.

```bash
node -v
```
El CLI de NestJS te facilita la creación de proyectos, módulos, controladores, servicios y más. Instálalo globalmente en tu máquina:
```bash
npm install -g @nestjs/cli
```
Finalizada la instalación, verificamos con el comando nest si quedo todo correcto:
```bash
nest
```
El comando anterior mostrará los comandos que podemos usar con nest.

## Crear un primer proyecto de nest
Para crear el proyecto usaremos el siguiente comando.

```bash
nest new firstApp
```
El CLI te preguntará qué administrador de paquetes quieres usar (npm o yarn). Elige el que prefieras.

## Ejecutar la aplicación
Para iniciar la aplicación, usa el siguiente comando en la carpeta del proyecto:

```bash
npm run start
```

Tu aplicación se ejecutará en el puerto 3000 por defecto. Abre un navegador y navega a [http://localhost:3000](http://localhost:3000) para ver la aplicación en funcionamiento.

Otro comando que usaremos a en desarrollo es:
```bash
npm run start:dev
```
Inicia nestjs en modo desarrollo o escucha


Ejecucion de lint
```bash
npm run lint    
```
