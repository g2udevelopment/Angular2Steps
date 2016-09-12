# Stap 1 - Packages en project setup
De volgende configuratie bestanden zijn nodig bij het maken van een angular 2 project.

- **package.json** defintie van de npm packages en eventueel script die je kunt gebruiken.
- **tsconfig.json** typescript configuratie
- **typings.json** Extra typescript definties, de meeste definities zitten al bij de npm packages
- **systemjs.config.js** We gebruiken nu systemjs en dit is het configuratie bestand, webpack is een andere package manager die veel wordt gebruikt.

Maak nu een directory aan waarin je je project wilt beginnen. Open deze directory vervolgens in visual studio code.

Neem de volgende code over in gelijknamige bestanden.

## package.json

```javascript
{
  "name": "angular2-demo",
  "version": "1.0.0",
  "scripts": {
    "start": "tsc && concurrently \"npm run tsc:w\" \"npm run lite\" ",
    "postinstall": "typings install",
    "tsc": "tsc",
    "tsc:w": "tsc -w",
    "typings": "typings"
  },
  "license": "ISC",
  "dependencies": {
    "@angular/compiler": "2.0.0-rc.6",
    "@angular/compiler-cli": "0.6.0",
    "@angular/core": "2.0.0-rc.6",
    "@angular/forms": "2.0.0-rc.6",
    "@angular/http": "2.0.0-rc.6",
    "@angular/platform-server": "2.0.0-rc.6",
    "@angular/platform-browser": "2.0.0-rc.6",
    "@angular/platform-browser-dynamic": "2.0.0-rc.6",
    "@angular/router": "3.0.0-rc.2",
    "@angular/upgrade": "2.0.0-rc.6",
    "core-js": "^2.4.1",
    "reflect-metadata": "^0.1.3",
    "rxjs": "5.0.0-beta.11",
    "systemjs": "0.19.27",
    "zone.js": "^0.6.17",
    "angular2-in-memory-web-api": "0.0.18",
    "bootstrap": "^3.3.6"
  },
  "devDependencies": {
    "concurrently": "^2.2.0",
    "lite-server": "^2.2.2",
    "typescript": "^2.0.2",
    "typings":"^1.3.2"
  }
}
```

## tsconfig.json

```javascript
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "moduleResolution": "node",
    "sourceMap": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "removeComments": false,
    "noImplicitAny": false
  }
}
```

## typings.json

```javascript
{
  "globalDependencies": {
    "core-js": "registry:dt/core-js#0.0.0+20160725163759",
    "jasmine": "registry:dt/jasmine#2.2.0+20160621224255",
    "node": "registry:dt/node#6.0.0+20160831021119"
  }
}
```

## systemjs.config.js

```javascript
/**
 * System configuration for Angular 2 samples
 * Adjust as necessary for your application needs.
 */
(function (global) {
  System.config({
    paths: {
      // paths serve as alias
      'npm:': 'node_modules/'
    },
    // map tells the System loader where to look for things
    map: {
      // our app is within the app folder
      app: 'app',
      // angular bundles
      '@angular/core': 'npm:@angular/core/bundles/core.umd.js',
      '@angular/common': 'npm:@angular/common/bundles/common.umd.js',
      '@angular/compiler': 'npm:@angular/compiler/bundles/compiler.umd.js',
      '@angular/platform-browser': 'npm:@angular/platform-browser/bundles/platform-browser.umd.js',
      '@angular/platform-browser-dynamic': 'npm:@angular/platform-browser-dynamic/bundles/platform-browser-dynamic.umd.js',
      '@angular/http': 'npm:@angular/http/bundles/http.umd.js',
      '@angular/router': 'npm:@angular/router/bundles/router.umd.js',
      '@angular/forms': 'npm:@angular/forms/bundles/forms.umd.js',
      // other libraries
      'rxjs':                       'npm:rxjs',
      'angular2-in-memory-web-api': 'npm:angular2-in-memory-web-api',
    },
    // packages tells the System loader how to load when no filename and/or no extension
    packages: {
      app: {
        main: './main.js',
        defaultExtension: 'js'
      },
      rxjs: {
        defaultExtension: 'js'
      },
      'angular2-in-memory-web-api': {
        main: './index.js',
        defaultExtension: 'js'
      }
    }
  });
})(this);
```

# Installeren package
Dit is de defintie van een standaard angular 2 project het is handig om deze bestanden ergens klaar te hebben staan als snelle bootstrap.
Voer nu het commando `npm install` uit npm zal nu alle package binnen halen en lokaal opslaan.

# Opdrachten
1. Voeg de volgende packages toe. angular/core(rc6)
2. Definieer een commando lite en voer het commando lite-server uit.
3. Voer commando’s uit om te kijken wat ze doen.
