# Circle

## Basic usage

```example height:400
<script>
    import {LeafletMap, Circle, Popup, TileLayer, Tooltip} from '$lib/vendor/svelte-leaflet';

    const mapOptions = {
        center: [1.250111, 103.830933],
        zoom: 14,
    };
    const tileUrl = "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png";
    const tileLayerOptions = {
        minZoom: 0,
        maxZoom: 20,
        maxNativeZoom: 19,
        attribution: "© OpenStreetMap contributors",
    };
</script>

<div class="example">
    <LeafletMap options={mapOptions}>
        <TileLayer url={tileUrl} options={tileLayerOptions}/>
        <Circle latLng={[1.250111, 103.830933]} radius={1000} color="#ff0000" fillColor="#ff0000">
            <Popup>Sentosa</Popup>
            <Tooltip>Sentosa</Tooltip>
        </Circle>
    </LeafletMap>
</div>
```

## Properties

See https://leafletjs.com/reference-1.7.1.html#circlemarker

```properties
latLngs     | Geographical points.    | LatLng[]
color       | Stroke color.           | String("#3388ff")
weight      | Stroke width in pixels. | Number(3)
opacity     | Stroke opacity.         | Number(1.0)
lineCap     | Line cap shape.         | String("round")
lineJoin    | Line join shape.        | String("round")
dashArray   | Dash pattern.           | String(null)
dashOffset  | Dash offset.            | String(null)
fill        | Fill flag.              | Boolean()
fillColor   | Fill color.             | String("#3388ff")
fillOpacity | Fill opacity.           | Number(0.2)
fillRule    | Fill rule.              | String("evenodd")
options     | Options.                | Object(undefined)
```

## Methods

| Name        | Description                                                                                                    |
| ----------- | -------------------------------------------------------------------------------------------------------------- |
| getCircle() | Returns the underlying Leaflet `Circle` object instance. See https://leafletjs.com/reference-1.7.1.html#circle |
