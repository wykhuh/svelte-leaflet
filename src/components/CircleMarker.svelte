<script>
  import { createEventDispatcher, getContext, onDestroy, setContext } from 'svelte';
  import L from 'leaflet';

  import EventBridge from '../lib/EventBridge';

  const { getMap } = getContext(L);

  export let latLng;
  export let radius = 10;
  export let color = '#3388ff';
  export let weight = 3;
  export let opacity = 1.0;
  export let lineCap = 'round';
  export let lineJoin = 'round';
  export let dashArray = null;
  export let dashOffset = null;
  export let fill = true;
  export let fillColor = '#3388ff';
  export let fillOpacity = 0.2;
  export let fillRule = 'evenodd';
  export let options = {};
  export let events = [];

  let circleMarker;

  setContext(L.Layer, {
    getLayer: () => circleMarker
  });

  const dispatch = createEventDispatcher();
  let eventBridge;

  $: {
    if (!circleMarker) {
      circleMarker = L.circleMarker(latLng, options).addTo(getMap());
      eventBridge = new EventBridge(circleMarker, dispatch, events);
    }
    circleMarker.setLatLng(latLng);
    circleMarker.setRadius(radius);
    circleMarker.setStyle({
      color: color,
      weight: weight,
      opacity: opacity,
      lineCap: lineCap,
      lineJoin: lineJoin,
      dashArray: dashArray,
      dashOffset: dashOffset,
      fill: fill,
      fillColor: fillColor,
      fillOpacity: fillOpacity,
      fillRule: fillRule
    });
  }

  onDestroy(() => {
    eventBridge.unregister();
    circleMarker.removeFrom(getMap());
  });

  export function getCircleMarker() {
    return circleMarker;
  }
</script>

<div>
  {#if circleMarker}
    <slot />
  {/if}
</div>
