
# simplicity-esri
A implementation of SimpliCity that operates on ESRI Feature Services through the ArcGIS REST API with a focus on redeployability

##GETTING STARTED

###Installation

You'll need Node.js to work on SimpliCity. 

1. Clone the repo, and cd into that directory
2. Install dependencies with npm

         npm install
3. You can serve it locally with gulp, and it'll watch for changes
         
        gulp serve


##OVERVIEW

So the [current version SimpliCity] (https://github.com/cityofasheville/simplicity-ui) built by the City of Asheville has some [associated Node.js/PostGIS jobs] (https://github.com/cityofasheville/simplicity-backend) that run everynight on an AWS server. These jobs build a [data cache](http://arcgis-arcgisserver1-1222684815.us-east-1.elb.amazonaws.com/arcgis/rest/services/opendata/FeatureServer/12) layer which is pushed nightly to an instance of ArcGIS for Server as a feature service, along with all the other feature services used by SimpliCity. These Node.js/PostGIS jobs perform a bunch spatial queries for each address point in the city. In fact, the client side code for SimpliCity doesn't perform a single spatial query, they are all pre-run by the Node.js/PostGIS jobs. The Node.js/PostGIS jobs builds the cache by attaching the ids resulting for each spatial query to properties on each address point. For instance, for each address in the city the Node.js/PostGIS jobs determine all the crimes that lie within a certain radius of each address, and then attaches the ids of those crimes to a properties the corespond to that radius. So when the user goes to look at crimes around an address within the search radius mentioned, all the client side code has to is look up the crimes by id using the ArcGIS REST API instead of performing a spatial query at runtime.

This was done to speed up the app, and to make it possible to use a non-spatial API like civicdata.com.

####BUT...

All this pre-processing of spatial queries makes SimpliCity hard to re-deploy. CFA Brigades and cities would have to stand up a server to preprocess the data, which is probably just not going to happen. Instead, it would be much easier to just have a client side javascript app with some simple configuraton files that just uses the spatial query functionality available through the ArcGIS REST API to do everything. A lot of cities have instances of ArcGIS for Server running, and if not they most likely have access to ArcGIS Online which allows you to publish feature services. There is definitely the potential to build simplicity-cartodb or simplicity-geoserver, too.

##GOALS

* Re-code SimpliCity so that no longer uses the data cache layer when it needs to perform a spatial query but instead uses the ArcGIS REST API's spatial methods
* Come up with an easier to configure new topics without writing custom code for each topic
* Actually write some unit tests
* MORE TO FOLLOW HERE

##IDEAS
* Use esri-leaflet, there some nice syntax for making queries, it's already being included but it could be used more
* Explore using Jekyll with Angular
  * Using Jeykll's yaml files for configuration could interesting
  * It would be nice to add information about city services in markdown
* Refactor code into components to bring code base more inline with React and the direction that Angular 2 is moving

##DATASOURCE
This is current ArcGIS Server instance used by 
[City of Asheville's SimpliCity ArcGIS Feature Server](http://arcgis-arcgisserver1-1222684815.us-east-1.elb.amazonaws.com/arcgis/rest/services/opendata/FeatureServer)
