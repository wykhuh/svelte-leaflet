<script>
  import { getContext, onDestroy } from 'svelte';
  import L from 'leaflet';

  const { getMap } = getContext(L);

  export let baseLayersData = {};
  export let overlayLayersData = {};

  let baseLayers = {};
  let overlayLayers = {};

  Object.entries(baseLayersData).forEach(([key, value], index) => {
    let layer = L.tileLayer(value.url, value.options);
    baseLayers[key] = layer;

    if (index === 0) {
      layer.addTo(getMap());
    }
  });

  Object.entries(overlayLayersData).forEach(([key, value], index) => {
    overlayLayersData[key] = L.tileLayer(value.url, value.options);
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
