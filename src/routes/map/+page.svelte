<script>
  import { onMount } from 'svelte';
  import 'ol/ol.css';
  import Map from 'ol/Map';
  import View from 'ol/View';
  import TileLayer from 'ol/layer/Tile';
  import OSM from 'ol/source/OSM';
  import VectorLayer from 'ol/layer/Vector';
  import VectorSource from 'ol/source/Vector';
  import GeoJSON from 'ol/format/GeoJSON';
  import axios from 'axios';

  let map;

  onMount(() => {
    // Initialize a map with full-screen width and height
    map = new Map({
      target: 'map',
      layers: [
        new TileLayer({
          source: new OSM(),
        }),
      ],
      view: new View({
        center: [0, 0],
        zoom: 2,
      }),
    });

    // Load and add the GeoJSON data to the map
    axios
      .get('https://raw.githubusercontent.com/datasets/geo-countries/master/data/countries.geojson')
      .then((response) => {
        const geoJsonSource = new VectorSource({
          features: new GeoJSON().readFeatures(response.data),
        });

        const geoJsonLayer = new VectorLayer({
          source: geoJsonSource,
          style: new Style({
            fill: new Fill({
              color: 'rgba(0, 106, 78, 0.75)', // #006a4e with opacity 0.75
            }),
            stroke: new Stroke({
              color: 'rgba(0, 0, 0, 0.2)',
              width: 2,
            }),
          }),
        });

        map.addLayer(geoJsonLayer);
      })
      .catch((error) => {
        console.error('Error loading GeoJSON data:', error);
      });
  });
</script>

<style>
  #map {
    width: 100%;
    height: 100vh;
  }
</style>

<div id="map"></div>