# Home Assistant Custom Sensor for GTFS Realtime Ireland
This project builds on the work of Zacs https://github.com/zacs/ha-gtfs-rt, the existing GTFS Integration in home assistant https://www.home-assistant.io/integrations/gtfs/ and others to provide a custom transport sensor for GTFS realtime in Ireland.  
This sensor consumes the GTFS realtime feed at "https://gtfsr.transportforireland.ie/v1"
The sensor also requires a download of the static schedule zip file which the sensor will automatically load into a SQLite database.   
Further details are at the Irish National Transport Authority website https://developer.nationaltransport.ie/api-details#api=gtfsr&operation=gtfsr

# Setup and Installation

Before starting the installation you will need to install 2 python modules:
* gtfs-realtime-bindings==0.0.7
* protobuf==3.20.1

1. Subscribe to the API on the Transport Authority's web page to obtain the API Token/Key which you will need to access the realtime data.  This is free and takes about 15 mins.
2. Download the static schedule data zip file provided on the same page.  It's called "GTFS info for use in conjunction with GTFS-R API"
3. Rename the download file as you like e.g. ireland.zip, this name will be part of the configuration in Home Assistant
4. Create a new directory in your Home Assistant config folder called "gtfs"
5. Upload the schedule data zip file e.g. ireland.zip to the gtfs folder 
6. Also create a new folder called "custom_components" and inside that folder create a folder called "gtfs-rt-irl"
7. Download this repository as a Zip file and unzip
6. Copy the unzipped files to the custom_components/gtfs-rt-irl folder
7. Configue the sensor in Home Assistant, see below
8. Restart Home Assistant.   The sensor will begin loading the database with the static data from the zip file and this will take some time depending on your technical set up, maybe hours.

# Configuration

Add the following configuration to your configuration.yaml file.  This example is for 2 bus stops / routes in Dublin.  Each one uses a different operator, i.e. Dublin Bus and Go Ahead

```sensor:
  - platform: gtfs-rt-irl
    trip_update_url: "https://gtfsr.transportforireland.ie/v1"
    api_key: "******** your API key ***************"
    schedule_zip_file: "ireland.zip"
    arrivals_limit: 40
    departures:
      - stop_name: "St Enda's Park, stop 2992"
        route: "16"
        operator: '978'

      - stop_name: "Eden Avenue, stop 2967"
        route: "175"
        operator: '03'
```        
       
# Configuration Variables:

* __trip_update_url__ (_Required_): The production realtime feed URL as provided by the transport authority. 
* __api_key__ (_Required_): Provided by the transport authority when you subscribe.
* __schedule_zip_file__ (_Required_): The name of the zip file in the gtfs folder containing the static schedule data.
* __arrivals_limit__ (_Optional default=30_):  The number of arrivals found to be returned, across all the stops required.
* __departures__(_Required_): The list of route / stop / operator combinations of interest.  At least 1 must be specified.
* __stop_name__ (_Required_): The required Stop. This should be an exact copy of the stop_name field in the stops.txt data file 
* __route__: (_Required_): The required Route. This should be an exact copy of the route_short_name field in the routes.txt data file
* __operator__: (_Required_): The required operator. This is the agency_id field in the agency.txt data file

# Extra Attributes

The sensor implementation provides some extra state attributes as follows:

* __Stop ID__:  The stop ID as opposed to the name, useful for verifying the configuration 
* __Route__:   The route ID, useful for verifying the configuration
* __Next arrival__:  The next arrival at the stop in minutes.  Can be used to set up a separate sensor
* __Arrivals__:  The number of arrivals found for this stop/route/operator within the overall limit set.
* __Departure time__:  E.g. 17:00.  If a vehicle is ahead of schedule, the state of the sensor can be negative, which is not much use.  

# Please Note

* this sensor implements version 1 of GTFS as provides by the Irish National Transport Authority which provides just one API for trip updates.  A vehicle position API has not been provided thus far.
* All subscribers have been notified that a new version (V2) will be released in early 2023 which will contain changes that may/will break existing implementations.
* For version 1 the static schedule data tends to be updated every few months, which means the SQLite database and Zip files should be deleted.  The replacement Zip file should be uploaded and Home Assistant restarted to re-generate the SQLite database.
* The plans announced are that V2 will contain 3 APIs in total.  A replacement for the existing trip updates API, a new API for vehicle positions and a new API to automate updates to the static schedule data.
