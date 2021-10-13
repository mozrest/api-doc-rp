---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://mozrest.com/contact' target='_blank'>Contact us for development API key</a>
  - <a href='https://mozrest.com' target='_blank'>Mozrest LTD</a>

includes:

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Mozrest Reservation Portal API
---

# Introduction

This documentation provides all necessary information to communicate with Mozrest Channel Hub API. Find here the guidelines, rules, formats and environments info to develop and go live with our system.

# Authentication

> To authorize, use this code:  
> EXAMPLE REQUEST:  

```shell
# With shell, you can just pass the correct header with each request
curl "api-pre.mozrest.com/v1.0/" \
  -H "Authorization: Bearer {api_token}"
```

> Make sure to replace `{api_token}` with your API key.

You authenticate to the Mozrest API by providing your API Token in the request.  
Your API Token carries many privileges, so be sure to keep it secret! You do not need to provide a password.  
All API requests must be made over HTTPS. Calls made over plain HTTP will fail. You must authenticate for all requests. 
  
**Your token will be provided by the Mozrest team.**
  
<aside class="notice">
You must replace <code>{api_token}</code> with your personal API key.
</aside>
  
# Errors
Mozrest uses conventional HTTP response codes to indicate success or failure of an API request.
In general, codes in the 2xx range indicate success, codes in the 4xx range indicate an error that resulted from the provided information, and codes in the 5xx range indicate an error coming from Mozrest's servers. However, all errors are not cleanly mapped to HTTP response codes. When a method call is not allowed, we return a 405 error code.  

## HTTP Status Code Summary
  
| Code | State | Description |
| --- | ----------- | --------- |
| 200 | OK | Everything worked as expected |
| 400 | Bar request | Often missing a required parameter |
| 401 | Unauthorized | No valid API key provided |
| 402 | Request Failed | Parameters were valid but request failed |
| 403 | Forbidden | Access forbidden |
| 404 | Not Found | The requested item doesn't exist |
| 5xx | Server errors | Something went wrong on Mozrest's end |  
  
# Versioning  
  
When we make backwards-incompatible changes to the API, we release new dated versions.  
**The current version is v1.0.**

# Pagination
Mozrest utilizes offset-limit pagination, using the parameter offset and limit.  
  
| Key | Constraints | Default | Description |
| --- | ----------- | --------- | ------------------ |
| offset | 0 to ... | 0 | The first element you want |
| limit | 0 to 50 | 10 | The number of elements you want |
  
# Environments
Production:  
_https://api.mozrest.com_ 
    
Pre-production:  
_https://api-pre.mozrest.com_  
  

# Venues

## The Venue Object  

> EXAMPLE JSON:

```json
{
    "id": "60e5a3ed409541da3650bd90",
    "name": "Restaurant Name",
    "countryCode": "GB",
    "address": "The Address 24, Picadilly",
    "city": "London",
    "postalCode": "W1J 9LL",
    "phoneNumber": "+44 20 7234 3456",
    "rms": "liveres",
    "latitude": null,
    "longitude": null,
    "capacity": 20,
    "category": "restaurant",
    "url": null,
    "minAdvanceBooking": 1,
    "minAdvanceOnlineCancelling": 24,
    "requireCreditCard": true,
    "paymentGateway": "stripe",
    "gatewayPublishableKey": "pk_test_2343823823823",
    "legalInvoicingName": "Restaurant Name LTD"
}
```

A Venue is a business location that offers bookable online services.  
  
| Key | Type | Description |
| --- | ---- | ----------- |
| id | String | Venue id |
| name | String | Venue name |
| countryCode | String | Country code where the venue is located (<a href="https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2" target="_blank">ISO2</a> format) |
| address | String | Address where the venue is located |
| city | String | City where the venue is located |
| postalCode | String | Postal Code where the venue is located |
| phoneNumber | String | Venue's phone number |
| rms | String | Reservation Management System used by the Venue `[liveres, guest-online]` |
| capacity | Int | Total capacity of the Venue |
| category | String | Category of the venue `[restaurant, garage, ...]` |
| url | String | Website of the Venue |
| minAdvanceBooking | Int | Minimum hours in advance that a booking have to be placed |
| minAdvanceOnlineCancelling | Int | Minimum hours in advance that a booking can be canceled |
| requireCreditCard | Boolean | Does the booking require Credit Card Guarantee? |
| paymentGateway | String | Payment gateway used `[ stripe ]` |
| gatewayPublishableKey | String | Publishable key for the platform gateway |
| legalInvoicingName | String | Legal name will appear on customer invoice |
| latitude | String | Latitude of the Venue |
| longitude | String | Longitude of the Venue |  


