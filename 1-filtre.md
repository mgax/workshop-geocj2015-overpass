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
[restaurantele](http://wiki.openstreetmap.org/wiki/Tag:amenity%3Drestaurant),
[hotelurile](http://wiki.openstreetmap.org/wiki/Tag:tourism%3Dhotel) și
[spitalele](http://wiki.openstreetmap.org/wiki/Tag:amenity%3Dhospital) din
zonă.


### query "și"
```
node
  [amenity=cafe]
  [wheelchair=yes]
  ({{bbox}});
out;
```

![amenity=cafe,wheelchair=yes](screenshots/amenity=cafe,wheelchair=yes.png)

Exercițiu: extrageți spitalele care au cameră de gardă (`emergency=yes`).


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

Exercițiu: extrageți toate [cafenelele](http://wiki.openstreetmap.org/wiki/Tag:amenity%3Dcafe), [barurile](http://wiki.openstreetmap.org/wiki/Tag:amenity%3Dbar), [pub-urile](http://wiki.openstreetmap.org/wiki/Tag:amenity%3Dpub) și [cluburile](http://wiki.openstreetmap.org/wiki/Tag:amenity%3Dnightclub).


### expresie regulată
```
node
  [amenity~"cafe|bar|restaurant"]
  ({{bbox}});
out;
```

![amenity~"cafe|bar|restaurant"](screenshots/amenity~"cafe|bar|restaurant".png)

Exercițiu: La fel ca punctul precedent, scris sub formă de expresie regulată


### query "tot"
```
(
  node({{bbox}});
  <;
);
out meta;
```

![toată informația din zonă](screenshots/all.png)

Returnează toate datele din regiunea vizibilă. Operatorul `<;` înseamnă "ia
obiectele părinte" (poligoane și relații care conțin noduri din zonă)


### query "unde sunt?"
```
[out:json];
is_in({{center}});
out;
```

![unde mă aflu?](screenshots/where.png)

Returnează toate poligoanele care conțin centrul hărții vizibile (clădiri,
teren, regiuni administrative)
