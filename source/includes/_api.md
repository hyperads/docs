# API

## Native ads API

### HTTP Request

`GET http://ad.dispply.com/v2/ads`

### Query Parameters

Parameter | Required | Default | Description
--------- | ------- | ------- | -----------
access_token | true | No | This is Access Token from one of your App ([here](http://dispply.com/publishers/apps)).
placement_id | true | No | Placement in the app ([here](http://dispply.com/publishers/placements))
ip | false | Requester's ip | Device IP
user_agent | false | Requester's user-agent | Device user-agent
platform | false | No | Device platform (ios/android)
device_type | false | phone | Device type (phone/tablet)
creative_type | false | image | Type of creative (image/video)
format | false | No | Creative's format (ex. 120x80)
content | false | title,icon,image | Required content separated by comma (ex. title,desc,icon,main)
callback | false | No | Callback function for [JSONP](https://en.wikipedia.org/wiki/JSONP) request

## Account API
