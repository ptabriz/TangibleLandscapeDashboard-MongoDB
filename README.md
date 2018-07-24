# TangibleLandscapeDashboard-MongoDB
This application is created for the Tangible Landscape project (see <https://github.com/tangible-landscape/grass-tangible-landscape>).
</br>
## Installation

#### Node.js
* see <https://nodejs.org/en/download>  

#### MongoDB  
1. Download installer from <https://www.mongodb.com/download-center#community> and follow instructions.  
2. Once the file is downloaded unzip it and rename (e.g., MongoDB) and move the folder to appropriate location (e.g., User directory).  
3. Create a new folder and name it as you wish (e.g., "mongo-data") in the same directory as you saved MongoDB folder. 
4. Open Command line and change directory to MongoDB folder  
```
 cd directory to your MongoDB folder/Server/3.4/bin  
```
5. Set the MongoDB data directory. This will start up the MongoDB connection. If you see the "waiting for connections on port 27017" in your CMD after runnning below code then, you MongoDB server is running.
```
./mongod.exe --dbpath path to MongoDB data folder you created in step 3
```

#### Robo3T (formerly Robomongo - GUI for MongoDB)
* Installation is optional.
* See <https://robomongo.org>
</br>

## Running Node.js server
1. Download or clone the "TangibleLandscapeDashboard-MongoDB" folder
2. Unzip it and move the folder to applicable directory.
3. Open CMD and change directory to the "TangibleLandscapeDashboard-MongoDB" folder
4. Install Node modules
```
npm install
```
5. Run Node server - it starts port 3000
```
node app.js
```
</br>

## API << UNDER CONSTRUCTION >>

* All parameters are required.
* See JSON data format in "**_/public/app/data_**" folder.
* Currently, all **error response code are 400**.

#### Files 

| URL | Method | URL Params | Data Params | Description | 
| --- | --- | --- | --- | --- |
charts/radarBaseline | POST | None | {u: {file: "*xxx.json*", locationId: "*locationId*"}} | Registers a radar chart baseline data file.
charts/barBaseline | POST | None | {u: {file: "*xxx.json*", locationId: "*locationId*"}} | Registers a bar chart baseline data file. 
charts/radar | POST | None |{u: {{file: "*xxx.json*", locationId: "*locationId*", eventId: "*eventId*", playerId: "*playerId*"}} | Registers individual player's radar chart data file.
charts/bar | POST | None | {u: {file: "*xxx.json*", locationId: "*locationId*", eventId: "*eventId*"}} | Registers bar chart data file.

File POST services success response example:
```
{
    "id": "5978c8211c6bed17f84dcb78",
    "fileName": "file-1501087777316.json",
    "metadata": {
        "originalName": "barchart_baseline.json",
        "info": {
            "locationId": "1"
        }
    },
    "state": "Success"
}
```
</br>
</br>

| URL | Method | URL Params | Data Params | Description | 
| --- | --- | --- | --- | --- |
charts/radarBaseline | GET | None | {locationId: "*locationId*"} | Retrieves the radar chart baseline data by location ID.
charts/radarBaselineId | GET | None | {locationId: "*locationId*"} | Retrieves the radar chart baseline file ID.
charts/barBaseline | GET | None | {locationId: "*locationId*"} | Retrieves the bar chart data by location ID.
charts/barBaselineId | GET | None | {locationId: "*locationId*"} | Retrieves the bar chart baseline file ID.
charts/radar | GET | None | {locationId: "*locationId*", eventId: "*eventId*", playerId: "*playerId*"} | Retrieves the radar chart data by given parameters.
charts/radarId | GET | None | {locationId: "*locationId*", eventId: "*eventId*", playerId: "*playerId*"} | Retrieves the radar chart file ID by given parameters.
charts/bar | GET | None | {locationId: "*locationId*", eventId: "*eventId*"} | Retrieves the bar chart data by given parameters.
charts/barId | GET | None | {locationId: "*locationId*", eventId: "*eventId*"} | Retrieves the bar chart file ID by given parameters.

</br>Retriving file data</br>
Example - _/charts/bar?locationId=1&eventId=1002_
```
[
    {
        "axis": "Number of Dead Oaks",
        "options": false,
        "values": [
            {"value": 56784, "playerName": "Baseline", "attempt": ""},
            {"value": 52725, "playerName": "James", "attempt": "1"},
            {"value": 58037, "playerName": "David", "attempt": "1"}
        ]
    }
]
```
<br>Retrieving file id</br>
Example - _/charts/barId?locationId=1&eventId=1002_
```
[
    {
        "_id": "596e01ed13de3c248091ef41",
        "filename": "file-1500381677591.json",
        "contentType": "application/octet-stream",
        "length": 8098,
        "chunkSize": 261120,
        "uploadDate": "2017-07-18T12:41:17.664Z",
        "aliases": null,
        "metadata": {
            "originalName": "barchart_data.json",
            "info": {
                "locationId": "1",
                "eventId": "1002"
            }
        },
        "md5": "4250a628392c694d96f7e0a61cdb2482"
    }
]
```
</br>
</br>

| URL | Method | URL Params | Data Params | Description | 
| --- | --- | --- | --- | --- |
charts/radarBaseline | DELETE | None | {locationId: "*locationId*"} | Deletes the radar chart file by location ID.
charts/barBaseline | DELETE | None |{locationId: "*locationId*"} | Deletes the bar chart file by location ID.
charts/radar/:id | DELETE | id=[ *radar chart file ID* ] | None | Deletes the individual player's radar chart data file.
charts/bar/:id | DELETE | id=[ *bar chart file ID* ] | None | Deletes the individual player's radar chart data file.

</br>File DELETE services success response example</br>
```
{
    "state": "Success",
    "message": "File deleted"
}
```
</br>
</br>

#### Other

| URL | Method | URL Params | Data Params | Description | Success Response |
| --- | --- | --- | --- | --- | --- |
/location | POST | None | {name: *name*, county: *county*, state: *state*} | Registers a new location. | Example {"__v": 0, "_id": 1, "name": "Raleigh", "county": "Wake", "state": "NC"}
/location/:id | GET | id=*locationId* | None | Retrieves location information by location ID. | Example [{"_id": 1, "name": "Raleigh", "Wake": "Wake", "state": "NC", __v": 0}]
/event | POST | None | {locationId: *locationId*, eventName: *eventName*} | Registers a event data. | {"__v": 0, "_id": 1000, "name": "Fire 1", "locationId": "1"}
/event/location/:id | GET | id=*locationId* | None | Retrieves all events for a location. | [{"_id": 1000, "name": "Fire 1", "locationId": "1", "__v": 0}]
/player | POST | None | {name: *name*} | Registers player name. | {"__v": 0, "_id": 6, "name": "Amanda", "image": "24.png"}
/player/players | GET | None | {_id: [*list of player ids*]} | Retrieves player names for provided ids. | [{"__v": 0, "_id": 6, "name": "Amanda", "image": "24.png"}, ...]
/player/:eventId | GET | eventId=*eventId* | None | Retrieve players by event Id | [{pyerId": "7", "playerName": "Tammy"}, ...]
/play | POST | None | {locationId: *locationId*, eventId: *eventId*, playerId: *playerId*} | Registers each play. | {"__v": 0, "_id": 13, "locationId": "1", "eventId": "1001",     "playerId": "7"}
/play/:eventId | GET | eventId=*eventId* | None | Retrieve the play data by event Id | [{"_id": 20, "locationId": "1", "eventId": "1001", "playerId": "7", "playerName": "Tammy", "__v": 0}, ...]
/current | POST | None | {locationId: *locationId*, eventId: *eventId*, playerId: *playerId*,playerName: *playerName*} | Register a current play information | {"__v": 0, "_id": 1, "locationId": "1", "eventId": "1000", "playerId": "8", "playerName": "Sasaki"}
/current | GET | None | None | Retrieve all current play infomation | {"__v": 0, "_id": 1, "locationId": "1", "eventId": "1000", "playerId": "8", "playerName": "Sasaki"}
/current | DELETE | None | None | Delete all current play data | {"delete": "success"}

</br>
</br>

## Snapshots
![index.html](https://github.com/mshukuno/TangibleLandscapeDashboard-MongoDB/blob/master/public/app/images/snapshots/index_small.png "index.html")
</br>
![tld.html#locations](https://github.com/mshukuno/TangibleLandscapeDashboard-MongoDB/blob/master/public/app/images/snapshots/tld-locations_small.png "tld.html#locations")
</br>
![tld.html#players](https://github.com/mshukuno/TangibleLandscapeDashboard-MongoDB/blob/master/public/app/images/snapshots/tld-players_small.png "tld.html#players")
</br>
![tld.html#play](https://github.com/mshukuno/TangibleLandscapeDashboard-MongoDB/blob/master/public/app/images/snapshots/tld-play_small.png "tld.html#play")
</br>
![tld.html#data](https://github.com/mshukuno/TangibleLandscapeDashboard-MongoDB/blob/master/public/app/images/snapshots/tld-data_small.png "tld.html#data")
</br>
</br>

## Resources
* Animal Icons - <http://graphicburger.com/71-free-animal-icons>
* Web Bootstrap Themes - <https://www.creative-tim.com>
* Images (index.html background & tbl.html cards) - <https://www.pexels.com>
* D3.js - <https://d3js.org>
* D3 Tooltip - <https://github.com/Caged/d3-tip>
* Radar Chart - <https://www.visualcinnamon.com/2015/10/different-look-d3-radar-chart.html>






