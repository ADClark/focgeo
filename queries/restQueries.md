# Complete documentation of REST API usage
> https://docs.geoserver.org/2.16.x/en/user/rest/index.html#rest

## To add a feature type and simultaneously create a PostGIS table:
> curl --verbose --user admin -XPOST -H "Content-type: text/xml" --data-binary @roiFeatureType.xml  http://localhost:9000/geoserver/rest/workspaces/focgeo/datastores/focgeo/featuretypes
*The above curl request will prompt for admin password*

## To add a feature to the previously created FeatureType:
> curl --verbose --user admin -XPOST -H "Content-type: text/xml" --data-binary @roiFeature.xml http://localhost:9000/geoserver/focgeo/wfs
*The above curl request will prompt for admin password*