## Get Available Venues  
  
This endpoint retrieves a list of Venues.

> EXAMPLE REQUEST:  

```shell
curl GET "https://api-pre.mozrest.com/v1.0/venues" \
  -H "Authorization: Bearer {{api_token}}" \
  -d "offset=0" \
  -d "limit=10" \
  -d "filters[criteria]=par" \
  -d "filters[city]=Par" \
  -d "filters[countryCode]=FR"
```
 > EXAMPLE JSON:  
   
```json
{
  "total": 4,
  "data": [
      {
        "id": "60e5a3ed409541da3650bd90",
        "name": "Restaurant Name",
        "countryCode": "GB",
        "address": "The Address 24, Picadilly",
        "city": "London",
        "postalCode": "W1J 9LL",
        "phoneNumber": "+44 20 7234 3456",
        "rms": "liveres",
        "latitude": null,
        "longitude": null,
        "capacity": 20,
        "category": "restaurant",
        "url": null,
        "minAdvanceBooking": 1,
        "minAdvanceOnlineCancelling": 24,
        "requireCreditCard": true,
        "paymentGateway": "stripe",
        "gatewayPublishableKey": "pk_test_2343823823823",
        "legalInvoicingName": "Restaurant Name LTD"
      },
      { ... }
  ]
}
```
  
### HTTP Request

`GET https://api-pre.mozrest.com/v1.0/venues`  
 
### Query Parameters

Parameter | Status | Description
--------- | ------- | -----------
filters[criteria] |  | Filter by Venue name `Like %criteria% format`
filters[city] |  | Filter by Venue city `Like %city% format`
filters[countryCode] |  | Filter by Country code (<a href="https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2" target="_blank">ISO2</a> format)
  
## Get a Specific Venue
  
> EXAMPLE REQUEST: 
  
```shell
curl GET "https://api-pre.mozrest.com/v1.0/venues/{venue_id}" \
  -H "Authorization: Bearer {{api_token}}" \
```
 > EXAMPLE JSON:  
   
```json
{
  "id": "60e5a3ed409541da3650bd90",
  "name": "Restaurant Name",
  "countryCode": "GB",
  "address": "The Address 24, Picadilly",
  "city": "London",
  "postalCode": "W1J 9LL",
  "phoneNumber": "+44 20 7234 3456",
  "rms": "liveres",
  "latitude": null,
  "longitude": null,
  "capacity": 20,
  "category": "restaurant",
  "url": null,
  "minAdvanceBooking": 1,
  "minAdvanceOnlineCancelling": 24,
  "requireCreditCard": true,
  "paymentGateway": "stripe",
  "gatewayPublishableKey": "pk_test_2343823823823",
  "legalInvoicingName": "Restaurant Name LTD"
}
```

This endpoint retrieves a specific Venue.

### HTTP Request

`GET https://api-pre.mozrest.com/v1.0/venues/{id}`

### URL Parameters

Parameter | Status | Description
--------- | ------- | -----------
id | **Mandatory** | Venue Id


# Availability

## The Availability Object  

> EXAMPLE JSON:

```json
{
  "venueId": "60e5a3ed409541da3650bd90",
  "session": "lunch",
  "date": "2020-03-20",
  "partySize": 4,
  "openSpots": 50,
  "slots": [
    {
      "time": "12:00"
    },
    {...},
    {
      "time": "14:00",
    }
  ]
}
```

Availability is a set of slots available for a Venue and a specific date, session and party size.
  
