# Stap 9 - Meerdere componenten
De applicatie wordt steeds groter en we voegen steeds meer functionaliteit toe aan 1 component. Het wordt tijd om functionaliteit te splitsen.
Dit zorgt ook voor herbruikbare componenten.

De detail weergaven van een held en het tonen van een lijst van helden zit nu in 1 component deze gaan we als eerste splitsen.

Maak een nieuw bestand aan binnen de `app` folder en noem deze `hero-detail.component.ts` Maak hierin een `HeroDetailComponent`

## hero-detail.component.ts
```javascript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'my-hero-detail',
})
export class HeroDetailComponent {
}
```

We maken een nieuw component dat beschikbaar zal worden binnen templates met de naam `my-hero-detail`. We importeren ook al zovast `Input` een component 
kan alleen middels Input en Output decorators communiceren met de buitenwereld en dus met andere componenten.

Ook de templates van het detail en lijst component zijn nog samen gevoegd, we gaan hier aparte templates voor maken. Voeg onderstaande code toe als template property bij het nieuwe HeroDetailComponent.

## hero-detail.component.ts
````html
template: `
  <div *ngIf="hero">
    <h2>{{hero.name}} details!</h2>
    <div><label>id: </label>{{hero.id}}</div>
    <div>
      <label>name: </label>
      <input [(ngModel)]="hero.name" placeholder="name"/>
    </div>
  </div>
`
````

We binden hier aan een `hero` property voeg deze toe aan de class `HeroDetailComponent`.

## hero-detail.component.ts
```javascript
hero: Hero;
``` 

We zien dat de Hero class nog niet beschikbaar is binnen ons nieuwe component. We gaan de hero class verplaatsen van `app.component.ts` naar een eigen `hero.ts`.
Maak een file `hero.ts` aan en verplaats de hero class.

## hero.ts
```javascript
export class Hero {
  id: number;
  name: string;
}
```

Nu moeten we deze nieuwe class nog importeren in zowel de `app.component.ts` als ook in `hero-detail.component.ts`. Plaats een import statement in beide files.

## hero-detail.component.ts en app.component.ts
```javascript
import { Hero } from './hero';
```

We gaan er nu voor zorgen dat de geselecteerde held in AppComponent ook wordt doorgegeven naar het detail component. We kunnen dit doen door een input decorator toe te voegen aan `hero-detail.component.ts`.
Voeg een `@Input()` decorator toe aan de hero property.

## hero-detail.component.ts
```javascript
@Input()
hero: Hero;
```

Nu ons detail component af is gaan we deze bekend maken binnen de module. Ga naar `app.module.ts` en importeer het detail component.

 ## app.module.ts
```javascript
import { HeroDetailComponent } from './hero-detail.component';
```

Voeg nu ook het detail component toe als declartion binnen de module.

## app.module.ts
```javascript
@NgModule({
  imports: [
    BrowserModule,
    FormsModule
  ],
  declarations: [
    AppComponent,
    HeroDetailComponent
  ],
  bootstrap: [ AppComponent ]
})
export class AppModule { }
```

We kunnen nu het stukje detail template gaan vervangen met ons nieuwe gemaakte detail component en we kunnen meteen de binding van de geselcteerde held opzetten. Vervang daarvoor in `app.component.ts`
De div met `*ngIf` (Dit is exact het stukje wat nu in een eigen component staat).

## app.component.ts
```html
<my-hero-detail [hero]="selectedHero"></my-hero-detail>
```

Controleer nu het resultaat. In deze stap hebben we een herbruikbaar component gemaakt en een waarde doorgegeven tussen
componenten.


