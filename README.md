# amplitude

![Build Status](https://travis-ci.org/crookedneighbor/amplitude.svg?branch=master) ![npm version](https://badge.fury.io/js/amplitude.svg)

Server side implimentation of [Amplitude](https://amplitude.com)'s http api.

## Install

```bash
npm install amplitude --save
```

## Basic Initialization

```javascript
var Amplitude = require('amplitude')
// The only required field is the api token
var amplitude = new Amplitude('api-token')
```

## Track an event

Pass in any keys listed on the [Amplitude http api](https://amplitude.zendesk.com/hc/en-us/articles/204771828-HTTP-API). The only required keys are `event_type` and either `user_id` or `device_id`. If you initialized the Amplitude object with a user/device id, they can be ignored when calling the track method.

```javascript
var data = {
  event_type: 'some value', // required
  user_id: 'some id', // only required if device id is not passed in
  device_id: 'some id', // only required if user id is not passed in
  event_properties: {
    //...
  },
  user_properties: {
    //...
  }
}
amplitude.track(data)
```

If you prefer camelCase variables, you can pass in the camelCase version instead:

```javascript
var data = {
  eventType: 'some value', // required
  userId: 'some id', // only required if device id is not passed in
  deviceId: 'some id', // only required if user id is not passed in
  eventProperties: {
    //...
  },
  userProperties: {
    //...
  }
}
amplitude.track(data)
```

This is the full list of properties that will be automatically transformed:

```
userId -> user_id
deviceId -> device_id
eventType -> event_type
eventProperties -> event_properties
userProperties -> user_properties
appVersion -> app_version
osName -> os_name
deviceBrand -> device_brand
deviceManufacturer -> device_manufacturer
deviceModel -> device_model
deviceType -> device_type
locationLat -> location_lat
locationLng -> location_lng
```

If the user/device id will always be the same, you can initialize the object with it. Passing a user id or device id in the track call will override the default value set at initialization.

```javascript
var amplitude = new Amplitude('api-token', { user_id: 'some-user-id' })
// or
var amplitude = new Amplitude('api-token', { device_id: 'some-device-id' })

amplitude.track({
  event_type: 'some value'
})

amplitude.track({
  event_type: 'some value',
  user_id: 'will-override-the-default-id'
})
```

The track method returns a promise.

```javascript
amplitude.track(data)
  .then(function(result) {
    //... do something
  }).catch(function(error) {
    //... do something
  })
```

## Export your data

The export method requires your [secret key](https://amplitude.zendesk.com/hc/en-us/articles/206728448-Where-can-I-find-my-app-s-API-Key-or-Secret-Key-) to be added when initializing the amplitude object. This method uses the [export api](https://amplitude.zendesk.com/hc/en-us/articles/205406637-Export-API-Export-your-app-s-event-data) and requires a start and end string in the format `YYYYMMDDTHH`.

The method returns a stream.

```javascript
var fs = require('fs')
var stream = fs.createWriteStream('./may-2016-export.zip')

var amplitude = new Amplitude('api-token', { secretKey: 'secret' })

amplitude.export({
  start: '20160501T20',
  end: '20160601T20'
}).pipe(stream)
```

## Changelog

View the [releases page](https://github.com/crookedneighbor/amplitude/releases) for changes in each version.

<!---
Do not change anything below this comment. It is generated automatically.
------>

## Contributors

+ [Erki Esken](http://deekit.net/)