| Key | Type | Description |
| --- | ---- | ----------- |
| venueId | String | Venue id |
| session | String | Session when the booking is taking place (ie. dinner) |
| date | String | Date requested for availability (format: YYYY-MM-DD) |
| partySize | Int | Number of persons |
| openSpots | Int | Total available spots for the session |
| slots | Array | List of available slots |

## Get Venue Availability
This endpoint retrieves a list of slots available for a specific Venue on a specific date, session and party size.

> EXAMPLE REQUEST:  

```shell
curl GET "https://api-pre.mozrest.com/v1.0/availability" \
  -H "Authorization: Bearer {{api_token}}" \
  -d "venueId='60e5a3ed409541da3650bd90'" \
  -d "date=1633930574" \
  -d "partySize=4"
```
 > EXAMPLE JSON:  
   
```json
{
  "venueId": "60e5a3ed409541da3650bd90",
  "session": "lunch",
  "date": "2020-03-20",
  "partySize": 4,
  "openSpots": 50,
  "slots": [
    {
      "time": "12:00"
    },
    {...},
    {
      "time": "14:00",
    }
  ]
}
```
  
### HTTP Request

`GET https://api-pre.mozrest.com/v1.0/availability`  
 
### Query Parameters

Parameter | Status | Description
--------- | ------- | -----------
venueId | **Mandatory** | Venue id
date | **Mandatory** | Date time in Timestamp UTC format (ie. Saturday, 16 October 2021 14:30 is **1634387400** on timestamp)
partySize | **Mandatory** | Number of persons (ie. **4**)  
  

# Bookings

## The Booking Object  

> EXAMPLE JSON:

```json
{
  "id": "60e890aca5f07b6ee5b950b1",
  "venueId": "60e890aca5f07b6ee5b950b1",
  "partySize": 4,
  "session": "lunch",
  "status": "confirmed",
  "date": "2021-11-06T12:00:00+02:00",
  "notes": "I'm alergic to peanuts",
  "cancelActor": null,
  "cancelReason": null,
  "canceledAt": null,
  "contact": {
    "id": "60e890aca5f07b6ee5b950b1",
    "firstname": "John",
    "lastname": "Doe",
    "email": "john.doe@gmail.com",
    "telephone": "44344223322",
    "locale": "en",
    "address": {
        "country": "GB",
        "city": "London",
        "region": "London",
        "address": "Picadilly Sq. 15",
        "postalCode": "W1J 9LL"
    },
    "optInConsent": true
  }
}
```
  
This is how the booking object structure looks like. The booking owner is presented as an array named "Contact".

| Key | Type | Description |
| --- | ---- | ----------- |
| id | String | Booking id |
| venueId | String | Venue id for booking |
| status | String | Booking status `[pending, confirmed, canceled, faked, honored]` |
| partySize | Int | Number of persons |
| session | String |Booking session in `[breakfast, brunch, lunch, afternoon, dinner, all_day]` |
| date | Datetime | Booking date and time (UTC) |
| notes | Text | User notes |
| cancelActor | String | Who had canceled the booking |
| cancelReason | Text | Reason why the booking has been canceled |
| canceledAt | Datetime | When the booking was canceled (UTC) |
| contact[firstname] | Text | Contact's firstname |
| contact[lastname] | Text | Contact's lastname |
| contact[email] | Text | Contact's email (ie. john.doe@email.com) |
| contact[telephone] | Int | Contact's telephone including country code but NO sign (ie. +44 20 7234 3456 -> 442072343456 ) |
| contact[locale] | Text | Contact's language (<a href="https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes" target="_blank">ISO2</a> format) |
| contact[address][country] | Text | Contact's country (<a href="https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2" target="_blank">ISO2</a> format) |
| contact[address][city] | Text | Contact's city |
| contact[address][region] | Text | Contact's region |
| contact[address][streetAddress] | Text | Contact's address |
| contact[address][postalCode] | Text | Contact's postal code |
| contact[optInConsent] | Boolean | Allow to get marketing emails and reminders (default: false) |

## Create Booking
  
This endpoint allows to create a Booking.

> EXAMPLE REQUEST:  

