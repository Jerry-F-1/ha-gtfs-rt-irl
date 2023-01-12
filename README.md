# Home Assistant Custom Sensor for GTFS Realtime Ireland
This project builds on the work of Zacs https://github.com/zacs/ha-gtfs-rt and the existing GTFS Integration in home assistant https://www.home-assistant.io/integrations/gtfs/ to provide a custom sensor for GTFS Ireland.  This sensor consumes the GTFS realtime feed at https://developers.google.com/transit/gtfs-realtime/guides/trip-updates. The sensor requires a download of the static schedule zip file which the sensor will automatically load into a sqllite database.   Further details are at the Irish Transport Authority website https://developer.nationaltransport.ie/api-details#api=gtfsr&operation=gtfsr

# Setup and Installation

1. Subscribe to the API on the Transport Authoirty's web page to obtain the API Token/Key which you will need to access the realtime data.  This is free and takes about 15 mins.
2. Download the static schedule data provided on the same page.  It's called "GTFS info for use in conjunction with GTFS-R API"
3. Rename the download file as you like e.g. ireland.zip
4. Create a new direstory in your Home Assistant config folder called "gtfs"
5. Upload the schedule data zip file e.g. ireland.zip to the gtfs folder 
5. Also create a new folder called "custom_omponents" and inside that folder create a folder called "gtfs-rt-irl"
