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

### Response

> The above endpoint returns JSON structured like this:

```json
{
  "status": "success",
  "ads": [
    {
      "title": "Ad title",
      "description": "Ad description",
      "creatives": {
        "icon": {
          "url": "http://cdn.com/icon",
          "width": "60",
          "height": "60"
        },
        "image": {
          "url": "http://cdn.com/image",
          "width": "1200",
          "height": "627"
        },
        "video": "VAST"
      },
      "beacons": ["http://pixel.com"]
    }
  ]
}
```

> For `JSONP` response like this:

```
callback({"status": "success", "ads": []})
```

## Account API


## Errors

The Dispply API uses the following error codes:

Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request sucks
401 | Unauthorized -- Your API key is wrong
404 | Not Found -- The specified kitten could not be found
406 | Not Acceptable -- You requested a format that isn't json
418 | I'm a teapot
429 | Too Many Requests -- You're requesting too many ads! Slow down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarially offline for maintanance. Please try again later.
