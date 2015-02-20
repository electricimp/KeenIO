#KeenIO
[Keen IO](http://keen.io) is a hosted service that allows you to easily push and query event based data.

This library wraps the Keen IO data collection API.

# Usage

## Instantiating the class
Instantiate the KeenIO class with your Project ID and Write API Key:

```squirrel
keen <- KeenIO(KEEN_PROJECT_ID, KEEN_WRITE_API_KEY);
```

## keen.sendEvent(collection, eventData, *callback*)
The **.sendEvent** method allows you to send an event to a particular method. If a third parameter, a callback function, is supplied, the request will be made asyncronously and the callback will be fired when the request is complete. If the callback function is ommited, the request will be made syncronously, and result will be returned. Examples of each usage are below:

```squirrel
eventData <- {
    location = {
        lat = 37.123
        lon = -122.123
    },
    temp = 20.4,
    humidity = 36.7
};

// send an event sycronously
local result = keen.sendEvent("tempBugs", eventData);
server.log(result.statuscode + ": " + result.body);

// send an event asyncronously
keen.sendEvent("tempBugs", eventData, function(resp) {
	server.log(resp.statuscode + ": " + resp.body);
});
```

## keen.getTimestamp(ts)
The **.getTimestamp** function can be used to return a KeenIO formated timestamp. The below example demonstrates how to format your data so Keen can take advantage of the timestamp:

```squirrel
eventData <- {
    keen = {
        timestamp = keen.getTimestamp(ts)
    },
    location = {
        lat = 37.123
        lon = -122.123
    },
    temp = 20.4,
    humidity = 36.7
};

keen.sendEvent("tempBugs", eventData)
```

# License
The Keen library is licensed under the [MIT License](./LICENSE).
