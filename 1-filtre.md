## Overpass - filtrare

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

![overpass - cafenele](screenshots/overpass.jpg)

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
