## Query la Overpass

http://overpass-turbo.eu/

### query "tot"
```
(
  node({{bbox}});
  <;
);
out meta;
```

* `<;` înseamnă "ia obiectele părinte" (poligoane și relații care conțin noduri
  din zonă)

### filtru după tag

```
node
  [amenity=cafe]
  ({{bbox}});
out;
```

screenshots/overpass.jpg

* ne uităm la datele osm xml
* export - geojson
* open in geojson.io
* export - save as gist

### query "și"
```
node
  [amenity=cafe]
  [wheelchair=yes]
  ({{bbox}});
out;
```

### query "sau"
```
(
  node
    [amenity=cafe]
    ({{bbox}});
  node
    [amenity=restaurant]
    ({{bbox}});
);
out;
```

### expresie regulată
```
node
  [amenity~"cafe|bar|restaurant"]
  ({{bbox}});
out;
```

### query "unde sunt?"


### query regiune
```
relation
  [place=city]
  [name="Cluj-Napoca"];

>;
out;
```

* `>;` înseamnă "ia obiectele copil" (nodurile care formează poligonul)


### limit query to city
```
area[place=city][name="Cluj-Napoca"];

node
  [amenity=cafe]
  (area);
out;
```

### zone
```
area[place=city][name="Cluj-Napoca"];

(
  way[leisure~"park|garden"](area);
  >;
);
out;
```

### structuri complexe
```
area[place=city][name="Cluj-Napoca"];

(
  relation
    [route=bus]
    [ref=22]
    (area);
  >;
);
out;
```

```
area[name="România"];

way
  (area)
  [power=line]
  [voltage~"...000"];
out geom;
```

### styling
```
area[name="România"];

way
  (area)
  [power=line]
  [voltage~"...000"];
out geom;

{{style:
line[voltage=400000]
{ color:red; }

line[voltage=220000]
{ color:yellow; }

}}
```

```
area[place=county][name="Cluj"];

node(area)[place~"city|town|village"];
out;

{{style:

node[place=village] { color: steelblue; }
node[place=town] { color: blue; }
node[place=city] { color: red; }
node { symbol-size: 2; }
node[population>100] { symbol-size: 4; }
node[population>10000] { symbol-size: 8; }
node[population>100000] { symbol-size: 12; }

}}
```


### Optimizare

* `out skel qt;` înseamnă "dă-mi rezultate fără tag-uri și nesortate" (mai rapid)


## Hartă web

<iframe
  src="https://rawgit.com/mgax/workshop-geocj2015-overpass/master/parks.html"
  style="border: 1px solid #888; height: 300px; width: 500px"
  ></iframe>


## QGIS

* open geojson file
* install openlayers plugin
* show osm background (harta a schimbat proiecția)
