# ECMAScript Raw aka CommonJS without the CommonJS Module Standard

Generell wird ECMAScript welches oft fälschlicherweise oft Javascript genannt wird aus Historischen gründen. Immer Seriell ausgeführt und hat eigentlich gar kein Module System
Mit ES 2015 auch ES6 genannt die 6the iteration die im Jahr 2015 Released wurde. Engines von heute sind nahe am standard und unterstützen generell sogar oft schon ES 2022

wurde ein Module Standard eingeführt der Sogenannte ESM Standard welcher Immer ASynchron ist das heißt er läuft immer On Top auf dem CJS Code welcher Seriell ausgeführt wird
er stellt die sogenannte umgebung Environment bereit und ist sogar in der lage diese zu Modifizieren man nennt das oft Monkey Patching. Die Global existierenden Variablen sind vom typ her 
normale JS Variablen die als wert zu dem nativen Pointer in der Engine zeigen die ECMAScript Implementiert.

Es ist immer ein Programm das ECMAScript Implementiert obwohl es sogar klar auch ECMAScript Engines geschrieben in ECMAScript gibt und alles in ECMAScript ist ein Objekt Auch
wenn es vom nativen code produziert wird am ende sind alles Objecte vom type Object die Implementierung darunter ist irrelevant. Da es ja eine Skriptsprache ist!

entrypoint // CJS Language pure ECMAScript/Javascript no module system involved
```
'use strict' // Gets executed First means everything that now follows is strict
// Hier könnten jetzt methoden implementiert werden wie module systeme oder loader die code
// während dieser code läuft ausführen. require zb ist eine funktion die vom nativen code des module systems welches direkt beim starten
// geladen wird per default. und dann gelinkt als globales object require. 
```

Jetzt sollten wir das ganze kapitel durch arbeiten und dazu auf beispiele eingehen https://developer.mozilla.org/de/docs/Web/JavaScript/Reference/Strict_mode

use strict ist default für ESM Module 

mein-module.mjs oder .js wenn das projekt via package.json als type module definiert wurde ist ESM Code und somit Async by Default der oben stehende code ist praktisch
trotzdem der der ausgeführt wird und dann dazu halt ```import('mein-module.mjs')```das heißt die Engine macht wenn das erste Module das geladen wird von ihr ein ESM Module ist trotzdem
erstmal einen kompletten loop durch den stack mit dem nativen code der nur die environment definiert und dann sofort nur das import binding erstellt und ausführt.
```
export {} // Marker das es sich um ein Module handelt.
```

wenn ihr bis hierhin durchgehalten habt wisst ihr ohne es zu wissen schon mehr als 90% der Aktuellen ECMAScript Javascript entwickler die bundler einsetzen.

## Wozu führt uns das

Wir wissen jetzt eigentlich gibt es kein module system und alles ist Serial ausgeführter Script Code in einer durch Objekte Repräsentierten Laufzeit Umgebung 
welcher selbst nur Objekte Erzeugt durch functions oder methodenaufrufe diese funktionen und methoden sind selbst Objekte welche Properties auf einem Objekt sind
Also die Sprache ist super Flexibel und eignet sich super um Polyglotten Code Darzustellen und Asynchronen Code zu schreiben.

Was in einer welt von Multi Core CPUs klar eine Stärke ist. Ähnlich wie Rust verhindert ECMAScript Memory Leaks by Design solange man sich an ein paar regeln hält
es fällt einem nicht sehr schwer über das Speicher Model in javascript nachzudenken da man Ihn geradezu als unheimlich bezeichnen kann solange 
die einheiten die verarbeitet werden klein genug sind. Stichwort Streaming Prozessing.

Also brauchen wir wenn wir code aus mehren datein zusammen stellen wollen immer ein module system das dies während der laufzeit tut oder einen bundler der 
aus den Modulen Seriellen Code Erstellt der ausgeführt wird.

eines der ältesten Module Systeme ist wohl der sogenannte Global Export bei dem auf dem Global Objekt ein Property mit dem Module Namen als Namespace Erstellt wird.
dies wird heutzutage oft als AntiPattern gesehen ist aber während der laufzeit meist eh der Standard. wenn wir kein use strict benutzen zb wird jede variable automatisch
dem globalen objekt zu geweisen unbewusst. 

was dazu führt das jede variable auch über das globalThis Object auf welches man sich heute geeinigt hat um es zu erleichtern das globale Objekt unter einem standartisierten
namen zu erreichen. es wurde mit es2018 eingeführt https://caniuse.com/?search=globalThis die seite ist eine super quelle wenn man sich mal fragt welche umgebung 
was unterstützt. Es ist eine User gepflegte quelle also nicht immer 100% akkurat. 

wenn wir also code schreiben der am ende ein property auf dem Globalen Objekt hinterlässt haben wir praktisch ein Module registriert für die spätere nutzung mit anderem code der später geladen werden kann
oder auch sogar schon geladen ist 

also regel kann man sagen alle objekte müssen erst zur laufzeit existieren das heist man kann immer mit funktionen als Module arbeiten was es klar leicht macht 
die abhängigkeiten der funktion sogar als parameter zu übergeben was zu sehr einfachen guten test möglichkeiten führt.

```
const myFn = (param1) => new param1()
globalThis.myFn = myFn;
myFn(Array).concat([]) // using the new Array der . operator arbeitet auf dem return Objekt der myFn Funktion
```

in dem beispiel ist die Implementierung von Array Total austauschbar. wir haben es hier nur als beispiel genommen.
wir haben hier also ein myFn Module welches wir später aufrufen können mit egal welcher Implementierung 

```
globalThis.myArray = globalThis.myArrayFn(Array)
myArray.concat([])
myArray.map()
```




