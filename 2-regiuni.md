## Overpass - regiuni

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
