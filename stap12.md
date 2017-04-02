# Stap 12 - HTTP
In deze stap gaan we de service aansluiten op een http service backend.

## Wijziging in systemjs

Pas systemjs.config als volgt aan.

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
      'angular-in-memory-web-api': 'npm:angular-in-memory-web-api/bundles/in-memory-web-api.umd.js'
    },
    // packages tells the System loader how to load when no filename and/or no extension
    packages: {
      app: {
        main: './main.js',
        defaultExtension: 'js'
      },
      rxjs: {
        defaultExtension: 'js'
      }
    }
  });
})(this);
```

De http module om te communiceren met een http backend zit niet standaard in de angular core. We moeten dus de http module importeren.
Ook systemjs moet weten dat we de http module gaan gebruiken *bekijk systemjs.config*

We kunnen nu de module importeren in onze applicatie.

## app.module.ts
```javascript
 import { HttpModule }     from '@angular/http';

 @NgModule({
      imports: [
        BrowserModule,
        FormsModule,
        HttpModule,
        routing
      ]
```

Omdat we nog geen echte http backend hebben kunnen we deze simuleren met `angular-in-memory-web-api` We kunnen zo wel de http client gebruiken
alleen wordt deze vervangen(Injection) door de in-memory-web-api.

We gaan daarvoor eerst een stub service maken. Maak het bestand `in-memory-data.service.ts` aan in de app.ts folder.

## in-memory-data.service.ts
```javascript
import { InMemoryDbService } from 'angular-in-memory-web-api';
export class InMemoryDataService implements InMemoryDbService {
  createDb() {
    let heroes = [
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
    return {heroes};
  }
}
```

We kunnen nu gebruik maken van de stub db service. We moeten hiervoor weer de module file `app.module.ts` aanpassen.

## app.module.ts
```javascript
// Imports for loading & configuring the in-memory web api
import { InMemoryWebApiModule } from 'angular-in-memory-web-api';
import { InMemoryDataService }  from './in-memory-data.service';

InMemoryWebApiModule.forRoot(InMemoryDataService),

```

We gaan zo dadelijk de heroes service gebruik laten we eerst de mock data weg gooien. Delete de file `mock-heroes.ts`

Ga nu naar de hero service getHeroes methode. Pas als eerste de import sectie aan.

## hero.service.ts
```javascript 
import { Injectable }    from '@angular/core';
import { Headers, Http } from '@angular/http';

import 'rxjs/add/operator/toPromise';

import { Hero } from './hero';
```

Pas de getHeroes methode aan.

## hero.service.ts
```javascript
private heroesUrl = 'app/heroes';  // URL to web api

  constructor(private http: Http) { }

  getHeroes(): Promise<Hero[]> {
    return this.http.get(this.heroesUrl)
               .toPromise()
               .then(response => response.json().data as Hero[])
               .catch(this.handleError);
  }
```

http.get geeft een Observable terug omdat we nu nog een Promise terug geven moeten we de Observable converten naar een promise. Dit doen we door
gebruik te maken van een conversie methode uit rxjs toPromise. We moeten ook nog de handleError methode toevoegen.

## hero.service.ts
```javascript
private handleError(error: any): Promise<any> {
  console.error('An error occurred', error); // for demo purposes only
  return Promise.reject(error.message || error);
}
```

We kunnen nu de applicatie gebruiken zoals daarvoor. Maar als we nu een hero updaten en weer teruggaan naar de lijst zien we dat de naam niet meer geupdate is.
De mock service had een array met heroes die we konden aanpassen en inladen. Dit betekent dat we db service moeten aanpassen om ook een hero te kunnen saven.

We gaan eerst een save knop toevoegen aan het detail component. bv naast de back button

## hero-detail.component.html
```html
<button (click)="save()">Save</button>
```

We moeten nu de save methode aanmaken die aangeroepen wordt door het click event.
## hero-detail.component.ts
```javascript
save(): void {
  this.heroService.update(this.hero)
    .then(() => this.goBack());
}
```

Er ontbreekt nog een save methode op de heroService.

## hero.service.ts
```javascript
private headers = new Headers({'Content-Type': 'application/json'});

update(hero: Hero): Promise<Hero> {
  const url = `${this.heroesUrl}/${hero.id}`;
  return this.http
    .put(url, JSON.stringify(hero), {headers: this.headers})
    .toPromise()
    .then(() => hero)
    .catch(this.handleError);
}
```

We zien dat de in-memory-api de vertaling maakt tussen de put methode en het updaten van de db array.

Nu dat we een held kunnen aanpassen willen we er ook graag 1 kunnen toevoegen. Voeg dit toe net onder de header.

## heroes.component.html
```html
<div>
  <label>Hero name:</label> <input #heroName />
  <button (click)="add(heroName.value); heroName.value=''">
    Add
  </button>
</div>
```

We zien hier dat er met `#heroName` gerefereerd kan worden naar een element. Voeg de add methode toe.

## {opdracht waar moet deze}
```javascript
add(name: string): void {
  name = name.trim();
  if (!name) { return; }
  this.heroService.create(name)
    .then(hero => {
      this.heroes.push(hero);
      this.selectedHero = null;
    });
}
```

En ook de service heeft een add method nodig.
## hero.service.ts
```javascript
create(name: string): Promise<Hero> {
  return this.http
    .post(this.heroesUrl, JSON.stringify({name: name}), {headers: this.headers})
    .toPromise()
    .then(res => res.json().data)
    .catch(this.handleError);
}
```

We krijgen nu steeds meer helden we willen er graag ook een paar kunnen weggooien, we gaan per held een delete button toevoegen.
Voeg het volgende stukje template toe in `heroes.component.html` net na de laatse span in het li element. mocht deze er niet staan voeg dan eerst
een `<span>` toe aan {{hero.name}}

## heroes.component.html
```html
<button class="delete"
  (click)="delete(hero); $event.stopPropagation()">x</button>
```

De stopPropagation is er voor om te zorgen dat na de delete actie de click niet verder bubbled naar het li element.
We moeten de delete actie nog toevoegen aan het component en de service.

## heroes.component.ts
``` javascript
delete(hero: Hero): void {
  this.heroService
      .delete(hero.id)
      .then(() => {
        this.heroes = this.heroes.filter(h => h !== hero);
        if (this.selectedHero === hero) { this.selectedHero = null; }
      });
}
```

## hero.service.ts
```javascript
delete(id: number): Promise<void> {
  let url = `${this.heroesUrl}/${id}`;
  return this.http.delete(url, {headers: this.headers})
    .toPromise()
    .then(() => null)
    .catch(this.handleError);
}
```

We kunnen nog wat styling toevoegen aan de delete button.

## heroes.component.css
```css
button.delete {
  float:right;
  margin-top: 2px;
  margin-right: .8em;
  background-color: gray !important;
  color:white;
}
```

Eerder hebben we de Observable die komt vanuit de http.get aanroept omgezet naar een Promise. We gaan nu direct met de Observable leren werken.
Een Observable representeerd een stroom aan events die we met array achtig functies kunnen bevragen en filteren. Een gewonen promise geeft altijd een blok met data terug
Het kan bijvoorbeeld niet omgaan met onderbrekingen ook kan het bijvoorbeeld niet filteren of een zelfde request net al is gedaan. In de volgende stappen gaan we een Observable gebruiken en zelf maken.
We doen dit door een extra zoek component te maken die het mogelijk maakt in de lijst met helden te zoeken en filteren.

Maak eerst een nieuw bestand aan `hero-search.service.ts`

## hero-search.service.ts
```javascript
    import { Injectable }     from '@angular/core';
    import { Http, Response } from '@angular/http';
    import { Observable } from 'rxjs';
    import { Hero }           from './hero';
    @Injectable()
    export class HeroSearchService {
      constructor(private http: Http) {}
      search(term: string): Observable<Hero[]> {
        return this.http
                   .get(`app/heroes/?name=${term}`)
                   .map((r: Response) => r.json().data as Hero[]);
      }
    }
```

Nu dat we de service hebben staan kunnen we een nieuw component gaan maken.

## hero-search.component.html
```html
<div id="search-component">
  <h4>Hero Search</h4>
  <input #searchBox id="search-box" (keyup)="search(searchBox.value)" />
  <div>
    <div *ngFor="let hero of heroes | async"
         (click)="gotoDetail(hero)" class="search-result" >
      {{hero.name}}
    </div>
  </div>
</div>
```

En een beetje styling

## hero-search.component.css
```css
    .search-result{
      border-bottom: 1px solid gray;
      border-left: 1px solid gray;
      border-right: 1px solid gray;
      width:195px;
      height: 20px;
      padding: 5px;
      background-color: white;
      cursor: pointer;
    }
    #search-box{
      width: 200px;
      height: 20px;
    }
```

Als we goed kijken naar de service en het template dan zien we wat aanpassingen. Er wordt nu direct een Observable terug gegeven.
`*ngFor` kan niet direct werken met een Observable daarom gebruiken we de `async` pipe, deze subscribed op de Observable. Als laatste nog het component zelf.

## hero-search.component.ts
```javascript
import { Component, OnInit } from '@angular/core';
import { Router }            from '@angular/router';
import { Observable }        from 'rxjs/Observable';
import { Subject }           from 'rxjs/Subject';
import { HeroSearchService } from './hero-search.service';
import { Hero } from './hero';
@Component({
  moduleId: module.id,
  selector: 'hero-search',
  templateUrl: 'hero-search.component.html',
  styleUrls: [ 'hero-search.component.css' ],
  providers: [HeroSearchService]
})
export class HeroSearchComponent implements OnInit {
  heroes: Observable<Hero[]>;
  private searchTerms = new Subject<string>();
  constructor(
    private heroSearchService: HeroSearchService,
    private router: Router) {}
  // Push a search term into the observable stream.
  search(term: string): void {
    this.searchTerms.next(term);
  }
  ngOnInit(): void {
    this.heroes = this.searchTerms
      .debounceTime(300)        // wait for 300ms pause in events
      .distinctUntilChanged()   // ignore if next search term is same as previous
      .switchMap(term => term   // switch to new observable each time
        // return the http search observable
        ? this.heroSearchService.search(term)
        // or the observable of empty heroes if no search term
        : Observable.of<Hero[]>([]))
      .catch(error => {
        // TODO: real error handling
        console.log(error);
        return Observable.of<Hero[]>([]);
      });
  }
  gotoDetail(hero: Hero): void {
    let link = ['/detail', hero.id];
    this.router.navigate(link);
  }
}
```

Belangrijk is het Subject searchTerms, een Subject is een producent van een observable stream. We zorgen dat een stream van searchterms via de http service
worden omgezet naar een stream van heroes.

We moeten eerst de reactive extensions importeren in onze AppModule. Maak hiervoor een bestand rxjs-extensions.ts voor aan

## rxjs-extensions.ts
```javascript
// Observable class extensions
import 'rxjs/add/observable/of';
import 'rxjs/add/observable/throw';

// Observable operators
import 'rxjs/add/operator/catch';
import 'rxjs/add/operator/debounceTime';
import 'rxjs/add/operator/distinctUntilChanged';
import 'rxjs/add/operator/do';
import 'rxjs/add/operator/filter';
import 'rxjs/add/operator/map';
import 'rxjs/add/operator/switchMap';
```

Voeg vervolgens een import toe aan `app-module.ts` bovenaan de imports. `import './rxjs-extensions';`

Nu we toch in AppModule zitten kunnen we het `HeroSearchComponent` toevoegen, voeg zelf een import toe en voeg het component ook toe aan de declarations.

De laatste stap is om het component ook echt toe te voegen aan het dashboardcomponent. Ga naar `dashboard.component.html` en voeg hier helemaal onderaan toe.
`<hero-search></hero-search>`, we kunnen nu zoeken vanuit het dashboard.





