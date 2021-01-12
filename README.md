![alt text](https://static.parclick.com/assets/img/logotipo-parclick.svg)

<p style="color:#999999;text-align:right;">Document version 1.5.6</p>

# <span style="color:#FF6600;">Parclick - API Reference</span>
#### <span style="color:#999;">TECHNICAL DOCUMENTATION</span>



### <span style="color:#FF6600;">Content table</span>

1. [API description](#description)
2. [Login](#login)
3. [Refresh token](#refresh)
4. [List parkings](#list_parkings)
5. [Get parking](#get_parking)
6. [Bestpass](#bestpass)
7. [Vehicle type](#vehicle_type)
8. [Create reservation](#create_reservation)
9. [Cancel reservation](#cancel_reservation)
10. [Modify reservation](#modify_reservation)
11. [List reservations](#list_reservation)
12. [Get voucher details](#voucher_details)
13. [Download voucher](#download_voucher)
13. [Worflows](#workflows)
    * [Workflow to create a booking reservation](#workflow_reservation)
    * [Workflow to cancel a booking](#workflow_cancel)
    * [Workflow to list booking](#workflow_list)
14. [Generate entry code](#entry_code)

<br>

### <a name="description"></a><span style="color:#FF6600;">API Description</span>

#### Authentication
Parclick gives a <a href="#1">REST API</a> [^1](#1) with some endpoints that require authentication. Security is based on <a href="#2">JSON Web Token (JWT)</a> [^2](#2) and is obtained through a successful authentication (view **/v1/login** endpoint). In the protected endpoints this token must be included in the header request as follow:

| Key | Value |
| ----------------- | ------------------------------------------ |
| Authorization | Bearer eyJhbGciOiJSUzI1NiJ9.eyJyb2xlcyI... |

Also is mandatory to include a custom header parameter **X-thirdparty-auth** to monitor the API traffic.

| Key | Value |
| ----------------- | ------------------------------------------ |
| X-thirdparty-auth | request the hash to Parclick  |

#### Environments
Parclick has two environments: [**development**](https://pre.api.parclick.com) (https://pre.api.parclick.com) and [**production**](https://api.parclick.com) (https://api.parclick.com).

This tutorial help to unsderstand the available endpoints and the flow to create, delete and list bookings. 

#### Internationalization, limit and group
As a general rule, the application manage the **locale** parameter to return the response in the selected language. The available languages are: spanish, english, french, italian, portuguese, german, dutch, russian and catalan.

| Language | Locale |
| ------------- | ------ |
| Catalan | ca_ES |
| Dutch | nl_NL |
| English | en_GB |
| French | fr_FR |
| German | de_DE |
| Italian | it_IT |
| Portuguese | pt_PT |
| Russian | ru_RU |
| Spanish | es_ES |

The **group** parameter in the endpoints that have it available allows to show more or less information in the answer. Each endpoint that has it available shows which is the optimal **group** option.

Some of the list endpoint return by defauls a maximum of 200 records. The **limit** parameter can be defined to customize the number of results. This parameter is defined in the documentation of each specific endpoint.

<br>

## <a name="login"></a><span style="color:#FF6600;">Login</span>

### <span style="color:#10a54a;">`POST`</span> `/v1/login`


This method allows third party systems to be authenticated and receive a key (token) to pass when calling the e-commerce web service methods.

### _Request:_

| Parameters | Type | Required |
| --------------- |:--------------- | -------- |
| \_username | string | true |
| \_password | string | true |



### _Response:_

**200 Ok**

| Parameters | Type | Description |
| --------------- |:-------------- | :----------------------------------- |
| token | string | token (expires in 1 hour) |
| refresh_token | string | token to refresh current valid token | 
| id | string | user id authenticated |


<details><summary style="color:#FF6600;"> Show response 200 Ok </summary>
<p>
<pre>
{
 "token": "eyJhbGciOiJSUzI1NiJ9.eyJyb2xlcyI6WyJST0xFX1NVUEVSX0FETUlOIiwiUk9MRV9VU0VSIl0sInVzZXJuYW1lIjoibWlraS5wZXRyb3ZpY0BwYXJjbGljay5jb20iLCJleHAiOjE1NTIzMjAyODgsImlhdCI6MTU1MjMxNjY4OH0.jXllvfZWoXvw-1QGkB4kQGccKVJOUBtFP5VF37Oq8GosLhO2xwqeUVEs3vIj4C030SaZBsmvDNgGY3os4_qMWFgw5EPtPF_n5bLRtQ-BcKAuvqnQmNA9dk5ExxTQEUI08opCDFTxp4nmd6tU97TtCUpKQaOcqR-KP1XEgKaZf2hQNykUPP1k_9Z4NEHdVTy5tSnroqP1hyoF_S7AaxM-XNlKmwnIhmbWgeYmOD41ogBXeRRgiUuP8SelgJchWd2rLcAl7MTDMnW_8EvrpfjaY8nVtENsMZu5_CZhdhUzSBKcWSJHMBGhWf7NWINBib23YNc0ut6sYdNCQ-BagXYq2Riy6gn1SbocUAe9Yt6nmL_9bTzqKhku85S6tZcr9IyRiGOlyU0vZfAOBt98FZ9SUemaDufyHOfY-fvyP7iq-efSyE1IY-FZnabwHQtDl3Z5j_VvMqrJs2tPJX27HI6gNIgE7rKXL-YFHK__nOofR2H9cUp6BSEcmAGjD7zVDilWgSUPZGOp6yLr4w-Fu1NgpPIJt6KZxqcpxCU-nhvbn3wIpGm6b-9mNDZy9yhJftJMuLGeqLMcO2IRnDTQK8fYEodTsfGmqU-b7bj_HEAdxgTFw0CeFT6cmMnVo-wUZHcrKWKgiJjBRUlHTM6wD6JBClHb0iTkjtQg7oqb9iHxzWA",
 "refresh_token": "81a2467f1093aeb96d83b1c46dfe3ad0e0f341f4190f73c17a37351e41693c546d70a01b01671f80962112a7a4090c37caa397161da87f69cd334bc460cae424",
 "id": 600116
}
</pre>
</p>
</details>
<br>

**401 Unauthorized**

| Parameters | Type | Description |
| --------------- |:--------------- | ---------------------------- |
| code | integer | http response code value |
| message | string | error description |

<details><summary style="color:#FF6600;">Show response 401 Unauthorized</summary>
<p>
<pre>
{
 "code": 401,
 "message": "Bad credentials"
}
</pre>
</p>
</details>

<br>
 
## <a name="refresh"></a><span style="color:#FF6600;">Refresh token</span>

### <span style="color:#10a54a;">`POST`</span> `/v1/token/refresh`

This method allows third party systems to refresh a valid token.

### _Request:_

| Parameters | Type | Required | Description |
| --------------- |:------------------- | ---------|------------------------- |
| refresh_token | string | true | third party identificator|


### _Response:_

**200 Ok**

| Parameters | Type | Description |
| --------------- |:------------------- |-------------------------------------------------- |
| token | string | token passed with each request (Expires in 1 hour)|
| refresh_token | string | token to refresh current valid token |


<details><summary style="color:#FF6600;">Show response 200 Ok</summary>
<p>
<pre>
{
 "token": "eyJhbGciOiJSUzI1NiJ9.eyJyb2xlcyI6WyJST0xFX1VTRVIiXSwidXNlcm5hbWUiOiJtaWtpLnBldHJvdmljQHBhcmNsaWNrLmNvbSIsImV4cCI6MTU1MjQ5MDgxNiwiaWF0IjoxNTUyNDg3MjE2fQ.dwp3pbUF4vktlX_vIhuiYJQ3XwXmJHJe_gl8LrZUyJ3VZ1pEsFsvtbl8Q3C82tJlxslO9SJWI1rDcHGsGni0FIUxHbKEuuTNwzM2n4EfS25HSTa-0FQJckBhQBa8CRDBTCbfNrNUxPM2tie0ILrTnZaGIMHjtL9px08M2fEqYkC0SF3haYnhvjg40DfxCIcba-_SkXJyQGDsJEQeMiCeNWQNe4gap-Vg_lLFmhKzuWvp5PHRMDqPZ1IbFXfqbZEn79dobwr93lBNce_tBmeOwBhOI-OtIeVcwhhy5998wV0V_KxlF-LgxBKsr6cyWwS1JGStNU2BbqQcjGh3wGqhU42g-29zhI0Pn2fLuUAqdwL73c4wVJ-iZx4UMIY1XI0poBBX6cjImuN8JzPFZbwjW4VHGZ6G7VNvBZsngrQ7QQngM3OFkQ19vTCo0O9kXJqjBkR53WjFxuHCiT7AaQd18DoR51L7HejthunO0sqQGNZoPhWtrLZMcnc9vq3DFJI5YxeFWuB5sAIwMUi-xLk3l8ijReSQ63sbUrVEOX3mrDE8N1zOfHt_OoniniVdYY7pUDczDmsybye_JQjxcu9o96Em0Pd37ABcf_-y8LjDgIgjkceFVJiBArAhcZZGtoxexPaRcRyzE698TARXsC6EmLih0TEaA8gZNsAOJL5U2e4",
 "refresh_token": "7635bc6f8970fa9042297a692dfba629a346dff4873fd34ddcea82c4be74e738414164e69d455ecafc29b5d5082bb2bff08082ec15f62dfc2a2bd47701eb7976"
}
</pre>
</p>
</details>

<br>

**401 Unauthorized**

| Parameters | Type | Description |
| --------------- |:--------------- | ---------------------------- |
| code | integer | http response code value |
| message | string | error description |

<details><summary style="color:#FF6600;">Show response 401 Unauthorized</summary>
<p>
<pre>
{
 "code": 401,
 "message": "Bad credentials"
}
</pre>
</p>
</details>

<br>

## <a name="list_parkings"></a><span style="color:#FF6600;">List parkings</span>

### <span style="color:#0f6ab4;">`GET`</span> `/v1/parking/list`


This method returns a list of car parks close to a location and based on the parameters used. In case you select one the **id** let get the parking details using the **get parking** endpoint. This method is secured via JWT token.
Depending on the group used, it is necessary to include certain parameters.<br>
Group export: locale, page, limit - Has pagination<br>
Group search: locale, latitude, longitude, radius, from, to, vehicleType, limit (with a maximum of 200 results) - Does not have pagination<br>
To import all car parks for caching, the group must be set to **export** without parameters except the page and limit for pagination.<br>
<strong>Parking schedule:</strong> There is a schedule object in the response that defines the car park timetable for the car park the staff or both. This object can arrive empty and is only for information purposes, as the bestpass endpoint checks for each booking between other things that the car park is open.
<p>
<pre>
"schedule": [
                {
                    "id": 1006,
                    "type": {
                        "id": 2,
                        "name": "PARKING"
                    },
                    "week_day": {
                        "key": "THURSDAY",
                        "order": 4
                    },
                    "intervals": [
                        {
                            "id": 1033,
                            "hour_from": "1970-01-01T07:00:00+0000",
                            "hour_to": "1970-01-01T23:59:00+0000"
                        }
                    ]
                },
                {
                    "id": 1007,
                    "type": {
                        "id": 2,
                        "name": "PARKING"
                    },
                    "week_day": {
                        "key": "FRIDAY",
                        "order": 5
                    },
                    "intervals": [
                        {
                            "id": 1034,
                            "hour_from": "1970-01-01T07:00:00+0000",
                            "hour_to": "1970-01-01T23:59:00+0000"
                        }
                    ]
                },
                {
                    "id": 1008,
                    "type": {
                        "id": 2,
                        "name": "PARKING"
                    },
                    "week_day": {
                        "key": "SATURDAY",
                        "order": 6
                    },
                    "intervals": [
                        {
                            "id": 3685,
                            "hour_from": "1970-01-01T07:00:00+0000",
                            "hour_to": "1970-01-01T23:59:00+0000"
                        }
                    ]
                },
                {
                    "id": 1009,
                    "type": {
                        "id": 2,
                        "name": "PARKING"
                    },
                    "week_day": {
                        "key": "SUNDAY",
                        "order": 7
                    },
                    "intervals": [
                        {
                            "id": 1036,
                            "hour_from": "1970-01-01T07:00:00+0000",
                            "hour_to": "1970-01-01T23:59:00+0000"
                        }
                    ]
                },
                {
                    "id": 1010,
                    "type": {
                        "id": 2,
                        "name": "PARKING"
                    },
                    "week_day": {
                        "key": "MONDAY",
                        "order": 1
                    },
                    "intervals": [
                        {
                            "id": 1037,
                            "hour_from": "1970-01-01T07:00:00+0000",
                            "hour_to": "1970-01-01T23:59:00+0000"
                        }
                    ]
                },
                {
                    "id": 1011,
                    "type": {
                        "id": 2,
                        "name": "PARKING"
                    },
                    "week_day": {
                        "key": "TUESDAY",
                        "order": 2
                    },
                    "intervals": [
                        {
                            "id": 1038,
                            "hour_from": "1970-01-01T07:00:00+0000",
                            "hour_to": "1970-01-01T23:59:00+0000"
                        }
                    ]
                },
                {
                    "id": 1012,
                    "type": {
                        "id": 2,
                        "name": "PARKING"
                    },
                    "week_day": {
                        "key": "WEDNESDAY",
                        "order": 3
                    },
                    "intervals": [
                        {
                            "id": 1039,
                            "hour_from": "1970-01-01T07:00:00+0000",
                            "hour_to": "1970-01-01T23:59:00+0000"
                        }
                    ]
                }
            ]
</pre>
</p>
The car park features are the following fields with their meaning:

| Field | Description | Type |
|--------------------|-----------------------------------|-------------|
| covered | Covered and guarded parking | boolean |
| handicapped_access | Disabled place | boolean |
| security | Security, CCTV | boolean |
| giving_keys | Mandatory to leave the keys | boolean |
| open_24h | 24h service | boolean |
| max_height | Maximum height in centimeters | integer |

To identify if a car park has a valid product for the dates and vehicle selected, it is necessary to access the *passes* node and select the first one which is the closest one. This result is indicative since it is always mandatory to consult the bestpass to confirm availability. If the node *passes* is empty this parking does not have availability. Another possibility is the AENA car parks that show a product with a price -127. These parkings should consult the price with the bestpass.

### _Header:_
Authorization: Bearer {JWT_TOKEN}

### _Request:_

| Parameters | Type | Required | Description | Default |
| --------------- |:---------------------- | -------- | --------------------------------------------------------------------------------- |-------------------- |
| locale | string | true | language in which the information will be returned | en_GB |
| group | string | true | **search** **export** | null |
| limit | integer | false | total number of records [1-200] | 200 |
| page | integer | false | page selected | 1 |
| from | string Y-m-d H:i | false | booking start date | null |
| to | string Y-m-d H:i | false | booking end date | null |
| latitude | float | false | valid latitude in which to look for | null |
| longitude | float | false | valid longitude in which to look for | null |
| radius | integer | false | search radius | null |
| vehicleType | integer | false | type of vehicle you wish to park [1 car, 2 van, 3 caravan, 4 bus, 5 truck, 6 motorbike, 7 small truck]| null |

### _Response:_

**200 Ok group search-joinup**

<details><summary style="color:#FF6600;">Show response 200 Ok</summary>
<p>
<pre>
{
    "items": [
        {
            "id": 260,
            "covered": true,
            "description": "\u003Cstrong\u003EThis car park is located in the area of CENTRAL MADRID.\u003C\/strong\u003E\u2028\r\nPlease note that \u003Cstrong\u003Eyou will only be able to enter Madrid Central WITHOUT A FINE if you are going to a car park\u003C\/strong\u003E (for vehicles with the label B or C). Don\u0027t take the risk of not having a space available when you arrive and end up with a fine because the car park was full... As of now, book your place in this car park to make sure you can park as soon as you arrive!\r\n\r\nIncredibly well-located car park right in the centre of Madrid, a 5-minute walk from the Puerta del Sol and Plaza Mayor, in the main tourist area which is full of shops, bars and theatres. The car park is ideal if you wish to leave your car here are walk to the Gran V\u00eda and the Royal Palace.",
            "flexible_entry": false,
            "latitude": 40.414339688923,
            "longitude": -3.7034317950878,
            "medias": [
                {
                    "id": 7315,
                    "url": "https:\/\/static.parclick.com\/parking\/2017\/07\/d85\/780\/c4\/d85780c4-bfb4-500f-ab89-1f47338e72ef.jpeg"
                },
                {
                    "id": 7316,
                    "url": "https:\/\/static.parclick.com\/parking\/2017\/07\/148\/8b0\/35\/1488b035-a4d3-5dc2-9e25-2f281d2dd183.jpeg"
                },
                {
                    "id": 7317,
                    "url": "https:\/\/static.parclick.com\/parking\/2017\/07\/d6d\/093\/70\/d6d09370-e3c3-58e6-991b-77f270ed232b.jpeg"
                },
                {
                    "id": 12127,
                    "url": "https:\/\/static.parclick.com\/parking\/2018\/12\/33d\/691\/76\/33d69176-b9bb-5b56-b4fa-6cef4d3faa37.jpeg"
                }
            ],
            "address": "Plaza Jacinto Benavente, S\/N",
            "city": "Madrid",
            "country": "Espa\u00f1a",
            "province": "Madrid",
            "zip": "28012",
            "provider": {
                "id": 19,
                "name": "EMT"
            },
            "handicapped_access": true,
            "security": true,
            "open_24h": true,
            "giving_keys": false,
            "exclude_checking": false,
            "name": "EMT Jacinto Benavente",
            "max_height": 180,
            "passes": [
                {
                    "id": 33982,
                    "price": 15,
                    "type": "pass",
                    "internal_name": "ONEPASS 6h (15.00\u20ac)",
                    "duration": 6,
                    "multiparking": false,
                    "multipass": false,
                    "frequency": "HOURLY_RANGE",
                    "_links": {
                        "self": [
                            {
                                "href": "https:\/\/loc.api.parclick.com\/v1\/product\/33982"
                            },
                            {
                                "href": "https:\/\/loc.api.parclick.com\/v1\/pass\/33982"
                            }
                        ]
                    }
                }
            ],
            "schedule": [],
            "voucher_needed": false,
            "multiparking": false,
            "cancellation_type": 2,
            "_links": {
                "self": {
                    "href": "https:\/\/loc.api.parclick.com\/v1\/parking\/260"
                }
            }
        },
        {
            "id": 1952,
            "covered": true,
            "description": "\u003Cstrong\u003EThis car park is located in the area of CENTRAL MADRID.\u003C\/strong\u003E\r\n\u2028Please note that \u003Cstrong\u003Eyou will only be able to enter Madrid Central WITHOUT A FINE if you are going to a car park\u003C\/strong\u003E (for vehicles with the label B or C). Don\u0027t take the risk of not having a space available when you arrive and end up with a fine because the car park was full... As of now, book your place in this car park to make sure you can park as soon as you arrive!\r\n\r\n\u003Cstrong\u003EIMPORTANT: HOW TO ACCESS THE CAR PARK.\u003C\/strong\u003E\r\nThe Jardines 16 car park is located in a pedestrian area and the customers who are directed to it have permission from the Madrid City Council to travel through these pedestrian streets to the car park. \r\n\u003Cstrong\u003EHOW TO GET THERE:\u003C\/strong\u003E from Cibeles, go up Calle Alcal\u00e1, turn right along Calle Virgen de los Peligros and then take the first left on Calle Aduana until Calle Montera. Turn right on \u003Cstrong\u003EMontera (it\u00b4s pedestrian, but you can drive)\u003C\/strong\u003E and turn right again on Calle Jardines, where you\u00b4ll find the car park.\r\n[If you\u00b4re using GPS, we recommend using \u00b4bicycle\u00b4 mode to get the precise directions]\r\n\r\n-------------------------\r\n\r\nParking at Puerta del Sol? No...because you can\u00b4t, but that\u00b4s what we\u00b4re here for! As an option, we offer you parking in Calle Jardines 16, which is one of Parclick\u00b4s most central car parks. Here you can leave your car just minutes from Kil\u00f3metro 0 or almost on Gran V\u00eda itself!\r\n\r\nShopping on Preciados, Montera or Fuencarral? Going to the cinema, to the theatre or to sing in some musical on Gran V\u00eda? This is your car park! On Calle Jardines 16 you can calmly leave your car for a few hours or even for a few days if, for example, you have reserved a hotel in the centre of Madrid and you want to get around by foot. It\u00b4s a \u003Cstrong\u003Esecure car park, open 24 hours\u003C\/strong\u003E and with staff always present, where you can \u003Cstrong\u003Epark your car or van\u003C\/strong\u003E safely. The car park is covered and also offers open spaces for vehicles which exceed 2.10 metres in height.\r\n\r\nA few minutes walk away from the car park you\u00b4ll find the Plaza del Carmen, the Real Academia de Bellas Artes de San Fernando or the Casino de Madrid, and, if you leave your car in Jardines 16 and cross Gran V\u00eda, you\u00b4ll be next to the Fundaci\u00f3n Telef\u00f3nica and the Chueca neighbourhood. So if the day is over before you realise...no problem! You won\u00b4t have to think about parking times because it is open and available at any time of the day!\r\n\r\nAlso you can leave your car in this very central car park and travel to other places in the city by metro, since, practically a stone\u00b4s throw away, you\u00b4ll find the Gran V\u00eda station (line 1) and the Sevilla station (line 2).\r\n\r\nDon\u00b4t hesitate and make your reservation for this car park in the centre of Madrid!\r\n\r\nAt this car park, you must leave your car keys with the staff.",
            "flexible_entry": true,
            "latitude": 40.418888333632,
            "longitude": -3.7010293058413,
            "medias": [
                {
                    "id": 9766,
                    "url": "https:\/\/static.parclick.com\/parking\/2017\/12\/63b\/a16\/b1\/63ba16b1-9060-5794-95d3-cbc41f96cf40.jpeg"
                },
                {
                    "id": 9767,
                    "url": "https:\/\/static.parclick.com\/parking\/2017\/12\/c5f\/cb4\/0d\/c5fcb40d-cebc-5f2e-9d74-6bb8344efa33.jpeg"
                },
                {
                    "id": 12128,
                    "url": "https:\/\/static.parclick.com\/parking\/2018\/12\/33d\/691\/76\/33d69176-b9bb-5b56-b4fa-6cef4d3faa37.jpeg"
                }
            ],
            "address": "Calle Jardines, 16",
            "city": "Madrid",
            "country": "Espa\u00f1a",
            "province": "Madrid",
            "zip": "28013",
            "provider": {
                "id": 422,
                "name": "MOVEL CASTRO"
            },
            "handicapped_access": false,
            "security": true,
            "open_24h": true,
            "giving_keys": true,
            "exclude_checking": false,
            "name": "Jardines 16 - Centro Madrid",
            "max_height": 295,
            "passes": [
                {
                    "id": 51580,
                    "price": 5.8,
                    "type": "pass",
                    "internal_name": "ONEPASS 2h (5.80\u20ac)",
                    "duration": 2,
                    "multiparking": false,
                    "multipass": false,
                    "frequency": "HOURLY",
                    "_links": {
                        "self": [
                            {
                                "href": "https:\/\/loc.api.parclick.com\/v1\/product\/51580"
                            },
                            {
                                "href": "https:\/\/loc.api.parclick.com\/v1\/pass\/51580"
                            }
                        ]
                    }
                }
            ],
            "schedule": [],
            "voucher_needed": false,
            "multiparking": false,
            "cancellation_type": 2,
            "_links": {
                "self": {
                    "href": "https:\/\/loc.api.parclick.com\/v1\/parking\/1952"
                }
            }
        }
    ],
        "total": 135,
        "params": {
            "locale": "en_GB",
            "group": "search-joinup",
            "latitude": "40.4167754",
            "longitude": "-3.7037901999999576",
            "radius": "4",
            "from": "2020-02-22 20:00",
            "to": "2020-02-22 22:00",
            "vehicleType": "1",
            "page": "1",
            "limit": ""
        }
    }
</pre>
</p>
</details>


### _Response:_

**200 Ok group export-joinup**

<details><summary style="color:#FF6600;">Show response 200 Ok group export-joinup</summary>
<p>
<pre>
{
    "page": "1",
    "limit": "2",
    "pages": 764,
    "items": [
        {
            "id": 4,
            "covered": true,
            "description": "Car park located in the district of Sant Gervasi-Galvany, near to Avenida Diagonal and a 10-minute walk from Barcelona Clinical and Provincial Hospital.\r\n\r\n<p>IMPORTANT: Remember when you book to give us the number of the mobile phone which you'll be using while visiting Barcelona. </p>\r\n<p>TELEPHONIC access to car park. REMOTE customer service. </p>",
            "flexible_entry": true,
            "latitude": 41.395107706311,
            "longitude": 2.1466153666872,
            "medias": [
                {
                    "id": 9396,
                    "url": "https://static.parclick.com/parking/2017/11/cca/e73/a1/ccae73a1-ae1d-563c-b275-14577efbdf1a.jpeg"
                },
                {
                    "id": 9397,
                    "url": "https://static.parclick.com/parking/2017/11/5ee/981/54/5ee98154-09fc-57ca-b3f0-d05ba085bba9.jpeg"
                },
                {
                    "id": 9398,
                    "url": "https://static.parclick.com/parking/2017/11/9ab/225/2e/9ab2252e-2977-52e3-8541-bb52d14a1eb3.jpeg"
                },
                {
                    "id": 9399,
                    "url": "https://static.parclick.com/parking/2017/11/77a/31f/88/77a31f88-89b8-5303-93d6-f18c48542057.jpeg"
                }
            ],
            "address": "Carrer de Santaló, 12",
            "city": "Barcelona",
            "country": "España",
            "province": "Barcelona",
            "zip": "8021",
            "provider": {
                "id": 2,
                "name": "NYN"
            },
            "handicapped_access": true,
            "security": true,
            "open_24h": true,
            "giving_keys": false,
            "exclude_checking": true,
            "name": "NN Santaló",
            "access_phone": "00 34 647 07 97 31",
            "max_height": 180,
            "schedule": [],
            "access": [
                {
                    "id": 938,
                    "type": "vehicle",
                    "address": "Carrer de Santaló 12",
                    "latitude": 41.395107706311,
                    "longitude": 2.1466153666872
                }
            ],
            "voucher_needed": false,
            "multiparking": false,
            "cancellation_type": 2,
            "_links": {
                "self": {
                    "href": "https://pre.api.parclick.com/v1/parking/4"
                }
            },
            "_embedded": {
                "city": {
                    "id": 6,
                    "name": "Barcelona",
                    "country": {
                        "id": 199,
                        "iso": "ES",
                        "nicename": "Spain",
                        "_links": {
                            "self": {
                                "href": "https://pre.api.parclick.com/v1/country/199"
                            }
                        }
                    },
                    "vat_tax": 21,
                    "time_zone": "Europe/Madrid",
                    "_links": {
                        "self": {
                            "href": "https://pre.api.parclick.com/v1/city/6"
                        }
                    }
                },
                "fieldsRequested": []
            }
        },
        {
            "id": 5,
            "covered": true,
            "description": "Car park close to the Hospital Clínic and the Teatre Villarroel. The metro stations Hospital Clínic (line 5) and Urgell (line 1) are both roughly a 10-minute walk away. Easy access to the Gran Vía de las Cortes Catalanas and Avenida Diagonal.",
            "flexible_entry": true,
            "latitude": 41.386092962311,
            "longitude": 2.1536075582364,
            "medias": [
                {
                    "id": 9420,
                    "url": "https://static.parclick.com/parking/2017/11/4da/76d/b3/4da76db3-f676-5850-843b-c1c1b02c36b6.jpeg"
                },
                {
                    "id": 9421,
                    "url": "https://static.parclick.com/parking/2017/11/8ad/b6c/ce/8adb6cce-f5de-54b3-b995-6900d2ab2db1.jpeg"
                },
                {
                    "id": 9422,
                    "url": "https://static.parclick.com/parking/2017/11/e86/ad9/60/e86ad960-f131-5ed4-8260-d123b8b88731.jpeg"
                },
                {
                    "id": 9423,
                    "url": "https://static.parclick.com/parking/2017/11/fa4/067/b8/fa4067b8-9ca3-5d87-81e0-d3d3a10133bc.jpeg"
                }
            ],
            "address": "Comte d'Urgell, 154",
            "city": "Barcelona",
            "country": "España",
            "province": "Barcelona",
            "zip": "8036",
            "provider": {
                "id": 2,
                "name": "NYN"
            },
            "handicapped_access": true,
            "security": true,
            "open_24h": true,
            "giving_keys": false,
            "exclude_checking": false,
            "name": "NN Urgell 2",
            "max_height": 195,
            "schedule": [],
            "access": [
                {
                    "id": 343,
                    "type": "vehicle",
                    "address": "Carrer del Comte d'Urgell 152",
                    "latitude": 41.386092962311,
                    "longitude": 2.1536075582364
                }
            ],
            "voucher_needed": false,
            "multiparking": false,
            "cancellation_type": 2,
            "_links": {
                "self": {
                    "href": "https://pre.api.parclick.com/v1/parking/5"
                }
            },
            "_embedded": {
                "city": {
                    "id": 6,
                    "name": "Barcelona",
                    "country": {
                        "id": 199,
                        "iso": "ES",
                        "nicename": "Spain",
                        "_links": {
                            "self": {
                                "href": "https://pre.api.parclick.com/v1/country/199"
                            }
                        }
                    },
                    "vat_tax": 21,
                    "time_zone": "Europe/Madrid",
                    "_links": {
                        "self": {
                            "href": "https://pre.api.parclick.com/v1/city/6"
                        }
                    }
                },
                "fieldsRequested": []
            }
        }
    ],
    "total": 1528,
    "params": {
        "locale": "en_GB",
        "group": "export-joinup",
        "latitude": "40.4167754",
        "longitude": "-3.7037901999999576",
        "radius": "4",
        "from": "2020-02-22 20:00",
        "to": "2020-02-22 22:00",
        "vehicleType": "1",
        "page": "1",
        "limit": "2"
    }
}
</pre>
</p>
</details>

### _Response:_
**200 Ok grooup search**

<details><sumary style="color:#FF6600;">Show response 200 Ok group search</sumary>
<p>
<pre>
{
    "items": [
        {
            "id": 260,
            "reviews_summary": {
                "totalScore": 4.069444444444445,
                "totalReviews": 72
            },
            "inMadridCentral": true,
            "covered": true,
            "flexible_entry": false,
            "guarded": true,
            "latitude": 40.414339688923,
            "image_list": "\/\/static.parclick.com\/parking\/2020\/01\/0c7\/58b\/28\/0c758b28-d18c-5e39-8a1c-7257a6884efe.jpeg",
            "longitude": -3.7034317950878,
            "is_cancellable": true,
            "extra_info": {
                "currency": "euro",
                "code": "EUR",
                "minor_unit": 2,
                "symbol": "\u20ac"
            },
            "parkingCategoryName": "City Center ZTL",
            "passes": [
                {
                    "id": 45439,
                    "name": "ONEPASS 7 heures",
                    "vehicle_type": {
                        "id": 1,
                        "type": "CAR",
                        "max_height": 190,
                        "max_length": 500
                    },
                    "warning_message": "",
                    "price": 18.5,
                    "price_with_administration_fee": 21.19,
                    "type": "pass",
                    "internal_name": "ONEPASS 7h (18.50\u20ac)",
                    "duration": 7,
                    "multiparking": false,
                    "multipass": false,
                    "frequency": "HOURLY",
                    "_links": {
                        "self": {
                            "href": "https:\/\/loc.api.parclick.com\/v1\/pass\/45439"
                        }
                    }
                }
            ],
            "address": "Plaza Jacinto Benavente, S\/N",
            "city": "Madrid",
            "country": "Espa\u00f1a",
            "province": "Madrid",
            "zip": "28012",
            "provider": {
                "id": 19,
                "name": "EMT"
            },
            "handicapped_access": true,
            "security": true,
            "open_24h": true,
            "washing": false,
            "giving_keys": false,
            "name": "EMT Jacinto Benavente",
            "max_height": 180,
            "vat_tax": 21,
            "subscriptions": [],
            "access": [
                {
                    "id": 65,
                    "type": "pedestrian",
                    "latitude": 40.41431740147913,
                    "longitude": -3.7031734820006634
                },
                {
                    "id": 768,
                    "type": "vehicle",
                    "latitude": 40.414339688923,
                    "longitude": -3.7034317950878
                },
                {
                    "id": 3378,
                    "type": "pedestrian",
                    "latitude": 40.4145215942,
                    "longitude": -3.7033531556872
                }
            ],
            "slug": "mm_jacinto_benavente",
            "multiparking": false,
            "passes_by_websocket": false,
            "_links": {
                "self": {
                    "href": "https:\/\/loc.api.parclick.com\/v1\/parking\/260"
                }
            },
            "_embedded": {
                "city": {
                    "id": 37,
                    "name": "Madrid",
                    "slug": "madrid",
                    "_links": {
                        "self": {
                            "href": "https:\/\/loc.api.parclick.com\/v1\/city\/37"
                        }
                    }
                }
            }
        },
        {
            "id": 1952,
            "reviews_summary": {
                "totalScore": 3.371794871794872,
                "totalReviews": 39
            },
            "inMadridCentral": true,
            "covered": true,
            "flexible_entry": true,
            "guarded": true,
            "latitude": 40.418888333632,
            "image_list": "\/\/static.parclick.com\/parking\/2020\/01\/aa4\/4d8\/60\/aa44d860-837b-5d5c-a23e-a9d78c5d3fc8.jpeg",
            "longitude": -3.7010293058413,
            "is_cancellable": true,
            "extra_info": {
                "currency": "euro",
                "code": "EUR",
                "minor_unit": 2,
                "symbol": "\u20ac"
            },
            "parkingCategoryName": "City Center ZTL",
            "passes": [
                {
                    "id": 51580,
                    "name": "ONEPASS 2 heures",
                    "vehicle_type": {
                        "id": 1,
                        "type": "CAR",
                        "max_height": 190,
                        "max_length": 500
                    },
                    "warning_message": "COMMENT ARRIVER : depuis Cibeles, remontez la rue Alcal\u00e1, tournez \u00e0 droite sur la rue Virgen de los Peligros, puis prenez la premi\u00e8re rue \u00e0 gauche (rue Aduana) jusqu\u0027\u00e0 la rue Montera. Tournez \u00e0 droite sur la rue Montera (elle est pi\u00e9tonne mais vous pouvez y circuler), puis tournez de nouveau \u00e0 droite sur la rue Jardines, o\u00f9 se trouve le parking.\r\n\r\nSi vous utilisez le GPS, nous vous conseillons de le mettre en mode \u0022v\u00e9lo\u0022 pour obtenir les indications pr\u00e9cises.\r\n\r\nVeuillez noter que vous devrez laisser les cl\u00e9s de votre v\u00e9hicule dans le parking.",
                    "price": 5.8,
                    "price_with_administration_fee": 5.8,
                    "type": "pass",
                    "internal_name": "ONEPASS 2h (5.80\u20ac)",
                    "duration": 2,
                    "multiparking": false,
                    "multipass": false,
                    "frequency": "HOURLY",
                    "_links": {
                        "self": {
                            "href": "https:\/\/loc.api.parclick.com\/v1\/pass\/51580"
                        }
                    }
                }
            ],
            "address": "Calle Jardines, 16",
            "city": "Madrid",
            "country": "Espa\u00f1a",
            "province": "Madrid",
            "zip": "28013",
            "provider": {
                "id": 422,
                "name": "MOVEL CASTRO"
            },
            "handicapped_access": false,
            "security": true,
            "open_24h": true,
            "washing": true,
            "giving_keys": true,
            "name": "Jardines 16 - Centro Madrid",
            "max_height": 295,
            "vat_tax": 21,
            "subscriptions": [],
            "access": [
                {
                    "id": 2444,
                    "type": "vehicle",
                    "latitude": 40.418888333632,
                    "longitude": -3.7010293058413
                }
            ],
            "slug": "jardines-16-centro-madrid",
            "multiparking": false,
            "passes_by_websocket": false,
            "_links": {
                "self": {
                    "href": "https:\/\/loc.api.parclick.com\/v1\/parking\/1952"
                }
            },
            "_embedded": {
                "city": {
                    "id": 37,
                    "name": "Madrid",
                    "slug": "madrid",
                    "_links": {
                        "self": {
                            "href": "https:\/\/loc.api.parclick.com\/v1\/city\/37"
                        }
                    }
                }
            }
        }
    ],
    "total": 2,
    "params": {
        "locale": "fr_FR",
        "latitude": "40.41677",
        "group": "search",
        "longitude": "-3.7037902",
        "radius": "200",
        "from": "2021-01-29 10:00",
        "to": "2021-01-29 12:00",
        "vehicleType": "1",
        "page": "1",
        "limit": "2",
        "freemium": false
    }
}
</pre>
</p>
</details>

### _Response:_

**200 Ok group export**

<details><summary style="color:#FF6600;">Show response 200 Ok group export</summary>
<p>
<pre>
{
    "page": "1",
    "limit": "2",
    "pages": 765,
    "items": [
        {
            "id": 4,
            "covered": true,
            "description": "Car park located in the district of Sant Gervasi-Galvany, near to Avenida Diagonal and a 10-minute walk from Barcelona Clinical and Provincial Hospital.\r\n\r\n<p>IMPORTANT: Remember when you book to give us the number of the mobile phone which you'll be using while visiting Barcelona. </p>\r\n<p>TELEPHONIC access to car park. REMOTE customer service. </p>",
            "flexible_entry": true,
            "guarded": true,
            "latitude": 41.395107706311,
            "image_list": "//static.parclick.com/parking/2017/11/cca/e73/a1/ccae73a1-ae1d-563c-b275-14577efbdf1a.jpeg",
            "longitude": 2.1466153666872,
            "image": {
                "id": 591
            },
            "is_cancellable": true,
            "extra_info": [],
            "address": "Carrer de Santaló, 12",
            "country": "España",
            "province": "Barcelona",
            "zip": "8021",
            "handicapped_access": true,
            "security": true,
            "open_24h": true,
            "giving_keys": false,
            "exclude_checking": true,
            "license_info": false,
            "vehicle_info": false,
            "flight": false,
            "return_flight_location": false,
            "train": false,
            "name": "NN Santaló",
            "access_phone": "00 34 647 07 97 31",
            "external": [],
            "max_height": 180,
            "passes": [
                {
                    "id": 520,
                    "type": "pass",
                    "internal_name": "MULTIPASS 1d (20.00€)",
                    "duration": 24,
                    "multiparking": false,
                    "multipass": true,
                    "frequency": "HOURLY",
                    "_links": {
                        "self": [
                            {
                                "href": "https://Ariel.api.parclick.com/v1/product/520"
                            },
                            {
                                "href": "https://Ariel.api.parclick.com/v1/pass/520"
                            }
                        ]
                    }
                },
                {
                    "id": 521,
                    "type": "pass",
                    "internal_name": "MULTIPASS 3d (40.00€)",
                    "duration": 72,
                    "multiparking": false,
                    "multipass": true,
                    "frequency": "HOURLY",
                    "_links": {
                        "self": [
                            {
                                "href": "https://Ariel.api.parclick.com/v1/product/521"
                            },
                            {
                                "href": "https://Ariel.api.parclick.com/v1/pass/521"
                            }
                        ]
                    }
                },
                {
                    "id": 44140,
                    "type": "pass",
                    "internal_name": "ONEPASS 10h (12.00€)",
                    "duration": 10,
                    "multiparking": false,
                    "multipass": false,
                    "frequency": "HOURLY",
                    "_links": {
                        "self": [
                            {
                                "href": "https://Ariel.api.parclick.com/v1/product/44140"
                            },
                            {
                                "href": "https://Ariel.api.parclick.com/v1/pass/44140"
                            }
                        ]
                    }
                },
                {
                    "id": 76460,
                    "type": "pass",
                    "internal_name": "MULTIPASS 90d (525.00€)",
                    "duration": 2160,
                    "multiparking": false,
                    "multipass": true,
                    "frequency": "HOURLY",
                    "_links": {
                        "self": [
                            {
                                "href": "https://Ariel.api.parclick.com/v1/product/76460"
                            },
                            {
                                "href": "https://Ariel.api.parclick.com/v1/pass/76460"
                            }
                        ]
                    }
                }
            ],
            "access": [
                {
                    "id": 938
                }
            ],
            "slug": "nn_santalo",
            "voucher_needed": false,
            "freemium": false,
            "multiparking": false,
            "instructions": "",
            "cancellation_type": 2,
            "_links": {
                "self": {
                    "href": "https://Ariel.api.parclick.com/v1/parking/4"
                }
            },
            "_embedded": {
                "city": {
                    "id": 6,
                    "name": "Barcelona",
                    "slug": "barcelona",
                    "latitude": 41.3851,
                    "longitude": 2.1734,
                    "time_zone": "Europe/Madrid",
                    "_links": {
                        "self": {
                            "href": "https://Ariel.api.parclick.com/v1/city/6"
                        }
                    }
                }
            }
        },
        {
            "id": 5,
            "covered": true,
            "description": "Car park close to the Hospital Clínic and the Teatre Villarroel. The metro stations Hospital Clínic (line 5) and Urgell (line 1) are both roughly a 10-minute walk away. Easy access to the Gran Vía de las Cortes Catalanas and Avenida Diagonal.",
            "flexible_entry": true,
            "guarded": true,
            "latitude": 41.3860929623107,
            "image_list": "//static.parclick.com/parking/2017/11/4da/76d/b3/4da76db3-f676-5850-843b-c1c1b02c36b6.jpeg",
            "longitude": 2.1536075582364447,
            "image": {
                "id": 1309
            },
            "is_cancellable": true,
            "extra_info": [],
            "address": "Comte d'Urgell, 154",
            "country": "España",
            "province": "Barcelona",
            "zip": "8036",
            "handicapped_access": true,
            "security": true,
            "open_24h": true,
            "giving_keys": false,
            "exclude_checking": false,
            "license_info": false,
            "vehicle_info": false,
            "flight": false,
            "return_flight_location": false,
            "train": false,
            "name": "NN Urgell 2",
            "external": [],
            "max_height": 195,
            "passes": [
                {
                    "id": 512,
                    "type": "pass",
                    "internal_name": "MULTIPASS 1d (18.00€)",
                    "duration": 24,
                    "multiparking": false,
                    "multipass": true,
                    "frequency": "HOURLY",
                    "_links": {
                        "self": [
                            {
                                "href": "https://Ariel.api.parclick.com/v1/product/512"
                            },
                            {
                                "href": "https://Ariel.api.parclick.com/v1/pass/512"
                            }
                        ]
                    }
                },
                {
                    "id": 513,
                    "type": "pass",
                    "internal_name": "MULTIPASS 3d (40.00€)",
                    "duration": 72,
                    "multiparking": false,
                    "multipass": true,
                    "frequency": "HOURLY",
                    "_links": {
                        "self": [
                            {
                                "href": "https://Ariel.api.parclick.com/v1/product/513"
                            },
                            {
                                "href": "https://Ariel.api.parclick.com/v1/pass/513"
                            }
                        ]
                    }
                }
            ],
            "access": [
                {
                    "id": 343
                }
            ],
            "slug": "nn_urgell_2",
            "voucher_needed": false,
            "freemium": false,
            "multiparking": false,
            "instructions": "",
            "cancellation_type": 2,
            "_links": {
                "self": {
                    "href": "https://Ariel.api.parclick.com/v1/parking/5"
                }
            },
            "_embedded": {
                "city": {
                    "id": 6,
                    "name": "Barcelona",
                    "slug": "barcelona",
                    "latitude": 41.3851,
                    "longitude": 2.1734,
                    "time_zone": "Europe/Madrid",
                    "_links": {
                        "self": {
                            "href": "https://Ariel.api.parclick.com/v1/city/6"
                        }
                    }
                }
            }
        }
    ],
    "total": 1529,
    "params": {
        "locale": "en_GB",
        "latitude": "40.4167754",
        "group": "export",
        "longitude": "-3.7037901999999576",
        "radius": "4",
        "from": "2020-01-29 10:00",
        "to": "2020-01-29 14:00",
        "vehicleType": "1",
        "page": "1",
        "limit": "2"
    }
}
</pre>
</p>
</details>

<br>

**400 Bad request - when parameters are missing or incorrect**

| Parameters | Type | Description |
| --------------- |:--------------- | ------------------------------ |
| code | string | http response code value |
| message | string | error description |
| errors | array | fields and the specific error |

<details><summary style="color:#FF6600;">Show response 400 Bad request</summary>
<p>
<pre>
{
  "code": 400,
  "message": "Validation Failed",
  "errors": {
    "children": {
      "locale": {},
      "group": {},
      "page": {},
      "limit": {},
      "sort": {},
      "direction": {},
      "from": {
        "errors": [
          "Date 2017-12-31 20:00 must be greater than now."
        ]
      },
      "to": {
        "errors": [
          "Date 2017-12-31 20:00 must be greater than now."
        ]
      },
      "latitude": {},
      "longitude": {},
      "radius": {},
      "vehicleType": {},
      "airport": {},
      "city": {},
      "country": {},
      "freemium": {},
      "provider": {},
      "subscriptionType": {},
      "name": {}
    }
  }
}
</pre>
</p>
</details>

<br>

## <a name="get_parking"></a><span style="color:#FF6600;">Get parking</span>

### <span style="color:#0f6ab4;">`GET`</span> `/v1/parking/{parking_id}/details`

This method returns parking information, products available and required fields in the **fieldsRequested** array key to make a reservation. Also gives information about the cancelation policy in the field **cancellation_type** with this options:

| key | value |
|-----|----------------------|
| 1 | Cancel before 23:59 |
| 2 | Cancel one hour left |
| 3 | Cancel disallowed |

The fieldsRequested node gives the additional fields required for a specific car park

| Field | Description | Type |
|------------------------|-------------------------------------------------------|---------|
| brand | Vehicle brand | string |
| model | Vehicle model | string |
| license_plate | Vehicle license plate | string |
| arrival_flight | Return flight number | string |
| departure_flight | One way flight number | string |
| return_flight_location | City from where you return | string |
| arrival_train | Return train number | string |
| departure_train | Departure train number | string |
| people_travelling | Number of passengers for the shuttle service | integer |
| cruise_ferry | Cruise or ferry flight number | string |
| cruise_ferry_name | Cruise Name | string |
| passenger_email | Passenger email | string |
| passenger_phone_number | Passenger Phone Number | string |
| name_hotel | Hotel name | string |
| color_vehicle | Vehicle color | string |
| departure_terminal | Departure terminal | string |


### _Header:_
Authorization: Bearer {JWT_TOKEN}

### _Request:_

| Parameters | Type | Required | Description | Default |
| --------------- |:--------------------- | --------- |--------------------------------------------------- |--------------- |
| group | string | false | property groups to be returned **detail** | null |
| locale | string | false | language in which the information will be returned | en_GB | 

### _Response:_

**200 Ok group export-joinup**

<details><summary style="color:#FF6600;">Show response 200 Ok group export-joinup</summary>
<p>
<pre>
{
    "id": 983,
    "covered": true,
    "description": "Car park in the center of Barcelona, in the Carrer de l'Hospital, just 500 meters from the famous Paseo de Las Ramblas and the Liceo Theater, known for its opera in Barcelona. Parking in El Nou Raval car park lets you visit this restaurant and leisure area with its numerous shops, restaurants, bars and street art. At the same distance from El Nou Raval car park, you’ll find the Rubió i Lluch Gardens, the Monastery of Sant Pau del Camp, and a little farther, you can visit the art district of Barcelona: the Museum of Contemporary Art (MACBA), the Center of Contemporary Culture of Barcelona (CCCB) and the Filmoteca de Catalunya. Parking in the heart of Barcelona is no longer a problem, for the Catalonia Square is located just 10 minutes walking from El Nou Raval car park, where you can enjoy the Francesc Macià Monument. Likewise, in just 10 minutes, you can visit one of the most famous Barcelona sites, the Güell Palace, or the Main Theater. El Nou Raval car park is also very close to the Rambla del Raval, the La Boquería market, the Sant Antoni market and the Basilica of Santa María del Pi. Also, El Nou Raval car park is located less than 15 minutes walking from the Plaça de Sant Jaume, where you’ll find the Barcelona City Hall, Palau de la Generalitat de Catalunya, Basilica of Sants Màrtirs Just i Pastor and the Roman Wall; likewise, right next door, you can visit the History Museum of Barcelona (MUHBA), the Mirador del Rei Martí, the Frederic Marès Museum and the Catalan Museum. El Nou Raval car park is ideal if you need to park in the Eixample Sant Antoni or the historic center of Barcelona, especially in the El Raval or the Gothic Quarter, as well as the University Plaza, where you’ll find the University of Barcelona’s Faculty of Mathematics, among others. El Nou Raval car park gives you the chance to leave your car in good hands anytime, thanks to their 24/7 schedule and video surveillance system. You can also get around Barcelona on public transit using line L2 from the Sant Antoni station, just 5 minutes from the car park, or L3 from the Liceu station.",
    "flexible_entry": false,
    "latitude": 41.37944030469,
    "longitude": 2.1672597228639,
    "medias": [
        {
            "id": 7571,
            "url": "https://static.parclick.com/parking/2017/07/85a/485/c4/85a485c4-d29d-5c3c-b9c3-ffc41de8b75f.jpeg"
        },
        {
            "id": 7572,
            "url": "https://static.parclick.com/parking/2017/07/62d/e02/10/62de0210-b385-5084-88b5-0ddf50d03781.jpeg"
        },
        {
            "id": 7573,
            "url": "https://static.parclick.com/parking/2017/07/6be/1b2/e5/6be1b2e5-f4f1-55da-8ecc-d299fa1c1f47.jpeg"
        },
        {
            "id": 7574,
            "url": "https://static.parclick.com/parking/2017/07/a8b/790/89/a8b79089-358b-59cc-8442-39f02ba424be.jpeg"
        }
    ],
    "address": "Carrer de l'Hospital, 141",
    "city": "Barcelona",
    "country": "España",
    "province": "Barcelona",
    "zip": "8001",
    "provider": {
        "id": 254,
        "name": "EL NOU RAVAL"
    },
    "handicapped_access": true,
    "security": true,
    "open_24h": true,
    "giving_keys": false,
    "exclude_checking": false,
    "name": "El Nou Raval",
    "max_height": 190,
    "schedule": [],
    "access": [
        {
            "id": 924,
            "type": "vehicle",
            "address": "Carrer de la Cera 1",
            "latitude": 41.37944030469,
            "longitude": 2.1672597228639
        }
    ],
    "voucher_needed": false,
    "multiparking": false,
    "cancellation_type": 2,
    "_links": {
        "self": {
            "href": "https://pre.api.parclick.com/v1/parking/983"
        }
    },
    "_embedded": {
        "city": {
            "id": 6,
            "name": "Barcelona",
            "country": {
                "id": 199,
                "iso": "ES",
                "nicename": "Spain",
                "_links": {
                    "self": {
                        "href": "https://pre.api.parclick.com/v1/country/199"
                    }
                }
            },
            "vat_tax": 21,
            "time_zone": "Europe/Madrid",
            "_links": {
                "self": {
                    "href": "https://pre.api.parclick.com/v1/city/6"
                }
            }
        },
        "fieldsRequested": []
    }
}
</pre>
</p>
</details>




### _Response:_

**200 Ok**

<details><summary style="color:#FF6600;">Show response 200 Ok group detail</summary>
<p>
<pre>
{
   "id":3180,
   "inMadridCentral":false,
   "covered":true,
   "description":"Vous avez un vol au d\u00e9part de l\u2019a\u00e9roport de Toulouse Blagnac et vous \u00eates \u00e0 la recherche d\u2019un parking pour y laisser votre v\u00e9hicule ? D\u2019un parking avec des prix raisonnables ? D\u2019un parking \u00e0 5 minutes de l\u2019a\u00e9roport ? Ce n\u2019est plus la peine de chercher ! Nous avons ce qu\u2019il vous faut ! Le parking Boxx\u0027in A\u00e9roport Toulouse - self parking est un parking s\u00e9curis\u00e9, ouvert tous les jours de 5h00 \u00e0 23h30. Alors ne perdez plus de temps et r\u00e9servez d\u00e8s maintenant votre place de stationnement dans le parking Boxx\u0027in A\u00e9roport Toulouse - self parking pour vous garer en toute simplicit\u00e9 avant de prendre votre avion gr\u00e2ce \u00e0 Parkingsdeparis ! :)\r\n\r\nDepuis le parking Boxx\u0027in A\u00e9roport Toulouse - self parking, une navette est mise \u00e0 votre disposition pour vous rendre au terminal de l\u0027a\u00e9roport de Toulouse-Blagnac. Celle-ci ne prendra que 3 minutes pour vous amener \u00e0 destination.\r\nLe parking Boxx\u0027in A\u00e9roport Toulouse - self parking vous offre \u00e9galement d\u2019autres services. Votre voiture pourra \u00eatre lav\u00e9e et le plein pourra \u00eatre fait pour vous !\r\n\r\nVotre vol part t\u00f4t dans la matin\u00e9e ? Choisissez d\u2019arriver la veille, laissez votre v\u00e9hicule dans le parking Boxx\u0027in A\u00e9roport Toulouse - self parking, et passez la nuit dans l\u2019un des h\u00f4tels suivants : l\u2019h\u00f4tel Kyriad Toulouse A\u00e9roport (4 rue Raymond Grimaud), le Best Western H\u00f4tel, l\u2019ibis Styles Toulouse-Blagnac A\u00e9roport (45 rue de Bordebasse), l\u2019h\u00f4tel Premi\u00e8re Classe Toulouse Ouest (2 rue Ren\u00e9 Rey) ou bien l\u2019Holiday Inn Toulouse Airport (Place de la r\u00e9volution).\r\n\r\nUne fois votre v\u00e9hicule gar\u00e9 dans le parking Boxx\u0027in A\u00e9roport Toulouse - self parking, vous souhaitez d\u00e9jeuner avant de rejoindre l\u2019a\u00e9roport ? Aucun probl\u00e8me, le quartier du parking Boxx\u0027in A\u00e9roport Toulouse - self parking regorge de restaurant en tout genre. Vous pourrez par exemple vous rendre \u00e0 La Tosca (16 rue Raymond Grimaud), \u00e0 L\u0027Esprit du Sud-Ouest (84 rue de Bordebasse), au restaurant vietnamien Kumquat (18 rue Raymond Grimaud), au restaurant Sushi tokoro Blagnac (3 rue Raymond Grimaud) ou encore au restaurant l\u0027entr\u0027acte (10 place de la R\u00e9volution Fran\u00e7aise).\r\n\r\nUne envie de faire du shopping de derni\u00e8re minute ? Le centre commercial Blagnac se trouve \u00e0 moins de 10 minutes de marche du parking Boxx\u0027in A\u00e9roport Toulouse - self parking. Vous y trouverez des magasins tels que Kiabi, Decathlon Toulouse Blagnac, H\u0026M, Bershka, Bocage Blagnac, Desigual, Brice, Jules et bien d\u2019autres encore \u2026 Et oui, le parking Boxx\u0027in A\u00e9roport Toulouse - self parking vous sera aussi tr\u00e8s pratique pour faire quelques achats !\r\n\r\nNe vous prenez plus jamais la t\u00eate pour garer votre voiture pr\u00e8s de l\u0027a\u00e9roport de Toulouse Blagnac gr\u00e2ce au parking Boxx\u0027in A\u00e9roport Toulouse - self parking ! R\u00e9servez votre place de stationnement \u00e0 l\u2019avance et en ligne dans le parking Boxx\u0027in A\u00e9roport Toulouse - self parking avec Parkingsdeparis ! Bon voyage ! :)",
   "flexible_entry":false,
   "guarded":true,
   "latitude":43.63178114954,
   "image_list":"\/\/static.parclick.com\/parking\/2020\/08\/61c\/a56\/75\/61ca5675-bdca-539c-a97d-4f3083497c4b.jpeg",
   "longitude":1.3795863202377,
   "medias":[
      {
         "id":14789,
         "url":"https:\/\/static.parclick.com\/parking\/2020\/08\/0f2\/fa1\/1e\/0f2fa11e-af31-5f34-8621-c736520e82a5.jpeg"
      },
      {
         "id":14790,
         "url":"https:\/\/static.parclick.com\/parking\/2020\/08\/c10\/421\/97\/c1042197-5ad6-5812-bac3-2bf0ad390ca3.jpeg"
      },
      {
         "id":14791,
         "url":"https:\/\/static.parclick.com\/parking\/2020\/08\/bba\/25d\/77\/bba25d77-b0ae-58cf-9b96-13a21fa12c28.jpeg"
      }
   ],
   "extra_info":{
      "currency":"euro",
      "code":"EUR",
      "minor_unit":2,
      "symbol":"\u20ac"
   },
   "parkingCategoryName":"Airport Official",
   "passes":[
      {
         "id":115401,
         "category":"ONEPASS",
         "instructions":"",
         "name":"ONEPASS 1 journ\u00e9e",
         "vehicle_type":{
            "id":2,
            "type":"VAN"
         },
         "warning_message":"",
         "administration_fee":0,
         "paypal_fee":1.2,
         "price":14.9,
         "status":"ACTIVE",
         "type":"pass",
         "internal_name":"ONEPASS 1d (14.90\u20ac)",
         "token":"",
         "days_of_week":"",
         "duration":24,
         "minutes_before_sell":1400,
         "multiparking":false,
         "multipass":false,
         "frequency":"HOURLY",
         "_links":{
            "self":{
               "href":"https:\/\/loc.api.parclick.com\/v1\/pass\/115401"
            }
         }
      },
      {
         "id":115631,
         "category":"ONEPASS",
         "instructions":"\u003Cp\u003EPendant le processus d\u0027achat, veuillez indiquer le jour o\u00f9 vous comptez arriver. Apr\u00e8s le paiement, vous recevrez par email un bon d\u0027\u00e9change contenant votre code de r\u00e9servation. Le parking vous enverra un SMS afin de vous confirmer votre s\u00e9jour. Le jour choisi, rendez-vous tout simplement au parking. Une fois gar\u00e9, rendez-vous au guichet avec votre bon d\u0027\u00e9change Parclick et votre ticket. Notre personnel v\u00e9rifiera votre r\u00e9servation \u00e0 l\u0027aide du code de r\u00e9servation. De l\u00e0, nous vous conduirons \u00e0 l\u0027a\u00e9roport de Toulouse-Blagnac dans notre v\u00e9hicule.\u003C\/p\u003E",
         "name":"ONEPASS 60 jours",
         "vehicle_type":{
            "id":1,
            "type":"CAR"
         },
         "warning_message":"Merci de noter que vous devrez laisser les cl\u00e9s de votre voiture dans le parking.",
         "administration_fee":0,
         "paypal_fee":11.4,
         "price":380,
         "status":"ACTIVE",
         "type":"pass",
         "internal_name":"ONEPASS 60d (380.00\u20ac)",
         "token":"",
         "days_of_week":"",
         "duration":1440,
         "minutes_before_sell":1400,
         "multiparking":false,
         "multipass":false,
         "frequency":"HOURLY",
         "_links":{
            "self":{
               "href":"https:\/\/loc.api.parclick.com\/v1\/pass\/115631"
            }
         }
      },
      {
         "id":115632,
         "category":"ONEPASS",
         "instructions":"\u003Cp\u003EPendant le processus d\u0027achat, veuillez indiquer le jour o\u00f9 vous comptez arriver. Apr\u00e8s le paiement, vous recevrez par email un bon d\u0027\u00e9change contenant votre code de r\u00e9servation. Le parking vous enverra un SMS afin de vous confirmer votre s\u00e9jour. Le jour choisi, rendez-vous tout simplement au parking. Une fois gar\u00e9, rendez-vous au guichet avec votre bon d\u0027\u00e9change Parclick et votre ticket. Notre personnel v\u00e9rifiera votre r\u00e9servation \u00e0 l\u0027aide du code de r\u00e9servation. De l\u00e0, nous vous conduirons \u00e0 l\u0027a\u00e9roport de Toulouse-Blagnac dans notre v\u00e9hicule.\u003C\/p\u003E",
         "name":"ONEPASS 90 jours",
         "vehicle_type":{
            "id":1,
            "type":"CAR"
         },
         "warning_message":"Merci de noter que vous devrez laisser les cl\u00e9s de votre voiture dans le parking.",
         "administration_fee":0,
         "paypal_fee":17.1,
         "price":570,
         "status":"ACTIVE",
         "type":"pass",
         "internal_name":"ONEPASS 90d (570.00\u20ac)",
         "token":"",
         "days_of_week":"",
         "duration":2160,
         "minutes_before_sell":1400,
         "multiparking":false,
         "multipass":false,
         "frequency":"HOURLY",
         "_links":{
            "self":{
               "href":"https:\/\/loc.api.parclick.com\/v1\/pass\/115632"
            }
         }
      }
   ],
   "address":"Rue Dieudonn\u00e9 Costes, 6",
   "city":"Blagnac",
   "country":"France",
   "province":"Haute-Garonne",
   "zip":"31700",
   "provider":{
      "id":224,
      "name":"BOXX\u2019IN"
   },
   "feature_electric_cars":true,
   "handicapped_access":true,
   "security":true,
   "open_24h":true,
   "giving_keys":true,
   "exclude_checking":false,
   "license_info":true,
   "vehicle_info":true,
   "flight":false,
   "return_flight_location":false,
   "train":false,
   "stuff_required_at_check_in":false,
   "name":"Boxx\u0027in A\u00e9roport Toulouse - self parking",
   "external":[
      
   ],
   "max_height":190,
   "vat_tax":20,
   "subscriptions":[
      
   ],
   "schedule":[
      {
         "id":11138,
         "type":{
            "id":1,
            "name":"PERSONAL"
         },
         "week_day":{
            "key":"MONDAY",
            "order":1
         },
         "intervals":[
            {
               "id":16303,
               "hour_from":"1970-01-01T05:00:00+0000",
               "hour_to":"1970-01-01T23:30:00+0000"
            }
         ]
      },
      {
         "id":11140,
         "type":{
            "id":1,
            "name":"PERSONAL"
         },
         "week_day":{
            "key":"TUESDAY",
            "order":2
         },
         "intervals":[
            {
               "id":16304,
               "hour_from":"1970-01-01T05:00:00+0000",
               "hour_to":"1970-01-01T23:30:00+0000"
            }
         ]
      },
      {
         "id":11141,
         "type":{
            "id":1,
            "name":"PERSONAL"
         },
         "week_day":{
            "key":"WEDNESDAY",
            "order":3
         },
         "intervals":[
            {
               "id":16305,
               "hour_from":"1970-01-01T05:00:00+0000",
               "hour_to":"1970-01-01T23:30:00+0000"
            }
         ]
      },
      {
         "id":11142,
         "type":{
            "id":1,
            "name":"PERSONAL"
         },
         "week_day":{
            "key":"THURSDAY",
            "order":4
         },
         "intervals":[
            {
               "id":16306,
               "hour_from":"1970-01-01T05:00:00+0000",
               "hour_to":"1970-01-01T23:30:00+0000"
            }
         ]
      },
      {
         "id":11143,
         "type":{
            "id":1,
            "name":"PERSONAL"
         },
         "week_day":{
            "key":"FRIDAY",
            "order":5
         },
         "intervals":[
            {
               "id":16307,
               "hour_from":"1970-01-01T05:00:00+0000",
               "hour_to":"1970-01-01T23:30:00+0000"
            }
         ]
      },
      {
         "id":11144,
         "type":{
            "id":1,
            "name":"PERSONAL"
         },
         "week_day":{
            "key":"SATURDAY",
            "order":6
         },
         "intervals":[
            {
               "id":16308,
               "hour_from":"1970-01-01T05:00:00+0000",
               "hour_to":"1970-01-01T23:30:00+0000"
            }
         ]
      },
      {
         "id":11145,
         "type":{
            "id":1,
            "name":"PERSONAL"
         },
         "week_day":{
            "key":"SUNDAY",
            "order":7
         },
         "intervals":[
            {
               "id":16309,
               "hour_from":"1970-01-01T05:00:00+0000",
               "hour_to":"1970-01-01T23:30:00+0000"
            }
         ]
      }
   ],
   "access":[
      {
         "id":3975,
         "type":"vehicle",
         "address":"Rue Dieudonn\u00e9 Costes 6",
         "latitude":43.63178114954,
         "longitude":1.3795863202377
      }
   ],
   "slug":"boxxin-aeroport-toulouse-self-parking-1",
   "voucher_needed":false,
   "freemium":false,
   "minutes_before_sell":0,
   "multiparking":false,
   "instructions":"",
   "cancellation_type":1,
   "_links":{
      "self":{
         "href":"https:\/\/loc.api.parclick.com\/v1\/parking\/3180"
      }
   },
   "_embedded":{
      "city":{
         "id":145,
         "name":"Toulouse",
         "slug":"toulouse",
         "extra_info":[
            
         ],
         "country_name":"FRANCE",
         "city_name":"Toulouse",
         "latitude":43.6,
         "longitude":1.4505,
         "vat_tax":20,
         "content":"\u003Cp\u003EParclick, c\u0027est une nouvelle fa\u00e7on de stationner au meilleur prix. Dans \u003Cstrong\u003Echacune des %cityNumber% villes de Parclick\u003C\/strong\u003E, vous pourrez r\u00e9server une place de stationnement avant d\u0027entreprendre votre voyage, dans le parking qui r\u00e9pondra le mieux \u00e0 vos besoins ; et ce, pr\u00e8s du centre-ville mais aussi des principaux points d\u0027int\u00e9r\u00eat de la ville, ou encore \u00e0 proximit\u00e9 des gares et a\u00e9roports. Vous n\u0027aurez plus \u00e0 vous soucier pour l\u0027endroit o\u00f9 laisser votre voiture au cours de vos voyages : Parclick a la solution ! De cette fa\u00e7on, vous profiterez de votre visite en toute s\u00e9r\u00e9nit\u00e9 !\u003C\/p\u003E\r\n\r\n\u003Cp\u003E\u0026nbsp;\u003C\/p\u003E\r\n\r\n\u003Cp\u003ESi vous avez planifi\u00e9 votre \u003Cstrong\u003Evoyage \u00e0 %city%\u003C\/strong\u003E et que vous vous demandez o\u00f9 vous pourrez bien laisser votre voiture \u00e0 votre arriv\u00e9e, Parclick peut vous aider ! Trouvez un parking proche de votre destination et au meilleur prix, avec tous les services dont vous aurez besoin. Vous pourrez visiter %city% sans tracas, en sachant votre v\u00e9hicule entre de bonnes mains. Indiquez l\u0027adresse de votre h\u00f4tel ou autre point de chute \u00e0 %city% o\u00f9 vous souhaitez vous garer, et Parclick vous montrera toutes les options disponibles.\u003C\/p\u003E\r\n\r\n\u003Cp\u003E\u0026nbsp;\u003C\/p\u003E\r\n\r\n\u003Cp\u003EChez Parclick, nous disposons de \u003Cstrong\u003E%parkingNumber% parkings \u00e0 %city%\u003C\/strong\u003E, que vous pourrez r\u00e9server pour vos s\u00e9jours de courte ou longue dur\u00e9e, au meilleur prix. Comparez nos offres et choisissez le parking qui s\u0027adapte le mieux \u00e0 vos besoins. N\u0027h\u00e9sitez plus, r\u00e9servez votre place de stationnement avec Parclick et profitez de \u003Cstrong\u003Evotre s\u00e9jour \u00e0 %city%\u003C\/strong\u003E avec votre voiture, sans contretemps.\u003C\/p\u003E\r\n\r\n\u003Cp\u003E\u0026nbsp;\u003C\/p\u003E",
         "time_zone":"Europe\/Madrid",
         "_links":{
            "self":{
               "href":"https:\/\/loc.api.parclick.com\/v1\/city\/145"
            }
         }
      },
      "fieldsRequested":[
         
      ],
      "features":[
         
      ]
   }
}
</pre>
</p>
</details>

<br>

**401 Bad request - when parameters are missing or incorrect**

<details><summary style="color:#FF6600;">Show response 401 Bad request</summary>
<p>
<pre>
{
  "code": 400,
  "message": "Validation Failed",
  "errors": {
    "children": {
      "locale": {},
      "group": {
        "errors": [
          "Invalid value invalid_group for group, group must be one of: bestpass, booking, box, card, company, coupon, detail, export, favourite, list, multiparking, parking, search, subscription, user"
        ]
      }
    }
  }
}
</pre>
</p>
</details>

<br>

## <a name="bestpass"></a><span style="color:#FF6600;">Get Bestpass</span>

### <span style="color:#0f6ab4;">`GET`</span> `/v1/pass/details`

This method returns the best product available on selected car park based on the stay provided. The token provided is mandatory to make a reservation.

### _Header:_
Authorization: Bearer {JWT_TOKEN}

### _Request:_

| Parameters | Type | Required | Description | Default |
| --------------- |:--------------------- | :------- |--------------------------------------------------- |--------------- |
| group | string | true | property groups to be returned **bestpass** |null |
| locale | string | true | language in which the information will be returned |en_GB | 
| parking | integer | true | parking identificator |null |
| vehicleType | integer | true | type of vehicle you wish to park [1 car, 2 van, 3 caravan, 4 bus, 5 truck, 6 motorbike, 7 small truck] |1 |
| from | string Y-m-d H:i | true | booking start date |null |
| to | string Y-m-d H:i | true | booking end date |null |


### _Response:_

**200 Ok**

<details><summary style="color:#FF6600;">Show response 200 Ok</summary>
<p>
<pre>
[
    {
        "id": 49166,
        "category": "ONEPASS",
        "name": "ONEPASS 4 hours",
        "parking": {
            "id": 852,
            "covered": true,
            "flexible_entry": true,
            "guarded": true,
            "is_cancellable": true,
            "countryCodeISO3166-1": "ES",
            "address": "Calle Relatores, 11",
            "city": "Madrid",
            "name": "Garaje Centro",
            "_links": {
                "self": {
                    "href": "https://pre.api.parclick.com/v1/parking/852"
                }
            },
            "_embedded": {
                "city": {
                    "id": 37,
                    "_links": {
                        "self": {
                            "href": "https://pre.api.parclick.com/v1/city/37"
                        }
                    }
                }
            }
        },
        "vehicle_type": {
            "id": 1,
            "type": "CAR"
        },
        "warning_message": "To avoid fines, we recommend accessing the car park from the Atocha / Conde Romanones streets, also Calle Segovia or Toledo / Colegiata.<br/><br/><br/><br/>At this car park, you must leave your car keys with the staff.",
        "administration_fee": 1.79,
        "paypal_fee": 1.21,
        "price": 12.3,
        "type": "pass",
        "internal_name": "ONEPASS 4h (12.30€)",
        "token": "2f40e7630c5db41ffc8255bdf236bfdd4c0e2ef1bfb1dfed304c3c1d7c12a95b",
        "extra_info": {
            "currency": "euro",
            "code": "EUR",
            "minor_unit": 2,
            "symbol": "€"
        },
        "duration": 4,
        "multiparking": false,
        "multipass": false,
        "frequency": "HOURLY",
        "_links": {
            "self": {
                "href": "https://pre.api.parclick.com/v1/pass/49166"
            }
        }
    }
]
</pre>
</p>
</details>

<br>

**200 Ok - No results**

<details><summary style="color:#FF6600;">Show response 200 No results</summary>
<p>
<pre>
[]
</pre>
</p>
</details>

<br>

**401 Bad request - when parameters are missing or incorrect**

<details><summary style="color:#FF6600;">Show response 401 Bad request</summary>
<p>
<pre>
{
  "code": 400,
  "message": "Validation Failed",
  "errors": {
    "errors": [
      "This form should not contain extra fields."
    ],
    "children": {
      "locale": {},
      "group": {},
      "page": {},
      "limit": {},
      "sort": {},
      "direction": {},
      "from": {},
      "to": {},
      "latitude": {},
      "longitude": {},
      "radius": {},
      "vehicleType": {},
      "status": {},
      "parking": {},
      "type": {},
      "frequency": {}
    }
  }
}
</pre>
</p>
</details>

<br>

## <a name="vehicle_type"></a><span style="color:#FF6600;">Vehicle type</span>

### <span style="color:#0f6ab4;">`GET`</span> `/v1/vehicle-type`

This method returns the vehicle type available. The vehicle id is required to make a reservation.

### _Request:_

| Parameters | Type | Required | Description | Default |
| --------------- |:--------------------- | :------- |--------------------------------------------------- |--------------- |
| group | string | true | property groups to be returned **list** | list |
| locale | string | true | language in which the information will be returned | en_GB |


### _Response:_

**200 Ok**

<details><summary style="color:#FF6600;">Show response 200 Ok</summary>
<p>
<pre>
{
  "page": 1,
  "limit": 10,
  "pages": 1,
  "items": [
    {
      "id": 1,
      "type": "CAR",
      "max_height": 190,
      "max_length": 500
    },
    {
      "id": 2,
      "type": "VAN",
      "max_height": 220,
      "max_length": 650
    },
    {
      "id": 3,
      "type": "CARAVAN",
      "max_height": 360,
      "max_length": 1000
    },
    {
      "id": 4,
      "type": "BUS",
      "max_height": 500,
      "max_length": 1200
    },
    {
      "id": 5,
      "type": "TRUCK",
      "max_height": 360,
      "max_length": 700
    },
    {
      "id": 6,
      "type": "MOTORBIKE",
      "max_length": 220
    },
    {
      "id": 7,
      "type": "TRUCK_S",
      "max_height": 270,
      "max_length": 600
    }
  ],
  "total": 7,
  "params": {
    "group": "list"
  },
  "_links": {
    "self": {
      "href": "/v1/vehicle-type?page=1&limit=10"
    },
    "first": {
      "href": "/v1/vehicle-type?page=1&limit=10"
    },
    "last": {
      "href": "/v1/vehicle-type?page=1&limit=10"
    }
  }
}
</pre>
</p>
</details>

<br>

**401 Bad request - when parameters are missing or incorrect**

<details><summary style="color:#FF6600;">Show response 401 Bad request</summary>
<p>
<pre>
{
  "code": 400,
  "message": "Validation Failed",
  "errors": {
    "children": {
      "locale": {},
      "group": {
        "errors": [
          "Invalid value incorrect_group for group, group must be one of: bestpass, booking, box, card, company, coupon, detail, export, favourite, list, multiparking, parking, search, subscription, user"
        ]
      },
      "page": {},
      "limit": {},
      "sort": {},
      "direction": {},
      "from": {},
      "to": {},
      "latitude": {},
      "longitude": {},
      "radius": {},
      "vehicleType": {}
    }
  }
}
</pre>
</p>
</details>

<br>

## <a name="create_reservation"></a><span style="color:#FF6600;">Create reservation</span>

### <span style="color:#10a54a;">`POST`</span> `/v1/booking/new`

This method creates a new reservation. The successful response shows three parameters **booking_id**, **voucher_id** and **voucher_code** that is used for the following:
The **booking_id** parameter is necessary to cancel the reservation.
The **voucher_id** parameter is necessary to get the voucher information.
The **voucher_code** parameter allows to quickly identify a reservation through the Parclick support department.

### _Header:_
Authorization: Bearer {JWT_TOKEN}

### _Request:_

| Parameters | Type | Required | Description | Default |
| --------------- |:--------------------- | ---------| ------------------------ |----------------------- |
| email | string | true | user email | null |
| token | string | true | checkout token | null |
| product | integer | true | product id | null |
| firstName | string | true | user fisrt name | null |
| lastName | string | true | user last name | null |
| from | string Y-m-d H:i | true | booking start date | null |
| to | string Y-m-d H:i | true | booking end date | null |
| group | string | true | **detail** | null |
| phone | string | true | user phone | null |
| prefix | string | true | country prefix i.e. 34 | null |
| brand | string | false | vehicle brand | null |
| model | string | false | vehicle model | null |
| licence_plate | string | false | vehicle licence plate | null |
| color_vehicle | string | false | vehicle color | null |
| departure_flight| string | false | departure flight voucher | null |
| arrival_flight | string | false | arrival flight voucher | null |
| train_number | string | false | train voucher | null |


### _Response:_

**200 Ok**

| Parameters | Type | Description |
| --------------- |:------------------- |-------------------------------------------- |
| booking_id | integer | booking order id |
| voucher_id | integer | new booking identifier (required to cancel) |
| voucher_code | string | new booking voucher |

<details><summary style="color:#FF6600;">Show response 200 Ok</summary>
<p>
<pre>
{
    "booking_id": 1055164,
    "voucher_id": 935489,
    "voucher_code": "L8K2WYJ",
    "net_price": 15.29,
    "vat_price": 3.21,
    "total_price": 21.19,
    "fees": [
        {
            "id": 757530,
            "net_price": 2.2231404958677685,
            "vat": 0.47,
            "total": 2.6931404958677687,
            "discriminator": "administration_fee"
        }   
    ]
    "currency": 
        {
            "currency": "euro",
            "code": "EUR",
            "minor_unit": 2,
            "symbol": "\u20ac"
        } 
}
</pre>
</p>
</details>

<br>

**401 Unauthorized**

| Parameters | Type | Description |
| --------------- |:--------------- | ---------------------------- |
| code | integer | http response code value |
| message | string | error description |

<details><summary style="color:#FF6600;">Show response 401 Unauthorized</summary>
<p>
<pre>
{
 "code": 401,
 "message": "Bad credentials"
}
</pre>
</p>
</details>

<br>

## <a name="cancel_reservation"></a><span style="color:#FF6600;">Cancel reservation</span>

### <span style="color:#10a54a;">`POST`</span> `/v1/booking/{booking_id}/cancelex`

This method allows cancel a reservation. The **booking_id** is obtained in the _new reservation_ endpoint response. The posible cancellation responses are


| Key | Value |
| ----------------------- | -------------------------------------------------------------------- |
| ERROR_NOT_FOUND | Booking not found |
| ERROR_EXPIRED | Booking expired |
| ERROR_CANCELED_ALLOW | Booking can not be canceled |
| ERROR_EXTERNAL_PROVIDER | Booking can not be canceled by external provider |
| ERROR_REFUND | Booking has been canceled but there is an error refund money to user |
| ERROR_ALREADY_CANCELED | Booking has already been canceled |
| UNDEFINED | Refund has failed for unknown reasons |


### _Header:_
Authorization: Bearer {JWT_TOKEN}

### _Request:_

| Parameters | Type | Required | Default | Description |
| --------------- | --------------------| -------- | ---------| ----------------- |
| booking_id | integer | true | null | voucher id |
| locale | string | false | en_GB | language in which the notification will be returned (email)|


### _Response:_

**200 Ok**

<details><summary style="color:#FF6600;">Show response 200 Ok</summary>
<p>
<pre>
{
 "code": 200,
 "message": "success"
}
</pre>
</p>
</details>

**400 Bad request**

| Parameters | Type | Description |
| --------------- |:---------------:| -----------------------------|
| code | integer | http response code value |
| message | string | error description |

<details><summary style="color:#FF6600;">Show response 401 Unauthorized</summary>
<p>
<pre>
{
 "code": 400,
 "message": "Booking has already been canceled"
}
</pre>
</p>
</details>

<br>

**401 Unauthorized**

| Parameters | Type | Description |
| --------------- |:---------------:| -----------------------------|
| code | integer | http response code value |
| message | string | error description |

<details><summary style="color:#FF6600;">Show response 401 Unauthorized</summary>
<p>
<pre>
{
 "code": 401,
 "message": "Bad credentials"
}
</pre>
</p>
</details>

<br>

**404 Not Found**

| Parameters | Type | Description |
| --------------- |:---------------:| -----------------------------|
| code | integer | http response code value |
| message | string | error description |

<details><summary style="color:#FF6600;">Show response 404 Not Found</summary>
<p>
<pre>
{
 "code": 404,
 "message": "Not Found"
}
</pre>
</p>
</details>

<br>

## <a name="modify_reservation"></a><span style="color:#FF6600;">Modify reservation</span>

### <span style="color:#10a54a;">`POST`</span> `/v1/booking/{booking_id}/edit`

This method allows the modification of a reservation. The **booking_id** is obtained in the _new reservation_ endpoint response. The *product* and *token* parameters are obtained through the endpoint _get bestpass_. It is also necessary to search if the parking requires additional fields through the endpoint _get parking_ (*fieldsRequested*). The process behind, delete the old reservation and create new one. The response shows the new reservation as well as the price difference between both reservations to proceed with the refund or charge.

### _Header:_
Authorization: Bearer {JWT_TOKEN}

### _Request:_

| Parameters | Type | Required | Default | Description |
| --------------- |:--------------------- | -------- | ---------------- | ----------------------- |
| token | string | true | null | checkout token |
| product | string | true | null | user email |
| from | string Y-m-d H:i | true | null | booking start date |
| to | string Y-m-d H:i | true | null | booking end date |
| locale | string | true | null | language in which the information will be returned |


### _Response:_

**200 Ok**

<details><summary style="color:#FF6600;">Show response 200 Ok</summary>
<p>
<pre>
{
 "booking_id": 1055145,
 "voucher_id": 935470,
 "voucher_code": "LWPMG3P",
 "cancelled_booking_price": 17.2,
 "new_booking_price": 17.2,
 "difference": 0
}
</pre>
</p>
</details>

<br>

**401 Unauthorized**

| Parameters | Type | Description |
| --------------- |:---------------:| -----------------------------|
| code | integer | http response code value |
| message | string | error description |

<details><summary style="color:#FF6600;">Show response 401 Bad credentials</summary>
<p>
<pre>
{
 "code": 401,
 "message": "Bad credentials"
}
</pre>
</p>
</details>


<br>

## <a name="list_reservation"></a><span style="color:#FF6600;">List reservations</span>

### <span style="color:#0f6ab4;">`GET`</span> `/v1/booking/list`

List all bookings that match filter criteria. The group **list** gives minimal booking info unlike the **booking** group which offers parking product and fees information.

### _Header:_
Authorization: Bearer {JWT_TOKEN}

### _Request:_

| Parameters | Type | Required | Default | Description |
| --------------- |:--------------------- | -------- | ---------------- | ----------------------- |
| group | string | true | null | **list** or **booking** |
| email | string | false | null | user email |
| voucher_code | string | false | null | voucher code |
| from | string Y-m-d H:i | false | null | booking start date |
| to | string Y-m-d H:i | false | null | booking end date |
| page | integer | false | 1 . | page selectec |
| limit | integer | false | 200 | results to show |


### _Response:_

**200 Ok**

<details><summary style="color:#FF6600;">Show response 200 Ok group list</summary>
<p>
<pre>
{
    "page": "1",
    "limit": "100",
    "pages": 1,
    "items": [
        {
            "id": 974973,
            "createdAt": "2019-05-30T11:53:17+0000",
            "end_booking_date": "2019-05-31T12:00:00+0000",
            "start_booking_date": "2019-05-31T10:00:00+0000",
            "total": 19.47,
            "booking_code": "QR23R45",
            "first_name": "John",
            "last_name": "Doe",
            "username": "jd@parclick.com",
            "voucher_id": 855300,
            "state": "CANCELED",
            "total_net_price": 16.09,
            "total_vat": 3.38
        },
        {
            "id": 974998,
            "createdAt": "2019-05-30T12:09:47+0000",
            "end_booking_date": "2019-05-31T12:00:00+0000",
            "start_booking_date": "2019-05-31T10:00:00+0000",
            "total": 19.47,
            "booking_code": "L7N6EP6",
            "first_name": "John",
            "last_name": "Doe",
            "username": "jd@parclick.com",
            "voucher_id": 855325,
            "state": "CONFIRMED",
            "total_net_price": 16.09,
            "total_vat": 3.38
        }
    ],
    "total": 2,
    "params": {
        "group": "list",
        "page": "1",
        "limit": "100"
    }
}
</pre>
</p>
</details>

<details><summary style="color:#FF6600;">Show response 200 Ok group booking</summary>
<p>
<pre>
{ 
   "page":"1",
   "limit":"100",
   "pages":1,
   "items":[ 
      { 
         "id":974973,
         "createdAt":"2019-05-30T11:53:17+0000",
         "end_booking_date":"2019-05-31T12:00:00+0000",
         "start_booking_date":"2019-05-31T10:00:00+0000",
         "total":19.47,
         "booking_code":"QR23R45",
         "first_name":"John",
         "last_name":"Doe",
         "username":"jd@parclick.com",
         "voucher_id":855300,
         "items":[ 
            { 
               "id":976152,
               "parking":{ 
                  "id":113,
                  "latitude":36.53671717596,
                  "image_list":"\/\/static.parclick.com\/parking\/2016\/06\/parking-188-large.png",
                  "longitude":-6.3022649907516,
                  "is_cancellable":true,
                  "address":"Paseo Santa B\u00e1rbara, S\/N",
                  "city":"C\u00e1diz",
                  "country":"Espa\u00f1a",
                  "zip":"11003",
                  "name":"IC Santa B\u00e1rbara",
                  "freemium":false,
                  "cancellation_type":2,
                  "_links":{ 
                     "self":{ 
                        "href":"https:\/\/loc.api.parclick.com\/v1\/parking\/113"
                     }
                  }
               },
               "product":{ 
                  "id":1586,
                  "instructions":"During the purchasing process, select the date you plan to arrive. After making the online payment you will receive a voucher via email with you reservation code. \r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003EOn the date of your reservation, enter the parking lot as usual, take a ticket at the entrance and park in any empty spot.\r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003EOnce you\u0027re out of the car, approach the control booth with the Parclick voucher and the ticket you took. Our staff there will check your reservation using the reservation code, and will give you a card that will allow you out.",
                  "name":"ONEPASS 1 day",
                  "vehicle_type":{ 
                     "id":1
                  },
                  "warning_message":"\u003Cp\u003EAlthough the car park is open 24 hours, you must redeem the receipt within Customer Service Hours from Monday to Sunday from 7h to 23h.\u003C\/p\u003E",
                  "list_price":18.85,
                  "price":17,
                  "type":"pass",
                  "_links":{ 
                     "self":[ 
                        { 
                           "href":"https:\/\/loc.api.parclick.com\/v1\/product\/1586"
                        },
                        { 
                           "href":"https:\/\/loc.api.parclick.com\/v1\/pass\/1586"
                        }
                     ]
                  }
               }
            }
         ],
         "fees":[ 
            { 
               "id":687791,
               "net_price":2.04,
               "vat":0.43,
               "total":2.47,
               "discriminator":"administration_fee"
            }
         ],
         "state":"CANCELED",
         "total_net_price":16.09,
         "total_vat":3.38
      },
      { 
         "id":974998,
         "createdAt":"2019-05-30T12:09:47+0000",
         "end_booking_date":"2019-05-31T12:00:00+0000",
         "start_booking_date":"2019-05-31T10:00:00+0000",
         "total":19.47,
         "booking_code":"L7N6EP6",
         "first_name":"John",
         "last_name":"Doe",
         "username":"jd@parclick.com",
         "voucher_id":855325,
         "items":[ 
            { 
               "id":976177,
               "parking":{ 
                  "id":113,
                  "latitude":36.53671717596,
                  "image_list":"\/\/static.parclick.com\/parking\/2016\/06\/parking-188-large.png",
                  "longitude":-6.3022649907516,
                  "is_cancellable":true,
                  "address":"Paseo Santa B\u00e1rbara, S\/N",
                  "city":"C\u00e1diz",
                  "country":"Espa\u00f1a",
                  "zip":"11003",
                  "name":"IC Santa B\u00e1rbara",
                  "freemium":false,
                  "cancellation_type":2,
                  "_links":{ 
                     "self":{ 
                        "href":"https:\/\/loc.api.parclick.com\/v1\/parking\/113"
                     }
                  }
               },
               "product":{ 
                  "id":1586,
                  "instructions":"During the purchasing process, select the date you plan to arrive. After making the online payment you will receive a voucher via email with you reservation code. \r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003EOn the date of your reservation, enter the parking lot as usual, take a ticket at the entrance and park in any empty spot.\r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003EOnce you\u0027re out of the car, approach the control booth with the Parclick voucher and the ticket you took. Our staff there will check your reservation using the reservation code, and will give you a card that will allow you out.",
                  "name":"ONEPASS 1 day",
                  "vehicle_type":{ 
                     "id":1
                  },
                  "warning_message":"\u003Cp\u003EAlthough the car park is open 24 hours, you must redeem the receipt within Customer Service Hours from Monday to Sunday from 7h to 23h.\u003C\/p\u003E",
                  "list_price":18.85,
                  "price":17,
                  "type":"pass",
                  "_links":{ 
                     "self":[ 
                        { 
                           "href":"https:\/\/loc.api.parclick.com\/v1\/product\/1586"
                        },
                        { 
                           "href":"https:\/\/loc.api.parclick.com\/v1\/pass\/1586"
                        }
                     ]
                  }
               }
            }
         ],
         "fees":[ 
            { 
               "id":687813,
               "net_price":2.04,
               "vat":0.43,
               "total":2.47,
               "discriminator":"administration_fee"
            }
         ],
         "state":"CONFIRMED",
         "total_net_price":16.09,
         "total_vat":3.38
      }
   ],
   "total":25,
   "params":{ 
      "group":"booking",
      "page":"1",
      "limit":"100"
   }
}
</pre>
</p>
</details>


<br>

**401 Unauthorized**

| Parameters | Type | Description |
| --------------- |:---------------:| -----------------------------|
| code | integer | http response code value |
| message | string | error description |

<details><summary style="color:#FF6600;">Show response 401 Bad credentials</summary>
<p>
<pre>
{
 "code": 401,
 "message": "Bad credentials"
}
</pre>
</p>
</details>

<br>

## <a name="voucher_details"></a><span style="color:#FF6600;">Get voucher details</span>

### <span style="color:#0f6ab4;">`GET`</span> `/v1/voucher/{voucher_id}/details`

This method return booking details. The **voucher_id** is obtained in the _new reservation_ endpoint response.


### _Header:_
Authorization: Bearer {JWT_TOKEN}

### _Request:_

| Parameters | Type | Required | Default | Description |
| --------------- |:--------------------| ---------| ---------| -------------------------------------------------- |
| voucher_id | integer | true | null | voucher id |
| locale | string | false | es_ES | language in which the information will be returned |
| notify | bool | false | false | send voucher by email |
| group | string | true | false | **detail** |


### _Response:_

**200 Ok**

| Parameters | Type | Description |
| ------------------------- | ---------------- | -------------------------------------------- |
| user_first_name | string | user first name |
| user_last_name | string | user last name |
| product | string | product description i.e. "MULTIPASS 2 horas" |
| product_instructions | string | product instructions |
| product_category | string | product category |
| product_warning | string | warning description | 
| product_net_price | float | product net price |
| product_vat_price | float | product vat price |
| product_vat_percent | string | product vat percent |
| product_gross_price | float | vat price |
| voucher_code | string | voucher code |
| voucher_id | int . | voucher id (id used to cancel a reservation) |
| voucher_description | array - string | voucher_description |
| booking_id | int | booking id |
| booking_state | string | booking status (CONFIRMED, CANCELLED, PENDING)|
| booking_is_cancellable | bool | is this booking cancellable |
| booking_cancellation_type | int | [1 cancel before 23:59, 2 cancel one hour left, 3 cancel disallowed] |
| booking_from | string d/m/Y H:i | booking start date |
| booking_to | string d/m/Y H:i | booking end date |
| total_net_price | float | total net price |
| total_vat | float | total vat |
| total | float | total amount |
| vehicle_type | string | type of vehicle you wish to park [1 car, 2 van, 3 caravan, 4 bus, 5 truck, 6 motorbike, 7 small truck] |
| external_code | string | booking external code |
| external_code_type | string | external code type |
| external_code_tech | string | external code tech |
| qr_hash | string | qr hash |
| booking_created_at | string | booking creation date d/m/Y H:i |
| extra_fields | array | all the extra fields filled in booking |
| parking_id | int | parking id |
| parking_name | string | parking name |
| parking_address | string | parking address |
| parking_zip | string | parking zip |
| parking_city | string | parking city |
| parking_province | string | parking province |
| parking_country | string | parking country |
| parking_maximum_height | string | parking maximum height |
| parking_description | string | parking description |
| parking_instructions | array | parking instructions () |
| parking_latitude | float | parking latitude |
| parking_longitude | float | parking longitude |
| parking_category | array | parking category [1 City Center, 2 Airport Official, 3 Train Station Official, 4 Port, 5 Hospital, 6 Coast, 7 Airport LowCost, 8 Airport Valet, 9 Train Station LowCost, 10 Train Station Valet] |
| parking_has_shuttle | bool | has shuttle service |
| parking_shuttle_schedule | array | schuttle schedule |
| parking_shuttle_frequency | string | frequency between shuttles |
| parking_airport_terminals | array | airport terminals |
| spot_name | string | spot name |
| is_box | bool | parking use a box |
| fee | array | array of fees |
| third_party_discriminator | string | third party discriminator |
| currency | string | currency code |
| has_voucher_notification | bool | include voucher in notification |
| has_receipt_notification | bool | include receipt in notification |
| locale | string | selected locale |
| application_name | string | application name - parclick |
| host | string | host - parclick.com |
| logo_image | string | logo image path |
| url_faqs | string | FAQ's path |
| emailt_contact | string | the contact email |
| from | string | email from |
| from_name | string | email from name |
| to | string | email to |
| to_name | string | email to name |
| cco | string | email carbon copy |
| subject | string | email subject | 
| currency | string | currency name ISO 4217 | 
| code | string | email currency code ISO 4217 | 
| minor_unit | int |  relationship between a major currency unit and its corresponding minor currency unit ISO 4217 | 
| symbol | string | currency symbol | 
| country_codeISO3166-1 | string | country code, two digits ISO 3166-1 |


<details><summary style="color:#FF6600;">Show response 200 Ok</summary>
<p>
<pre>
{
    "data": {
        "user": {
            "user_first_name": "Perpe",
            "user_last_name": "Perez",
            "user_phone": "34 666339900"
        },
        "booking": {
            "booking_id": 1280840,
            "booking_state": "CONFIRMED",
            "booking_is_cancellable": true,
            "booking_cancellation_type": 2,
            "booking_from": "2020-06-01 10:00:00",
            "booking_to": "2020-06-01 11:00:00",
            "total_net_price": 1.69,
            "total_vat": 0.35,
            "total": 2.04,
            "vehicle_type": "CAR",
            "booking_created_at": "2020-06-01 07:57:54",
            "extra_fields": []
        },
        "parking": {
            "parking_id": 2622,
            "parking_name": "Hospital Quirón San José",
            "parking_phone": "+34 91 415 85 69",
            "parking_address": "Calle Mataelpino, 5",
            "parking_zip": "28002",
            "parking_city": "Madrid",
            "parking_province": "Madrid",
            "parking_country": "SPAIN",
            "parking_maximum_height": "2.1 m.",
            "parking_description": "Si necesitas aparcar en el Hospital Quirón San José de Madrid, sin duda este parking en Madrid será tu mejor opción. Reservar tu plaza en el parking Hospital Quirón San José te facilitará llegar a tiempo a esa cita a primera hora de la mañana sin tener que madrugar demasiado. Sí, este es el santo que hará ese milagro, y el de ofrecerte un abono mensual a un buen precio también. Si eres un trabajador del hospital o de alguna empresa cercana este abono será tu mayor tesoro.\r\n\r\nAdemás, si necesitas ir a cualquier zona de Madrid en transporte público no hay problema. En este parking vigilado podrás aparcar a tan solo 3 min a pie del intercambiador de Avenida de América, que conecta múltiples líneas de autobús y metro.\r\n\r\nEste parking al lado del Hotel Abba y el Hotel Puerta de América es un aparcamiento 24h, por lo que podrás moverte por la ciudad sin necesidad de pensar en la hora. Aunque cuidado, los hoteles pueden no tener el mismo horario, no queremos que tengas que dormir en el parking Hospital Quirón San José (más que nada porque no tienes desayuno incluido, lo mejor de un hotel sin duda).\r\n\r\nAsí que, ya sabes, ¡reserva en este parking low-cost en el barrio de Prosperidad! No será una buena opción para dormir, pero para dejar tu vehículo a buen recaudo no está nada mal ;)",
            "parking_instructions": [],
            "parking_latitude": 40.439955887755,
            "parking_longitude": -3.6745087236644,
            "parking_category": 1,
            "parking_shuttle_schedule": [],
            "is_box": false,
            "slug": "/estacionamento-madrid/hospital-quiron-san-jose",
            "extra_info": {
                "currency": "euro",
                "code": "EUR",
                "minor_unit": 2,
                "symbol": "€"
            },
            "country_codeISO3166-1": "ES"
        },
        "product": {
            "product": "MULTIPASS",
            "product_instructions": "PARA ABRIR A BARREIRA: guarde o ticket. Estacione em qualquer lugar livre. Dirija-se ao balcão do parque com a sua reserva Parclick e o ticket.\r\nPARA SAIR: Dirija-se ao balcão do parque com a sua reserva Parclick.\r\n\r\nSE O SEU PASSE PERMITE ENTRADAS E SAIDAS ILIMITADAS: Siga as mesmas instruções indicadas anteriormente para entrar e sair.",
            "product_category": "MULTIPASS",
            "product_warning": "",
            "product_net_price": 1.69,
            "product_vat_price": 0.35,
            "product_vat_percent": "21%",
            "product_gross_price": 2.04,
            "product_total_time": 1
        },
        "voucher": {
            "voucher_code": "LG513XN",
            "voucher_id": 1160780,
            "voucher_description": [
                "PARA ABRIR A BARREIRA: guarde o ticket. Estacione em qualquer lugar livre. Dirija-se ao balcão do parque com a sua reserva Parclick e o ticket.\r",
                "PARA SAIR: Dirija-se ao balcão do parque com a sua reserva Parclick.\r",
                "SE O SEU PASSE PERMITE ENTRADAS E SAIDAS ILIMITADAS: Siga as mesmas instruções indicadas anteriormente para entrar e sair."
            ]
        },
        "fee": [],
        "third_party_discriminator": "JOINUP",
        "currency": "EUR",
        "has_voucher_notification": false,
        "has_receipt_notification": false,
        "url_faqs": "https://supportparclick.zendesk.com/hc/es",
        "email_contact": "parclick-info@parclick.com"
    },
    "email": {
        "from": "noreply@parclick.com",
        "from_name": "Parclick",
        "to": "parking@parclick.com",
        "to_name": "PARCLICK",
        "cco": "archive@parclick.com",
        "subject": "[Joinup-Parclick] Reserva LG513XN | Parque de estacionamento Hospital Quirón San José, MULTIPASS"
    },
    "locale": "pt_PT",
    "application_name": "Parclick",
    "host": "parclick.pt",
    "logo_image": "https://s3-eu-west-1.amazonaws.com/static.parclick.com/assets/img/Logo-Parclick.png"
}
</pre>
</p>
</details>

<br>

**401 Unauthorized**

| Parameters | Type | Description |
| --------------- |:---------------:| -----------------------------|
| code | integer | http response code value |
| message | string | error description |

<details><summary style="color:#FF6600;">Show response 401 Unauthorized</summary>
<p>
<pre>
{
 "code": 401,
 "message": "Bad credentials"
}
</pre>
</p>
</details>

<br>

## <a name="download_voucher"></a><span style="color:#FF6600;">Download voucher</span>

### <span style="color:#0f6ab4;">`GET`</span> `/v1/booking/{booking_id}/download`

This endpoint returns a link to download a voucher in PDF format. The **booking_id** is obtained in the _new reservation_ endpoint response.


### _Header:_
Authorization: Bearer {JWT_TOKEN}

### _Request:_

| Parameters | Type | Required | Default | Description |
| --------------- |:--------------------| ---------| ---------| -------------------------------------------------- |
| booking_id | int | true | none | the booking reservation ID |
| locale | string | true | en_GB | language in which the information will be returned |


### _Response:_

**200 Ok**

| Parameters | Type | Description |
| ------------------------- | ---------------- | -------------------------------------------- |
| file | string | This endpoint returns a link to download a voucher in PDF format. The link is valid for 30 days. After this date this endpoint must be called to regenerate the PDF document again.|


<details><summary style="color:#FF6600;">Show response 200 Ok</summary>
<p>
<pre>
{
    "file": "https://s3.eu-west-1.amazonaws.com/documents.parclick.com/pre/2020/12/booking/VRKWM9M/es_ES/Parclick-receipt-VRKWM9M.pdf?response-content-disposition=attachment%3B%20filename%3D%22Parclick-receipt-VRKWM9M.pdf%22&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIASQIX4YYHWIBHDHH3%2F20201223%2Feu-west-1%2Fs3%2Faws4_request&X-Amz-Date=20201223T171442Z&X-Amz-SignedHeaders=host&X-Amz-Expires=518400&X-Amz-Signature=850f8bdd3fba283993277173eb9988d8068cb3ffaabff5782e37428f04810905"
}
</pre>
</p>
</details>

<br>

**401 Unauthorized**

| Parameters | Type | Description |
| --------------- |:---------------:| -----------------------------|
| code | integer | http response code value |
| message | string | error description |

<details><summary style="color:#FF6600;">Show response 401 Unauthorized</summary>
<p>
<pre>
{
 "code": 401,
 "message": "Bad credentials"
}
</pre>
</p>
</details>

<br>

## <a name="workflows"></a><span style="color:#FF6600;">Workflows for booking reservation, cancel and list</span>


![alt text](https://static.parclick.com/docs/external_integration_grap.png)

As a first step and part of the development by the integrator it is necessary to select the vehicles available **Vehicle type /v1/vehicle-type**, prior to select the list of car parks for certain coordinates and a specific type of vehicle and dates. For this task it is necessary to call the endpoint **List parkings /v1/parking/list** to get the parking id. With this data it is possible to show a map with the available car parks. Once a specific car park has been selected, it is necessary to obtain the best pass (product ID and token `2338bb72051ae11083a20cd94f3b3183ede3333708b1bc7b50e6af509100ef14`) **get bestpass /v1/pass/details** for the selected period. The additional fields for that car park **get parking /v1/parking/{parking_id}/details** located in response fieldsRequested array.

### <a name="workflow_reservation"></a><span style="color:#FF6600;"> 1- Workflow to create a booking reservation</span>


1. Get the parking id and timespan (from, to)
2. Get the required fields for the selected parking using the parking endpoint `/v1/parking/{parking_id}/details?locale=en_GB&group=detail` In the response, the _embedded node display the required fields for the seleted parking. 
3. Generate the token by calling the bestpass endpoint `/v1/pass/details`. In the response get the provided token `2338bb72051ae11083a20cd94f3b3183ede3333708b1bc7b50e6af509100ef14` for each product available
4. Collect all required fields (email, token, product, firstName, lastName, from, to) to fill the New endpoint `v1/booking/new`

*Parking response, required fields example*

<details><summary style="color:#FF6600;">Show response 200 Ok</summary>
<p>
<pre>
{ 
   "_embedded":{ 
      "fieldsRequested":[ 
         { 
            "id":1,
            "name":"brand",
            "type":"Symfony\\Component\\Form\\Extension\\Core\\Type\\TextType",
            "label":"Vehicle make",
            "error":"Please enter the vehicle make\/brand",
            "required":true,
            "sequence":1
         },
         { 
            "id":2,
            "name":"model",
            "type":"Symfony\\Component\\Form\\Extension\\Core\\Type\\TextType",
            "label":"Vehicle model",
            "error":"Please enter the vehicle model",
            "required":true,
            "sequence":2
         },
         { 
            "id":3,
            "name":"license_plate",
            "type":"Symfony\\Component\\Form\\Extension\\Core\\Type\\TextType",
            "label":"Vehicle registration number",
            "error":"Please enter the vehicle\u0027s registration number",
            "required":true,
            "sequence":3
         }
      ]
   }
}
</pre>
</p>
</details>


*Get token via API pass*

<details><summary style="color:#FF6600;">Show response 200 Ok</summary>
<p>
<pre>
{ 
   "page":1,
   "limit":10,
   "pages":1,
   "items":[ 
      { 
         "id":100,
         "category":"ONEPASS",
         "name":"ONEPASS 100 days",
         "parking":{ 
            "id":1202,
            "covered":true,
            "flexible_entry":true,
            "guarded":true,
            "is_cancellable":true,
            "airport":{ 
               "id":58,
               "terminal":[ 
                  { 
                     "id":99,
                     "terminal":{ 
                        "id":4
                     },
                     "time_to_terminal":4
                  }
               ],
               "category_name":"Official",
               "category":3,
               "has_shuttle":true
            },
            "address":"Avenida de la Hispanidad s\/n. Aeropuerto de Madrid Barajas, Terminal 4. Parking Larga Estancia\/Bajo Coste T4",
            "city":"Madrid",
            "name":"AENA Aeropuerto de Madrid-Barajas - Larga Estancia T4",
            "_links":{ 
               "self":{ 
                  "href":"https:\/\/api.parclick.com\/v1\/parking\/1202"
               }
            },
            "_embedded":{ 
               "city":{ 
                  "id":37,
                  "_links":{ 
                     "self":{ 
                        "href":"https:\/\/api.parclick.com\/v1\/city\/37"
                     }
                  }
               }
            }
         },
         "vehicle_type":{ 
            "id":1,
            "type":"CAR"
         },
         "warning_message":"At the entrance, the ticket is issued automatically by approaching the gate.",
         "administration_fee":0,
         "paypal_fee":1.36,
         "price":37,
         "type":"pass",
         "internal_name":"ONEPASS 5d (37\u20ac)",
         "token":"6eb569c3c645afcb6c8aa439f758137f6233b98aba97fddecd45ceefdc92e4b0",
         "duration":98,
         "multiparking":false,
         "multipass":false,
         "frequency":"HOURLY",
         "_links":{ 
            "self":[ 
               { 
                  "href":"https:\/\/api.parclick.com\/v1\/product\/100"
               },
               { 
                  "href":"https:\/\/api.parclick.com\/v1\/pass\/100"
               }
            ]
         }
      }
   ],
   "total":1,
   "params":{ 
      "locale":"en_GB",
      "group":"bestpass",
      "parking":"1202",
      "from":"2019-03-27 10:00",
      "to":"2019-03-31 12:00",
      "vehicleType":"1"
   },
   "_links":{ 
      "self":{ 
         "href":"\/v1\/pass?page=1\u0026limit=10"
      },
      "first":{ 
         "href":"\/v1\/pass?page=1\u0026limit=10"
      },
      "last":{ 
         "href":"\/v1\/pass?page=1\u0026limit=10"
      }
   }
}
</pre>
</p>
</details>


### <a name="workflow_cancel"></a><span style="color:#FF6600;">2. Workflow to cancel a booking</span>

1. To cancel a booking is mandatory to know the booking id (E.g. 685435) and then proceed.

<br>

### <a name="workflow_list"></a><span style="color:#FF6600;">3. Workflow to list bookings</span>

1. To list bookings you can filter with this available fields:


| Parameters | Type | Description |
| --------------- |:------------------- | ----------------------------------------------------------- |
| group | string | **list** to show details |
| email | string | search by useremail in whole or in part |
| status | string | booking status available: not_modified, confirmed, canceled |
| firstName | string | string to find |
| lastName | string | string to find |
| from | string Y-m-d | booking start date |
| to | string Y-m-d | booking end date |

<br>

## <a name="entry_code"></a><span style="color:#FF6600;">Generate entry codes</span>

Some car parks require the generation of access codes (QR Code, Barcode...) to gain access. If the selected car park requires a QRCode, barcode or some kind of specific code, the _get voucher data_ endpoint response specifies, if necessary, the type of code in the **external\_code\_tech** field. Parclick handles the following types of access codes:



#### <span style="color:#FF6600;">`qrcode_1`</span>

This QR code is used in SAEMES and others car parks and uses the following syntax to generate it.
![alt text](https://static.parclick.com/docs/voucher_saemes.png)

<br>

#### <span style="color:#FF6600;">`barcode_1`</span>

This barcode (type code39) is used in Parkia car parks and uses the following syntax to generate it.
![alt text](https://static.parclick.com/docs/voucher_parkia.png)
<br>

#### <span style="color:#FF6600;">`qrcode_2`</span>

This QR code is used in San Marcos car parks and uses the following syntax to generate it.
![alt text](https://static.parclick.com/docs/voucher_sanmarcos.png)
<br>

#### <span style="color:#FF6600;">`barcode_2`</span>

This barcode (type code128) is used in Marco Polo car parks and uses the following syntax to generate it.
![alt text](https://static.parclick.com/docs/voucher_marcopolo.png)
<br>

#### <span style="color:#FF6600;">`code_1`</span>

This code is used in AENA car parks and uses the following syntax to generate it. This provider require the licence plate to identify and gain access.
![alt text](https://static.parclick.com/docs/voucher_aena.png)
<br>

#### <span style="color:#FF6600;">`code_2`</span>

This code is used in Firenze Parcheggi car parks and uses the following syntax to generate it. This provider require the booking ID to identify and gain access.
![alt text](https://static.parclick.com/docs/voucher_firenze.png)
<br>

#### <span style="color:#FF6600;">`code_3`</span>

This code is used in Copark car parks and uses the following syntax to generate it. This provider require the external code to identify and gain access.
![alt text](https://static.parclick.com/docs/voucher_niza.png)

<br>

<p style="font-size:11px;"> <a name="1"></a>[^1]: The REST architectural style describes six constraints: uniform interface, stateless, cacheable, client-server, layered system, code on demand (optional). [REST API Tutorial](https://www.restapitutorial.com/) - [REST API: What is it, and what are its advantages in project development?](https://bbvaopen4u.com/en/actualidad/rest-api-what-it-and-what-are-its-advantages-project-development)</p>

<p style="font-size:11px;"> <a name="2"></a>[^2]: JSON Web Token (JWT) is a compact, URL-safe means of representing claims to be transferred between two parties - [Documentation](https://jwt.io/).</p>

<br>

<p style="font-size:11px;color:#999999;text-align:center;">2020 Copyright Parclick S.L. All rights reserved.</p>
