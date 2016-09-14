# Stap 11 - Routing
De applicatie bevat nu 1 pagina met een master / detail view. We willen echter wat meer interactie in onze applicatie. In deze stap gaan we een dashbord toevoegen met top helden.
Ook gaan we routing toevoegen zodat we van view kunnen wisselen.

Ons AppComponent bevat zowel hero functionaliteit als ook de shell om de applicatie op te starten. Het eerste doel is om de hero functionaliteit een eigen component te geven.

Omdat AppComponent eigenlijk al het hero component is kunnen we deze het besten renamen. Rename app.component.ts naar heroes.component.ts. Verander de class
naam van AppComponent in HeroesComponent en verander de selector van my-app in my-heroes. De wijziging zien er dan als volgt uit.

## heroes.component.ts
```javascript
@Component({
  selector: 'my-heroes',
})
export class HeroesComponent implements OnInit {
}
```

Nu dat we een eigen hero component hebben moeten we een nieuw AppComponent gaan maken. De eerste stap is om een nieuw `app.component.ts` bestand aan te maken.

## app.component.ts
```javascript
    import { Component } from '@angular/core';
    @Component({
      selector: 'my-app',
      template: `
        <h1>{{title}}</h1>
        <my-heroes></my-heroes>
      `
    })
    export class AppComponent {
      title = 'Tour of Heroes';
    }
```

Je kunt nu uit `heroes.component.ts` de title property uit de class halen en het stukje template `<h1>{{title}}</h1>` ook kan de service provider weg omdat we deze op module niveau gaan declareren, het import
statement laten we staan. Het nieuwe `HeroesComponent` is nog niet beschikbaar binnen onze module. We passen nu `app.module.ts` aan om het component te importeren en toe te voegen aan de module.

## app.module.ts
```javascript
    import { NgModule }       from '@angular/core';
    import { BrowserModule }  from '@angular/platform-browser';
    import { FormsModule }    from '@angular/forms';
    import { AppComponent }        from './app.component';
    import { HeroDetailComponent } from './hero-detail.component';
    import { HeroesComponent }     from './heroes.component';
    import { HeroService }         from './hero.service';
    @NgModule({
      imports: [
        BrowserModule,
        FormsModule
      ],
      declarations: [
        AppComponent,
        HeroDetailComponent,
        HeroesComponent
      ],
      providers: [
        HeroService
      ],
      bootstrap: [ AppComponent ]
    })
    export class AppModule {
    }
```

We hebben nu het heroes component gerefactored naar een eigen component en een shell toegevoegd voor het laden van onze applicatie. In de volgende stappen zullen we routering toe gaan voegen.
Dit gebeurt dmv een optionele routing module. Als we gebruik gaan maken van angular 2 routing is het belangrijk om de `base-ref` te zetten. Voeg onderstaande toe aan index.html (vervang de head tag).

## index.html
```html
<head>
  <base href="/">
```

We kunnen nu de routing zelf gaan toevoegen. Maak een bestand aan in de `app` directory `app.routing.ts`.

## app.routing.ts
```javascript
import { ModuleWithProviders }  from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

import { HeroesComponent }      from './heroes.component';

const appRoutes: Routes = [
  {
    path: 'heroes',
    component: HeroesComponent
  }
];

export const routing: ModuleWithProviders = RouterModule.forRoot(appRoutes);
```

De export `routing` levert een geconfigureerde routing module op die we kunnen importeren in onze AppModule. Dit gaan we doen door een import toe te voegen en de `routing` export
toe te voegen aan de imports array.

## app.module.ts (alleen wijzigingen)
```javascript
import { routing } from './app.routing';

@NgModule({
  imports: [
    BrowserModule,
    FormsModule,
    routing
  ],
})
export class AppModule {
}
```

De routing in de applicatie is nu klaar echter weet de router nog niet waar de componenten geplaats moeten worden. Angular 2 routing gebruik hiervoor `<router-outlet>`.
Ook willen we een link plaatsen in `app.component.ts` om te navigeren naar ons component. We gebruikeren hiervoor de property `routerLink` op een `<a>` tag. Omdat we geen rechte of ronde haken gebruiken
in de binding is dit een zogenaamde one-time binding. Dit betekent dat de property maar 1x wordt gebind. Pas het template in `app.component.ts` aan.

