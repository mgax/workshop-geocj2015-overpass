## Overpass - filtrare

http://overpass-turbo.eu/

![overpass](screenshots/overpass.jpg)

### filtru după tag

```
node
  [amenity=cafe]
  ({{bbox}});
out;
```

![amenity=cafe](screenshots/amenity=cafe.png)

În tab-ul "data" din dreapta putem vedea rezultatul în format OpenStreetMap
XML.

Exercițiu: extrageți
[restaurantele](http://wiki.openstreetmap.org/wiki/Tag:amenity%3Drestaurant) și
[hotelurile](http://wiki.openstreetmap.org/wiki/Tag:tourism%3Dhotel) din zonă.

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

### query "tot"
```
(
  node({{bbox}});
  <;
);
out meta;
```

Returnează toate datele din regiunea vizibilă. Operatorul `<;` înseamnă "ia
obiectele părinte" (poligoane și relații care conțin noduri din zonă)


### query "unde sunt?"
