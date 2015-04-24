## Overpass - regiuni

Până acum, toate query-urile noastre au fost limitate la `{{bbox}}`, regiunea
vizibilă din hartă. Mai departe vom folosi poligoanele
[place](http://wiki.openstreetmap.org/wiki/Key:place) (localități, regiuni)
pentru a defini limite geografice.


### query după numele unui loc
```
relation
  [place=city]
  [name="Cluj-Napoca"];

>;
out;
```

![Poligon Cluj](screenshots/place=city,name="Cluj-Napoca".png)

* `>;` înseamnă "ia obiectele copil" (nodurile care formează poligonul)


### limită query la o regiune
```
area[place=city][name="Cluj-Napoca"];

node
  [amenity=cafe]
  (area);
out;
```

![Cafenele din Cluj](screenshots/cluj-amenity=cafe.png)

Definim `area` ca limitele orașului Cluj și îl folosim ca filtru spațial pentru
noduri.


### zone
```
area[place=city][name="Cluj-Napoca"];

(
  way
    [leisure~"park|garden"]
    (area);
  >;
);
out;
```

![Parcuri din Cluj](cluj-[leisure~"park|garden"].png)

Căutăm obiecte de tip "way" (linii, poligoane) care au tag `leisure=park` sau
`leisure=garden`.

Multe din rezultatele care apar ca puncte sunt de fapt poligoane, dar sunt
afișate ca puncte, din cauza simplificărilor făcute de viewer-ul din Overpass
Turbo. Datele exportate vor fi de tip poligon.


### centrele poligoanelor
```
area[place=city][name="Cluj-Napoca"];

way
  [leisure~"park|garden"]
  (area);
out center;
```

Nu mai cerem elementele copil de la poligoane (nodurile), vrem doar centrele.


### structuri complexe
```
area[place=city][name="Cluj-Napoca"];

(
  relation
    [route=bus]
    [ref=27]
    (area);
  >;
);
out;
```

![Autobuzul 27](cluj-route=bus,ref=27.png)

Plecând de la relația `[route=bus][ref=27]`, cerem toate obiectele copil
(way-uri și noduri, adică traseul și stațiile).
