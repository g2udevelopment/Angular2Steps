# Stap 6 uitvoeren en bewerken

Onze applicatie is nu klaar we kunnen deze nu uitvoeren `npm start`

Dit script voert 2 commando's uit.
- tsc in watch mode, dit betekent dat elke aanpassing in een typescript file automatisch een recompile triggert.
- lite-server is een static file server die ook in staat is tot live reloading er is dus geen F5 nodig.

# Opdracht
Maak nu wat live aanpassingen aan de template. Probeer ook eens een variabele toe te voegen aan de class `AppComponent`
en voeg deze toen aan de template `{{}}` binding.

# Oeps!
We worden geconfronteerd met een fout uit tsc, er is iets mis met de typings.
Het probleem is dat de core-js typings nu op een andere manier opgenomen worden in de typescript compiler.
Verwijder uit typings.js de regel met `core-js` en voeg het volgende toe aan `tsconfig.json`, property `compileroptions`
    `"lib": ["dom","es2015"]` verwijder `node_modules` en voer een `npm install` uit. Daarna kun je de applicatie weer starten.