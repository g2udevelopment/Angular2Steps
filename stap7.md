# Stap 7 - Hero Editor
We gaan nu beginnen met een 'grotere' applicatie, het is handig als je `npm start` op de achtergrond laat draaien zodat je telkens je wijzigingen in de browser kunt zien.
Zo kun je na elke stap even kunt kijken wat het resultaat is.

We passen eerst het AppComponent aan om onze applicatie een naam te geven en onze eerste held te maken.

Pas de code in AppComponent als volgt aan.

## app.component.ts
```javascript
export class AppComponent {
  title = 'Tour of Heroes';
  hero = 'Windstorm';
}
```

Pas nu het template aan
## app.component.ts
```javascript
template: '<h1>{{title}}</h1><h2>{{hero}} details!</h2>'
```

Onze held is nu nog een string het wordt tijd om hier een echte class van te maken. Plaats de volgende code
in de AppComponent file als losse class.

## app.component.ts
```javascript
export class Hero {
  id: number;
  name: string;
}
```

Nu we een hero class hebben kunnen we dit ook refactoren in onze code. update de hero property als volgt.

## app.component.ts
```javascript
hero: Hero = {
  id: 1,
  name: 'Windstorm'
};
```

Nu moeten we ook het template aanpassen om gebruik te maken van de nieuwe properties. Omdat we graag alle properties zien van onze
held en om een beetje het overzicht te behouden maken we gebruik van de multi-line strings van typescript.

## app.component.ts

````html
template: `<h1>{{title}}</h1>
  <h2>{{hero.name}} details!</h2>
  <div><label>id: </label>{{hero.id}}</div>
  <div><label>name: </label>{{hero.name}}</div>`
````

We willen graag de details van een hero kunnen bewerken we veranderen daarom het template als volgt.

## app.component.ts
````html
  template:`<h1>{{title}}</h1>
  <h2>{{hero.name}} details!</h2>
  <div><label>id: </label>{{hero.id}}</div>
  <div>
    <label>name: </label>
    <input value="{{hero.name}}" placeholder="name">
  </div>`
````

We kunnen nu de naam wijzigen, echter als we de naam aanpassen dan zien we dit niet terug in de naam bovenaan de pagina.
Dit komt omdat de standaard binding in angular 2 een one-way binding is. We gaan hier nu een two-binding van maken.
In angular 2 zit `ng-model`in een aparte module en we zullen deze eerst moeten laden. We passen hiervoor
`app.module.ts` aan.

## app.module.ts
```javascript
    import { NgModule }      from '@angular/core';
    import { BrowserModule } from '@angular/platform-browser';
    import { FormsModule }   from '@angular/forms';
    import { AppComponent }  from './app.component';
    @NgModule({
      imports: [
        BrowserModule,
        FormsModule
      ],
      declarations: [
        AppComponent
      ],
      bootstrap: [ AppComponent ]
    })
    export class AppModule { }
```

We importeren de forms module en zorgen dat deze middels het imports attribuut beschikbaar is.
FormsModule is nu beschikbaar in onze AppModule.

Two-way binding in angular 2 gaat middels een speciale syntax de banana in a box `[()]`. Pas nu het input
veld aan in de AppComponent.

## app.component.ts
```html
<input [(ngModel)]="hero.name" placeholder="name">
```

We kunnen de naam aanpassen en zien deze aanpassing ook terug in de titel.