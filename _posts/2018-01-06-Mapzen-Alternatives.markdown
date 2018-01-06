---
layout: post
title: Mapzen shutdown :(  --->  Alternatives
date: 2018-01-06
---

> As this gorgeus service goes offline on 1 February 2018, I would like to save here their farewell message:

# Mapzen Alternatives

Mapzen will cease operations at the end of January 2018, and its hosted APIs and all related support and services will turn off on February 1, 2018.

Your two options are to switch to another hosted API that offers similar functionality, or to run your own servers with the open-source projects that powered Mapzen services. We list several options below to make this as easy as possible.

# Alternate services and hosted APIs

## Mapzen Vector Tiles and Cartography

### For basemaps and vector tiles, you may want to consider:

- [Mapbox](https://www.mapbox.com/maps/)
- [OpenMapTiles](https://openmaptiles.org/)
- [Thunderforest](https://thunderforest.com/docs/vector-maps-api/)

Terrain Tiles will continue to be available as an [Amazon Public Dataset](https://aws.amazon.com/public-datasets/terrain/).

Mapzen Vector Tiles and Cartography are based on the [Tangram](https://github.com/tangrams) and [Tilezen](https://github.com/tilezen/) open-source projects. Read below for information on how to run these services locally.

Tangram scene files saved in a [Tangram Play](https://github.com/tangrams/tangram-play) account can be saved to a [GitHub Gist](https://gist.github.com/) or downloaded locally, while Mapzen services are still running.

## Mapzen Turn-by-Turn routing

For routing and navigation, you may want to consider these services:

- [GraphHopper](https://www.graphhopper.com/)
- [Mapbox Directions API](https://www.mapbox.com/navigation/)
- [Project OSRM](http://project-osrm.org/)

Mapzen’s routing and navigation projects were backed by Valhalla, an open-source project. Read below for how to run Valhalla locally.

## Mapzen Search

For search and geocoding, you may want to consider these services:

- [Nominatim](http://nominatim.openstreetmap.org/)
- [OpenCage Data](https://geocoder.opencagedata.com/)
- [OSMNames](https://osmnames.org/)
- [Mapbox Search API](https://www.mapbox.com/geocoding/)

Mapzen Search was backed by [Pelias](https://github.com/pelias/), an open source project. Read below for how to run this service locally.

## Mobile

For Android and iOS, you may want to consider these third party SDKs:

- [Mapbox](https://www.mapbox.com/mobile/)

In the event you decide to host a version of Valhalla and Pelias locally, you can continue to use our mobile SDKs we’ve built to interact with that service.

Mapzen iOS and Android SDKs, along with On The Road SDKs for Valhalla will be available at [https://github.com/nextzen](https://github.com/nextzen).

Our LOST project for Android will be available at [https://github.com/lostzen/](https://github.com/lostzen/).

Pelias and tangram-es SDKs will continue to be available from their respective repositories.

## Transitland

Transitland will continue as an open-data project at https://transit.land

Transitland’s server and data operations through 2018 will be supported by Amazon Web Services, thanks to an award from the [Earth on AWS Cloud Credits for Research Program](https://aws.amazon.com/earth/research-credits/).

Continue to follow @transitland on Twitter for updates.

## Who’s On First / Mapzen Places

ata (individual records) and source code for Who’s On First will continue to be hosted at https://github.com/whosonfirst, but *whosonfirst.mapzen.com* and the Mapzen Places API will go offline.

Who’s On First will continue (in a scaled back version, for now) at https://whosonfirst.org.

- Data (individual records) and source code are at https://github.com/whosonfirst.
- All of the data and code and blog posts have also been cloned to the Internet Archive.
- The Spelunker will go offline, in the short term.
- Batch processing and cascading edits will stop in the short term.
- A limited version of the API that returns individual records will be at https://whosonfirst.org, with additional functionality growing over time (spatial queries, point-in-polygon, editorial tool, cascading updates).

Follow along at https://www.whosonfirst.org for updates.

## Metro Extracts

Metro Extracts will no longer be updated, and the custom extract service will no longer be available. We are working on making a copy of the existing extracts available.

The Humanitarian Open Streetmap Team has created the [HOT Export Tool](https://export.hotosm.org/) that creates customized extracts of up-to-date OSM data in various file formats, including PBFs. Please consider [donating to HOT](https://www.hotosm.org/donate), whether or not you use this service.

For non planet-sized PBF extracts, you may want to consider [Geofabrik](https://www.geofabrik.de/) or the [Overpass API](http://wiki.openstreetmap.org/wiki/Overpass_API).

## Documentation and blog posts

Blog posts will be archived on https://medium.com/postzen.

Documentation will be available in each open-source GitHub repo:

| Documentation section | Source location |
|:----------------------|:----------------|
| Overview    | https://github.com/mapzen/mapzen-docs-generator/tree/master/docs |
| mapzen.js   | https://github.com/mapzen/mapzen.js/tree/master/docs |
| Tangram | https://github.com/tangrams/tangram-docs |
| Vector tiles    | https://github.com/tilezen/vector-datasource/tree/master/docs |
| Search  | https://github.com/pelias/pelias-doc |
| Mobility    | https://github.com/valhalla/valhalla-docs |
| Metro Extracts  | https://github.com/mapzen/metro-extracts/tree/master/docs |
| Terrain tiles   | https://github.com/tilezen/joerd |
| Elevation   | https://github.com/valhalla/valhalla-docs |
| Android SDK | https://github.com/mapzen/android/tree/master/docs |
| iOS SDK | https://github.com/mapzen/ios/blob/master/docs |
| Address parsing/libpostal   | https://github.com/whosonfirst/go-whosonfirst-libpostal/blob/master/docs |
| Who’s On First  | https://github.com/whosonfirst/whosonfirst-www-api/tree/master/docs |
| Cartography | https://github.com/tangrams/cartography-docs/ |
| | |

## Run open-source versions of Mapzen services

Mapzen services are backed by open-source software projects, and use open data. Even with limited technical knowledge, you can run these yourself for metro-sized areas using a local web server or Docker containers.

## Tangram and Tilezen (Cartography and Vector Tiles)

Documentation for Tangram is available at https://github.com/tangrams/tangram-docs.

[tangram.js](https://github.com/tangrams/tangram), [tangram.es](https://github.com/tangrams/tangram-es), Mapzen.js, [Tangram Play](https://github.com/tangrams/tangram-play)xcxxc  are all open-source projects that can be run locally.

## Basemap styles

[Refill](https://github.com/tangrams/bubble-wrap), [Walkabout](https://github.com/tangrams/walkabout-style), [Bubble-wrap]https://github.com/tangrams/bubble-wrap, [Tron](https://github.com/tangrams/tron-style) and [Cinnabar](https://github.com/tangrams/cinnabar-style) basemap styles will continue to be available in their repositories in https://github.com/tangrams and served via tangrams.github.io.

## Map data

While Tangram can import non-tiled GeoJSON and TopoJSON, vector tiles are what makes it interesting.

The Docker version of Tilezen can dynamically serve tiles from a Metro Extract or reasonably-sized PBF.

- Tilezen in Docker https://github.com/tilezen/tileserver/blob/master/DOCKER.md

To run a map for a city or a region with no network dependencies:

- Save mapzen.js / tangram.js, index.html and scene.yaml file in a directory
- Extract a pyramid of tiles for area of interest using tilepack and save them in that same directory. (Note you don’t need to go beyond z15 tiles as they contain all the data in higher (>= 16) tiles, and Tangram overzooms.)

To download tiles from our hosted services before shutdown:

```
https://github.com/tilezen/tilepacks MAPZEN_API_KEY=mapzen-xxxxxx tilepack -122.51489 37.70808 -122.35698 37.83239 10 15 sf_mvt_10_15 --tile-size 512
```

Size and download speeds makes this viable for a city and perhaps a region. The example above generates a 30MB pyramid of MVT tiles, or 138MB of topojson, for San Francisco. For GeoJSON, SF is 67MB at z14, and 130MB at z15.

- Serve tiles via a web server (localhost or hosted).
- Create a scene file for Tangram. Note you will need to override the mapzen source in the scene file if you are importing a basemap.

```
sources:
    mapzen:
        type: MVT
        url:  localhost:8000/sf_mvt_10_15/all/{z}/{x}/{y}.mvt
        tile_size: 512
        max_zoom: 15
```

## Valhalla (routing)

Documentation for Valhalla is available at https://github.com/valhalla/valhalla-docs.

Instructions on running Valhalla in Docker using data from a Metro Extract or PBF are at https://github.com/valhalla/docker.

## Pelias (search)

Documentation for Pelias is available at http://pelias.io and https://github.com/pelias/pelias-doc.

Instructions on how to run Pelias in Docker with data from a Metro Extract or PBF and OpenAddresses data are at https://github.com/pelias/dockerfiles/blob/master/how_to_guide.pdf.

General instructions on running and installing Pelias and Elasticsearch: http://pelias.io/install.html

## Mobile

Android and iOS SDK source code and LOST will available at https://github.com/lostzen

***

# Original link - https://mapzen.com/blog/migration/