# Stap 5 - Html startup pagina
We zijn nu klaar met onze basis applicatie nu moeten we de applicatie nog laden in de browser.
Maak hiervoor een nieuw bestand in de root directory aan `index.html`.

## index.html
```html
<html>
  <head>
    <title>Angular 2 QuickStart</title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="styles.css">
    <!-- 1. Load libraries -->
     <!-- Polyfill(s) for older browsers -->
    <script src="node_modules/core-js/client/shim.min.js"></script>
    <script src="node_modules/zone.js/dist/zone.js"></script>
    <script src="node_modules/reflect-metadata/Reflect.js"></script>
    <script src="node_modules/systemjs/dist/system.src.js"></script>
    <!-- 2. Configure SystemJS -->
    <script src="systemjs.config.js"></script>
    <script>
      System.import('app').catch(function(err){ console.error(err); });
    </script>
  </head>
  <!-- 3. Display the application -->
  <body>
    <my-app>Loading...</my-app>
  </body>
</html>
```

De pagina is als volgt opgebouwd.

1. Laden van polyfills voor oudere browsers en systemjs.
2. Configuratie inladen van systemjs
3. Het laden van ons component. Componenten zijn beschikbaar middels de tag die gedefineerd is in het component via de selector.

Om wat styling toe te voegen kun je een bestand `styles.css` aanmaken en onderstaande stylesheet daarin plakken.

## styles.css
```css
/* #docregion , quickstart, toh */
/* Master Styles */
h1 {
  color: #369;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 250%;
}
h2, h3 {
  color: #444;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: lighter;
}
body {
  margin: 2em;
}
/* #enddocregion quickstart */
body, input[text], button {
  color: #888;
  font-family: Cambria, Georgia;
}
/* #enddocregion toh */
a {
  cursor: pointer;
  cursor: hand;
}
button {
  font-family: Arial;
  background-color: #eee;
  border: none;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
  cursor: hand;
}
button:hover {
  background-color: #cfd8dc;
}
button:disabled {
  background-color: #eee;
  color: #aaa;
  cursor: auto;
}

/* Navigation link styles */
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

/* items class */
.items {
  margin: 0 0 2em 0;
  list-style-type: none;
  padding: 0;
  width: 24em;
}
.items li {
  cursor: pointer;
  position: relative;
  left: 0;
  background-color: #EEE;
  margin: .5em;
  padding: .3em 0;
  height: 1.6em;
  border-radius: 4px;
}
.items li:hover {
  color: #607D8B;
  background-color: #DDD;
  left: .1em;
}
.items li.selected:hover {
  background-color: #BBD8DC;
  color: white;
}
.items .text {
  position: relative;
  top: -3px;
}
.items {
  margin: 0 0 2em 0;
  list-style-type: none;
  padding: 0;
  width: 24em;
}
.items li {
  cursor: pointer;
  position: relative;
  left: 0;
  background-color: #EEE;
  margin: .5em;
  padding: .3em 0;
  height: 1.6em;
  border-radius: 4px;
}
.items li:hover {
  color: #607D8B;
  background-color: #DDD;
  left: .1em;
}
.items li.selected {
  background-color: #CFD8DC;
  color: white;
}

.items li.selected:hover {
  background-color: #BBD8DC;
}
.items .text {
  position: relative;
  top: -3px;
}
.items .badge {
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
/* #docregion toh */
/* everywhere else */
* {
  font-family: Arial, Helvetica, sans-serif;
}

```
