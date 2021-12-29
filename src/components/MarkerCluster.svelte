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
    }

    markerCluster = L.markerClusterGroup({ singleMarkerMode: true });

    items.forEach((ob) => {
      // BUG: circle marker does not work with singleMarkerMode
      markerCluster.addLayer(L.marker([ob.latitude, ob.longitude]));
    });
    markerCluster.addTo(getMap());
  }

  onDestroy(() => {
    markerCluster.remove();
  });

  export function getLayerControl() {
    return markerCluster;
  }
</script>
