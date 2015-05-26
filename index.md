## Overpass - filtrare

[Overpass API](http://wiki.openstreetmap.org/wiki/Overpass_API) este un
serviciu de interogat date Ã®n OpenStreetMap. [Overpass
Turbo](http://overpass-turbo.eu/) este interfaÈ›a pentru rulat query-uri È™i
vizualizat rezultate.

![overpass](screenshots/overpass.png)


### filtru dupÄƒ tag
```
node
  [amenity=cafe]
  ({{bbox}});
out;
```

![amenity=cafe](screenshots/amenity=cafe.png)

ÃŽn tab-ul "data" din dreapta putem vedea rezultatul Ã®n format OpenStreetMap
XML.

**ExerciÈ›iu**: extrageÈ›i
[restaurantele](http://wiki.openstreetmap.org/wiki/Tag:amenity%3Drestaurant),
[hotelurile](http://wiki.openstreetmap.org/wiki/Tag:tourism%3Dhotel) È™i
[spitalele](http://wiki.openstreetmap.org/wiki/Tag:amenity%3Dhospital) din
zonÄƒ.


### query "È™i"
```
node
  [amenity=cafe]
  [wheelchair=yes]
  ({{bbox}});
out;
```

![amenity=cafe,wheelchair=yes](screenshots/amenity=cafe,wheelchair=yes.png)

**ExerciÈ›iu**: extrageÈ›i spitalele care au camerÄƒ de gardÄƒ (`emergency=yes`).


### query "sau"
```
(
  node
    [amenity=cafe]
    ({{bbox}});
  node
    [tourism=hotel]
    ({{bbox}});
);
out;
```

![amenity=cafe+tourism=hotel](screenshots/amenity=cafe+tourism=hotel.png)

**ExerciÈ›iu**: extrageÈ›i toate
[cafenelele](http://wiki.openstreetmap.org/wiki/Tag:amenity%3Dcafe),
[barurile](http://wiki.openstreetmap.org/wiki/Tag:amenity%3Dbar),
[pub-urile](http://wiki.openstreetmap.org/wiki/Tag:amenity%3Dpub) È™i
[cluburile](http://wiki.openstreetmap.org/wiki/Tag:amenity%3Dnightclub).


### expresie regulatÄƒ
```
node
  [amenity~"cafe|bar|restaurant"]
  ({{bbox}});
out;
```

![amenity~"cafe|bar|restaurant"](screenshots/amenity~"cafe|bar|restaurant".png)

**ExerciÈ›iu**: La fel ca punctul precedent, scris sub formÄƒ de expresie
regulatÄƒ


### query "tot"
```
(
  node({{bbox}});
  <;
);
out meta;
```

![toatÄƒ informaÈ›ia din zonÄƒ](screenshots/all.png)

ReturneazÄƒ toate datele din regiunea vizibilÄƒ. Operatorul `<;` Ã®nseamnÄƒ "ia
obiectele pÄƒrinte" (poligoane È™i relaÈ›ii care conÈ›in noduri din zonÄƒ)


### query "unde sunt?"
```
[out:json];
is_in({{center}});
out;
```

![unde mÄƒ aflu?](screenshots/where.png)

ReturneazÄƒ toate poligoanele care conÈ›in centrul hÄƒrÈ›ii vizibile (clÄƒdiri,
teren, regiuni administrative). Am cerut rezultatele Ã®n format JSON ca sÄƒ fie
mai uÈ™or de citit.

## Overpass - regiuni

PÃ¢nÄƒ acum, toate query-urile noastre au fost limitate la `{{bbox}}`, regiunea
vizibilÄƒ din hartÄƒ. Mai departe vom folosi poligoanele
[place](http://wiki.openstreetmap.org/wiki/Key:place) (localitÄƒÈ›i, regiuni)
pentru a defini limite geografice.


### query dupÄƒ numele unui loc
```
relation
  [place=city]
  [name="Cluj-Napoca"];

>;
out;
```

![Poligon Cluj](screenshots/place=city,name="Cluj-Napoca".png)

* `>;` Ã®nseamnÄƒ "ia obiectele copil" (nodurile care formeazÄƒ poligonul)

**ExerciÈ›iu**: extrageÈ›i conturul altor localitÄƒÈ›i.


### limitÄƒ query la o regiune
```
area[place=city][name="Cluj-Napoca"];

node
  [amenity=cafe]
  (area);
out;
```

![Cafenele din Cluj](screenshots/cluj-amenity=cafe.png)

Definim `area` ca limitele oraÈ™ului Cluj È™i Ã®l folosim ca filtru spaÈ›ial pentru
noduri.

**ExerciÈ›iu**: extrageÈ›i hotelurile din Cluj.


### zone
```
area[place=city][name="Cluj-Napoca"];

(
  way
    [leisure=park]
    (area);
  >;
);
out;
```

![Parcuri din Cluj](screenshots/cluj-leisure=park.png)

CÄƒutÄƒm obiecte de tip "way" poligoane care au tag `leisure=park`.

Multe din rezultatele care apar ca puncte sunt de fapt poligoane, dar sunt
afiÈ™ate ca puncte, din cauza simplificÄƒrilor fÄƒcute de viewer-ul din Overpass
Turbo. Datele exportate vor fi de tip poligon.

**ExerciÈ›iu**: extrageÈ›i È™i grÄƒdinile (`leisure=garden`) Ã®n acelaÈ™i query.


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

![Autobuzul 27](screenshots/cluj-route=bus,ref=27.png)

PlecÃ¢nd de la relaÈ›ia `[route=bus][ref=27]`, cerem toate obiectele copil
(way-uri È™i noduri, adicÄƒÂ traseul È™i staÈ›iile).

**ExerciÈ›iu**: extrageÈ›i magistrala de metrou
[M2](http://www.openstreetmap.org/relation/2947020) din BucureÈ™ti.


### centrele poligoanelor
```
area[place=city][name="Cluj-Napoca"];

way
  [leisure=park]
  (area);
out center;
```

![Centrele parcurilor din Cluj](screenshots/cluj-leisure=park-center.png)

Nu mai cerem elementele copil de la poligoane (nodurile de pe contur), vrem
doar centrele.

**ExerciÈ›iu**: extrageÈ›i centrele aeroporturilor din RomÃ¢nia (poligoane
`aeroway=aerodrome`).

**ExerciÈ›iu**: extrageÈ›i centrele localitÄƒÈ›ilor din judeÈ›ul TimiÈ™oara
(`[place~"city|town|village"]`). Nu uitaÈ›i sÄƒÂ modificaÈ›i È™i `area` de cÄƒutare.

## Overpass Turbo - simbolizare

Overpass Turbo (interfaÈ›a de rulat query-uri È™i vizualizat rezultate) poate sÄƒ
simbolizeze rezultatul conform unor reguli definite de noi. Sintaxa, MapCSS,
este [explicatÄƒ pe
wiki](http://wiki.openstreetmap.org/wiki/Overpass_turbo/MapCSS).


### Cafenele, pub-uri
```
area[place=city][name="Cluj-Napoca"];

node[amenity~"^cafe|pub$"](area);
out;

{{style:
  node { symbol-size: 8; }
  node[amenity=cafe] { color: blue; fill-color: blue; }
  node[amenity=pub] { color: green; fill-color: green; }
}}
```

![Cafenele È™i pub-uri](screenshots/mapcss-cafe-pub.png)

Prima regulÄƒ de CSS spune cÄƒÂ toate simbolurile au razÄƒ 8. UrmÄƒtoarele douÄƒ
coloreazÄƒ simbolurile Ã®n funcÈ›ie de tag-uri.

La expresia regulatÄƒ am adÄƒugat regulile `^` È™i `$` (Ã®nceput È™i sfÃ¢rÈ™it de
text), ca sÄƒ primim rezultate exacte `pub` È™i `cafe`, fÄƒrÄƒ rezultate de tipul
`amenity=public_building`, care conÈ›in Ã®n parte expresia cÄƒutatÄƒ.

**ExerciÈ›iu**: simbolizaÈ›i spitalele din Cluj. Cele care au camerÄƒ de gardÄƒ sÄƒ
fie colorate cu roÈ™u, restul albastru.


### Linie de autobuz
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

{{style:
  node { symbol-shape: none; }
  relation node[public_transport=stop_position] {
    symbol-shape: circle; symbol-size: 5;
  }
}}
```

![Autobuzul 27, simbolizat](screenshots/mapcss-bus27.png)

Ascundem toate nodurile, mai puÈ›in cele care fac parte din relaÈ›ie, È™i sunt
marcate ca staÈ›ie.

**ExerciÈ›iu**: simbolizaÈ›i magistrala de metrou M2 din BucureÈ™ti.


### ReÈ›eaua de Ã®naltÄƒ tensiune
```
area[name="RomÃ¢nia"];

way
  (area)
  [power=line]
  [voltage~"...000"];
out geom;

{{style:
  node[voltage=400000],
  line[voltage=400000]
  { color:red; fill-color: red; }

  node[voltage=220000],
  line[voltage=220000]
  { color:blue; fill-color: blue; }

  node[voltage=110000],
  line[voltage=110000]
  { color:green; fill-color: green; }
}}
```

![Linii de Ã®naltÄƒ tensiune](screenshots/mapcss-power.png)

Linii de Ã®naltÄƒ tensiune (110kV, 220kV È™i 400kV). Expresia regulatÄƒ filtreazÄƒ
liniile care au 3 cifre urmate de 3 zero-uri.

Din cauza simplificÄƒrilor fÄƒcute de Overpass Turbo, la zoom out, unele segmente
de linie sunt reprezentate ca noduri, de aceea regulile de simbolizare opereazÄƒ
È™i pe noduri È™i pe linii.

**ExerciÈ›iu**: simbolizaÈ›i reÈ›eaua de autostrÄƒzi (`highway=motorway`) din
RomÃ¢nia È™i coloraÈ›i fiecare autostradÄƒ (A1, A2...) diferit.


### LocalitÄƒÈ›i dupÄƒ tip È™iÂ mÄƒrime
```
area[place=county][name="Cluj"];

node(area)[place~"city|town|village"];
out;

{{style:
  node { symbol-size: 2; }
  node[population>100] { symbol-size: 4; }
  node[population>10000] { symbol-size: 8; }
  node[population>100000] { symbol-size: 12; }
  node[place=village] { color: brown; }
  node[place=town] { color: blue; }
  node[place=city] { color: red; }
}}
```

![LocalitÄƒÈ›i](screenshots/mapcss-places.png)

Cuoarea reprezintÄƒ tipul localitÄƒÈ›ii, mÄƒrimea reprezintÄƒ populaÈ›ia.

**ExerciÈ›iu**: simbolizaÈ›i localitÄƒÈ›ile din alt judeÈ›.

## Overpass - export

Putem salva rezultatele unui query Ã®n format GeoJSON. Alternativ, putem
construi o hartÄƒ care, la fiecare Ã®ncÄƒrcare, executÄƒ un query Overpass È™i
afiÈ™eazÄƒ rezultatele.


### Export GeoJSON

DupÄƒ ce am rulat un query Ã®n Overpass Turbo putem exporta rezultatul Ã®n format
GeoJSON din meniul "Export". Putem vizualiza È™i modifica fiÈ™ierul exportat prin
serviciul [geojson.io]().

![FiÈ™ier deschis Ã®n geojson.io](screenshots/geojson.io.png)

**ExerciÈ›iu**: RulaÈ›i un query din exercÈ›iile anterioare, salvaÈ›i rezultatul ca
GeoJSON, È™i Ã®ncarcaÈ›i-l Ã®n geojson.io.


### HartÄƒ web cu query live

HartÄƒ Leaflet care afiÈ™eazÄƒ rezultate din Overpass. Codul este generic,
afiÈ™eazÄƒ orice rezultate primim de la Overpass, Ã®n urma query-ului. Rezultatele
vor fi tot timpul la zi cu actualizÄƒrile din OpenStreetMap.

[parks.html]()

![Cafenele Ã®n Leaflet](screenshots/leaflet-cafes.png)

**ExerciÈ›iu**: SalvaÈ›i local fiÈ™ierul HTML È™i deschideÈ›i-l Ã®n browser.
ÃŽnlocuiÈ›i query-ul cu diverse query-uri din exerciÈ›iile anterioare.


### QGIS

[QGIS](https://www.qgis.org) deschide fiÈ™iere GeoJSON ca vector layer. DacÄƒ
vrem sÄƒ verificÄƒm poziÈ›ia datelor, putem instala plugin-ul "OpenLayers Plugin",
È™i din menul "web > OpenLayers Plugin", alegem o hartÄƒ ca background. ProiecÈ›ia
proiectului curent va deveni "EPSG:3857" â€“Â Web Mercator â€“ proiecÈ›ia folositÄƒ de
majoritatea slippy map-urilor.

![Cafenele Ã®n QGIS](screenshots/qgis-cafes.png)