```shell
curl POST "https://api-pre.mozrest.com/v1.0/booking" \
  -H "Authorization: Bearer {{api_token}}" \
  -d "venueId=60e5a3ed409541da3650bd90" \
  -d "partySize=4" \
  -d "status=confirmed" \
  -d "date=1634387400" \
  -d "notes=I'm alergic to peanuts" \
  -d "contact[firstname]=John" \
  -d "contact[lastname]=Doe" \
  -d "contact[email]=john.doe@gmail.com" \
  -d "contact[telephone]=Doe" \
  -d "contact[locale]=en" \
  -d "contact[address][country]=gb" \
  -d "contact[address][city]=London" \
  -d "contact[address][region]=London" \
  -d "contact[address][streetAddress]=Picadilly Sq. 15" \
  -d "contact[address][postalCode]=W1J 9LL" \
  -d "contact[optInConsent]=true" 
```
 > EXAMPLE JSON:  
   
```json
{
  "id": "60e890aca5f07b6ee5b950b1",
  "venueId": "60e890aca5f07b6ee5b950b1",
  "partySize": 4,
  "status": "confirmed",
  "date": "2021-11-06T12:00:00+02:00",
  "notes": "'m alergic to peanuts",
  "contact": {
    "id": "60e890aca5f07b6ee5b950b1",
    "firstname": "John",
    "lastname": "Doe",
    "email": "john.doe@gmail.com",
    "telephone": "44344223322",
    "locale": "en",
    "address": {
        "country": "GB",
        "city": "London",
        "region": "London",
        "address": "Picadilly Sq. 15",
        "postalCode": "W1J 9LL"
    },
    "optInConsent": true
  }
}
```
  
### HTTP Request

`POST https://api-pre.mozrest.com/v1.0/booking`  
 
### Query Parameters

Parameter | Status | Description
--------- | ------- | -----------
| venueId | **Mandatory** | Venue id for booking |
| status | **Mandatory** | Booking status `[pending, confirmed, canceled, faked, honored]` |
| partySize | **Mandatory** | Number of persons |
| date | **Mandatory** | Date time in Timestamp UTC format (ie. Saturday, 16 October 2021 14:30 is **1634387400** on timestamp) |
| notes | **Optional** | User notes |
| contact[firstname] | **Mandatory** | Contact's firstname |
| contact[lastname] | **Mandatory** | Contact's lastname |
| contact[email] | **Mandatory** | Contact's email |
| contact[telephone] | **Optional** | Contact's telephone (including country code but NO sign) |
| contact[locale] | **Mandatory** | Contact's language (<a href="https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes" target="_blank">ISO2</a> format) |
| contact[address][country] | **Mandatory** | Contact's country (<a href="https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2" target="_blank">ISO2</a> format) |
| contact[address][city] | **Optional** | Contact's city |
| contact[address][region] | **Optional** | Contact's region |
| contact[address][streetAddress] | **Optional** | Contact's address |
| contact[address][postalCode] | **Optional** | Contact's postal code |
| contact[optInConsent] | **Optional** | Allow to get marketing emails and reminders (default: false) |
  

## Amend Booking
  
This endpoint allows to amend a pending status booking.

> EXAMPLE REQUEST:  

```shell
curl PUT "https://api-pre.mozrest.com/v1.0/booking/{id}/amend" \
  -H "Authorization: Bearer {{api_token}}" \
```
 > EXAMPLE JSON:  
   
```json
{
  "id": "60e890aca5f07b6ee5b950b1",
  "venueId": "60e890aca5f07b6ee5b950b1",
  "partySize": 4,
  "status": "confirmed",
  "date": "2021-11-06T12:00:00+02:00",
  "notes": "'m alergic to peanuts",
  "contact": {
    "id": "60e890aca5f07b6ee5b950b1",
    "firstname": "John",
    "lastname": "Doe",
    "email": "john.doe@gmail.com",
    "telephone": "44344223322",
    "locale": "en",
    "address": {
        "country": "GB",
        "city": "London",
        "region": "London",
        "address": "Picadilly Sq. 15",
        "postalCode": "W1J 9LL"
    },
    "optInConsent": true
  }
}
```
  
### HTTP Request

