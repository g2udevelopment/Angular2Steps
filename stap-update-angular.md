# Stap updaten - Updaten naar Angular 2 Final Release
Angular 2 is eindelijk uit als final release. In deze stap gaan we de packages updaten om gebruik te kunnen maken van deze final release.
We gaan hiervoor `packages.json` aanpassen. De file die je nu maakt kan goed als basis dienen voor een echt angular 2 project.

## package.json vervang dependecies en devDependencies
```javascript
  "dependencies": {
    "@angular/common": "2.0.0",
    "@angular/compiler": "2.0.0",
    "@angular/core": "2.0.0",
    "@angular/forms": "2.0.0",
    "@angular/http": "2.0.0",
    "@angular/platform-browser": "2.0.0",
    "@angular/platform-browser-dynamic": "2.0.0",
    "@angular/router": "3.0.0",
    "@angular/upgrade": "2.0.0",
    "core-js": "^2.4.1",
    "reflect-metadata": "^0.1.3",
    "rxjs": "5.0.0-beta.12",
    "systemjs": "0.19.27",
    "zone.js": "^0.6.23",
    "angular2-in-memory-web-api": "0.0.20",
    "bootstrap": "^3.3.6"
  },
  "devDependencies": {
    "concurrently": "^2.2.0",
    "lite-server": "^2.2.2",
    "typescript": "^2.0.2",
    "typings":"^1.3.2"
  }
```

Ook de typings kunnen we updaten.

`npm run typings -- install dt~core-js --global --save`

`npm run typings -- install dt~node --global --save`

We kunnen nu `npm install` runnen om de nieuwste packages binnen te halen.