## app.component.ts
````html
template: `
   <h1>{{title}}</h1>
   <a routerLink="/heroes">Heroes</a>
   <router-outlet></router-outlet>
 `
````

We hebben nu een basis routing werkend binnen ons applicatie. Maar we hebben niet zoveel aan routing met maar 1 component. We voegen daartoe een nieuw component toe aan onze applicatie `DashboardComponent`.

## dashboard.component.ts
```javascript
import { Component } from '@angular/core';

@Component({
  selector: 'my-dashboard',
  template: '<h3>My Dashboard</h3>'
})
export class DashboardComponent { }
```

We gaan nu het nieuwe component bekend maken bij de routing module in `app.routing.ts` voeg het onderstaande object toe aan de `Routes` array als nieuw item. Importeer ook het `DashboardComponent` (bedenk zelf hoe dit moet).

## app.routing.ts
```javascript
{
  path: 'dashboard',
  component: DashboardComponent
},
```

Het component is nog niet bekend in de `app.module.ts`. Importeer het `DashboardComponent` en voeg deze toe aan de `declarations` array (Doe dit zelf).

Als je nu na deze stappen gaat naar de url `/dashboard` dan zul je op het nieuwe component uitkomen en van uit hier kan je weer navigeren naar onze helden. Als we nu naar de `/` van applicatie
gaan dan selecteerd de router standaard de eerste route die hij tegenkomt. We kunnen er ook voor zorgen dat we standaard naar het dashboard navigeren. Voeg het volgende routing object toe aan de array met `Routes`
in `app.routing.ts`

## app.routing.ts
```javascript
{
  path: '',
  redirectTo: '/dashboard',
  pathMatch: 'full'
},
```

In `app.component.ts` kunnen we nu ook een link plaatsen naar het dashboard. Doen dit door de `template` property in `app.component.ts` aan te passen.

## app.component.ts
````html
template: `
   <h1>{{title}}</h1>
   <nav>
     <a routerLink="/dashboard">Dashboard</a>
     <a routerLink="/heroes">Heroes</a>
   </nav>
   <router-outlet></router-outlet>
 `
````

We kunnen nu wisselen tussen het dashboard en de lijst met helden. In de volgende stappen gaan we het dashboard vullen. Kleine stukje template kunnen we opnemen in het component zelf echter
als de templates groter worden is het handiger om deze in een apart bestand te bewaren. We kunnen hier voor de `templateUrl` property gebruiken van de `@Component` decorator.
Vervang de template property in `dashboard.component.ts` in het volgende.

## dashboard.component.ts
```html
templateUrl: 'app/dashboard.component.html',
```

Maak in de app folder een nieuwe bestand aan `dashboard.component.html` met onderstaande inhoud.

## dashboard.component.html
```html
<h3>Top Heroes</h3>
<div class="grid grid-pad">
  <div *ngFor="let hero of heroes" (click)="gotoDetail(hero)" class="col-1-4">
    <div class="module hero">
      <h4>{{hero.name}}</h4>
    </div>
  </div>
</div>
```

We moeten nu de helden gaan ophalen dmv. onze eerder gemaakte service, we hebben eerder al de service beschikbaar gemaakt op module niveau. Ook moeten we de methode
`gotoDetail(hero)` implementeren. We passen het dashboard component aan eerst voegen we de nodige imports toe en daarna passen we de class aan.

## dashboard.component.ts (imports)
```javascript
import { Component, OnInit } from '@angular/core';

import { Hero } from './hero';
import { HeroService } from './hero.service';
```

## dashboard.component.ts (class)
```javascript
export class DashboardComponent implements OnInit {

  heroes: Hero[] = [];

  constructor(private heroService: HeroService) { }

  ngOnInit(): void {
    this.heroService.getHeroes()
      .then(heroes => this.heroes = heroes.slice(1, 5));
  }

  gotoDetail(hero: Hero): void { /* not implemented yet */}
}
```

