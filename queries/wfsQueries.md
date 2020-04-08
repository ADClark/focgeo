# Complete documentation of WFS usage
> https://docs.geoserver.org/stable/en/user/services/wfs/index.html

## To query the service and return all existing "Region of Interest" (ROI) features as JSON:
> http://localhost:9000/geoserver/focgeo/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=focgeo:roi&outputFormat=JSON

## Query geoserver WFS with WKT polygon, check for intersections with any existing "Region of Interest" (ROI) features, and return those intersected features as JSON:
> http://localhost:9000/geoserver/focgeo/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=focgeo:roi&outputFormat=JSON&CQL_FILTER=INTERSECTS(geometry, POLYGON((-93.17 42.857,-93.389 42.457,-93.504 41.527,-95.535 41.311,-96.675 40.66,-99.617 40.519,-100.784 40.931,-101.661 40.94,-102.818 40.624,-103.451 40.14,-104.335 39.995,-104.918 39.679,-105.45 40.018,-105.336 40.182,-104.925 40.061,-104.488 40.322,-103.631 40.455,-103.008 40.931,-101.731 41.293,-100.75 41.29,-99.112 40.851,-98.43 40.997,-96.811 40.996,-95.676 41.644,-93.881 41.755,-93.755 41.81,-93.748 42.51,-93.528 42.918,-93.446 44.985,-93.224 45.153,-93.088 45.006,-93.17 42.857)))
