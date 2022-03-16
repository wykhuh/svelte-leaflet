<script>
  import { getContext, onDestroy } from 'svelte';
  import L from 'leaflet';

  const { getMap } = getContext(L);

  export let baseLayersData = {};
  export let overlayLayersData = [];

  let baseLayers = {};
  let overlayLayers = {};

  Object.entries(baseLayersData).forEach(([key, value], index) => {
    let layer = L.tileLayer(value.url, value.options);
    baseLayers[key] = layer;

    if (index === 0) {
      layer.addTo(getMap());
    }
  });

  function onEachFeature(feature, layer, overlay) {
    if (feature.properties) {
      let popupContent = '';
      if (overlay.popup_fields) {
        popupContent += '<table class="popup-table not-prose">\n';
        overlay.popup_fields.forEach((field) => {
          popupContent += '<tr>\n';
          popupContent += '<th>' + field.name + '</th>';
          popupContent += '<td>' + feature.properties[field.original_name] + '</td>';
          popupContent += '</tr>\n';
        });
        popupContent += '</table>';
      } else {
        popupContent = JSON.stringify(feature.properties);
      }
      layer.bindPopup(popupContent);
    }
  }

  overlayLayersData.forEach((overlay) => {
    let layer;
    let options = {
      ...overlay.options,
      onEachFeature: (feature, layer) => onEachFeature(feature, layer, overlay),
      style: function (feature) {
        return {
          color: feature.properties.color,
          opacity: overlay.opacity || 1,
          fillOpacity: overlay.fillOpacity || 0.2
        };
      }
    };
    if (overlay.type === 'geojson') {
      layer = L.geoJSON(overlay.data, options);
      overlayLayers[overlay.name] = layer;
    }
  });

  let layerControl;

  $: {
    if (!layerControl) {
      layerControl = L.control.layers(baseLayers, overlayLayers).addTo(getMap());
    }
  }

  onDestroy(() => {
    layerControl.remove();
  });

  export function getLayerControl() {
    return layerControl;
  }
</script>