Omdat we vanaf verschillende plaatsen in de applicatie willen kunnen navigeren naar het detail componenten gaan we een aantal aanpassingen maken in de routing.
we beginnen met een route definitie die het mogelijk maakt een parameter door te geven. Voeg het volgende object toe aan de `Routes` array. Importeer ook het HeroDetailComponent.

## app.routing.ts
```javascript
{
  path: 'detail/:id',
  component: HeroDetailComponent
},
```  

Omdat de manier waarop we een detail gaan opvragen veranderd moeten we ook het DetailComponent aanpassen. In plaats van een object mee te sturen gaan we details ophalen op basis van het id
dat we meegeven. Pas als eerste de imports aan. Laat de `Hero` import staan.

## hero-detail.component.ts
```javascript
// Keep the Input import for now, we'll remove it later:
import { Component, Input, OnInit } from '@angular/core';
import { ActivatedRoute, Params } from '@angular/router';

import { HeroService } from './hero.service';
```

Voeg een constructor toe zodat we gebruik kunnen maken van de `ActivatedRoute` service en de `HeroService`.

## hero-detail.component.ts
```javascript
constructor(
  private heroService: HeroService,
  private route: ActivatedRoute) {
}
```

We gaan nu in de `ngOnInit` de `params` observable gebruiken en de details opvragen. Een observable geeft een promise terug die resolved als er iets veranderd aan de collectie.
Pas eerst de class definitie aan.

## hero-detail.component.ts
```javascript
export class HeroDetailComponent implements OnInit {
```

Voeg nu de `ngOnInit` methode toe aan het `HeroDetailComponent`.

## hero-detail.component.ts
```javascript
ngOnInit(): void {
  this.route.params.forEach((params: Params) => {
    let id = +params['id'];
    this.heroService.getHero(id)
      .then(hero => this.hero = hero);
  });
}
```

We moeten nog een extra functie toevoegen aan onze `HeroService` om een held op basis van id op te halen. Voeg de volgende functie toe aan `hero.service.ts`

## hero.service.ts
```javascript
getHero(id: number): Promise<Hero> {
  return this.getHeroes()
             .then(heroes => heroes.find(hero => hero.id === id));
}
```

Het zou ook handig zijn als we terug kunnen navigeren vanuit het detail component. Voeg de volgende functie toe in `hero-detail.component.ts`

## hero-detail.component.ts
```javascript
goBack(): void {
  window.history.back();
}
```

Het template moet ook worden aangepast, we gebruiken dit moment om het template meteen te verplaatsen naar een eigen file maar `hero-detail.component.html` aan en plaats daarin 
de volgende inhoud, verander de property template ook in een templateUrl en wijs naar `app/hero-detail.component.html`

## hero-detail.component.html
```html
<div *ngIf="hero">
  <h2>{{hero.name}} details!</h2>
  <div>
    <label>id: </label>{{hero.id}}</div>
  <div>
    <label>name: </label>
    <input [(ngModel)]="hero.name" placeholder="name" />
  </div>
  <button (click)="goBack()">Back</button>
</div>
```

We gaan het nu mogelijk maken om ook vanuit het dashboard een held te selecteren en naar het detail overzicht te navigeren. We importeren daarvoor eerst de router in het DashboardComponent.

## dashboard.component.ts
```javascript
import { Router } from '@angular/router';

constructor(
  private router: Router,
  private heroService: HeroService) {
}
```

Nu kunnen we de funtie `gotoDetail` invullen.

## dashboard.component.ts
```javascript
gotoDetail(hero: Hero): void {
  let link = ['/detail', hero.id];
  this.router.navigate(link);
}
```

Het is nu tijd om het `HeroComponent` aan te pakken. Haal eerst het stukje `<my-hero-detail>` weg en vervang dit door het volgende template.

## heroes.component.ts
```html
<div *ngIf="selectedHero">
  <h2>
    {{selectedHero.name | uppercase}} is my hero
  </h2>
  <button (click)="gotoDetail()">View Details</button>
</div>
```

