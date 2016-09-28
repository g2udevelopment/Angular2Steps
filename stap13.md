# Stap 13 Angular testing.
In deze stap gaan we kennis maken met het testen in en van angular. We beginnen met het clonen van een angular quickstart. Deze quickstart
heeft alle setup voor het testen van angular, zoals karma en jasmine. Dit is ook een mooie basis mocht je ooit een angular 2 project willen starten.

`git clone https://github.com/angular/quickstart.git Angular2Testing`

Draai `npm install` en daarna `npm start`

We gaan onze eerste spec maken. maak een nieuw bestand `1st.spec.ts` aan in de `app` folder.

## 1st.spec.ts
```javascript
describe('1st tests', () => {
  it('true is true', () => expect(true).toBe(true));
});
```

Voeg ook een `systemjs.config.extras.js` toe aan de root folder.

## systemjs.config.extras.js 
```javascript
/** App specific SystemJS configuration */
System.config({
  packages: {
    // barrels
    'app/model': {main:'index.js', defaultExtension:'js'},
    'app/model/testing': {main:'index.js', defaultExtension:'js'}
  }
});
```

Verwijder app.component.spec.ts en app.component.spec.js

Voer nu een `npm test` uit. Karma wordt opgestart en in je console zie je test output van de eerste spec.

Pas nu de verwachting aan van true naar false. De spec zal worden opgepakt er komt nu een FAILED uit, verander de expectation weer terug.

We gaan nu gebruik maken van het Angular Testing framework. Pas de volgende files aan of voeg ze toe.

## app.component.ts
```javascript
import { Component } from '@angular/core';

@Component({
    selector: 'my-app',
    template: '<app-banner></app-banner>'
})
export class AppComponent { }
```

## banner.component.ts
```javascript
import { Component }   from '@angular/core';

@Component({
  selector: 'app-banner',
  template: '<h1>{{title}}</h1>'
})
export class BannerComponent {
  title = 'Test Tour of Heroes';
}
```

## banner.component.ts
```javascript
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { AppComponent }  from './app.component';
import { BannerComponent } from './banner.component';

@NgModule({
  imports: [ BrowserModule ],
  declarations: [ AppComponent, BannerComponent ],
  bootstrap: [ AppComponent ]
})
export class AppModule { }
```

Maak nu een bestand `banner.component.spec.ts`

## banner.component.spec.ts
```javascript
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { By }              from '@angular/platform-browser';
import { DebugElement }    from '@angular/core';

import { BannerComponent } from './banner.component';

let comp:    BannerComponent;
let fixture: ComponentFixture<BannerComponent>;
let de:      DebugElement;
let el:      HTMLElement;

describe('BannerComponent', () => {
  beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [ BannerComponent ], // declare the test component
    });

    fixture = TestBed.createComponent(BannerComponent);

    comp = fixture.componentInstance; // BannerComponent test instance

    // query for the title <h1> by CSS element selector
    de = fixture.debugElement.query(By.css('h1'));
    el = de.nativeElement;

  });
});

``` 

We zetten hier een angular testbed op voor het testen van het bannercomponent. met `TestBed.createComponent` kunnen we een nieuw component maken.
We kunnen nu een aantal testen toe gaan voegen doe dit binnen het describe blok.

We testen eerst dat de header leeg is zonder een detectChanges().
## banner.component.spec.ts
```javascript
it('no title in the DOM until manually call `detectChanges`', () => {
    expect(el.textContent).toEqual('');
  });
```

Daarna roepen we detectChanges aan dit zet alles binnen angular ingang voor het binden etc..
## banner.component.spec.ts
```javascript
it('should display original title', () => {
    fixture.detectChanges();
    expect(el.textContent).toContain(comp.title);
  });
```

We kunnen ook nog een titel aan passen, draai de testen en kijk eens wat er gebeurt als je bv `comp.title` aanpast.
## banner.component.spec.ts
```javascript
it('should display a different test title', () => {
    comp.title = 'Test Title';
    fixture.detectChanges();
    expect(el.textContent).toContain('Test Title');
  });
```

We gaan nu een service dependency testen.

Voeg de volgende bestanden toe.

 ## user-service.ts
 ```javascript
 import { Injectable } from '@angular/core';
@Injectable()
export class UserService 
{  isLoggedIn = true;  
  user = {name: 'Sam Spade'};
}
 ```

 ## welcome.component.ts
 ```javascript
 import { Component, OnInit } from '@angular/core';
import { UserService }       from './user.service';

@Component({
  selector: 'app-welcome',
  template: '<h3 class="welcome" ><i>{{welcome}}</i></h3>'
})
export class WelcomeComponent  implements OnInit {
  welcome = '-- not initialized yet --';
  constructor(private userService: UserService) { }

  ngOnInit(): void {
    this.welcome = this.userService.isLoggedIn ?
      'Welcome, ' + this.userService.user.name :
      'Please log in.';
  }
}
 ```

 We kunnen nu het testbed op zetten.

 ## welcome.component.spec.ts
 ```javascript
 import { ComponentFixture, inject, TestBed } from '@angular/core/testing';
import { By }                                from '@angular/platform-browser';
import { DebugElement }                      from '@angular/core';
import { UserService }      from './user.service';
import { WelcomeComponent } from './welcome.component';
describe('WelcomeComponent', () => 
{  
  let comp: WelcomeComponent;  
  let fixture: ComponentFixture<WelcomeComponent>;  
  let componentUserService: UserService; // the actually injected service  
  let userService: UserService; // the TestBed injected service  
  let de: DebugElement;  // the DebugElement with the welcome message  
  let el: HTMLElement; // the DOM element with the welcome message  
  let userServiceStub: 
  {    
    isLoggedIn: boolean;    user: { name: string}  
};  

  beforeEach(() => {
    // stub UserService for test purposes
    userServiceStub = {
      isLoggedIn: true,
      user: { name: 'Test User'}
    };

    TestBed.configureTestingModule({
       declarations: [ WelcomeComponent ],
       providers:    [ {provide: UserService, useValue: userServiceStub } ]
    });

    fixture = TestBed.createComponent(WelcomeComponent);
    comp    = fixture.componentInstance;

    // UserService from the root injector
    userService = TestBed.get(UserService);

    //  get the "welcome" element by CSS selector (e.g., by class name)
    de = fixture.debugElement.query(By.css('.welcome'));
    el = de.nativeElement;
  });

  
});
 ```

We zien dat er een stub service wordt gemaakt en geregistreerd als provider.
We kunnen nu de testen toevoegen.

## welcome.component.ts
```javascript
it('should welcome the user', () => {
  fixture.detectChanges();
  const content = el.textContent;
  expect(content).toContain('Welcome', '"Welcome ..."');
  expect(content).toContain('Test User', 'expected name');
});

it('should welcome "Bubba"', () => {
  userService.user.name = 'Bubba'; // welcome message hasn't been shown yet
  fixture.detectChanges();
  expect(el.textContent).toContain('Bubba');
});

it('should request login if not logged in', () => {
  userService.isLoggedIn = false; // welcome message hasn't been shown yet
  fixture.detectChanges();
  const content = el.textContent;
  expect(content).not.toContain('Welcome', 'not welcomed');
  expect(content).toMatch(/log in/i, '"log in"');
});
```



