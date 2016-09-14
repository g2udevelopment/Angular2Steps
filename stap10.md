# Stap 10 - Services
De data zit op dit moment nog in het AppComponent we gaan dit nu omzetten naar een herbruikbare service.
We beginnen met een nieuw bestand `hero.service.ts` we plaatsen deze weer in de app folder met de volgende inhoud.

## hero.service.ts
```javascript
import { Injectable } from '@angular/core';

@Injectable()
export class HeroService {
  getHeroes(): void {} // stub
}
```

`@Injectable` is nodig als de service ook andere service gebruikt, het wordt echter aanbevolen om deze decorator altijd toe te voegen aan een service.
zodat je deze nooit kunt vergeten.

De stub data staat ook nog in het `AppComponent` we gaan deze eruit halen en in een eigen file plaatsen.Haal `const Heroes:...` uit `AppComponent` en plaats deze in een nieuwe file
`mock-heroes.ts`.

## mock-heroes.ts
```javascript
  import { Hero } from './hero';
  export const HEROES: Hero[] = [
    {id: 11, name: 'Mr. Nice'},
    {id: 12, name: 'Narco'},
    {id: 13, name: 'Bombasto'},
    {id: 14, name: 'Celeritas'},
    {id: 15, name: 'Magneta'},
    {id: 16, name: 'RubberMan'},
    {id: 17, name: 'Dynama'},
    {id: 18, name: 'Dr IQ'},
    {id: 19, name: 'Magma'},
    {id: 20, name: 'Tornado'}
  ];
```

verander de hero property van het AppComponent nu als volgt.

## app.component.ts
```javascript
heroes: Hero[];
```

We kunnen nu de mock helden gaan terug geven als resultaat van onze service. Pas `hero.service.ts` aan en importeer en gebruik de mock helden.

## hero.service.ts
```javascript
import { Injectable } from '@angular/core';

import { Hero } from './hero';
import { HEROES } from './mock-heroes';

@Injectable()
export class HeroService {
  getHeroes(): Hero[] {
    return HEROES;
  }
}
```

Nu dat we de service gemaakt hebben en deze aangesloten hebben op de mock data kunnen we de service zelf gaan gebruiken. We doen dit dmv dependecy injection. We importeren eerst de service in ons
AppComponent.

## app.component.ts
```javascript
import { HeroService } from './hero.service';
```

Angular 2 doet DI op basis van het type en we kunnen dus een constructor aan `AppComponent` toevoegen.

## app.component.ts (constructor)
```javascript
constructor(private heroService: HeroService) { }
```

Als we nu de code uitvoeren zien we een exception, de provider is namelijk nog niet geregistreerd. Voeg een property providers toe aan de @Component decorator.

## app.component.ts
```javascript
providers: [HeroService]
```

Angular 2 weet nu welke service we willen gebruiken. We kunnen nu een methode toevoegen aan `AppComponent` toevoegen en onze service gebruiken. Voeg onderstaande methode toe aan AppComponent.

## app.component.ts
```javascript
getHeroes(): void {
    this.heroes = this.heroService.getHeroes();
  }
```

De constructor is geen goede plaats om de data initeel op te halen. Als we bijvoorbeeld een unit test schrijven willen we niet dat er een aanroep wordt gedaan naar een methode die misschien wel een service
gaat aanroepen. We kunnen daar voor beter een lifecycle event van angular 2 gebruiken namelijk `ngOnInit`. We importeren de `OnInit` module en we voegen de lifecycle event toe aan de `AppComponent` class.
pas `app.component.ts` als volgt aan, let op dat je de ngOnInit methode toevoegd en niet de hele AppComponent class overschrijft.

## app.component.ts
```javascript
import { Component, OnInit } from '@angular/core';

export class AppComponent implements OnInit {
  ngOnInit(): void {
    this.getHeroes();
  }
}
```

De applicatie moet op dit moment weer een lijst tonen.De helden worden nu nog synchroon opgehaald dit is nu nog geen probleem maar als we er bv. een http service achter gaan zetten blokt dit de UI. Het 
is daarom goed om gebruik te maken van `Promises`. We gaan de service aanpassen en een promise terug geven.

## hero.service.ts
```javascript
getHeroes(): Promise<Hero[]> {
  return Promise.resolve(HEROES);
}
```

We kunnen nu de data asynchroon ophalen, pas daarvoor `app.component.ts` aan.

## app.component.ts
```javascript
getHeroes(): void {
  this.heroService.getHeroes().then(heroes => this.heroes = heroes);
}
```

# Opdracht
We kunnen een trage verbinding simuleren en kijken hoe dat er in de UI uitziet. Voeg daarvoor een method toe aan de `HeroService`.

## hero.service.ts
```javascript
getHeroesSlowly(): Promise<Hero[]> {
  return new Promise<Hero[]>(resolve =>
    setTimeout(resolve, 2000)) // delay 2 seconds
    .then(() => this.getHeroes());
}
```

pas nu de call aan in `AppComponent` van `getHeroes()` naar `getHeroesSlowly()`
