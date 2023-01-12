# Home Assistant Custom Sensor for GTFS Realtime Ireland
This project builds on the work of Zacs https://github.com/zacs/ha-gtfs-rt, existing GTFS Integration in home assistant https://www.home-assistant.io/integrations/gtfs/ and others to provide a custom transport sensor for GTFS realtime  This sensor consumes the GTFS realtime feed at https://developers.google.com/transit/gtfs-realtime/guides/trip-updates. The sensor requires a download of the static schedule zip file which the sensor will automatically load into a sqllite database.   Further details are at the Irish Transport Authority website https://developer.nationaltransport.ie/api-details#api=gtfsr&operation=gtfsr

# Setup and Installation

1. Subscribe to the API on the Transport Authoirty's web page to obtain the API Token/Key which you will need to access the realtime data.  This is free and takes about 15 mins.
2. Download the static schedule data zip file provided on the same page.  It's called "GTFS info for use in conjunction with GTFS-R API"
3. Rename the download file as you like e.g. ireland.zip, this name will be part of the configuration in Home Assistant
4. Create a new direstory in your Home Assistant config folder called "gtfs"
5. Upload the schedule data zip file e.g. ireland.zip to the gtfs folder 
6. Also create a new folder called "custom_omponents" and inside that folder create a folder called "gtfs-rt-irl"
7. Download this reporistory as a Zip file and unzip
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

* trip_update_url (Required): The production realtime feed URL as provided by the transport authority.
* api_key (Required): Provided by the transport authority when you subscribe.
* schedule_zip_file (Required): The name of the zip file in the gtfs folder containing the static schedule data.
* arrivals_limit (Optional default=30):  The number of arrivals found to be returned, across all the stops required.
* depratures(Required): The list of route / stop / operator combinations of interest.  At least 1 must be specified.
* stop_name (Required): The required stop. This should be an exact copy of the stop_name field in the stops.txt data file 
* route: (Required): this should be an exact copy of the route_short_name field in the routes.txt data file
* operator: the agency_id field in the agency.txt data file
