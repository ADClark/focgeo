# Complete documentation of REST API usage
> https://docs.geoserver.org/2.16.x/en/user/rest/index.html#rest

## REST data can also be accessed via web browser at the appropriate URLS, after a prompt to enter username and password
For example, to get a list of feature types for the workspace and datastore, visit this link with your browser:
> http://localhost:9000/geoserver/rest/workspaces/focgeo/datastores/focgeo/featuretypes
*The above URL will prompt for username and password*

## The in-browser data can be viewed as html or xml by changing the extension of the URL:
> http://localhost:9000/geoserver/rest/workspaces/focgeo/datastores/focgeo/featuretypes/roi.html
-or-
> http://localhost:9000/geoserver/rest/workspaces/focgeo/datastores/focgeo/featuretypes/roi.xml

## To add a feature type and simultaneously create a PostGIS table:
> curl --verbose --user admin -XPOST -H "Content-type: text/xml" --data-binary @roiFeatureType.xml  http://localhost:9000/geoserver/rest/workspaces/focgeo/datastores/focgeo/featuretypes
*The above curl request will prompt for admin password*

## To add a feature to the previously created FeatureType:
> curl --verbose --user admin -XPOST -H "Content-type: text/xml" --data-binary @roiFeature.xml http://localhost:9000/geoserver/focgeo/wfs
*The above curl request will prompt for admin password*

## To update the XML of an individual featuretype (this example uses the ROI featuretype):
> curl --verbose --user admin -XPUT -H "Content-type: text/xml" --data-binary @roiFeatureType.xml http://localhost:9000/geoserver/rest/workspaces/focgeo/datastores/focgeo/featuretypes/roi.xml 
