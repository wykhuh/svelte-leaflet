<script>
  import { getContext, onDestroy } from 'svelte';
  import L from 'leaflet';
  import 'leaflet.markercluster';
  import 'leaflet.markercluster/dist/MarkerCluster.css';
  import 'leaflet.markercluster/dist/MarkerCluster.Default.css';

  const { getMap } = getContext(L);

  export let items;

  let markerCluster;

  $: {
    if (markerCluster) {
      markerCluster.remove();
      markerCluster = null;
    }

    if (!markerCluster) {
      markerCluster = L.markerClusterGroup({ singleMarkerMode: true }).addTo(getMap());
    }

    if (items.length > 0) {
      items.forEach((ob) => {
        // BUG: circle marker does not work with singleMarkerMode
        markerCluster.addLayer(L.marker([ob.latitude, ob.longitude]));
      });
    }
  }

  onDestroy(() => {
    markerCluster.remove();
  });

  export function getLayerControl() {
    return markerCluster;
  }
</script>
