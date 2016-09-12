# Stap 2 - Een angular 2 applicatie.
Een angular 2 applicatie bestaat uit modules. Dit zijn classes met een `@NgModule` decorator.
Op deze manier kun je functionaliteit groeperen. Een module bijvoorbeeld componenten, service of directives bevatten.

In de systemjs configuratie hebben we aangegeven dat onze applicatie zich bevindt in de directory **app**. Maak deze  directory nu aan.
`app.module.js` is aangewezen als startup bestand. Maak een bestand `app.module.ts` aan in de app folder.

## app.module.ts
```javascript
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

@NgModule({
  imports:      [ BrowserModule ]
})
export class AppModule { }
```

Dit is de kleinste functionele angular 2 module. We gebruiken hier `BrowserModule` omdat we de applicatie in een browser gaan draaien in de module bevindt zich code specifiek 
voor het draaien van een angular 2 app in de browser.

Als we nu `npm run tsc` uitvoeren dan wordt ook de javascript voor de app.module gemaakt. In visual studio code is het erg handig om snel de javascript file te verbergen.
Open met `Ctrl+Shift+P` het command pallete en zoek user settings. Voeg hier het volgende blok aan toe tussen de root {}.

```javascript
"files.exclude": {
        "**/.git": true,
        "**/.svn": true,
        "**/.hg": true,
        "**/.DS_Store": true,
        "**/*.js": {"when": "$(basename).ts"},
        "**/*.js.map": true
    }
  ```