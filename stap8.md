# Stap 8 - Master / Detail
We willen graag een lijst van helden hebben en deze kunnen bewerken in deze stap gaan we dat toevoegen.

We beginnen met een array van helden dit is vooralsnog een vast lijstje maar later kan dit uit een service komen.

Voeg het volgende toe in AppComponent naast de classes.

## app.component.ts
```javascript
    const HEROES: Hero[] = [
      { id: 11, name: 'Mr. Nice' },
      { id: 12, name: 'Narco' },
      { id: 13, name: 'Bombasto' },
      { id: 14, name: 'Celeritas' },
      { id: 15, name: 'Magneta' },
      { id: 16, name: 'RubberMan' },
      { id: 17, name: 'Dynama' },
      { id: 18, name: 'Dr IQ' },
      { id: 19, name: 'Magma' },
      { id: 20, name: 'Tornado' }
    ];
```

We kunnen nu deze lijst als propery aan het AppComponent toevoegen. Voeg het toe binnen de class AppComponent.

## app.component.ts
```javascript
heroes: HEROES;
```

We kunnen nu de lijst met helden opbouwen binnen onze template. Voeg de volgende code toe in de AppComponent
template onder de titel en boven de details.

## app.component.ts
```html
<ul class="heroes">
        <li *ngFor="let hero of heroes">
          <span class="badge">{{hero.id}}</span> {{hero.name}}
        </li>
</ul>
```

We gaan hier door de lijst heen met `ngFor` het sterretje is erg belangrijk dit geeft namelijk aan dat het volgende stukje
html een template is en kan worden toegevoegd / verwijderd aan de DOM.

Probeer nu eens om een extra held toe te voegen aan de array.

Het lijstje is nu wel erg saai, we kunenn aan ieder component styling toevoegen. doe dit door de volgende 
styling toe tevoegen als property binnen de decorator `@Component`

## app.component.ts 
````html
styles: [`
  .selected {
    background-color: #CFD8DC !important;
    color: white;
  }
  .heroes {
    margin: 0 0 2em 0;
    list-style-type: none;
    padding: 0;
    width: 15em;
  }
  .heroes li {
    cursor: pointer;
    position: relative;
    left: 0;
    background-color: #EEE;
    margin: .5em;
    padding: .3em 0;
    height: 1.6em;
    border-radius: 4px;
  }
  .heroes li.selected:hover {
    background-color: #BBD8DC !important;
    color: white;
  }
  .heroes li:hover {
    color: #607D8B;
    background-color: #DDD;
    left: .1em;
  }
  .heroes .text {
    position: relative;
    top: -3px;
  }
  .heroes .badge {
    display: inline-block;
    font-size: small;
    color: white;
    padding: 0.8em 0.7em 0 0.7em;
    background-color: #607D8B;
    line-height: 1em;
    position: relative;
    left: -1px;
    top: -4px;
    height: 1.8em;
    margin-right: .8em;
    border-radius: 4px 0 0 4px;
  }
`]
````

Deze styling behoort alleen tot dit componenten en kan nooit naar buiten lekken (dankzij shadow dom).

We gaan nu het master / detail gedeelte toevoegen. We willen graag de details zien van de geselecteerde held.
We doen dit door het click event te binden. Binding doen we door middels van de ronde haken `(event)`.

Pas het `<li>` stukje van de template als volgt aan.

# app.component.ts
```html
    <li *ngFor="let hero of heroes" (click)="onSelect(hero)">
      <span class="badge">{{hero.id}}</span> {{hero.name}}
    </li>
  ```

We gaan nu bijhouden welke held geselecteerd is, we passen daarvoor is de statische hero property aan naar een selectedHero.
De applicatie gaat nu errors geven tot we een `ngIf` gaan toevoegen.

# app.component.ts
```javascript
selectedHero: Hero;
```

Nu moeten we ook het template aanpassen om niet de statische held weer te geven maar de geselecteerd.

# app.component.ts
```html
    <h2>{{selectedHero.name}} details!</h2>
    <div><label>id: </label>{{selectedHero.id}}</div>
    <div>
        <label>name: </label>
        <input [(ngModel)]="selectedHero.name" placeholder="name"/>
    </div>
```

We moeten ook nog onze click handler in de AppComponent plaatsen. Plaats de volgende code als functie
binnen de AppComponent class.

# app.component.ts
```javascript
    onSelect(hero: Hero): void {
      this.selectedHero = hero;
    }
```

Als er geen held is geselecteerd willen we de details ook niet weergeven we kunnen hier `*ngIf` voor gebruiken.
De template wordt alleen toegevoegd aan de shadow dom als het statement naar true evalueerd.

Pas daarom nogmaals het template aan.
# app.component.ts
```html
    <div *ngIf="selectedHero">
      <h2>{{selectedHero.name}} details!</h2>
      <div><label>id: </label>{{selectedHero.id}}</div>
      <div>
        <label>name: </label>
        <input [(ngModel)]="selectedHero.name" placeholder="name"/>
      </div>
    </div>
```

Op dit moment moet je applicatie weer werken en kun je een held selecteren.
