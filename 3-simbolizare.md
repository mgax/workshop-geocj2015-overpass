## Overpass Turbo - simbolizare

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

node[place=village] { color: brown; }
node[place=town] { color: blue; }
node[place=city] { color: red; }
node { symbol-size: 2; }
node[population>100] { symbol-size: 4; }
node[population>10000] { symbol-size: 8; }
node[population>100000] { symbol-size: 12; }

}}
```
