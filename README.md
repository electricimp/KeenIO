# Keen IO #

[Keen IO](http://keen.io) is a hosted service that allows you to easily push and query event-based data. This library wraps the Keen IO data collection API.

**Note** This library is supported by Electric Imp not by KeenIO. If you find any issues or have a request, please post your findings [here](https://github.com/electricimp/keenio/issues).

**To add this library to your project, add** `#require "KeenIO.class.nut:1.0.0"` **to the top of your agent code**

## Class Usage ##

### Constructor: KeenIO(*projectID, writeAPIKey*)

You instantiate the KeenIO class with your Keen Project ID and Write API Key:

```squirrel
keen <- KeenIO(KEEN_PROJECT_ID, KEEN_WRITE_API_KEY)
```

## Class Methods ##

### sendEvent(*collectionName, eventData[, callback]*) ###

Thismethod allows you to send an event to a particular method. It takes the name of the collection you are posting to as a string, and the event data to be pushed. You can also specify a third, optional parameter: a callback function. If you provide a callback, the request will be made asyncronously and the callback will be fired when the request is complete. If the callback function is ommited, the request will be made syncronously, and result will be returned. The following example illustrates both modes:

```squirrel
eventData <- { "location": { "lat": 37.123,
                             "lon": -122.123 },
               "temp": 20.4,
               "humidity": 36.7 };

// Send an event synchronously
local result = keen.sendEvent("tempBugs", eventData);
server.log(result.statuscode + ": " + result.body);

// Send an event asynchronously
keen.sendEvent("tempBugs", eventData, function(response) {
	server.log(response.statuscode + ": " + response.body);
})
```

### getTimestamp(*timestamp[, millis]*) ###

This method can be used to return a KeenIO-formatted timestamp. The first parameter is a Unix timestamp such as that returned by Squirrel’s [**time()**](https://developer.electricimp.com/squirrel/system/time) function. The second parameter is optional: a millisecond value. The example below demonstrates how to format your data so Keen can take advantage of the timestamp:

```squirrel
eventData <- { "keen": { "timestamp": keen.getTimestamp(time()) },
               "location": { "lat": 37.123,
                             "lon": -122.123 },
               "temp": 20.4,
               "humidity": 36.7 

keen.sendEvent("tempBugs", eventData);
```

## License ##

The Keen library is licensed under the [MIT License](./LICENSE).
