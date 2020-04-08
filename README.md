# focgeo

**Dockerized GeoServer 2.16.2 + PostGIS 12 orchestrated via Docker-compose**

This application is a customization of Kartoza's Docker-GeoServer recipe (https://github.com/kartoza/docker-geoserver), and Kartoza's PostGIS (https://hub.docker.com/r/kartoza/postgis) image.

The application's intended use is to store and serve geospatial regions of scientific interest to the Flyover Country app and encourage users passing through those regions to collect data through the GLOBE Observer app.


## Quick Start

1) Pull the images:
>        docker pull adclark/focgeo:geoserver.2.16.2
>        docker pull kartoza/postgis:11.5-2.5

2) Clone or download this repository

3) Update any environmental variables for the application by editing the values in docker-env/geoserver.env

**Recommended variables to change:**
* GEOSERVER_ADMIN_PASSWORD
* Variables related to self-signed certificate: PKCS12_PASSWORD, JKS_KEY_PASSWORD, JKS_STORE_PASSWORD.
* **NOTE** *The values assigned to each of the three keystore-related variables should be equivalent*

4) Execute the following command from within the root of the project directory
>       docker-compose up -d

5) Access the GeoServer Web UI
>       http://localhost:9000/geoserver
>       https://localhost:9443/geoserver


6) Log in as admin, using the password set in geoserver.env


## Security

In order to share the repository and image openly without revealing passwords or secrets, this project invokes Kartoza's run-time script "update_passwords.sh", which will set the admin password, as well as the SSL keystore passwords, based on the environmental variables assigned in docker-env/geoserver.env

An alternative approach would be to supply a pre-configured security settings directory at resources/overlays/opt/geoserver/data_dir/security

> If you have an already existing `data_dir` with a security setup from another Geoserver: set `EXISTING_DATA_DIR=true`.
> This will keep the passwords from getting changed by docker.

**Recommended settings to change after starting docker geoserver, via Web UI**

* Strong PBE.

* Encrypted web admin URL parameters

* Change the Root user password: Security->Passwords: Near the top of the page, select "change password". By default, the root user cannot login as admin.


## Data Store

Settings for the 'db' PostGIS Docker service can be modified via docker-env/db.env

In this case, the db service is accessible only via the docker network, and it will start up with a database named "focgeo".

To connect to the PostGIS database as a Geoserver Vector Data Source for a given workspace, select "Stores->Add new Store->PostGIS." Use the settings specified below:

* Connection Parameters:
  - host : db
  - port : 25432
  - database : focgeo
  - schema : public
  - user : *specified in db.env*
  - passwd : *specified in db.env*
  - Loose bbox : unchecked
  - Estimated extends : unchecked
  - Encode functions : unchecked
  - Support on the fly geometry simplification: unchecked

The proposed schema for the regions of interest is defined in ./schema/roiFeatureType.xml

An example instance of a single feature following the roiFeatureType schema is provided in ./schema/roiFeature.xml


### Using the WFS API

A few basic sample queries are defined under ./queries/wfsQueries.md


### Using the REST API

A few basic sample queries are defined under ./queries/restQueries.md

## Project-specific customizations

* Image was built using ./build.sh with pre-configured geoserver data_dir overlay. Settings applied via overlay:
  - WFS is the only service enabled
    - Encode canonical WFS schema location
    - Encode response with Multiple "featureMember" elements
  - Disabled raster rendering and caches

* Application memory limited to max of 512m via docker-compose:
    * PostGIS mem_limit: 128m
    * Geoserver mem_limit: 384m

* While not included in this recipe or image, additional customizations will be made post-deployment



#### Active GeoServer Extensions

* app-schema: https://docs.geoserver.org/stable/en/user/data/app-schema/

* importer: https://docs.geoserver.org/stable/en/user/extensions/importer/index.html

* Kartoza default extensions loaded in setup.sh:
  * vectortiles
  * wps
  * printing
  * libjpeg-turbo
  * control-flow
  * pyramid
  * gdal



#### Kartoza Docker-PostGIS

* Provides ssl support out of the box
* Connections are restricted to the docker subnet
* Replication support included
* Ability to create multiple database when you spin the database.
* Enable multiple extensions in the database when setting it up
* Gdal drivers automatically registered for pg raster
* Support for out-of-db rasters

#### Extended Documentation

Geoserver (2.16.x) Documentation: https://docs.geoserver.org/2.16.x/en/user/

Geoserver (2.16.2) PDF User Manual Download: http://sourceforge.net/projects/geoserver/files/GeoServer/2.16.2/geoserver-2.16.2-user-manual.pdf

Geoserver WFS Documentation: https://docs.geoserver.org/2.16.x/en/user/services/wfs/index.html

Geoserver Security Documentation: https://docs.geoserver.org/2.16.x/en/user/security/

Geoserver Production Deployment considerations: https://docs.geoserver.org/2.16.x/en/user/production/index.html
