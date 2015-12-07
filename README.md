# acuRead.js
Node.js library for reading data (and updates) from AcuRite weather stations (currently supporting 02064C)

# Usage



## Methods
* **refresh()**
  * ***Description:*** Refreshes (currently just adds to) the list of known weather stations
  * ***Arguments:*** None
  * ***Returns:*** Array of "station" objects (see "stations" property below)

## Properties
* **stations**
  * ***Type:*** Array
  * ***Description:*** Array of "station" objects (see "Station" below)



## Station
* ***Type:*** Object
* ***Description:*** Object containing weather data, USB information, and change events for the respective station

### Methods
* **refresh(diff)**
  * ***Description:*** Refreshes the data property with the latest weather data. Though, please note that only half the data refreshes every 18 seconds.
  * ***Arguments:*** 1 (truthy/falsy). True returns only the data which has changed with the update. False, or no arguments, will return all weather data.
  * ***Returns:*** Changed data, or all weather data, depending upon the first argument.
* **on(type, eventFn)**
  * ***Description:*** Register a function for the specified event, similar to jQuery's "on()"
  * ***Arguments:*** 2
    1. Event Type (string). Currently, "change" is the only event.
    2. Event Function (function). Change events are called with the first argument being all weather data, and the second being changes only.
  * ***Returns:*** Station (parent)

### Properties
* **data**
  * ***Type:*** Object
  * ***Description:*** Contains weather data if station's refresh() or on() methods have been called. If an event has been registered with the on() method, the data will be kept up to date and fire "change" events. If not, data will only be updated when refresh() is called. This ensures no 'background processes' if they are not needed. The following data is included:
    * ***channel*** - (string) "A", "B", or "C". Should match the channel selected on the unit.
    * ***sensorID*** - (number) Manufacturer ID number for the sensor unit.
    * ***battery*** - (string) "OK" or "Low". Untested, and I don't know how/when the unit determines it is low. But, should match what is shown on the display.
    * ***speed*** - (number) Wind speed in miles per hour (MPH).
    * ***signal*** - (number) Values between 0 and 3 indicating signal strength.
    * ***pressure*** - (number) Atmospheric pressure in millibars, read from the inside sensor in the display.
    * ***inTemp*** - (number) Temperature in Farenheit, read from the inside sensor in the display.
    * ***dir*** - (string) Wind direction, represented as an abbreviation of (possible 16) secondary-intercardinal directions.
    * ***rain*** - (number) Rainfall in hundredths of an inch.
    * ***outTemp*** - (number) Temperature in Farenheit, read from the outside sensor
    * ***humidity*** - (number) Humidity percentage, read from the ***outside*** sensor
    