`PUT https://api-pre.mozrest.com/v1.0/booking/{id}/amend`  
 
### Query Parameters

Parameter | Status | Description
--------- | ------- | -----------
id | **Mandatory** | Booking id to amend


## Update Booking
  
This endpoint allows to amend a pending status booking.

> EXAMPLE REQUEST:  

```shell
curl PUT "https://api-pre.mozrest.com/v1.0/booking" \
  -H "Authorization: Bearer {{api_token}}" \
  -d "date=1636282800" \
  -d "notes=I'm alergic to peanuts and broccoli"
```
 > EXAMPLE JSON:  
   
```json
{
  "id": "60e890aca5f07b6ee5b950b1",
  "venueId": "60e890aca5f07b6ee5b950b1",
  "partySize": 4,
  "status": "confirmed",
  "date": "2021-11-07T12:00:00+02:00",
  "notes": "I'm alergic to peanuts and broccoli",
  "contact": {
    "id": "60e890aca5f07b6ee5b950b1",
    "firstname": "John",
    "lastname": "Doe",
    "email": "john.doe@gmail.com",
    "telephone": "44344223322",
    "locale": "en",
    "address": {
        "country": "GB",
        "city": "London",
        "region": "London",
        "address": "Picadilly Sq. 15",
        "postalCode": "W1J 9LL"
    },
    "optInConsent": true
  }
}
```
  
### HTTP Request

`PUT https://api-pre.mozrest.com/v1.0/booking/{id}`  
 
### Query Parameters

Parameter | Status | Description
--------- | ------- | -----------
| status | **Optional** | Booking status `[confirmed, canceled]` |
| partySize | **Optional** | Number of persons |
| date | **Optional** | Date time in Timestamp UTC format (ie. Saturday, 16 October 2021 14:30 is **1634387400** on timestamp) |
| notes | **Optional** | User notes |
| contact[firstname] | **Optional** | Contact's firstname |
| contact[lastname] | **Optional** | Contact's lastname |
| contact[email] | **Optional** | Contact's email |
| contact[telephone] | **Optional** | Contact's telephone (including country code but NO sign) |
| contact[locale] | **Optional** | Contact's language (<a href="https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes" target="_blank">ISO2</a> format) |
| contact[address][country] | **Optional** | Contact's country (<a href="https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2" target="_blank">ISO2</a> format) |
| contact[address][city] | **Optional** | Contact's city |
| contact[address][region] | **Optional** | Contact's region |
| contact[address][streetAddress] | **Optional** | Contact's address |
| contact[address][postalCode] | **Optional** | Contact's postal code |
| contact[optInConsent] | **Optional** | Allow to get marketing emails and reminders (default: false) | 
  

<aside class="notice">
At least one of the above parameters must be provided.
</aside>  

## Cancel Booking
  
This endpoint allows to cancel a booking.

> EXAMPLE REQUEST:  

```shell
curl PUT "https://api-pre.mozrest.com/v1.0/booking/{id}" \
  -H "Authorization: Bearer {{api_token}}" \  
  -d "cancelActor='user'" \  
  -d "cancelReason='I can't finally attend'" \
```
 > EXAMPLE JSON:  
   
```json
{
  "id": "60e890aca5f07b6ee5b950b1",
  "venueId": "60e890aca5f07b6ee5b950b1",
  "partySize": 4,
  "status": "canceled",
  "date": "2021-11-07T12:00:00+02:00",
  "notes": "I'm alergic to peanuts and broccoli",
  "cancelActor": "user",
  "cancelReason": "I can't finally attend",
  "canceledAt": "2021-10-20T12:00:00+02:00",
  "contact": {
    "id": "60e890aca5f07b6ee5b950b1",
    "firstname": "John",
    "lastname": "Doe",
    "email": "john.doe@gmail.com",
    "telephone": "44344223322",
    "locale": "en",
    "address": {
        "country": "GB",
        "city": "London",
        "region": "London",
        "address": "Picadilly Sq. 15",
        "postalCode": "W1J 9LL"
    },
    "optInConsent": true
  }
}
```
  
### HTTP Request

