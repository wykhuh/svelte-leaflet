<script>
  import { getContext, onDestroy } from 'svelte';
  import L from 'leaflet';
  import 'leaflet-easybutton';
  import 'leaflet-easybutton/src/easy-button.css';

  const { getMap } = getContext(L);

  export let icon;
  export let callback;
  export let states;
  export let title;

  let button;

  $: {
    if (!button) {
      if (icon && callback) {
        button = L.easyButton(icon, callback, title).addTo(getMap());
      } else {
        button = L.easyButton(states).addTo(getMap());
      }
    }
  }

  onDestroy(() => {
    button.remove();
  });

  export function getButton() {
    return button;
  }
</script>
