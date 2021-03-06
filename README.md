# GeoJSON API

A Node web server that periodically loads a GeoJSON file with points in it and exposes an API to find the nearest points from a lat/lon.

Every 60 seconds, the server loads a URL, parses it as GeoJSON, and dumps the `Point`s contained therein into a [`sphere-knn`](https://github.com/darkskyapp/sphere-knn) object. It exposes an API to query that KNN dataset by lat/lon or zipcode. Since the data is stored in memory and KNN is a very fast algorithm, searching can be performed very quickly: usually in under 10ms.

This is a more generic version of [`divvyapiapi`](https://github.com/iandees/divvyapiapi/).

## Configuration

The GeoJSON URL must be configured by setting the `GEOJSON_URL` environment variable to an HTTP/HTTPS URL to a GeoJSON with a `FeatureCollection` of `Point` type `Feature`s.

## API

The service exposes a single API endpoint:

### `GET /points/nearby`

Returns the nearest points to a given location. The location can be specified with a `lat`/`lon` pair or a `zipcode`.

The API responds with a [`FeatureCollection`](https://tools.ietf.org/html/rfc7946#section-3.3) of the results.

| Query Arg Name | Description |
|:---------------|:------------|
| `lat`          | The latitude for the center point to search from. |
| `lon`          | The longitude for the center point to search from. |
| `zipcode`      | A US Zipcode to use as the center point to search from. |
| `limit`        | An integer 1 to 50. Defaults to 5. The maximum number of points to return. |
