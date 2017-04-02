# Stap 3 - Een angular 2 component.
Componenten zijn de bouwstenen van een angular 2 applicatie. Ze zijn een combinatie van logica en presentatie. Standaard zijn alle properties van een component
beschikbaar binnen het template. Elke applicatie heeft minstens 1 component, nameljk het *rootcomponent*.

Maak nu een component aan binnen de app directory `app.component.ts`

## app.component.ts
```javascript
import { Component } from '@angular/core';
@Component({
  selector: 'my-app',
  template: '<h1>My First Angular App</h1>'
})
export class AppComponent { }
```

Nu hebben we het eerste component gemaakt. We moeten deze nu nog toevoegen aan de module.

## Opdracht
- Importeer eerst het component in de module. *elke file is een module je kunt die importeren met ./app.component.ts*
- Voeg het AppComponent toe als declarations property.*array*
- Voeg het AppComponent toe als bootstrap property. *array* 

## Tips
- [Modules Import/Exports](https://www.typescriptlang.org/docs/handbook/modules.html)
- [NgModule](https://angular.io/docs/ts/latest/guide/ngmodule.html#!#angular-modularity)