---
id: getting-started
title: Getting Started
---

Instalar Jest usando [`yarn`](https://yarnpkg.com/en/package/jest):

```bash
yarn add --dev jest
```

Or [`npm`](https://www.npmjs.com/):

```bash
npm install --save-dev jest
```

Nota: En la documentación de Jest se utilizan comandos `yarn`, aunque `npm` también funciona. Puedes comparar los comandos `yarn` y `npm` en la [documentación de yarn, aquí](https://yarnpkg.com/en/docs/migrating-from-npm#toc-cli-commands-comparison).

Comencemos escribiendo una prueba para una función hipotética que sume dos números. Primero, creamos un archivo `sum.js`:

```javascript
function sum(a, b) {
  return a + b;
}
module.exports = sum;
```

Luego, creamos un archivo llamado `sum.test.js`. Esto contendrá nuestra prueba real:

```javascript
const sum = require('./sum');

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```

Añadimos la siguiente sección a nuestro `package.json`:

```json
{
  "scripts": {
    "test": "jest"
  }
}
```

Por útimo, ejecutamos `yarn test` o `npm run test` y Jest nos mostrará el siguiente mensaje:

```bash
PASS  ./sum.test.js
✓ adds 1 + 2 to equal 3 (5ms)
```

**¡Acabas de escribir con éxito tu primera prueba usando Jest!**

Esta prueba usó `expect` y `toBe` para probar que dos valores eran exactamente idénticos. Para aprender sobre las otras cosas que Jest puede probar, ver [Using Matchers](UsingMatchers.md).

## Ejecutando desde línea de comandos

Puedes ejecutar Jest directamente desde el CLI (si está disponible globalmente en su `PATH`, por ejemplo, mediante` yarn global add jest` o `npm install jest --global`) con una variedad de opciones útiles.

A continuación, se explica cómo ejecutar Jest en los archivos que coincidan con `my-test`, usando `config.json` como archivo de configuración y mostrar una notificación nativa del sistema operativo después de la ejecución:

```bash
jest my-test --notify --config=config.json
```

Si deseas obtener más información sobre cómo ejecutar `jest` a través de la línea de comandos, echa un vistazo a la página [Opciones de CLI de Jest] (CLI.md).

## Configuración adicional

### Generar un archivo de configuración básico.

Basándose en tu proyecto, Jest te hará algunas preguntas y creará un archivo de configuración básico con una breve descripción de cada opción:

```bash
jest --init
```

### Utilizando Babel

Para utilizar [Babel](http://babeljs.io/), instalaremos las dependencias necesarias vía `yarn`:

```bash
yarn add --dev babel-jest @babel/core @babel/preset-env
```

Configure Babel para que apunte a tu versión actual de Node creando un archivo `babel.config.js` en la raíz de tu proyecto:

```javascript
// babel.config.js
module.exports = {
  presets: [
    [
      '@babel/preset-env',
      {
        targets: {
          node: 'current',
        },
      },
    ],
  ],
};
```

**La configuración ideal para Babel dependerá de tu proyecto.** Consulte [Documentación de Babel](https://babeljs.io/docs/en/) para obtener más detalles.

Jest establecerá `process.env.NODE_ENV` en` 'test'` si no está configurado en otra opción. Puedes usar esto en tu configuración para configurar condicionalmente solo la compilación necesaria para Jest, por ejemplo.

```javascript
// babel.config.js
module.exports = api => {
  const isTest = api.env('test');
  // You can use isTest to determine what presets and plugins to use.

  return {
    // ...
  };
};
```

> Nota: `babel-jest` se instala automáticamente al instalar Jest y transformará automáticamente los archivos si existe una configuración de babel en tu proyecto. Para evitar este comportamiento, puedes restablecer explícitamente la opción de configuración `transform`:

```javascript
// jest.config.js
module.exports = {
  transform: {},
};
```

#### Babel 6

Jest 24 eliminó el soporte para Babel 6. Te recomendamos encarecidamente que actualices a Babel 7, que se mantiene activamente. Sin embargo, si no puedes actualizar a Babel 7, sige usando Jest 23 o actualiza a Jest 24 con `babel-jest` bloqueado en la versión 23, como en el siguiente ejemplo:

```
"dependencies": {
  "babel-core": "^6.26.3",
  "babel-jest": "^23.6.0",
  "babel-preset-env": "^1.7.0",
  "jest": "^24.0.0"
}
```

Si bien generalmente recomendamos usar la misma versión de cada paquete Jest, esta solución le permitirá continuar usando por ahora la última versión de Jest con Babel 6.

### Usando webpack

Jest se puede usar en proyectos que usan [webpack] (https://webpack.github.io/) para administrar activos, estilos y compilación. El webpack ofrece algunos desafíos únicos sobre otras herramientas. Consulte la [guía del paquete web] (Webpack.md) para comenzar.

### Usando TypeScript

Jest es compatible con TypeScript externamente a través de Babel.

Sin embargo, hay algunas advertencias sobre el uso de Typescript con Babel, consultar http://artsy.github.io/blog/2017/11/27/Babel-7-and-TypeScript/. Otra advertencia es que Jest no revisará las pruebas. Si lo desea, puede usar [ts-jest](https://github.com/kulshekhar/ts-jest).