Merk op dat we hier een pipe gebruiken. Het component wordt nu wel erg groot. Verplaats de template naar een bestand `heroes.component.html` en voeg een templateUrl property toe.
Verplaats ook de css en plaats deze in een bestand `heroes.component.css` en voeg een styleUrls property toe.

## heroes.component.ts
```javascript
@Component({
  selector: 'my-heroes',
  templateUrl: 'app/heroes.component.html',
  styleUrls:  ['app/heroes.component.css']
})
```

Importeer nu de router `import { Router } from '@angular/router';` en injecteer deze in de constructor. Implementeer nu de functie `gotoDetail` in het heroes component.

## heroes.component.ts
```javascript
gotoDetail(): void {
    this.router.navigate(['/detail', this.selectedHero.id]);
  }
```

We zijn nu klaar met de navigatie echter de applicatie ziet er nog niet zo mooi uit, we gaan wat styling toevoegen per component. Er volgen  nu een aantal stukken css.
De bedoeling is om ieder stukje in een aparte nieuwe file te zetten en de stylesUrl property van het corresponderen component aan te passen (Dit staat in het kopje) eventueele
style properties kunnen worden overschreven.

## hero-detail.component.css (styleUrls: ['app/hero-detail.component.css'])
```css
label {
  display: inline-block;
  width: 3em;
  margin: .5em 0;
  color: #607D8B;
  font-weight: bold;
}
input {
  height: 2em;
  font-size: 1em;
  padding-left: .4em;
}
button {
  margin-top: 20px;
  font-family: Arial;
  background-color: #eee;
  border: none;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer; cursor: hand;
}
button:hover {
  background-color: #cfd8dc;
}
button:disabled {
  background-color: #eee;
  color: #ccc; 
  cursor: auto;
}
```

## dashboard.component.css (styleUrls: ['app/dashboard.component.css'])
```css
[class*='col-'] {
  float: left;
}
*, *:after, *:before {
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
}
h3 {
  text-align: center; margin-bottom: 0;
}
[class*='col-'] {
  padding-right: 20px;
  padding-bottom: 20px;
}
[class*='col-']:last-of-type {
  padding-right: 0;
}
.grid {
  margin: 0;
}
.col-1-4 {
  width: 25%;
}
.module {
    padding: 20px;
    text-align: center;
    color: #eee;
    max-height: 120px;
    min-width: 120px;
    background-color: #607D8B;
    border-radius: 2px;
}
h4 {
  position: relative;
}
.module:hover {
  background-color: #EEE;
  cursor: pointer;
  color: #607d8b;
}
.grid-pad {
  padding: 10px 0;
}
.grid-pad > [class*='col-']:last-of-type {
  padding-right: 20px;
}
@media (max-width: 600px) {
    .module {
      font-size: 10px;
      max-height: 75px; }
}
@media (max-width: 1024px) {
    .grid {
      margin: 0;
    }
    .module {
      min-width: 60px;
    }
}
```

## app.component.css (styleUrls: ['app/app.component.css'])
```css
h1 {
  font-size: 1.2em;
  color: #999;
  margin-bottom: 0;
}
h2 {
  font-size: 2em;
  margin-top: 0;
  padding-top: 0;
}
nav a {
  padding: 5px 10px;
  text-decoration: none;
  margin-top: 10px;
  display: inline-block;
  background-color: #eee;
  border-radius: 4px;
}
nav a:visited, a:link {
  color: #607D8B;
}
nav a:hover {
  color: #039be5;
  background-color: #CFD8DC;
}
nav a.active {
  color: #039be5;
}
```

Angular 2 router heeft een directive `routerLinkActive` die op basis van de actieve route een css class kan toevoegen. Verander het template in `app.component.ts` als volgt

## app.component.ts
````html
template: `
  <h1>{{title}}</h1>
  <nav>
    <a routerLink="/dashboard" routerLinkActive="active">Dashboard</a>
    <a routerLink="/heroes" routerLinkActive="active">Heroes</a>
  </nav>
  <router-outlet></router-outlet>
`,
````  

















