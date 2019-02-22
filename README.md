# Como instalar nightwatch y cucumber javascript

_Este tutorial fue probado en macOS, cualquier aporte para mejorar se agradece_


### Pre-requisitos 📋

_Necesitas tener instalado la ultima version de Node_

```
https://nodejs.org/es/
```

### Instalación 🔧

_Crear una carpeta vacia y agrega el archivo package.json con solo {} dentro_
_Luego por consola instala cucumber_
```
npm install cucumber --save-dev
```
_Abrir Package.json y modificar por este codigo_
```
{
  "name": "hellocucumber",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "cucumber-js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "cucumber": "^5.0.3" // aqui dejar la ultima version instalada manualmente
 }
  
```
_Crea un archivo llamado cucumber.js y escribe esto dentro_
```
module.exports = {
  default: `--format-options '{"snippetInterface": "synchronous"}'`
}
```

_luego se agrega en la raiz las carpetas features y step-defitions, y en la carpeta step-definitions crea un archivo llamado steps.js y agregale esto_

```
const assert = require('assert');
const { Given, When, Then } = require('cucumber');
```
_Verifique la instalacion escribiendo en consola con npm test, debe devolver:_ 

```
0 Scenarios
0 steps
0m00.000s
```
_Luego crea un archivo en la carpeta features llamado prueba.feature y le agregamos esto_ 
```
Feature: prueba
  
  Scenario: Estamos probando la instalacion
    Given Hoy es Lunes
    When Y ayer fue domingo

```

_y el en steps.js le agregamos esto:_

```
 Given('Hoy es Lunes', function () {
          this.hoy = 'Lunes' 
         });
   When('Y ayer fue domingo', function () {
           this.ayer = 'Domingo';
         });
```
_Luego corremos los test con npm test y van a pasar todos los escenarios._ 
_Ahora vamos a instalar Nigthwatch para trabajarlo con Cucumber_

```
npm install --save-dev nightwatch-api nightwatch cucumber chromedriver cucumber-pretty
```
_Creamos un archivo nightwatch.conf.js y le agregamos todo esto_
```
const { setDefaultTimeout, AfterAll, BeforeAll } = require('cucumber');
const { createSession, closeSession, startWebDriver, stopWebDriver } = require('nightwatch-api');

setDefaultTimeout(60000);

BeforeAll(async () => {
  await startWebDriver();
  await createSession();
});

AfterAll(async () => {
  await closeSession();
  await stopWebDriver();
});
```
_Luego modificamos nuestro prueba.feature y le agregamos esto_
```
Scenario: Searching Google

  Given I open Google's search page
  Then the title is "Google"
  And the Google search form exists
```

_Tambien modificamos el steps.js_ 
```
const { client } = require('nightwatch-api');
const { Given, Then, When } = require('cucumber');

Given(/^I open Google's search page$/, () => {
  return client.url('http://google.com').waitForElementVisible('body', 1000);
});

Then(/^the title is "([^"]*)"$/, title => {
  return client.assert.title(title);
});

Then(/^the Google search form exists$/, () => {
  return client.assert.visible('input[name="q"]');
});
```

_Y agregamos el script a package.json para poder correrlo_

```
"scripts": {
    "e2e-test": "cucumber-js --require cucumber-setup.js --require step-definitions --format node_modules/cucumber-pretty",
    ...
  }
```
_y hacemos npm run e2e-test y hemos corrido exitosamente cucumber con nightwatch._ 




## Construido con 🛠️

_Menciona las herramientas que utilizaste para crear tu proyecto_

* [Cucumber](https://docs.cucumber.io/guides/10-minute-tutorial/) - Cucumber
* [Nightwatch](https://nightwatch-api.netlify.com/cucumber/) - Nightwatch

## Autores ✒️

* **Andrea Guevara** - 

## Expresiones de Gratitud 🎁

La idea de este repo es poder a ayudar a quienes no saben como unificar los dos paquetes para los test de automatizacion.
Saludos
# 
