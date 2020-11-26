# Barcelona Atlas TopoJSON
This repository provides a simple script to generate TopoJSON files from [a mirror](https://github.com/martgnz/bcn-shp-zip) of the Barcelona City Council's [vector data](http://w20.bcn.cat/cartobcn/).

## Usage
In a browser (using [d3-geo](https://github.com/d3/d3-geo) and SVG):

```html
<!DOCTYPE html>
<svg width="960" height="500"></svg>
<script src="https://d3js.org/d3.v5.min.js"></script>
<script src="https://d3js.org/topojson.v3.min.js"></script>
<script>

const svg = d3.select("svg");
const path = d3.geoPath();

d3.json("https://unpkg.com/barcelona-atlas@0.2.0/barcelona/census_tracts.json")
  .then(barcelona => {
    svg
      .append('path')
      .attr('d', path(topojson.mesh(barcelona)))
      .attr('fill', 'none')
      .attr('stroke', 'black');
  })
  .catch(err => console.warn(err));

</script>

```

In Node (using [d3-geo](https://github.com/d3/d3-geo) and [node-canvas](https://github.com/Automattic/node-canvas)):

```js
var fs = require("fs"),
  d3 = require("d3-geo"),
  topojson = require("topojson-client"),
  Canvas = require("canvas"),
  barcelona = require("./node_modules/barcelona-atlas/barcelona/census_tracts.json");

var canvas = new Canvas(960, 500),
    context = canvas.getContext("2d"),
    path = d3.geoPath().context(context);

context.beginPath();
path(topojson.mesh(barcelona));
context.stroke();

canvas.pngStream().pipe(fs.createWriteStream("preview.png"));
```
## Generating the files
Clone or download the repo, start a terminal and run `npm install` in the folder. This command will run the script and move the generated files to the `barcelona` folder.

If you need to make further adjustments (simplification, quantization) you can change the `prepublish` script and run `npm install` again. 

## File Reference
<a href="#barcelona/census_tracts.json" name="barcelona/census_tracts.json">#</a> <b>barcelona/census_tracts.json</b> [Download TopoJSON](https://unpkg.com/barcelona-atlas@1.0.0/barcelona/census_tracts.json)

A preprojected TopoJSON ([EPSG:3043](http://spatialreference.org/ref/epsg/etrs89-etrs-tm31/)) which contains six objects: *census tracts*, *basic statistic areas*, *neighborhoods*, *big neighborhoods*, *districts* and *city*. Every tract, neighborhood and district has its corresponding identifier, so it's easy to get started. See [reference metadata](http://w20.bcn.cat/cartobcn/getFile.ashx?t=bdd&f=42588360097904).

<a href="#barcelona/census_tracts.json_census_tracts" name="barcelona/census_tracts.json_census_tracts">#</a> *barcelona*.objects.<b>census_tracts</b>

<img src="https://cloud.githubusercontent.com/assets/1236790/22386529/ca66b7ae-e4d7-11e6-942b-19f83226bccc.png" width="480" height="auto">

<a href="#barcelona/census_tracts.json_aeb" name="barcelona/census_tracts.json_aeb">#</a> *barcelona*.objects.<b>aeb</b>

<img src="https://cloud.githubusercontent.com/assets/1236790/22386553/e0337b58-e4d7-11e6-935b-7cb6f84f1d70.png" width="480" height="auto">

<a href="#barcelona/census_tracts.json_neighborhoods" name="barcelona/census_tracts.json_neighborhoods">#</a> *barcelona*.objects.<b>neighborhoods</b>

<img src="https://cloud.githubusercontent.com/assets/1236790/22386579/097eeea2-e4d8-11e6-9286-5218a5494292.png" width="480" height="auto">

<a href="#barcelona/census_tracts.json_big_neighborhoods" name="barcelona/census_tracts.json_big_neighborhoods">#</a> *barcelona*.objects.<b>big_neighborhoods</b>

<img src="https://user-images.githubusercontent.com/1236790/62585037-f07c2980-b8ae-11e9-85ec-ae63c12779e8.png" width="480" height="auto">

<a href="#barcelona/census_tracts.json_districts" name="barcelona/census_tracts.json_districts">#</a> *barcelona*.objects.<b>districts</b>

<img src="https://cloud.githubusercontent.com/assets/1236790/22386590/187484d0-e4d8-11e6-83e7-bbccb071b25b.png" width="480" height="auto">

<a href="#barcelona/census_tracts.json_city" name="barcelona/census_tracts.json_city">#</a> *barcelona*.objects.<b>city</b>

<img src="https://cloud.githubusercontent.com/assets/1236790/22386606/2f1ee3e2-e4d8-11e6-985b-be1d8fb66c81.png" width="480" height="auto">

## Source

Ajuntament de Barcelona / [CartoBCN](http://w20.bcn.cat/cartobcn/) ([CC-BY](http://w133.bcn.cat/geoportal/descargas/ca_ca_cond_us_carto.pdf)).

Last data update: 11/03/2020.

### Inspiration

The original idea and implementation comes from Mike Bostock’s [us-atlas](https://github.com/topojson/us-atlas) and [world-atlas](https://github.com/topojson/world-atlas).

Check out [es-atlas](https://github.com/martgnz/es-atlas) and [madrid-atlas](https://github.com/martgnz/madrid-atlas), which provide other Spanish administrative divisions with the same format.
