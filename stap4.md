# Stap 4 - Bootstrappen van een angular 2 applicatie
Het bootstrappen van een angular 2 applicatie is platform specifiek. We gebruiken daarom functies uit de 
module `@angular/platform-browser-dynamic`. Op een mobiel platform is het bijvoorbeeld mogelijk om Cordova of NativeScript
te gebruiken. 

Het is een best practice om bootstrap code en module defintief te splitsen, zodat het opstarten van je applicatie
los staat en het makkelijk is om modules los te testen of te delen binnen projecten.

Maak een nieuwe file `main.ts` aan en voeg de volgende code toe

## main.ts

```javascript
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

const platform = platformBrowserDynamic();
```

# Opdracht

Het bestand `main.ts` is nog niet compleet. Onze module wordt nog niet geimporteerd en ge-bootstraped.
Import de module en roep een method aan op platform om de module te bootstrappen.