`PUT https://api-pre.mozrest.com/v1.0/booking/{id}/cancel`  
 
### Query Parameters

Parameter | Status | Description
--------- | ------- | -----------
id | **Mandatory** | Booking id to cancel  
cancelActor | **Mandatory** | Who had canceled the booking
cancelReason | **Optional** | Reason why the booking has been canceled
  
## Get a specific booking
  
This endpoint allows to get a specific booking.

> EXAMPLE REQUEST:  

```shell
curl GET "https://api-pre.mozrest.com/v1.0/booking/{id}" \
  -H "Authorization: Bearer {{api_token}}" 
```
 > EXAMPLE JSON:  
   
```json
{
  "id": "60e890aca5f07b6ee5b950b1",
  "venueId": "60e890aca5f07b6ee5b950b1",
  "partySize": 4,
  "status": "confirmed",
  "date": "2021-11-07T12:00:00+02:00",
  "notes": "I'm alergic to peanuts and broccoli",
  "contact": {
    "id": "60e890aca5f07b6ee5b950b1",
    "firstname": "John",
    "lastname": "Doe",
    "email": "john.doe@gmail.com",
    "telephone": "44344223322",
    "locale": "en",
    "address": {
        "country": "GB",
        "city": "London",
        "region": "London",
        "address": "Picadilly Sq. 15",
        "postalCode": "W1J 9LL"
    },
    "optInConsent": true
  }
}
```
  
### HTTP Request

`GET https://api-pre.mozrest.com/v1.0/booking/{id}`  
 
### Query Parameters

Parameter | Status | Description
--------- | ------- | -----------
id | **Mandatory** | Booking id to retrieve


## Get a list of bookings
  
This endpoint allows to retrieve a list of bookings.

> EXAMPLE REQUEST:  

```shell
curl GET "https://api-pre.mozrest.com/v1.0/booking" \
  -H "Authorization: Bearer {{api_token}}" \  
  -d "offset=0" \
  -d "limit=10" \
  -d "userId='60e5a3ed409541da3650bd90'" \  
  -d "venueId='60e5a3ed409541da3650bd90'" \  
  -d "date='2021-10-24'" \
```
 > EXAMPLE JSON:  
   
```json
{
  "total": 16,
  "data": [
      {
        "id": "60e890aca5f07b6ee5b950b1",
        "venueId": "60e890aca5f07b6ee5b950b1",
        "partySize": 4,
        "status": "confirmed",
        "date": "2021-11-07T12:00:00+02:00",
        "notes": "I'm alergic to peanuts and broccoli",
        "contact": {
          "id": "60e890aca5f07b6ee5b950b1",
          "firstname": "John",
          "lastname": "Doe",
          "email": "john.doe@gmail.com",
          "telephone": "44344223322",
          "locale": "en",
          "address": {
              "country": "GB",
              "city": "London",
              "region": "London",
              "address": "Picadilly Sq. 15",
              "postalCode": "W1J 9LL"
          },
          "optInConsent": true
        }
      },
      { ... }
  ]
}
```
  
### HTTP Request

`GET https://api-pre.mozrest.com/v1.0/booking/{id}`  
 
### Query Parameters

Parameter | Status | Description
--------- | ------- | -----------
userId | **Optional** | User id that owns the booking
venueId | **Optional** | Venue id where the booking takes place
date | **Optional** | Date of the booking (format 2021-10-24) 

<aside class="notice">
At least one of the above parameters must be provided.
</aside>  
  

# Webhooks

> UPDATE EXAMPLE JSON:

```json
{
  "entity": "booking",
  "entity_id": "60e5a3ed409541da3650bd90",
  "action": "update"
}
```
> CANCELED EXAMPLE JSON:

```json
{
  "entity": "booking",
  "entity_id": "60e5a3ed409541da3650bd90",
  "action": "canceled"
}
```

Mozrest provides a feature to subscribe to a webhook system to be notified when any relevant information affecting your bookings has an update. Mozrest will send a notification to the given endpoint providing the entity that have been modified.

<aside class="success">
On the onboarding process, you will provide the Mozrest team with the endpoint you wish to be notified.
</aside>  
  