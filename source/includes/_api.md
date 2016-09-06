# API

## Native ads API

### HTTP Request

`GET http://ad.dispply.com/v2/ads/PLACEMENT`

### Query Parameters

Parameter | Required | Default | Description
--------- | ------- | ------- | -----------
ip | false | Requester's ip | Device IP
user_agent | false | Requester's user-agent | Device user-agent
platform | false | No | Device platform (ios/android)
device_type | false | phone | Device type (phone/tablet)
creative_type | false | image | Type of creative (image/video)
format | false | No | Creative's format (ex. 120x80)
content | false | title,icon,main | Required content separated by comma (ex. title,desc,icon,main)
callback | false | No | Callback function for [JSONP](https://en.wikipedia.org/wiki/JSONP) request
ios_idfa | false| No | iOS advertising ID
gaid | false | No | Android advertising ID

### Response

> The above endpoint returns JSON structured like this:

```json
{
  "status": "success",
  "ads": [
    {
      "title": "Ad title",
      "description": "Ad description",
      "rating": "3.44",
      "ratings_count": "9872",
      "click_url": "http://click.com",
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

## Developer Reports

### HTTP Request

`GET http://hyperadx.com/publishers/api/v1/developer/reports`

### Query Parameters

Parameter | Required | Default | Description
--------- | ------- | ------- | -----------
access_token | true | | Publisher access token (can be obtained in publisher's profile)
attributes | false | app | Attributes for grouping result (ex. app,placement,country)
page | false | 1 | Result page
date_start | false | | Date start (format is \"YYYY-MM-DD\")
date_end | false | | Date end (format is \"YYYY-MM-DD\")
app_id | false | | Comma delimited list of app ids
placement_id | false | | Comma delimited list of placements ids
country | false | | Country

### Response

> http://hyperadx.com/publishers/api/v1/developer/reports?access_token={access_token}
> returns JSON structured like this:

```json
{
  "page": 1,
  "total_pages": 1,
  "total_count": 1,
  "time_zone": "Moscow",
  "data": [
    {
      "app": "Kuphal, Walter and Conroy",
      "app_id": 3,
      "clicks": 456,
      "impressions": 1780,
      "ctr": 230,
      "payout": 110
    }
  ]
}
```

> http://hyperadx.com/publishers/api/v1/developer/reports?access_token={access_token}&attributes=app,placement,country
> returns JSON structured like this:

```json
{
  "page": 1,
  "total_pages": 1,
  "total_count": 1,
  "time_zone": "Moscow",
  "data": [
    {
    "app": "Kuphal, Walter and Conroy",
    "app_id": 3,
    "placement": "Kuhn Inc",
    "placement_id": 2,
    "country": "RU",
    "clicks": 456,
    "impressions": 1780,
    "ctr": 230,
    "payout": 110
    }
  ]
}
```

## Affilate Reports

### HTTP Request

`GET http://hyperadx.com/publishers/api/v1/reports`

### Query Parameters

Parameter | Required | Default | Description
--------- | ------- | ------- | -----------
access_token | true | | Publisher access token (can be obtained in publisher's profile)
attributes | true | app | Comma-separated list of attributes: traffic_source, traffic_source_id, ad_unit, ad_unit_id, campaign, campaign_id, clicks, installs, cr, validated_installs, validated_payout, day, hour, year
page | false | 1 | Result page
date_start | false | | Date start (format is \"YYYY-MM-DD\")
date_end | false | | Date end (format is \"YYYY-MM-DD\")
campaign_id | false | | Campaign ID filter
country | false | | Country

### Response

> http://hyperadx.com/publishers/api/v1/reports?access_token={access_token}
> returns JSON structured like this:

```json
{
  "page": 1,
  "total_pages": 1,
  "total_count": 1,
  "time_zone": "Moscow",
  "data": [
    {
      "campaign_id": 3,
      "clicks": 456,
      "installs": 11,
      "payout": "0.11",
      "cr": 13,
      "day": "2016-09-06"
    }
  ]
}
```

> http://hyperadx.com/publishers/api/v1/reports?access_token={access_token}&attributes=clicks
> returns JSON structured like this:

```json
{
  "page": 1,
  "total_pages": 1,
  "total_count": 1,
  "time_zone": "Moscow",
  "data": [
    {
      "clicks": 456
    }
  ]
}
```

## Campaigns List

### HTTP Request

`GET http://hyperadx.com/publishers/api/v1/campaigns?access_token={access_token}`

### Query Parameters

Parameter | Required | Default | Description
--------- | ------- | ------- | -----------
access_token | true | | Publisher access token (can be obtained in publisher's profile)

### Response

> GET http://hyperadx.com/publishers/api/v1/campaigns?access_token={access_token}
> returns JSON structured like this:

```json
{
  "data": [
    {
      "id": 3,
      "name": "Bernier, Berge and O'Keefe1",
      "description": "Considine-Cassin",
      "require_approval": false,
      "payout_type": "cpc",
      "cap": null,
      "daily_cap": null,
      "payout": 500,
      "currency": "USD",
      "geo": null,
      "status": "active",
      "platform": "ios",
      "tracking_link": "http://localhost:9292/tracker/clicks?data=RzFR",
      "app": {
        "name": "Hodkiewicz-Murray",
        "icon": "http://lesch.org/camren",
        "url": "http://satterfieldboehm.name/curt"
      }
    }
  ],
  "total_pages": 1,
  "total_count": 1
}
```

## Campaigns Request Approval

### HTTP Request

`POST http://hyperadx.com/publishers/api/v1/campaigns/{campaign_id}/request_approval?access_token={access_token}`

### Query Parameters

Parameter | Required | Default | Description
--------- | ------- | ------- | -----------
access_token | true | | Publisher access token (can be obtained in publisher's profile)
campaign_id | true | | Campaign ID

### Response

> POST http://hyperadx.com/publishers/api/v1/campaigns/3/request_approval?access_token={access_token}
> returns JSON structured like this:

```json
{
  "id": 3,
  "name": "Bernier, Berge and O'Keefe1",
  "description": "Considine-Cassin",
  "require_approval": false,
  "payout_type": "cpc",
  "cap": null,
  "daily_cap": null,
  "payout": 500.0,
  "currency": "USD",
  "geo": null,
  "status": "active",
  "platform": "ios",
  "tracking_link": "http://localhost:9292/tracker/clicks?data=RzFR",
  "app": {
    "name": "Hodkiewicz-Murray",
    "icon": "http://lesch.org/camren",
    "url": "http://satterfieldboehm.name/curt"
  }
}
```

## Error codes

The HyperAdX API uses the following error codes:

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
