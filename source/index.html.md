---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://mozrest.com/contact' target='_blank'>Contact us for development API key</a>
  - <a href='https://mozrest.com' target='_blank'>Mozrest LTD</a>

api_version: v1 (1.7.0)

last_update: 2022-09-27 06:10 UTC

includes:

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Mozrest Booking Channel API
---

# Introduction

This documentation provides all necessary information to communicate with Mozrest Booking Channel API and to manage the following content:

1. [Venues](#venues)
2. [Availability](#availability)
3. [Pending Bookings](#pending-bookings)
4. [Bookings](#bookings)
5. [Webhook](#webhooks)

Find here the guidelines, rules, formats and environments info to develop and go live with our system.

# Authentication

> To authorize, use this code:  
> EXAMPLE REQUEST:  

```shell
# With shell, you can just pass the correct header with each request
curl "api-sandbox.mozrest.com/v1/bc/" \
  -H "Authorization: Bearer {api_key}"
```

> Make sure to replace `{api_key}` with your API key.

You must provide your API key to authenticate to the Mozrest API.
Your API key carries many privileges, so be sure to keep it secret! You do not need to provide a password.  
All API requests must be made over HTTPS. Calls made over plain HTTP will fail. You must authenticate for all requests.

**Your API key will be provided by the Mozrest team.**

<aside class='notice'>
You must replace <code>{api_key}</code> with your personal API key.
</aside>

# Errors
Mozrest uses conventional HTTP response codes to indicate success or failure of an API request.
In general, codes in the 2xx range indicate success, codes in the 4xx range indicate an error that resulted from the provided information, and codes in the 5xx range indicate an error coming from Mozrest's servers. However, all errors are not cleanly mapped to HTTP response codes. When a method call is not allowed, we return a 403 error code.

## HTTP Status Code Summary

| Code | State | Description |
| --- | ----------- | --------- |
| 200 | OK | Everything worked as expected |
| 400 | Bad request | Missing a required parameter |
| 401 | Unauthorized | API key not valid |
| 402 | Request Failed | Parameters were valid but request failed |
| 403 | Forbidden | Access forbidden |
| 404 | Not Found | The requested item does not exist |
| 5xx | Server errors | Something went wrong on Mozrest's server |  

# Versioning

When Mozrest makes backwards-incompatible changes to the Mozrest Partner API, a new dated version is released.  
**The current version is v1.**

# Pagination
Mozrest utilizes offset-limit pagination, using the parameter offset and limit.

| Key | Constraints | Default | Description |
| --- | ----------- | --------- | ------------------ |
| offset | 0 to ... | 0 | The first requested element |
| limit | 0 to 50 | 10 | The requested number of elements |
  
# Environments
Production:  
_https://api.mozrest.com/v1/bc_ 
    
Sandbox:  
_https://api-sandbox.mozrest.com/v1/bc_  
  

# Venues

A Venue is a business location that offers online booking services.

Find [here](#the-venue-object) the object definition.

## Get Available Venues  

This endpoint retrieves a list of available Venues.

> EXAMPLE REQUEST:  

```shell
curl GET "https://api-sandbox.mozrest.com/v1/bc/venues" \
  -H "Authorization: Bearer {{api_key}}" \
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
      "phoneNumber": "442072343456",
      "latitude": null,
      "longitude": null,
      "capacity": 20,
      "url": null,
      "minAdvanceBooking": 1,
      "minAdvanceOnlineCancelling": 24,
      "paymentDefinition": {
        "requireCreditCard": true,
        "deposit": {
          "type": "fixed_price",
          "amount": 2000
        },
        "noShowFee": {
          "type": "fixed_price",
          "amount": 2000
        }
      },
      "paymentGateway": {
        "gateway": "stripe",
        "apiVersion": "2020-08-27",
        "sdkVersion": null,
        "publishKey": "pk_test_2343823823823",
        "merchantId": null,
        "legalInvoicingName": "Venue Name LTD",
        "paymentCountryCode": "GB"
      },
      "currency": "GBP"
    },
    { ... }
  ]
}
```
  
### HTTP Request

`GET https://api-sandbox.mozrest.com/v1/bc/venues`  
 
### Query Parameters

Parameter | Type | Description
--------- |------| -----------
filters[criteria] | **Optional** | Filter by Venue name `Like %criteria% format`
filters[city] | **Optional** | Filter by Venue city `Like %city% format`
filters[countryCode] | **Optional** | Filter by Country code (<a href="https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2" target="_blank">ISO2</a> format)
  
## Get a Specific Venue
  
> EXAMPLE REQUEST: 
  
```shell
curl GET "https://api-sandbox.mozrest.com/v1/bc/venues/{venue_id}" \
  -H "Authorization: Bearer {{api_key}}" \
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
  "phoneNumber": "442072343456",
  "latitude": null,
  "longitude": null,
  "capacity": 20,
  "url": null,
  "minAdvanceBooking": 1,
  "minAdvanceOnlineCancelling": 24,
  "paymentDefinition": {
    "requireCreditCard": true,
    "deposit": {
      "type": "fixed_price",
      "amount": 2000
    },
    "noShowFee": {
      "type": "fixed_price",
      "amount": 2000
    }
  },
  "paymentGateway": {
    "gateway": "stripe",
    "apiVersion": "2020-08-27",
    "sdkVersion": null,
    "publishKey": "pk_test_2343823823823",
    "merchantId": null,
    "legalInvoicingName": "Venue Name LTD",
    "paymentCountryCode": "GB"
  },
  "currency": "GBP"
}
```

This endpoint retrieves a specific Venue.

### HTTP Request

`GET https://api-sandbox.mozrest.com/v1/bc/venues/{id}`

### URL Parameters

Parameter | Type          | Description
--------- |---------------| -----------
id | **Mandatory** | Venue id to retrieve.


# Availability

Find [here](#the-availability-object) the availability object definition.

## Get Venue Availability
This endpoint retrieves a list of slots available for a specific Venue on a specific date, session and party size.

> EXAMPLE REQUEST:  

```shell
curl GET "https://api-sandbox.mozrest.com/v1/bc/availability" \
  -H "Authorization: Bearer {{api_key}}" \
  -d "venueId='60e5a3ed409541da3650bd90'" \
  -d "date=1633930574" \
  -d "partySize=4"
```
 > EXAMPLE JSON:  
   
```json
{
  "date": "2020-03-20",
  "partySize": 4,
  "slots": [
    {
      "time": "12:00"
    },
    {...},
    {
      "time": "14:00",
    }
  ],
  "openingTimes": [
    {
      "time": "09:00"
    },
    {...},
    {
      "time": "21:00"
    }
  ],
  "nextAvailability": {
    "date": 1664282700,
    "slots": [
      {
        "time": "12:45" //Just if the next availability is for the same requested day
      }
    ]
  },
  "previousAvailability": {
    "date": 1664282700,
    "slots": [
      {
        "time": "12:45" //Just if the previous availability is for the same requested day
      }
    ]
  }
}
```
  
### HTTP Request

`GET https://api-sandbox.mozrest.com/v1/bc/availability`  
 
### Query Parameters

Parameter | Type          | Description
--------- |---------------| -----------
venueId | **Mandatory** | Venue id
date | **Mandatory** | Date time in Timestamp UTC format (ie. Saturday, 16 October 2021 14:30 is **1634387400** on timestamp)
partySize | **Mandatory** | Number of persons (ie. **4**)  

# Pending Bookings  

A pending booking represents a booking during the online reservation process. It holds the seats during the booking process. It allows to finalize the process (fillings contact fields or payment). A pending booking is valid for a limited amount of time, since the creation. After this time it is destroyed and the seats are released. The exact amount of time will depends on the RMS that provides the availability. After creating a pending booking, the response will include the time in seconds.

In order to be transformed into a Booking, the PendingBooking must be committed.

Find [here](#the-pending-booking-object) the pending booking object definition.

## Create Pending Booking

This endpoint allows to create a Pending Booking.

> EXAMPLE REQUEST:

```shell
curl POST "https://api-sandbox.mozrest.com/v1/bc/pending-booking" \
  -H "Authorization: Bearer {{api_key}}" \ 
  --data-raw '{
    "venueId": "619decd55bf9cb36aaa00dd6",
    "partySize": 4,
    "date": 1642339800
  }'
```
> EXAMPLE JSON:

```json
{
  "id": "61bb23a701a5ac259b85e3f3",
  "notes": {
    "en": "Reservation is for Taverna(1.floor). Ristorante (2. floor) is closed every Sunday.",
    "fi": "Varaus on Tavernan (1.krs) puolelle. Ristorante (2.krs) on sunnuntaisin suljettu."
  },
  "holdingTime": 420
}
```

### HTTP Request

`POST https://api-sandbox.mozrest.com/v1/bc/pending-booking`

### Query Parameters

Parameter | Status | Description
--------- | ------- | -----------
| venueId | **Mandatory** | Venue id for booking |
| partySize | **Mandatory** | Number of persons |
| date | **Mandatory** | Date time in Timestamp UTC format (ie. Saturday, 16 October 2021 14:30 is **1634387400** on timestamp) |


## Commit Pending Booking

This endpoint allows to commit a Pending Booking. After a Pending Booking is committed it becomes a Booking and on the response a Booking with a Booking id is returned. 

> EXAMPLE REQUEST:

```shell
curl PUT "https://api-sandbox.mozrest.com/v1/bc/pending-booking/61bb23a701a5ac259b85e3f3" \
  -H "Authorization: Bearer {{api_key}}" \ 
  --data-raw '{
    "notes": "Customer notes or requests",
    "paymentGatewayToken": "pm_1JkkRzAY52ufXkvaHWKbasua"
    "contact": {
        "firstname": "John",
        "lastname": "Doe",
        "email": "john.doe@gmail.com",
        "telephone": "44646673377",
        "locale": "en",
        "address": {
            "country": "GB",
            "city": "London",
            "region": "London",
            "address": "Picadilly Sq. 15",
            "postalCode": "W1J 9LL"
        },
        "optInConsent": "true"
    }'
```
> EXAMPLE JSON:

```json
{
  "id": "61bb24011271104c686f4db5",
  "venueId": "619decd55bf9cb36aaa00dd6",
  "partySize": 4,
  "session": "dinner",
  "status": "confirmed",
  "date": "2022-01-16T13:30:00Z",
  "notes": "Customer notes or requests",
  "cancelActor": null,
  "cancelReason": null,
  "canceledAt": null,
  "paymentGatewayToken": "pm_1JkkRzAY52ufXkvaHWKbasua",
  "contact": {
    "id": "61b78753b419978323e22aa3",
    "firstname": "John",
    "lastname": "Doe",
    "email": "john.doe@gmail.com",
    "telephone": "44646673377",
    "locale": "en",
    "address": {
      "country": "GB",
      "city": null,
      "region": null,
      "street": null,
      "postalCode": null
    },
    "optInConsent": true
  }
}
```

### HTTP Request

`PUT https://api-sandbox.mozrest.com/v1/bc/pending-booking/{id}`

### Query Parameters

Parameter | Status | Description                                                                                                          
--------- | ------- |----------------------------------------------------------------------------------------------------------------------
| notes | **Optional** | User notes                                                                                                           |
| paymentGatewayToken | **Optional** | Payment gateway token for customers credit card                                                                      |
| contact[firstname] | **Mandatory** | Contact's firstname                                                                                                  |
| contact[lastname] | **Mandatory** | Contact's lastname                                                                                                   |
| contact[email] | **Mandatory** | Contact's email                                                                                                      |
| contact[telephone] | **Optional** | Contact's telephone (including country code but NO sign)                                                             |
| contact[locale] | **Mandatory** | Contact's language (<a href="https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes" target="_blank">ISO2</a> format) |
| contact[address][country] | **Mandatory** | Contact's country (<a href="https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2" target="_blank">ISO2</a> format)       |
| contact[address][city] | **Optional** | Contact's city                                                                                                       |
| contact[address][region] | **Optional** | Contact's region                                                                                                     |
| contact[address][street] | **Optional** | Contact's address                                                                                                    |
| contact[address][postalCode] | **Optional** | Contact's postal code                                                                                                |
| contact[optInConsent] | **Optional** | Allow to get marketing emails and reminders (default: false)                                                         |

# Bookings

This is how the booking object structure looks like. The booking owner is presented as an array named "Contact".

Find [here](#the-booking-object) the object definition.

## Create Booking
  
This endpoint allows to create a Booking.

> EXAMPLE REQUEST:  

```shell
curl POST "https://api-sandbox.mozrest.com/v1/bc/booking" \
  -H "Authorization: Bearer {{api_key}}" \ 
  --data-raw '{
    "venueId": "60e5a3edfc8747ddfe1c10da",
    "partySize": 2,
    "date": 1634832900,
    "notes": "I'\''m alergic to peanuts",
    "paymentGatewayToken": "pm_1JkkRzAY52ufXkvaHWKbasua",
    "contact": {
        "firstname": "john",
        "lastname": "Doe",
        "email": "john.doe@gmail.com",
        "telephone": "34655040452",
        "locale": "en",
        "address": {
            "country": "GB",
            "city": "London",
            "region": "London",
            "street": "Picadilly Sq. 15",
            "postalCode": "W1J 9LL"
        },
        "optInConsent": true
    }
  }'
```
 > EXAMPLE JSON:  
   
```json
{
  "id": "60e890aca5f07b6ee5b950b1",
  "venueId": "60e890aca5f07b6ee5b950b1",
  "partySize": 4,
  "date": "2021-11-06T12:00:00+02:00",
  "notes": "I'm alergic to peanuts",
  "paymentGatewayToken": "pm_1JkkRzAY52ufXkvaHWKbasua",
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

`POST https://api-sandbox.mozrest.com/v1/bc/booking`  
 
### Query Parameters

Parameter | Status | Description
--------- | ------- | -----------
| venueId | **Mandatory** | Venue id for booking |
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
| contact[address][street] | **Optional** | Contact's address |
| contact[address][postalCode] | **Optional** | Contact's postal code |
| contact[optInConsent] | **Optional** | Allow to get marketing emails and reminders (default: false) |


## Update Booking
  
This endpoint allows to amend a pending status booking.

> EXAMPLE REQUEST:  

```shell
curl PUT "https://api-sandbox.mozrest.com/v1/bc/booking" \
  -H "Authorization: Bearer {{api_key}}" \
  --data-raw '{
    "partySize": 4,
    "date": 1634401800
  }'
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
  "paymentGatewayToken":null,
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

`PUT https://api-sandbox.mozrest.com/v1/bc/booking/{id}`  
 
### Query Parameters

Parameter | Status | Description
--------- | ------- | -----------
| status | **Optional** | Booking status `[confirmed, canceled]` |
| partySize | **Optional** | Number of persons |
| date | **Optional** | Date time in Timestamp UTC format (ie. Saturday, 16 October 2021 14:30 is **1634387400** on timestamp) |
| contact[firstname] | **Optional** | Contact's firstname |
| contact[lastname] | **Optional** | Contact's lastname |
| contact[email] | **Optional** | Contact's email |
| contact[telephone] | **Optional** | Contact's telephone (including country code but NO sign) |
| contact[locale] | **Optional** | Contact's language (<a href="https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes" target="_blank">ISO2</a> format) |
| contact[address][country] | **Optional** | Contact's country (<a href="https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2" target="_blank">ISO2</a> format) |
| contact[address][city] | **Optional** | Contact's city |
| contact[address][region] | **Optional** | Contact's region |
| contact[address][street] | **Optional** | Contact's address |
| contact[address][postalCode] | **Optional** | Contact's postal code |
| contact[optInConsent] | **Optional** | Allow to get marketing emails and reminders (default: false) | 
  

<aside class="notice">
At least one of the above parameters must be provided.
</aside>  

## Cancel Booking
  
This endpoint allows to cancel a booking.

> EXAMPLE REQUEST:  

```shell
curl PUT "https://api-sandbox.mozrest.com/v1/bc/booking/{id}" \
  -H "Authorization: Bearer {{api_key}}" \  
  --data-raw '{
    "cancelActor": "me",
    "cancelReason": "test cancel"
  }'
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
  "paymentGatewayToken":null,
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

`PUT https://api-sandbox.mozrest.com/v1/bc/booking/{id}/cancel`  
 
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
curl GET "https://api-sandbox.mozrest.com/v1/bc/booking/{id}" \
  -H "Authorization: Bearer {{api_key}}" 
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
  "paymentGatewayToken":null,
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

`GET https://api-sandbox.mozrest.com/v1/bc/booking/{id}`  
 
### Query Parameters

Parameter | Status | Description
--------- | ------- | -----------
id | **Mandatory** | Booking id to retrieve


## Get a list of bookings
  
This endpoint allows to retrieve a list of bookings.

> EXAMPLE REQUEST:  

```shell
curl GET "https://api-sandbox.mozrest.com/v1/bc/booking" \
  -H "Authorization: Bearer {{api_key}}" \  
  -d "offset=0" \
  -d "limit=10" \
  -d "email='john.doe@gmail.com'" \  
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
        "paymentGatewayToken":null,
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

`GET https://api-sandbox.mozrest.com/v1/bc/booking/{id}`  
 
### Query Parameters

Parameter | Status | Description
--------- | ------- | -----------
email | **Optional** | User email that owns the booking
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
  "action": "update",
  "data": {
    "id": "60e890aca5f07b6ee5b950b1",
    "venueId": "60e890aca5f07b6ee5b950b1",
    "partySize": 4,
    "status": "confirmed",
    "date": "2021-11-06T12:00:00+02:00",
    "notes": "I'm alergic to peanuts",
    "paymentGatewayToken":null,
    "contact": {
      "id": "60e890aca5f07b6ee5b950b1",
      "firstname": "John",a
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
}
```
> CANCELED EXAMPLE JSON:

```json
{
  "entity": "booking",
  "entity_id": "60e5a3ed409541da3650bd90",
  "action": "canceled",
  "data": {
    "id": "60e890aca5f07b6ee5b950b1",
    "venueId": "60e890aca5f07b6ee5b950b1",
    "partySize": 4,
    "status": "canceled",
    "date": "2021-11-06T12:00:00+02:00",
    "notes": "I'm alergic to peanuts",
    "cancelActor": "restaurant",
    "cancelReason": "Canceled due internal issues",
    "contact": {
      "id": "60e890aca5f07b6ee5b950b1",
      "firstname": "John",a
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
}
```

Mozrest provides a feature to subscribe to a webhook system to be notified when any relevant information affecting your bookings has an update. Mozrest will send a notification to the given endpoint providing the entity that have been modified, the action and the full object mofified on a data parameter.

<aside class="success">
On the onboarding process, you will provide the Mozrest team with the endpoint you wish to be notified.
</aside>  

# Objects definition
## The Venue Object

> EXAMPLE JSON:

```json
{
  "name": "Venue Name",
  "country": "GB",
  "address": "The Address 24, Picadilly",
  "city": "London",
  "postalCode": "W1J 9LL",
  "phoneNumber": "442072343456",
  "url": "http://your-url.com",
  "latitude": "51.56797",
  "longitude": "-0.28122",
  "capacity": 100,
  "minAdvanceBooking": 1,
  "minAdvanceOnlineCanceling": 24,
  "paymentDefinition": {
    "requireCreditCard": "true",
    "deposit": {
      "type": "fixed_price",
      "amount": 2000
    },
    "noShowFee": {
      "type": "fixed_price",
      "amount": 2000
    }
  },
  "paymentGateway": {
    "gateway": "stripe",
    "apiVersion": "2020-08-27",
    "sdkVersion": null,
    "publishKey": "pk_test_2343823823823",
    "merchantId": null,
    "legalInvoicingName": "Venue Name LTD",
    "paymentCountryCode": "GB"
  },
  "currency": "GBP",
  "timezone": "Europe/London"
}
```

| Key | Type         | Description                                                                                                                                                     |
| --- |--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id | String       | Venue id                                                                                                                                                        |
| name | String       | Venue name                                                                                                                                                      |
| country | String       | Country code where the venue is located (<a href="https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2" target="_blank">ISO2</a> format)                            |
| address | String       | Address where the venue is located                                                                                                                              |
| city | String       | City where the venue is located                                                                                                                                 |
| postalCode | String       | Postcode where the venue is located                                                                                                                             |
| phoneNumber | String       | Venue's phone number                                                                                                                                            |
| url | String       | Venue's website URL                                                                                                                                             |
| latitude | String       | Venue's latitude                                                                                                                                                |
| longitude | String       | Venue's longitude                                                                                                                                               |
| minAdvanceBooking | Int          | Minimum hours in advance a booking have to be placed                                                                                                            |
| minAdvanceOnlineCancelling | Int          | String hours in advance a booking can be canceled                                                                                                               |
| paymentDefinition[requireCreditCard] | Int          | Does the booking require a Credit Card Guarantee? `["true", "false"]`                                                                                           |
| paymentDefinition[deposit][type] | String       | What type of deposit is? `["fixed_price", "per_person"]`                                                                                                        |
| paymentDefinition[deposit][amount] | Int          | Deposit amount. Integer multiply per 100. Ie. 20,00€: 2000                                                                                                      |
| paymentDefinition[noShowFee][type] | String       | What type of no show fee is? `["fixed_price", "per_person"]`                                                                                                    |
| paymentDefinition[noShowFee][amount] | Int          | No show fee amount. Integer multiply per 100. Ie. 20,00€: 2000                                                                                                  |
| paymentGateway[gateway] | String       | Payment gateway used `[ stripe, braintree, adyen ]`                                                                                                             |
| paymentGateway[apiVersion] | String       | Payment gateway API version you use. Ie: Stripe API version: 2020-08-27                                                                                         |
| paymentGateway[sdkVersion] | String       | Payment gateway SDK version you use. Ie: Braintree: 3.85.2                                                                                                      |
| paymentGateway[publishKey] | String       | Publishable key for the payment gateway                                                                                                                         |
| paymentGateway[merchantId] | **Optional** | Your Merchant Id for the payment gateway                                                                                                                        |
| paymentGateway[legalInvoicingName] | String       | Name displayed on the customer's invoice                                                                                                                        |
| paymentGateway[paymentCountryCode] | String       | Country code where the payment is legally charged (<a href="https://en.wikipedia.org/wiki/ISO_4217" target="_blank">ISO2</a> format)                            |
| currency | String       | Currency code used for payments (<a href="https://en.wikipedia.org/wiki/ISO_4217" target="_blank">ISO 4217</a> format)                                          |
| timezone | String       | Time zone where the Venue is located (see possible <a href="https://en.wikipedia.org/wiki/List_of_tz_database_time_zones" target="_blank">time zone values</a>) |

## The Availability Object

> EXAMPLE JSON:

```json
{
  "date": 1664377200,
  "partySize": 4,
  "slots": [
    {
      "time": "14:00"
    },
    {
      "time": "14:15"
    },
    {
      "time": "14:30"
    },
    {
      "time": "14:45"
    },
    {
      "time": "15:00"
    },
    {
      "time": "15:15"
    },
    {
      "time": "15:30"
    },
    {
      "time": "15:45"
    },
    {
      "time": "16:00"
    }
  ],
  "openingTimes": [
    {
      "time": "09:00"
    },
    ...
    {
      "time": "21:00"
    }
  ]
}
```

| Key                        | Type | Description                                                                                                                     |
|----------------------------| ---- |---------------------------------------------------------------------------------------------------------------------------------|
| date                       | String | Date requested for availability (format: YYYY-MM-DD)                                                                            |
| partySize                  | Int | Number of people                                                                                                                |
| slots                      | Array | List of maximum 9 available slots around the selected time                                                                      |
| openingTimes               | Array | Full list of available slots                                                                                                    |
| nextAvailability[date]     | Array | Next slot available. Just available when there is not availability for the requested slot.                                      |
| nextAvailability[slots]    | Array | Next slot available in HH:ii format (ie. 14:45). Just shown when the next available slot is for the same requested day.         |
| previousAvailability[date] | Array | Previous slot available for the same day if there is not availability for the requested slot.                                   |
| previousAvailability[slots]    | Array | Previous slot available in HH:ii format (ie. 12:45). Just shown when the previous available slot is for the same requested day. |

## The Pending Booking Object

> EXAMPLE JSON:

```json
{
  "id": "61bb23a701a5ac259b85e3f3",
  "notes": {
    "en": "Reservation is for Taverna(1.floor). Ristorante (2. floor) is closed every Sunday.",
    "fi": "Varaus on Tavernan (1.krs) puolelle. Ristorante (2.krs) on sunnuntaisin suljettu."
  },
  "holdingTime": 420
}
```

This is how the pending booking object structure looks like.

| Key | Type | Description |
| --- | ---- | ----------- |
| id | String | Pending Booking id |
| notes | Text | Collection of notes provided by the venue |
| holdingTime | Integer | How long the RMS will hold the booking (in seconds) |

## The Booking Object

> EXAMPLE JSON:

```json
{
  "id": "60e890aca5f07b6ee5b950b1",
  "venueId": "60e890aca5f07b6ee5b950b1",
  "bookingChannelId": "60e890aca5f07b6ee5b950b3",
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


| Key                          | Type | Description                                                                                                           |
|------------------------------| ---- |-----------------------------------------------------------------------------------------------------------------------|
| id                           | String | Booking id                                                                                                            |
| venueId                      | String | Venue id for booking                                                                                                  |
| bookingChannelId             | String | Booking channel id where the customer placed the booking                                                              |
| status                       | String | Booking status `[pending, confirmed, canceled, faked, honored]`                                                       |
| partySize                    | Int | Number of people                                                                                                      |
| session                      | String | Booking session                                                                                                       |
| date                         | Datetime | Booking date and time in Timestamp UTC format (ie. Saturday, 16 October 2021 14:30 is 1634387400 on timestamp)        |
| notes                        | Text | User notes                                                                                                            |
| cancelActor                  | String | Who had canceled the booking                                                                                          |
| cancelReason                 | Text | Why the booking has been canceled                                                                                     |
| canceledAt                   | Datetime | When the booking has been canceled (UTC)                                                                              |
| paymentGatewayToken          | String | Payment gateway token for customers credit card                                                                       |
| contact[firstname]           | Text | Customer's first name                                                                                                 |
| contact[lastname]            | Text | Customer's last name                                                                                                  |
| contact[email]               | Text | Customer's email (ie. john.doe@email.com)                                                                             |
| contact[telephone]           | Int | Customer's telephone including country code but NO sign (ie. +44 20 7234 3456 -> 442072343456 )                       |
| contact[locale]              | Text | Customer's language (<a href="https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes" target="_blank">ISO2</a> format) |
| contact[address][country]    | Text | Customer's country (<a href="https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2" target="_blank">ISO2</a> format)       |
| contact[address][city]       | Text | Customer's city                                                                                                       |
| contact[address][region]     | Text | Customer's region                                                                                                     |
| contact[address][street]     | Text | Customer's address                                                                                                    |
| contact[address][postalCode] | Text | Customer's postal code                                                                                                |
| contact[optInConsent]        | Boolean | Allow to receive marketing emails from the Venue and the Partner (default: false)                                     |

