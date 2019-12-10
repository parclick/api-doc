![alt text](https://static.parclick.com/assets/img/logotipo-parclick.svg)

<p style="color:#999999;text-align:right;">Document version 1.5.0</p>

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
13. [Worflows](#workflows)
 	- [Workflow to create a booking reservation](#workflow_reservation)
	- [Workflow to cancel a booking](#workflow_cancel)
	- [Workflow to list booking](#workflow_list)
14. [Generate entry code](#entry_code)

<br>

### <a name="description"></a><span style="color:#FF6600;">API Description</span>

#### Authentication
Parclick gives a <a href="#1">REST API</a> [^1](#1) with some endpoints that require authentication. Security is based on <a href="#2">JSON Web Token (JWT)</a> [^2](#2) and is obtained through a successful authentication (view **/v1/login** endpoint). In the protected endpoints this token must be included in the header request as follow:

| Key               | Value                                      |
| ----------------- | ------------------------------------------ |
| Authorization     | Bearer eyJhbGciOiJSUzI1NiJ9.eyJyb2xlcyI... |
| X-thirdparty-auth | kjhawdjh3gJiiHkqjhw787hawdk09HjhaKkdhawdad |

#### Environments
Parclick has two environments: [**development**](https://pre.api.parclick.com) (https://pre.api.parclick.com) and [**production**](https://api.parclick.com) (https://api.parclick.com).

This tutorial help to unsderstand the available endpoints and the flow to create, delete and list bookings. 

#### Internationalization, limit and group
As a general rule, the application manage the **locale** parameter to return the response in the selected language. The available languages are: Spanish, English, French, Italian, Portuguese, German, Dutch, Russian and Catalan.

| Language      | Locale |
| ------------- | ------ |
| Catalan       | ca_ES  |
| Dutch         | nl_NL  |
| English       | en_GB  |
| French        | fr_FR  |
| German        | de_DE  |
| Italian       | it_IT  |
| Portuguese    | pt_PT  |
| Russian       | ru_RU  |
| Spanish       | es_ES  |

The **group** parameter in the endpoints that have it available allows to show more or less information in the answer. Each endpoint that has it available shows which is the optimal **group** option.

Some of the list endpoint return by defauls a maximum of 200 records. The **limit** parameter can be defined to customize the number of results. This parameter is defined in the documentation of each specific endpoint.

<br>

## <a name="login"></a><span style="color:#FF6600;">Login</span>

### <span style="color:#10a54a;">`POST`</span> `/v1/login`


This method allows third party systems to be authenticated and receive a key (token) to pass when calling the e-commerce web service methods.

### _Request:_

| Parameters      | Type            | Required |
| --------------- |:--------------- | -------- |
| \_username      | string          | true     |
| \_password      | string          | true     |



### _Response:_

**200 Ok**

| Parameters      | Type           | Description                          |
| --------------- |:-------------- | :----------------------------------- |
| token           | string         | token (expires in 1 hour)            |
| refresh_token   | string         | token to refresh current valid token |                       
| id              | string         | user id authenticated                |


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

| Parameters      | Type            | Description                  |
| --------------- |:--------------- | ---------------------------- |
| code            | integer         | http response code value     |
| message         | string          | error description            |

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

| Parameters      | Type                | Required | Description              |
| --------------- |:------------------- | ---------|------------------------- |
| refresh_token   | string              | true     | third party identificator|


### _Response:_

**200 Ok**

| Parameters      | Type                | Description                                       |
| --------------- |:------------------- |-------------------------------------------------- |
| token           | string              | token passed with each request (Expires in 1 hour)|
| refresh_token   | string              | token to refresh current valid token              |


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

| Parameters      | Type            | Description                  |
| --------------- |:--------------- | ---------------------------- |
| code            | integer         | http response code value     |
| message         | string          | error description            |

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
To import all parkings for caching, the group must be set to **export** without parameters except the page and limit for pagination.

### _Header:_
Authorization: Bearer {JWT_TOKEN}

### _Request:_

| Parameters      | Type                   | Required | Description                                                                       | Default             |
| --------------- |:---------------------- | -------- | --------------------------------------------------------------------------------- |-------------------- |
| locale          | string                 | true     | language in which the information will be returned                                | en_GB               |
| group           | string                 | true     | **search** **export**                                                                        | null                |
| limit           | integer                | false     | total number of records [1-200]                                                   | 200                 |
| page            | integer                | false     | page selected                                                   | 1                 |
| from            | date Y-m-d H:i         | false     | booking start date                                                                | null                |
| to              | date Y-m-d H:i         | false     | booking end date                                                                  | null                |
| latitude        | float                  | false     | valid latitude in which to look for                                               | null                |
| longitude       | float                  | false     | valid longitude in which to look for                                              | null                |
| radius          | integer                | false    | search radius                                                                     | null                |
| vehicleType     | integer                | false     | type of vehicle you wish to park [1 car, 2 van, 3 caravan, 4 bus, 5 truck, 6 motorbike, 7 small truck]| null                |
| freemium        | bool                   | false    | car parks where reservations cannot be booked                                     | false                |



### _Response:_

**200 Ok**

<details><summary style="color:#FF6600;">Show response 200 Ok</summary>
<p>
<pre>
{
    "page": 1,
    "limit": 10,
    "pages": 208,
    "items": [
        {
            "id": 4,
            "extra_info": [],
            "address": "Carrer de Santal\u00f3, 12",
            "city": "Barcelona",
            "country": "Espa\u00f1a",
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
            "name": "NN Santal\u00f3",
            "access_phone": "00 34 647 07 97 31",
            "max_height": 180,
            "slug": "nn_santalo",
            "voucher_needed": false,
            "freemium": false,
            "multiparking": false,
            "instructions": "",
            "cancellation_type": 2,
            "_links": {
                "self": {
                    "href": "https:\/\/loc.api.parclick.com\/v1\/parking\/4"
                }
            },
            "_embedded": {
                "city": {
                    "id": 6,
                    "name": "Barcelona",
                    "slug": "barcelona",
                    "latitude": 41.3851,
                    "longitude": 2.1734,
                    "time_zone": "Europe\/Madrid",
                    "_links": {
                        "self": {
                            "href": "https:\/\/loc.api.parclick.com\/v1\/city\/6"
                        }
                    }
                }
            }
        },
        {
            "id": 5,
            "extra_info": [],
            "address": "Comte d\u0027Urgell, 154",
            "city": "Barcelona",
            "country": "Espa\u00f1a",
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
            "max_height": 195,
            "slug": "nn_urgell_2",
            "voucher_needed": false,
            "freemium": false,
            "multiparking": false,
            "instructions": "",
            "cancellation_type": 2,
            "_links": {
                "self": {
                    "href": "https:\/\/loc.api.parclick.com\/v1\/parking\/5"
                }
            },
            "_embedded": {
                "city": {
                    "id": 6,
                    "name": "Barcelona",
                    "slug": "barcelona",
                    "latitude": 41.3851,
                    "longitude": 2.1734,
                    "time_zone": "Europe\/Madrid",
                    "_links": {
                        "self": {
                            "href": "https:\/\/loc.api.parclick.com\/v1\/city\/6"
                        }
                    }
                }
            }
        },
        {
            "id": 6,
            "extra_info": [],
            "address": "Comte Borrell, 26-28",
            "city": "Barcelona",
            "country": "Espa\u00f1a",
            "province": "Barcelona",
            "zip": "8015",
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
            "name": "NN Borrell",
            "access_phone": "00 34 634 30 42 14",
            "max_height": 210,
            "slug": "nn_borrell",
            "voucher_needed": false,
            "freemium": false,
            "multiparking": false,
            "instructions": "",
            "cancellation_type": 2,
            "_links": {
                "self": {
                    "href": "https:\/\/loc.api.parclick.com\/v1\/parking\/6"
                }
            },
            "_embedded": {
                "city": {
                    "id": 6,
                    "name": "Barcelona",
                    "slug": "barcelona",
                    "latitude": 41.3851,
                    "longitude": 2.1734,
                    "time_zone": "Europe\/Madrid",
                    "_links": {
                        "self": {
                            "href": "https:\/\/loc.api.parclick.com\/v1\/city\/6"
                        }
                    }
                }
            }
        },
        {
            "id": 7,
            "extra_info": [],
            "address": "Valencia, 243",
            "city": "Barcelona",
            "country": "Espa\u00f1a",
            "province": "Barcelona",
            "zip": "8007",
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
            "name": "NN Valencia II",
            "max_height": 200,
            "slug": "nn_valencia_ii",
            "voucher_needed": false,
            "freemium": false,
            "multiparking": false,
            "instructions": "",
            "cancellation_type": 2,
            "_links": {
                "self": {
                    "href": "https:\/\/loc.api.parclick.com\/v1\/parking\/7"
                }
            },
            "_embedded": {
                "city": {
                    "id": 6,
                    "name": "Barcelona",
                    "slug": "barcelona",
                    "latitude": 41.3851,
                    "longitude": 2.1734,
                    "time_zone": "Europe\/Madrid",
                    "_links": {
                        "self": {
                            "href": "https:\/\/loc.api.parclick.com\/v1\/city\/6"
                        }
                    }
                }
            }
        },
        {
            "id": 8,
            "extra_info": [],
            "address": "Valencia, 511",
            "city": "Barcelona",
            "country": "Espa\u00f1a",
            "province": "Barcelona",
            "zip": "8013",
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
            "name": "NN Valencia III",
            "max_height": 190,
            "slug": "nn_valencia_iii",
            "voucher_needed": false,
            "freemium": false,
            "multiparking": false,
            "instructions": "",
            "cancellation_type": 2,
            "_links": {
                "self": {
                    "href": "https:\/\/loc.api.parclick.com\/v1\/parking\/8"
                }
            },
            "_embedded": {
                "city": {
                    "id": 6,
                    "name": "Barcelona",
                    "slug": "barcelona",
                    "latitude": 41.3851,
                    "longitude": 2.1734,
                    "time_zone": "Europe\/Madrid",
                    "_links": {
                        "self": {
                            "href": "https:\/\/loc.api.parclick.com\/v1\/city\/6"
                        }
                    }
                }
            }
        },
        {
            "id": 9,
            "extra_info": [],
            "address": "Rocafort, 64",
            "city": "Barcelona",
            "country": "Espa\u00f1a",
            "province": "Barcelona",
            "zip": "8015",
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
            "name": "NN Rocafort",
            "access_phone": "00 34 687 61 05 23",
            "max_height": 190,
            "slug": "nn_rocafort",
            "voucher_needed": false,
            "freemium": false,
            "multiparking": false,
            "instructions": "",
            "cancellation_type": 2,
            "_links": {
                "self": {
                    "href": "https:\/\/loc.api.parclick.com\/v1\/parking\/9"
                }
            },
            "_embedded": {
                "city": {
                    "id": 6,
                    "name": "Barcelona",
                    "slug": "barcelona",
                    "latitude": 41.3851,
                    "longitude": 2.1734,
                    "time_zone": "Europe\/Madrid",
                    "_links": {
                        "self": {
                            "href": "https:\/\/loc.api.parclick.com\/v1\/city\/6"
                        }
                    }
                }
            }
        },
        {
            "id": 10,
            "extra_info": [],
            "address": "Tarragona, 141",
            "city": "Barcelona",
            "country": "Espa\u00f1a",
            "province": "Barcelona",
            "zip": "8014",
            "handicapped_access": true,
            "security": true,
            "open_24h": false,
            "giving_keys": false,
            "exclude_checking": false,
            "license_info": false,
            "vehicle_info": false,
            "flight": false,
            "return_flight_location": false,
            "train": false,
            "name": "NN Tarragona",
            "max_height": 210,
            "slug": "nn_tarragona",
            "voucher_needed": false,
            "freemium": false,
            "multiparking": false,
            "instructions": "",
            "cancellation_type": 2,
            "_links": {
                "self": {
                    "href": "https:\/\/loc.api.parclick.com\/v1\/parking\/10"
                }
            },
            "_embedded": {
                "city": {
                    "id": 6,
                    "name": "Barcelona",
                    "slug": "barcelona",
                    "latitude": 41.3851,
                    "longitude": 2.1734,
                    "time_zone": "Europe\/Madrid",
                    "_links": {
                        "self": {
                            "href": "https:\/\/loc.api.parclick.com\/v1\/city\/6"
                        }
                    }
                }
            }
        },
        {
            "id": 14,
            "extra_info": [],
            "address": "Passeig de Colom,",
            "city": "Barcelona",
            "country": "Espa\u00f1a",
            "province": "Barcelona",
            "zip": "08002",
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
            "name": "BSM Moll de la Fusta",
            "max_height": 195,
            "slug": "bsm_moll_de_la_fusta",
            "voucher_needed": false,
            "freemium": false,
            "multiparking": true,
            "instructions": "",
            "cancellation_type": 1,
            "_links": {
                "self": {
                    "href": "https:\/\/loc.api.parclick.com\/v1\/parking\/14"
                }
            },
            "_embedded": {
                "city": {
                    "id": 6,
                    "name": "Barcelona",
                    "slug": "barcelona",
                    "latitude": 41.3851,
                    "longitude": 2.1734,
                    "time_zone": "Europe\/Madrid",
                    "_links": {
                        "self": {
                            "href": "https:\/\/loc.api.parclick.com\/v1\/city\/6"
                        }
                    }
                }
            }
        },
        {
            "id": 18,
            "extra_info": [],
            "address": "C. Carreras i Candi, 65",
            "city": "Barcelona",
            "country": "Espa\u00f1a",
            "province": "Barcelona",
            "zip": "8028",
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
            "name": "Parking Viajeros",
            "max_height": 210,
            "slug": "parking_viajeros_1",
            "voucher_needed": false,
            "freemium": false,
            "multiparking": false,
            "instructions": "",
            "cancellation_type": 2,
            "_links": {
                "self": {
                    "href": "https:\/\/loc.api.parclick.com\/v1\/parking\/18"
                }
            },
            "_embedded": {
                "city": {
                    "id": 6,
                    "name": "Barcelona",
                    "slug": "barcelona",
                    "latitude": 41.3851,
                    "longitude": 2.1734,
                    "time_zone": "Europe\/Madrid",
                    "_links": {
                        "self": {
                            "href": "https:\/\/loc.api.parclick.com\/v1\/city\/6"
                        }
                    }
                }
            }
        },
        {
            "id": 19,
            "extra_info": [],
            "address": "Aeropuerto de Alicante",
            "city": "Alicante",
            "country": "Espa\u00f1a",
            "province": "Alicante",
            "zip": "3195",
            "handicapped_access": false,
            "security": true,
            "open_24h": false,
            "giving_keys": true,
            "exclude_checking": false,
            "license_info": true,
            "vehicle_info": true,
            "flight": false,
            "return_flight_location": false,
            "train": false,
            "name": "ALC Valet Parking - Aeropuerto Alicante",
            "slug": "alc_valet_parking_aeropuerto_alicante",
            "voucher_needed": false,
            "freemium": true,
            "multiparking": false,
            "instructions": "",
            "cancellation_type": 2,
            "_links": {
                "self": {
                    "href": "https:\/\/loc.api.parclick.com\/v1\/parking\/19"
                }
            },
            "_embedded": {
                "city": {
                    "id": 3,
                    "name": "Alicante",
                    "slug": "alicante",
                    "latitude": 38.3453,
                    "longitude": -0.4831,
                    "time_zone": "Europe\/Madrid",
                    "_links": {
                        "self": {
                            "href": "https:\/\/loc.api.parclick.com\/v1\/city\/3"
                        }
                    }
                }
            }
        }
    ],
    "total": 2072,
    "params": {
        "locale": "en_EN",
        "group": "export",
        "limit": "10",
        "page": "1"
    }
}
</pre>
</p>
</details>

<br>

**400 Bad request -  when parameters are missing or incorrect**

| Parameters      | Type            | Description                    |
| --------------- |:--------------- | ------------------------------ |
| code            | string          | http response code value       |
| message         | string          | error description              |
| errors          | array           | fields and the specific error  |

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

| key | value                |
|-----|----------------------|
| 1   | Cancel before 23:59  |
| 2   | Cancel one hour left |
| 3   | Cancel disallowed    |


### _Header:_
Authorization: Bearer {JWT_TOKEN}

### _Request:_

| Parameters      | Type                  | Required  | Description                                        | Default        |
| --------------- |:--------------------- | --------- |--------------------------------------------------- |--------------- |
| group           | string                | false     | property groups to be returned **detail**          | null           |
| locale          | string                | false     | language in which the information will be returned | en_GB          | 



### _Response:_

**200 Ok**

<details><summary style="color:#FF6600;">Show response 200 Ok</summary>
<p>
<pre>
{
    "id": 1047,
    "covered": false,
    "description": "<p>Car park at the Nice Airport, located a few kilometers away. The Aéroport Nice Côte d'Azur ECTOR car park has outdoor parking spots with all-day surveillance. Although it’s an uncovered lot, you don’t have to worry about leaving your car here. The Aéroport Nice Côte d'Azur ECTOR car park has the lowest market rates and also offers a pick-up and drop-off service at your airport terminal. </p> \r\n<p>The difficulty of accessing car parks near the Niza Costa Azul Airport makes their valet service perfect for avoiding traffic jams and lets you save time by going directly to the Airport and leaving your car in the good hands of the Aéroport Nice Côte d'Azur ECTOR car park staff. Unlike other car parks, the Aéroport Nice Côte d'Azur ECTOR car park provides direct access to the terminal, so your trip will go more smoothly knowing your car is parked at our 24-hour enclosed, guarded and monitored facilities. </p> \r\n<p>The procedure is very simple: a day before your arrival at the Aéroport Nice Côte d'Azur ECTOR car park, you’ll receive a message with the phone number of the staff member who will pick you up the next day, so all you have to do is call this number 15 minutes beforehand to advise of your arrival, and for your return, you should do the same. </p> \r\n<p>The staff is completely qualified to drive any kind of vehicle, but the Aéroport Nice Côte d'Azur ECTOR car park provides insurance that covers 100% of any damage that may occur to your vehicle from the moment you leave it to the moment it’s returned. Parking at the Nice Airport is much easier and comfortable with the Aéroport Nice Côte d'Azur ECTOR car park, and thanks to their valet service, you don’t have to lose any time, only head to the terminal and they’ll pick up your car and park it safely while you fully enjoy your vacation. </p>\r\nPlease mind you need to leave the car keys with us.",
    "flexible_entry": false,
    "guarded": true,
    "latitude": 43.660659839364,
    "image_list": "//static.parclick.com/parking/2017/10/72f/462/d8/72f462d8-ba0d-5f77-9bde-9be1d3767ecb.jpeg",
    "longitude": 7.2046668846012,
    "medias": [
        {
            "id": 8914,
            "url": "https://static.parclick.com/parking/2017/10/72f/462/d8/72f462d8-ba0d-5f77-9bde-9be1d3767ecb.jpeg"
        }
    ],
    "airport": {
        "id": 19,
        "terminal": [
            {
                "id": 25,
                "terminal": {
                    "id": 8,
                    "name": "T2",
                    "latitude": 43.660056,
                    "longitude": 7.205496
                },
                "in_bound_meeting_point": "",
                "out_bound_meeting_point": ""
            }
        ],
        "category_name": "Valet",
        "shuttle_schedule": [],
        "category": 2,
        "additional_information": "",
        "special_needs": "Car wash.",
        "call_reserve_bus": false,
        "vehicle_inspection": true,
        "has_shuttle": false,
        "shuttle_child_sit": false,
        "minutes_call_before_out_bound": 15,
        "call_to_get_car": true,
        "call_parking_out_bound": false,
        "out_bound_meeting_point": "",
        "out_bound_custumer_service_control": false,
        "in_bound_custumer_service_control": false,
        "customer_service_place": "",
        "additional_instructions_beginning": "THE DAY BEFORE YOUR ARRIVAL: You’ll receive an email and an SMS with the name and number of the person who will pick up your car at the terminal. If these don't come through, call the number on the Parclick voucher an hour before your pick-up time. You must call from the number linked with your reservation, so they put you in contact with the right person.",
        "additional_instructions_return": "When you return, they will leave a bottle of water for you in your car. \r\n \r\nThe car park constantly follows the status of your flight, so there will definitely be someone waiting for you, even if your flight arrives early or late."
    },
    "address": "T2 Aéroport de Nice-Côte d'Azur, Nice, France",
    "city": "Nice",
    "country": "France",
    "province": "Alpes-Maritimes",
    "zip": "06200",
    "provider": {
        "id": 276,
        "name": "ECTOR"
    },
    "handicapped_access": true,
    "security": false,
    "open_24h": false,
    "giving_keys": true,
    "exclude_checking": false,
    "license_info": true,
    "vehicle_info": true,
    "flight": true,
    "return_flight_location": false,
    "train": false,
    "name": "T2 ECTOR Aéroport - Service Voiturier - Nice",
    "external": [
        {
            "id": 259,
            "discriminator": "ector"
        }
    ],
    "max_height": 190,
    "passes": [
        {
            "id": 27032,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 1 day",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 4.62,
            "price": 154,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 16d (154.00€)",
            "token": "",
            "duration": 366,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27032"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27032"
                    }
                ]
            }
        },
        {
            "id": 27036,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 2 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 1.8,
            "price": 60,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 2d (60.00€)",
            "token": "",
            "duration": 48,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27036"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27036"
                    }
                ]
            }
        },
        {
            "id": 27040,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 3 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 2.1,
            "price": 70,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 3d (70.00€)",
            "token": "",
            "duration": 72,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27040"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27040"
                    }
                ]
            }
        },
        {
            "id": 27044,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 4 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 2.4,
            "price": 80,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 4d (80.00€)",
            "token": "",
            "duration": 96,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27044"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27044"
                    }
                ]
            }
        },
        {
            "id": 27048,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 5 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 2.7,
            "price": 90,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 5d (90.00€)",
            "token": "",
            "duration": 120,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27048"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27048"
                    }
                ]
            }
        },
        {
            "id": 27052,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 6 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 3,
            "price": 100,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 6d (100.00€)",
            "token": "",
            "duration": 144,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27052"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27052"
                    }
                ]
            }
        },
        {
            "id": 27056,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 7 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 3.15,
            "price": 105,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 7d (105.00€)",
            "token": "",
            "duration": 168,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27056"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27056"
                    }
                ]
            }
        },
        {
            "id": 27060,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 8 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 3.45,
            "price": 115,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 8d (115.00€)",
            "token": "",
            "duration": 192,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27060"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27060"
                    }
                ]
            }
        },
        {
            "id": 27064,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 9 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 3.75,
            "price": 125,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 9d (125.00€)",
            "token": "",
            "duration": 216,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27064"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27064"
                    }
                ]
            }
        },
        {
            "id": 27068,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 10 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 4.05,
            "price": 135,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 10d (135.00€)",
            "token": "",
            "duration": 240,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27068"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27068"
                    }
                ]
            }
        },
        {
            "id": 27072,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 11 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 4.35,
            "price": 145,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 11d (145.00€)",
            "token": "",
            "duration": 264,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27072"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27072"
                    }
                ]
            }
        },
        {
            "id": 27076,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 12 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 4.65,
            "price": 155,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 12d (155.00€)",
            "token": "",
            "duration": 288,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27076"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27076"
                    }
                ]
            }
        },
        {
            "id": 27080,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 13 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 4.95,
            "price": 165,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 13d (165.00€)",
            "token": "",
            "duration": 312,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27080"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27080"
                    }
                ]
            }
        },
        {
            "id": 27084,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 14 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 5.1,
            "price": 170,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 14d (170.00€)",
            "token": "",
            "duration": 336,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27084"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27084"
                    }
                ]
            }
        },
        {
            "id": 27088,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 15 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 5.25,
            "price": 175,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 15d (175.00€)",
            "token": "",
            "duration": 360,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27088"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27088"
                    }
                ]
            }
        },
        {
            "id": 27092,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 16 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 5.55,
            "price": 185,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 16d (185.00€)",
            "token": "",
            "duration": 384,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27092"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27092"
                    }
                ]
            }
        },
        {
            "id": 27096,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 17 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 5.85,
            "price": 195,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 17d (195.00€)",
            "token": "",
            "duration": 408,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27096"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27096"
                    }
                ]
            }
        },
        {
            "id": 27100,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 18 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 6.15,
            "price": 205,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 18d (205.00€)",
            "token": "",
            "duration": 432,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27100"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27100"
                    }
                ]
            }
        },
        {
            "id": 27104,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 19 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 6.45,
            "price": 215,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 19d (215.00€)",
            "token": "",
            "duration": 456,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27104"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27104"
                    }
                ]
            }
        },
        {
            "id": 27108,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 20 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 6.75,
            "price": 225,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 20d (225.00€)",
            "token": "",
            "duration": 480,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27108"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27108"
                    }
                ]
            }
        },
        {
            "id": 27112,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 21 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 6.75,
            "price": 225,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 21d (225.00€)",
            "token": "",
            "duration": 504,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27112"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27112"
                    }
                ]
            }
        },
        {
            "id": 27116,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 22 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 7.05,
            "price": 235,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 22d (235.00€)",
            "token": "",
            "duration": 528,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27116"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27116"
                    }
                ]
            }
        },
        {
            "id": 27120,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 23 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 7.35,
            "price": 245,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 23d (245.00€)",
            "token": "",
            "duration": 552,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27120"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27120"
                    }
                ]
            }
        },
        {
            "id": 27124,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 24 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 7.65,
            "price": 255,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 24d (255.00€)",
            "token": "",
            "duration": 576,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27124"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27124"
                    }
                ]
            }
        },
        {
            "id": 27128,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 25 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 7.8,
            "price": 260,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 25d (260.00€)",
            "token": "",
            "duration": 600,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27128"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27128"
                    }
                ]
            }
        },
        {
            "id": 27132,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 26 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 7.8,
            "price": 260,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 26d (260.00€)",
            "token": "",
            "duration": 624,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27132"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27132"
                    }
                ]
            }
        },
        {
            "id": 27136,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 27 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 7.8,
            "price": 260,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 27d (260.00€)",
            "token": "",
            "duration": 648,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27136"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27136"
                    }
                ]
            }
        },
        {
            "id": 27140,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 28 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 7.8,
            "price": 260,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 28d (260.00€)",
            "token": "",
            "duration": 672,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27140"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27140"
                    }
                ]
            }
        },
        {
            "id": 27144,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 29 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 8.1,
            "price": 270,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 29d (270.00€)",
            "token": "",
            "duration": 696,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27144"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27144"
                    }
                ]
            }
        },
        {
            "id": 27148,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 30 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 8.4,
            "price": 280,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 30d (280.00€)",
            "token": "",
            "duration": 720,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/27148"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/27148"
                    }
                ]
            }
        },
        {
            "id": 40455,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 31 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 8.7,
            "price": 290,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 31d (290.00€)",
            "token": "",
            "duration": 744,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/40455"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/40455"
                    }
                ]
            }
        },
        {
            "id": 40456,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 32 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 6.6,
            "price": 220,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 32d (220.00€)",
            "token": "",
            "duration": 768,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/40456"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/40456"
                    }
                ]
            }
        },
        {
            "id": 40457,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 33 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 6.9,
            "price": 230,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 33d (230.00€)",
            "token": "",
            "duration": 792,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/40457"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/40457"
                    }
                ]
            }
        },
        {
            "id": 40458,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 34 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 7.2,
            "price": 240,
            "status": "INACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 34d (240.00€)",
            "token": "",
            "duration": 816,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/40458"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/40458"
                    }
                ]
            }
        },
        {
            "id": 65880,
            "category": "ONEPASS",
            "instructions": "While you are making your purchase choose the date on which you are going to arrive. After making the payment online you will receive a billing confirmation by email containing the reference number of your reservation. THE DAY OF YOUR RESERVATION: Let us know by telephone 15 minutes before arriving at the airport. The driver will direct you to which point of the terminal you can hand your vehicle over to us. Show your Parclick booking confirmation to the driver before handing over your keys. UPON YOUR RETURN: Let us know by telephone when you are picking up your luggage (or once you have landed if you do not have luggage). We will be waiting for you with your vehicle at the same point of the Departure Terminal where we picked it up.",
            "name": "ONEPASS 100 days",
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.\r\n<br/>It is important to arrive at the hour indicated on your reservation, since that our staff will be there waiting to serve you.\r\n<br/>You can contact our car park on +339 73 72 88 55 if you have any query or if any incident occurs.",
            "administration_fee": 0,
            "paypal_fee": 1.2,
            "price": 0,
            "status": "ACTIVE",
            "type": "pass",
            "internal_name": "ONEPASS 100d (0.00€)",
            "token": "",
            "duration": 2400,
            "multiparking": false,
            "multipass": false,
            "frequency": "HOURLY",
            "_links": {
                "self": [
                    {
                        "href": "https://loc.api.parclick.com/v1/product/65880"
                    },
                    {
                        "href": "https://loc.api.parclick.com/v1/pass/65880"
                    }
                ]
            }
        }
    ],
    "subscriptions": [],
    "schedule": [
        {
            "id": 2372,
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
                    "id": 2605,
                    "hour_from": "1970-01-01T06:00:00+0000",
                    "hour_to": "1970-01-01T00:00:00+0000"
                }
            ]
        },
        {
            "id": 2373,
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
                    "id": 2606,
                    "hour_from": "1970-01-01T06:00:00+0000",
                    "hour_to": "1970-01-01T00:00:00+0000"
                }
            ]
        },
        {
            "id": 2374,
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
                    "id": 2607,
                    "hour_from": "1970-01-01T06:00:00+0000",
                    "hour_to": "1970-01-01T00:00:00+0000"
                }
            ]
        },
        {
            "id": 2375,
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
                    "id": 2608,
                    "hour_from": "1970-01-01T06:00:00+0000",
                    "hour_to": "1970-01-01T00:00:00+0000"
                }
            ]
        },
        {
            "id": 2376,
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
                    "id": 2609,
                    "hour_from": "1970-01-01T06:00:00+0000",
                    "hour_to": "1970-01-01T00:00:00+0000"
                }
            ]
        },
        {
            "id": 2377,
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
                    "id": 2613,
                    "hour_from": "1970-01-01T06:00:00+0000",
                    "hour_to": "1970-01-01T00:00:00+0000"
                }
            ]
        },
        {
            "id": 2378,
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
                    "id": 2611,
                    "hour_from": "1970-01-01T06:00:00+0000",
                    "hour_to": "1970-01-01T00:00:00+0000"
                }
            ]
        }
    ],
    "access": [
        {
            "id": 1021,
            "type": "vehicle",
            "address": "Dépose-minute terminal 2",
            "latitude": 43.660659839364,
            "longitude": 7.2046668846012
        }
    ],
    "slug": "aeroport-nice-cote-d-azur-ector-service-voiturier-exterieur",
    "voucher_needed": false,
    "freemium": false,
    "multiparking": false,
    "instructions": "",
    "cancellation_type": 1,
    "_links": {
        "self": {
            "href": "https://loc.api.parclick.com/v1/parking/1047"
        }
    },
    "_embedded": {
        "city": {
            "id": 115,
            "name": "Nice",
            "slug": "nice",
            "vat_tax": 20,
            "_links": {
                "self": {
                    "href": "https://loc.api.parclick.com/v1/city/115"
                }
            }
        },
        "fieldsRequested": [
            {
                "id": 1,
                "name": "brand",
                "type": "Symfony\\Component\\Form\\Extension\\Core\\Type\\TextType",
                "label": "Vehicle make",
                "error": "Please enter the vehicle make/brand",
                "required": true,
                "sequence": 1
            },
            {
                "id": 2,
                "name": "model",
                "type": "Symfony\\Component\\Form\\Extension\\Core\\Type\\TextType",
                "label": "Vehicle model",
                "error": "Please enter the vehicle model",
                "required": true,
                "sequence": 2
            },
            {
                "id": 3,
                "name": "license_plate",
                "type": "Symfony\\Component\\Form\\Extension\\Core\\Type\\TextType",
                "label": "Vehicle registration number",
                "error": "Please enter the vehicle's registration number",
                "required": true,
                "sequence": 3
            },
            {
                "id": 4,
                "name": "arrival_flight",
                "type": "Symfony\\Component\\Form\\Extension\\Core\\Type\\TextType",
                "label": "Return flight number",
                "error": "Please enter your return flight number",
                "required": true,
                "sequence": 4
            },
            {
                "id": 5,
                "name": "departure_flight",
                "type": "Symfony\\Component\\Form\\Extension\\Core\\Type\\TextType",
                "label": "Departing flight number",
                "error": "Please enter your departing flight number",
                "required": true,
                "sequence": 5
            },
            {
                "id": 15,
                "name": "color_vehicle",
                "type": "Symfony\\Component\\Form\\Extension\\Core\\Type\\TextType",
                "label": "Colour of vehicle",
                "error": "Please, indicate the colour of your vehicle.",
                "required": true,
                "sequence": 15
            }
        ]
    }
}
</pre>
</p>
</details>

<br>

**401 Bad request -  when parameters are missing or incorrect**

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

| Parameters      | Type                  | Required | Description                                        | Default        |
| --------------- |:--------------------- | :------- |--------------------------------------------------- |--------------- |
| group           | string                | true     | property groups to be returned **bestpass**        |null            |
| locale          | string                | true     | language in which the information will be returned |en_GB           | 
| parking         | integer               | true     | parking identificator                              |null            |
| vehicleType     | integer               | true     | type of vehicle you wish to park [1 car, 2 van, 3 caravan, 4 bus, 5 truck, 6 motorbike, 7 small truck]    |1               |
| from            | date Y-m-d H:i        | true     | booking start date                                 |null            |
| to              | date Y-m-d H:i        | true     | booking end date                                   |null            |


### _Response:_

**200 Ok**

<details><summary style="color:#FF6600;">Show response 200 Ok</summary>
<p>
<pre>
[
    {
        "id": 54332,
        "category": "ONEPASS",
        "name": "ONEPASS 2 hours",
        "parking": {
            "id": 1988,
            "covered": false,
            "flexible_entry": true,
            "guarded": true,
            "is_cancellable": true,
            "address": "Calle de Fray Luis de León, 11",
            "city": "Madrid",
            "name": "Fray Luis de León - Atocha",
            "_links": {
                "self": {
                    "href": "https://loc.api.parclick.com/v1/parking/1988"
                }
            },
            "_embedded": {
                "city": {
                    "id": 37,
                    "_links": {
                        "self": {
                            "href": "https://loc.api.parclick.com/v1/city/37"
                        }
                    }
                }
            }
        },
        "vehicle_type": {
            "id": 1,
            "type": "CAR"
        },
        "warning_message": "For the QR reader to work properly, you must have the latest version of the app installed.",
        "administration_fee": 0,
        "paypal_fee": 1.21,
        "price": 3,
        "type": "pass",
        "internal_name": "ONEPASS 2h (3.00€)",
        "token": "2dd05be64d5f3bf031eff042565e788b84d81fa543bcac6ad3d4ac814a952241",
        "duration": 2,
        "multiparking": false,
        "multipass": false,
        "frequency": "HOURLY",
        "_links": {
            "self": [
                {
                    "href": "https://loc.api.parclick.com/v1/product/54332"
                },
                {
                    "href": "https://loc.api.parclick.com/v1/pass/54332"
                }
            ]
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

**401 Bad request -  when parameters are missing or incorrect**

<details><summary style="color:#FF6600;">Show response 401 Bad request</summary>
<p>
<pre>
{
	{  
	   "code":400,
	   "message":"Validation Failed",
	   "errors":{  
	      "errors":[  
	         "This form should not contain extra fields."
	      ],
	      "children":{  
	         "locale":{  
	         },
	         "group":{  
	         },
	         "page":{  
	         },
	         "limit":{  
	         },
	         "sort":{  
	         },
	         "direction":{  
	         },
	         "from":{  
	         },
	         "to":{  
	         },
	         "latitude":{  
	         },
	         "longitude":{  
	         },
	         "radius":{  
	         },
	         "vehicleType":{  
	         },
	         "status":{  
	         },
	         "parking":{  
	         },
	         "type":{  
	         },
	         "frequency":{  
	         }
	      }
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

| Parameters      | Type                  | Required | Description                                        | Default        |
| --------------- |:--------------------- | :------- |--------------------------------------------------- |--------------- |
| group           | string                | true     | property groups to be returned **list**            | list           |
| locale          | string                | true     | language in which the information will be returned | en_GB          |


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

**401 Bad request -  when parameters are missing or incorrect**

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

| Parameters      | Type                  | Required | Description              | Default                |
| --------------- |:--------------------- | ---------| ------------------------ |----------------------- |
| email           | string                | true     | user email               | null                   |
| token           | string                | true     | checkout token           | null                   |
| product         | integer               | true     | product id               | null                   |
| firstName       | string                | true     | user fisrt name          | null                   |
| lastName        | string                | true     | user last name           | null                   |
| from            | date Y-m-d H:i        | true     | booking start date       | null                   |
| to              | date Y-m-d H:i        | true     | booking end date         | null                   |
| brand           | string                | false    | vehicle brand            | null                   |
| model           | string                | false    | vehicle model            | null                   |
| licence_plate   | string                | false    | vehicle licence plate    | null                   |
| color_vehicle   | string                | false    | vehicle color            | null                   |
| departure_flight| string                | false    | departure flight voucher | null                   |
| arrival_flight  | string                | false    | arrival flight voucher   | null                   |
| train_number    | string                | false    | train voucher            | null                   |


### _Response:_

**200 Ok**

| Parameters      | Type                | Description                                 |
| --------------- |:------------------- |-------------------------------------------- |
| booking_id      | integer             | booking order id                            |
| voucher_id      | integer             | new booking identifier (required to cancel) |
| voucher_code    | string              | new booking voucher                         |

<details><summary style="color:#FF6600;">Show response 200 Ok</summary>
<p>
<pre>
{
    "booking_id": 805097,
    "voucher_id": 685447,
    "voucher_code": "L5N9675",
}
</pre>
</p>
</details>

<br>

**401 Unauthorized**

| Parameters      | Type            | Description                  |
| --------------- |:--------------- | ---------------------------- |
| code            | integer         | http response code value     |
| message         | string          | error description            |

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


| Key                     | Value                                                                |
| ----------------------- | -------------------------------------------------------------------- |
| ERROR_NOT_FOUND         | Booking not found                                                    |
| ERROR_EXPIRED           | Booking expired                                                      |
| ERROR_CANCELED_ALLOW    | Booking can not be canceled                                          |
| ERROR_EXTERNAL_PROVIDER | Booking can not be canceled by external provider                     |
| ERROR_REFUND            | Booking has been canceled but there is an error refund money to user |
| ERROR_ALREADY_CANCELED  | Booking has already been canceled                                    |
| UNDEFINED               | Refund has failed for unknown reasons                                |


### _Header:_
Authorization: Bearer {JWT_TOKEN}

### _Request:_

| Parameters      | Type                | Required | Default  | Description       |
| --------------- | --------------------| -------- | ---------| ----------------- |
| booking_id      | integer             | true     | null     | voucher id        |


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

| Parameters      | Type            | Description                  |
| --------------- |:---------------:| -----------------------------|
| code            | integer         | http response code value     |
| message         | string          | error description            |

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

| Parameters      | Type            | Description                  |
| --------------- |:---------------:| -----------------------------|
| code            | integer         | http response code value     |
| message         | string          | error description            |

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

| Parameters      | Type            | Description                  |
| --------------- |:---------------:| -----------------------------|
| code            | integer         | http response code value     |
| message         | string          | error description            |

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

This method allows the modification of a reservation. The **booking_id** is obtained in the _new reservation_ endpoint response. The *product* and *token* parameters are obtained through the endpoint _list products_. It is also necessary to search if the parking requires additional fields through the endpoint _get parking_ (*fieldsRequested*). The process behind, delete the old reservation and create new one. The response shows the new reservation as well as the price difference between both reservations to proceed with the refund or charge.

### _Header:_
Authorization: Bearer {JWT_TOKEN}

### _Request:_

| Parameters      | Type                  | Required | Default          | Description             |
| --------------- |:--------------------- | -------- | ---------------- | ----------------------- |
| token           | string                | true     | null             | checkout token          |
| product         | string                | true     | null             | user email              |
| from            | date Y-m-d H:i        | true     | null             | booking start date      |
| to              | date Y-m-d H:i        | true     | null             | booking end date        |
| locale          | string                | true     | null             | language in which the information will be returned |


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

| Parameters      | Type            | Description                  |
| --------------- |:---------------:| -----------------------------|
| code            | integer         | http response code value     |
| message         | string          | error description            |

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

| Parameters      | Type                  | Required | Default          | Description             |
| --------------- |:--------------------- | -------- | ---------------- | ----------------------- |
| group           | string                | true     | null             | **list** or **booking** |
| email           | string                | false    | null             | user email              |
| voucher_code    | string                | false    | null             | voucher code            |
| from            | date Y-m-d H:i        | false    | null             | booking start date      |
| to              | date Y-m-d H:i        | false    | null             | booking end date        |
| page            | integer               | false    | 1 .              | page selectec           |
| limit           | integer               | false    | 200              | results to show         |


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
            "id": 1055138,
            "createdAt": "2019-11-18T07:40:41+0000",
            "end_booking_date": "2019-12-31T12:00:00+0000",
            "start_booking_date": "2019-12-31T10:00:00+0000",
            "total": 35,
            "booking_code": "QEG233D",
            "first_name": "Miki",
            "last_name": "Joinup",
            "username": "joinup2@parclick.com",
            "voucher_id": 935463,
            "state": "CANCELED",
            "total_net_price": 29.17,
            "total_vat": 5.83
        },
        {
            "id": 1055139,
            "createdAt": "2019-11-18T07:53:36+0000",
            "end_booking_date": "2019-12-31T12:00:00+0000",
            "start_booking_date": "2019-12-31T10:00:00+0000",
            "total": 35,
            "booking_code": "V49YDDK",
            "first_name": "Miki",
            "last_name": "Joinup",
            "username": "joinup2@parclick.com",
            "voucher_id": 935464,
            "state": "CANCELED",
            "total_net_price": 29.17,
            "total_vat": 5.83
        },
        {
            "id": 1055140,
            "createdAt": "2019-11-18T14:42:38+0000",
            "end_booking_date": "2019-12-31T12:00:00+0000",
            "start_booking_date": "2019-12-31T10:00:00+0000",
            "total": 35,
            "booking_code": "LGDWPP4",
            "first_name": "Miki",
            "last_name": "Joinup",
            "username": "joinup2@parclick.com",
            "voucher_id": 935465,
            "state": "CANCELED",
            "total_net_price": 29.17,
            "total_vat": 5.83
        },
        {
            "id": 1055141,
            "createdAt": "2019-11-18T14:46:36+0000",
            "end_booking_date": "2019-12-31T12:00:00+0000",
            "start_booking_date": "2019-12-31T10:00:00+0000",
            "total": 35,
            "booking_code": "V8K2WWJ",
            "first_name": "Miki",
            "last_name": "Joinup",
            "username": "joinup2@parclick.com",
            "voucher_id": 935466,
            "state": "CANCELED",
            "total_net_price": 29.17,
            "total_vat": 5.83
        },
        {
            "id": 1055142,
            "createdAt": "2019-11-18T14:52:29+0000",
            "end_booking_date": "2019-12-31T12:00:00+0000",
            "start_booking_date": "2019-12-31T10:00:00+0000",
            "total": 35,
            "booking_code": "Q2Z3XDG",
            "first_name": "Miki",
            "last_name": "Joinup",
            "username": "joinup2@parclick.com",
            "voucher_id": 935467,
            "state": "CANCELED",
            "total_net_price": 29.17,
            "total_vat": 5.83
        },
        {
            "id": 1055143,
            "createdAt": "2019-11-19T09:54:38+0000",
            "end_booking_date": "2019-12-31T22:00:00+0000",
            "start_booking_date": "2019-12-31T10:00:00+0000",
            "total": 17.2,
            "booking_code": "LY4YGP0",
            "first_name": "Miki",
            "last_name": "Joinup",
            "username": "joinup2@parclick.com",
            "voucher_id": 935468,
            "state": "CANCELED",
            "total_net_price": 14.1,
            "total_vat": 3.1
        },
        {
            "id": 1055144,
            "createdAt": "2019-11-19T13:17:45+0000",
            "end_booking_date": "2019-12-31T22:00:00+0000",
            "start_booking_date": "2019-12-31T10:00:00+0000",
            "total": 17.2,
            "booking_code": "VME5G1X",
            "first_name": "Miki",
            "last_name": "Joinup",
            "username": "joinup2@parclick.com",
            "voucher_id": 935469,
            "state": "CANCELED",
            "total_net_price": 14.1,
            "total_vat": 3.1
        },
        {
            "id": 1055145,
            "createdAt": "2019-11-20T11:40:52+0000",
            "end_booking_date": "2019-12-31T22:00:00+0000",
            "start_booking_date": "2019-12-31T10:00:00+0000",
            "total": 17.2,
            "booking_code": "LWPMG3P",
            "first_name": "Miki",
            "last_name": "Joinup",
            "username": "joinup2@parclick.com",
            "voucher_id": 935470,
            "state": "CANCELED",
            "total_net_price": 14.1,
            "total_vat": 3.1
        },
        {
            "id": 1055146,
            "createdAt": "2019-11-20T14:11:41+0000",
            "end_booking_date": "2019-12-31T22:00:00+0000",
            "start_booking_date": "2019-12-31T10:00:00+0000",
            "total": 42.29,
            "booking_code": "LJK1GWN",
            "first_name": "Miki",
            "last_name": "Joinup",
            "username": "joinup2@parclick.com",
            "voucher_id": 935471,
            "state": "CANCELED",
            "total_net_price": 35.24,
            "total_vat": 7.05
        }
    ],
    "total": 10,
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
    "page": 1,
    "limit": 200,
    "pages": 1,
    "items": [
        {
            "id": 974973,
            "createdAt": "2019-05-30T11:53:17+0000",
            "end_booking_date": "2019-05-31T12:00:00+0000",
            "start_booking_date": "2019-05-31T10:00:00+0000",
            "total": 19.47,
            "booking_code": "QR23R45",
            "first_name": "miki0",
            "last_name": "mikimoto0",
	    "voucher_id": 935441,
            "username": "TEST_IT\uf8ffmikimoto0@parclick.com\uf8ffTEST_IT",
            "items": [
                {
                    "id": 976152,
                    "parking": {
                        "id": 113,
                        "latitude": 36.53671717596,
                        "image_list": "\/\/static.parclick.com\/parking\/2016\/06\/parking-188-large.png",
                        "longitude": -6.3022649907516,
                        "is_cancellable": true,
                        "address": "Paseo Santa B\u00e1rbara, S\/N",
                        "city": "C\u00e1diz",
                        "country": "Espa\u00f1a",
                        "zip": "11003",
                        "name": "IC Santa B\u00e1rbara",
                        "freemium": false,
                        "cancellation_type": 2,
                        "_links": {
                            "self": {
                                "href": "https:\/\/loc.api.parclick.com\/v1\/parking\/113"
                            }
                        }
                    },
                    "product": {
                        "id": 1586,
                        "instructions": "During the purchasing process, select the date you plan to arrive. After making the online payment you will receive a voucher via email with you reservation code. \r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003EOn the date of your reservation, enter the parking lot as usual, take a ticket at the entrance and park in any empty spot.\r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003EOnce you\u0027re out of the car, approach the control booth with the Parclick voucher and the ticket you took. Our staff there will check your reservation using the reservation code, and will give you a card that will allow you out.",
                        "name": "ONEPASS 1 day",
                        "vehicle_type": {
                            "id": 1
                        },
                        "warning_message": "\u003Cp\u003EAlthough the car park is open 24 hours, you must redeem the receipt within Customer Service Hours from Monday to Sunday from 7h to 23h.\u003C\/p\u003E",
                        "list_price": 18.85,
                        "price": 17,
                        "type": "pass",
                        "_links": {
                            "self": [
                                {
                                    "href": "https:\/\/loc.api.parclick.com\/v1\/product\/1586"
                                },
                                {
                                    "href": "https:\/\/loc.api.parclick.com\/v1\/pass\/1586"
                                }
                            ]
                        }
                    }
                }
            ],
            "fees": [
                {
                    "id": 687791,
                    "net_price": 2.04,
                    "vat": 0.43,
                    "total": 2.47,
                    "discriminator": "administration_fee"
                }
            ],
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
            "first_name": "miki1",
            "last_name": "mikimoto1",
	    "voucher_id": 935442,
            "username": "TEST_IT\uf8ffmikimoto10@parclick.com\uf8ffTEST_IT",
            "items": [
                {
                    "id": 976177,
                    "parking": {
                        "id": 113,
                        "latitude": 36.53671717596,
                        "image_list": "\/\/static.parclick.com\/parking\/2016\/06\/parking-188-large.png",
                        "longitude": -6.3022649907516,
                        "is_cancellable": true,
                        "address": "Paseo Santa B\u00e1rbara, S\/N",
                        "city": "C\u00e1diz",
                        "country": "Espa\u00f1a",
                        "zip": "11003",
                        "name": "IC Santa B\u00e1rbara",
                        "freemium": false,
                        "cancellation_type": 2,
                        "_links": {
                            "self": {
                                "href": "https:\/\/loc.api.parclick.com\/v1\/parking\/113"
                            }
                        }
                    },
                    "product": {
                        "id": 1586,
                        "instructions": "During the purchasing process, select the date you plan to arrive. After making the online payment you will receive a voucher via email with you reservation code. \r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003EOn the date of your reservation, enter the parking lot as usual, take a ticket at the entrance and park in any empty spot.\r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003EOnce you\u0027re out of the car, approach the control booth with the Parclick voucher and the ticket you took. Our staff there will check your reservation using the reservation code, and will give you a card that will allow you out.",
                        "name": "ONEPASS 1 day",
                        "vehicle_type": {
                            "id": 1
                        },
                        "warning_message": "\u003Cp\u003EAlthough the car park is open 24 hours, you must redeem the receipt within Customer Service Hours from Monday to Sunday from 7h to 23h.\u003C\/p\u003E",
                        "list_price": 18.85,
                        "price": 17,
                        "type": "pass",
                        "_links": {
                            "self": [
                                {
                                    "href": "https:\/\/loc.api.parclick.com\/v1\/product\/1586"
                                },
                                {
                                    "href": "https:\/\/loc.api.parclick.com\/v1\/pass\/1586"
                                }
                            ]
                        }
                    }
                }
            ],
            "fees": [
                {
                    "id": 687813,
                    "net_price": 2.04,
                    "vat": 0.43,
                    "total": 2.47,
                    "discriminator": "administration_fee"
                }
            ],
            "state": "CONFIRMED",
            "total_net_price": 16.09,
            "total_vat": 3.38
        },
        {
            "id": 979834,
            "createdAt": "2019-06-03T13:16:35+0000",
            "end_booking_date": "2019-06-30T12:00:00+0000",
            "start_booking_date": "2019-06-30T10:00:00+0000",
            "total": 19.47,
            "booking_code": "QXP26EW",
            "first_name": "miki00",
            "last_name": "mikimoto00",
	    "voucher_id": 935442,
            "username": "TEST_IT\uf8ffmikimoto00@parclick.com\uf8ffTEST_IT",
            "items": [
                {
                    "id": 981013,
                    "parking": {
                        "id": 113,
                        "latitude": 36.53671717596,
                        "image_list": "\/\/static.parclick.com\/parking\/2016\/06\/parking-188-large.png",
                        "longitude": -6.3022649907516,
                        "is_cancellable": true,
                        "address": "Paseo Santa B\u00e1rbara, S\/N",
                        "city": "C\u00e1diz",
                        "country": "Espa\u00f1a",
                        "zip": "11003",
                        "name": "IC Santa B\u00e1rbara",
                        "freemium": false,
                        "cancellation_type": 2,
                        "_links": {
                            "self": {
                                "href": "https:\/\/loc.api.parclick.com\/v1\/parking\/113"
                            }
                        }
                    },
                    "product": {
                        "id": 1586,
                        "instructions": "During the purchasing process, select the date you plan to arrive. After making the online payment you will receive a voucher via email with you reservation code. \r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003EOn the date of your reservation, enter the parking lot as usual, take a ticket at the entrance and park in any empty spot.\r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003EOnce you\u0027re out of the car, approach the control booth with the Parclick voucher and the ticket you took. Our staff there will check your reservation using the reservation code, and will give you a card that will allow you out.",
                        "name": "ONEPASS 1 day",
                        "vehicle_type": {
                            "id": 1
                        },
                        "warning_message": "\u003Cp\u003EAlthough the car park is open 24 hours, you must redeem the receipt within Customer Service Hours from Monday to Sunday from 7h to 23h.\u003C\/p\u003E",
                        "list_price": 18.85,
                        "price": 17,
                        "type": "pass",
                        "_links": {
                            "self": [
                                {
                                    "href": "https:\/\/loc.api.parclick.com\/v1\/product\/1586"
                                },
                                {
                                    "href": "https:\/\/loc.api.parclick.com\/v1\/pass\/1586"
                                }
                            ]
                        }
                    }
                }
            ],
            "fees": [
                {
                    "id": 691876,
                    "net_price": 2.04,
                    "vat": 0.43,
                    "total": 2.47,
                    "discriminator": "administration_fee"
                }
            ],
            "state": "CANCELED",
            "total_net_price": 16.09,
            "total_vat": 3.38
        },
        {
            "id": 1055102,
            "createdAt": "2019-10-30T09:57:38+0000",
            "end_booking_date": "2019-12-31T12:00:00+0000",
            "start_booking_date": "2019-12-31T10:00:00+0000",
            "total": 3,
            "booking_code": "L982J29",
            "first_name": "Miki",
            "last_name": "Test1",
	    "voucher_id": 935443,
            "username": "TEST_IT\uf8ffmiki.petrovic.rios@parclick.com\uf8ffTEST_IT",
            "items": [
                {
                    "id": 1056281,
                    "parking": {
                        "id": 1988,
                        "latitude": 40.404219150793,
                        "image_list": "\/\/static.parclick.com\/parking\/2018\/04\/452\/59f\/ee\/45259fee-65be-5489-b618-81095a4280c5.jpeg",
                        "longitude": -3.6981113117529,
                        "is_cancellable": true,
                        "address": "Calle de Fray Luis de Le\u00f3n, 11",
                        "city": "Madrid",
                        "country": "Spain",
                        "zip": "28012",
                        "name": "Fray Luis de Le\u00f3n - Atocha",
                        "freemium": false,
                        "cancellation_type": 2,
                        "_links": {
                            "self": {
                                "href": "https:\/\/loc.api.parclick.com\/v1\/parking\/1988"
                            }
                        }
                    },
                    "product": {
                        "id": 54332,
                        "instructions": "ARRIVAL:\r\n\u003Cbr\/\u003EPresent the QR code on your reservation (on your phone or a printed version) to the orange Parclick reader, that you will find next to the entrance barrier. The light will turn green and the barrier will open (in 3-4 seconds). Park in any of the identified spaces for Parclick customers:\r\n\u003Cbr\/\u003E- Outdoor spaces: no 3, 7, 8, 9, 11.\r\n\u003Cbr\/\u003E- Underground spaces (floor -2): no 24, 25, 26, 40, 41.\r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003EDEPARTURE:\r\n\u003Cbr\/\u003EShow your QR code to the reader. If the light turns red, contact the operator through the barrier interphone.\r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003EIF YOUR BOOKING ALLOWS UNLIMITED ENTRANCE AND EXIT:\r\n\u003Cbr\/\u003EFollow the same process, indicated before, to enter and exit.\r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003ECOURTESY TIME AND EXCEEDED TIME:\r\n\u003Cbr\/\u003EYou have 30 minutes\u2019 courtesy time to enter the car park before the time indicated on your booking and 30 minutes of courtesy time to leave after. If your stay exceeds the courtesy time, it will automatically charge you the difference on your card which you used to make your booking  (+0.38\u20ac \/ 15 mins).",
                        "name": "ONEPASS 2 hours",
                        "vehicle_type": {
                            "id": 1
                        },
                        "warning_message": "For the QR reader to work properly, you must have the latest version of the app installed.",
                        "price": 3,
                        "type": "pass",
                        "_links": {
                            "self": [
                                {
                                    "href": "https:\/\/loc.api.parclick.com\/v1\/product\/54332"
                                },
                                {
                                    "href": "https:\/\/loc.api.parclick.com\/v1\/pass\/54332"
                                }
                            ]
                        }
                    }
                }
            ],
            "fees": [],
            "state": "CANCELED",
            "total_net_price": 2.48,
            "total_vat": 0.52
        },
        {
            "id": 1055103,
            "createdAt": "2019-10-30T16:41:39+0000",
            "end_booking_date": "2019-12-31T12:00:00+0000",
            "start_booking_date": "2019-12-31T10:00:00+0000",
            "total": 3,
            "booking_code": "QXPWGW0",
            "first_name": "Miki",
            "last_name": "Test1",
	    "voucher_id": 935444,
            "username": "TEST_IT\uf8ffmiki.petrovic.rios@parclick.com\uf8ffTEST_IT",
            "items": [
                {
                    "id": 1056282,
                    "parking": {
                        "id": 1988,
                        "latitude": 40.404219150793,
                        "image_list": "\/\/static.parclick.com\/parking\/2018\/04\/452\/59f\/ee\/45259fee-65be-5489-b618-81095a4280c5.jpeg",
                        "longitude": -3.6981113117529,
                        "is_cancellable": true,
                        "address": "Calle de Fray Luis de Le\u00f3n, 11",
                        "city": "Madrid",
                        "country": "Spain",
                        "zip": "28012",
                        "name": "Fray Luis de Le\u00f3n - Atocha",
                        "freemium": false,
                        "cancellation_type": 2,
                        "_links": {
                            "self": {
                                "href": "https:\/\/loc.api.parclick.com\/v1\/parking\/1988"
                            }
                        }
                    },
                    "product": {
                        "id": 54332,
                        "instructions": "ARRIVAL:\r\n\u003Cbr\/\u003EPresent the QR code on your reservation (on your phone or a printed version) to the orange Parclick reader, that you will find next to the entrance barrier. The light will turn green and the barrier will open (in 3-4 seconds). Park in any of the identified spaces for Parclick customers:\r\n\u003Cbr\/\u003E- Outdoor spaces: no 3, 7, 8, 9, 11.\r\n\u003Cbr\/\u003E- Underground spaces (floor -2): no 24, 25, 26, 40, 41.\r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003EDEPARTURE:\r\n\u003Cbr\/\u003EShow your QR code to the reader. If the light turns red, contact the operator through the barrier interphone.\r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003EIF YOUR BOOKING ALLOWS UNLIMITED ENTRANCE AND EXIT:\r\n\u003Cbr\/\u003EFollow the same process, indicated before, to enter and exit.\r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003ECOURTESY TIME AND EXCEEDED TIME:\r\n\u003Cbr\/\u003EYou have 30 minutes\u2019 courtesy time to enter the car park before the time indicated on your booking and 30 minutes of courtesy time to leave after. If your stay exceeds the courtesy time, it will automatically charge you the difference on your card which you used to make your booking  (+0.38\u20ac \/ 15 mins).",
                        "name": "ONEPASS 2 hours",
                        "vehicle_type": {
                            "id": 1
                        },
                        "warning_message": "For the QR reader to work properly, you must have the latest version of the app installed.",
                        "price": 3,
                        "type": "pass",
                        "_links": {
                            "self": [
                                {
                                    "href": "https:\/\/loc.api.parclick.com\/v1\/product\/54332"
                                },
                                {
                                    "href": "https:\/\/loc.api.parclick.com\/v1\/pass\/54332"
                                }
                            ]
                        }
                    }
                }
            ],
            "fees": [],
            "state": "CANCELED",
            "total_net_price": 2.48,
            "total_vat": 0.52
        },
        {
            "id": 1055104,
            "createdAt": "2019-11-04T09:09:14+0000",
            "end_booking_date": "2019-12-31T12:00:00+0000",
            "start_booking_date": "2019-12-31T10:00:00+0000",
            "total": 3,
            "booking_code": "Q3XDYDJ",
            "first_name": "Miki",
            "last_name": "Test_pre",
	    "voucher_id": 935445,
            "username": "TEST_IT\uf8ffmiki.petrovic@parclick.com\uf8ffTEST_IT",
            "items": [
                {
                    "id": 1056283,
                    "parking": {
                        "id": 1988,
                        "latitude": 40.404219150793,
                        "image_list": "\/\/static.parclick.com\/parking\/2018\/04\/452\/59f\/ee\/45259fee-65be-5489-b618-81095a4280c5.jpeg",
                        "longitude": -3.6981113117529,
                        "is_cancellable": true,
                        "address": "Calle de Fray Luis de Le\u00f3n, 11",
                        "city": "Madrid",
                        "country": "Spain",
                        "zip": "28012",
                        "name": "Fray Luis de Le\u00f3n - Atocha",
                        "freemium": false,
                        "cancellation_type": 2,
                        "_links": {
                            "self": {
                                "href": "https:\/\/loc.api.parclick.com\/v1\/parking\/1988"
                            }
                        }
                    },
                    "product": {
                        "id": 54332,
                        "instructions": "ARRIVAL:\r\n\u003Cbr\/\u003EPresent the QR code on your reservation (on your phone or a printed version) to the orange Parclick reader, that you will find next to the entrance barrier. The light will turn green and the barrier will open (in 3-4 seconds). Park in any of the identified spaces for Parclick customers:\r\n\u003Cbr\/\u003E- Outdoor spaces: no 3, 7, 8, 9, 11.\r\n\u003Cbr\/\u003E- Underground spaces (floor -2): no 24, 25, 26, 40, 41.\r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003EDEPARTURE:\r\n\u003Cbr\/\u003EShow your QR code to the reader. If the light turns red, contact the operator through the barrier interphone.\r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003EIF YOUR BOOKING ALLOWS UNLIMITED ENTRANCE AND EXIT:\r\n\u003Cbr\/\u003EFollow the same process, indicated before, to enter and exit.\r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003ECOURTESY TIME AND EXCEEDED TIME:\r\n\u003Cbr\/\u003EYou have 30 minutes\u2019 courtesy time to enter the car park before the time indicated on your booking and 30 minutes of courtesy time to leave after. If your stay exceeds the courtesy time, it will automatically charge you the difference on your card which you used to make your booking  (+0.38\u20ac \/ 15 mins).",
                        "name": "ONEPASS 2 hours",
                        "vehicle_type": {
                            "id": 1
                        },
                        "warning_message": "For the QR reader to work properly, you must have the latest version of the app installed.",
                        "price": 3,
                        "type": "pass",
                        "_links": {
                            "self": [
                                {
                                    "href": "https:\/\/loc.api.parclick.com\/v1\/product\/54332"
                                },
                                {
                                    "href": "https:\/\/loc.api.parclick.com\/v1\/pass\/54332"
                                }
                            ]
                        }
                    }
                }
            ],
            "fees": [],
            "state": "CANCELED",
            "total_net_price": 2.48,
            "total_vat": 0.52
        }
    ],
    "total": 6,
    "params": {
        "group": "booking",
        "page": "1",
        "limit": "200"
    }
}
</pre>
</p>
</details>


<br>

**401 Unauthorized**

| Parameters      | Type            | Description                  |
| --------------- |:---------------:| -----------------------------|
| code            | integer         | http response code value     |
| message         | string          | error description            |

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

| Parameters      | Type                | Required | Default  | Description                                        |
| --------------- |:--------------------| ---------| ---------| -------------------------------------------------- |
| voucher_id      | integer             | true     | null     | voucher id                                         |
| locale          | string              | false    | es_ES    | language in which the information will be returned |


### _Response:_

**200 Ok**

| Parameters                | Type             | Description                                  |
| ------------------------- | ---------------- | -------------------------------------------- |
| user_first_name           | string           | user first name                              |
| user_last_name            | string           | user last name                               |
| product                   | string           | product description i.e. "MULTIPASS 2 horas" |
| product_instructions      | string           | product instructions                         |
| product_category          | string           | product category                             |
| product_warning           | string           | warning description                          | 
| product_net_price         | float            | product net price                            |
| product_vat_price         | float            | product vat price                            |
| product_vat_percent       | string           | product vat percent                          |
| product_gross_price       | float            | vat price                                    |
| voucher_code              | string           | voucher code                                 |
| voucher_id                | int .            | voucher id (id used to cancel a reservation) |
| voucher_description       | array - string   | voucher_description                          |
| booking_id                | int              | booking id                                   |
| booking_state             | string           | booking status (CONFIRMED, CANCELLED, PENDING)|
| booking_is_cancellable    | bool             | is this booking cancellable                  |
| booking_cancellation_type | int              | [1 cancel before 23:59, 2 cancel one hour left, 3 cancel disallowed] |
| booking_from              | string d/m/Y H:i | booking start date                           |
| booking_to                | string d/m/Y H:i | booking end date                             |
| total_net_price           | float            | total net price                              |
| total_vat                 | float            | total vat                                    |
| total                     | float            | total amount                                 |
| vehicle_type              | string           | type of vehicle you wish to park [1 car, 2 van, 3 caravan, 4 bus, 5 truck, 6 motorbike, 7 small truck]  |
| external_code             | string           | booking external code                        |
| external_code_type        | string           | external code type                           |
| external_code_tech        | string           | external code tech                           |
| qr_hash                   | string           | qr hash                                      |
| booking_created_at        | string           | booking creation date d/m/Y H:i              |
| extra_fields              | array            | all the extra fields filled in booking       |
| parking_id                | int              | parking id                                   |
| parking_name              | string           | parking name                                 |
| parking_address           | string           | parking address                              |
| parking_zip               | string           | parking zip                                  |
| parking_city              | string           | parking city                                 |
| parking_province          | string           | parking province                             |
| parking_country           | string           | parking country                              |
| parking_maximum_height    | string           | parking maximum height                       |
| parking_description       | string           | parking description                          |
| parking_instructions      | array            | parking instructions ()                      |
| parking_latitude          | float            | parking latitude                             |
| parking_longitude         | float            | parking longitude                            |
| parking_category          | array            | parking category [1 City Center, 2 Airport Official, 3 Train Station Official, 4 Port, 5 Hospital, 6 Coast, 7 Airport LowCost, 8 Airport Valet, 9 Train Station LowCost, 10 Train Station Valet]                            |
| parking_has_shuttle       | bool             | has shuttle service                          |
| parking_shuttle_schedule  | array            | schuttle schedule                            |
| parking_shuttle_frequency | string           | frequency between shuttles                   |
| parking_airport_terminals | array            | airport terminals                            |
| spot_name                 | string           | spot name                                    |
| is_box                    | bool             | parking use a box                            |
| fee                       | array            | array of fees                                |
| third_party_discriminator | string           | third party discriminator                    |
| currency                  | string           | currency code                                |
| has_voucher_notification  | bool             | include voucher in notification              |
| has_receipt_notification  | bool             | include receipt in notification              |
| locale                    | string           | selected locale                              |
| application_name          | string           | application name - parclick                  |
| host                      | string           | host - parclick.com                          |
| logo_image                | string           | logo image path                              |
| url_faqs                  | string           | FAQ's path                                   |
| emailt_contact            | string           | the contact email                            |
| from                      | string           | email from                                   |
| from_name                 | string           | email from name                              |
| to                        | string           | email to                                     |
| to_name                   | string           | email to name                                |
| cco                       | string           | email carbon copy                            |
| subject                   | string           | email subject                                | 

<details><summary style="color:#FF6600;">Show response 200 Ok</summary>
<p>
<pre>
{
    "data": {
        "user": {
            "user_first_name": "Miki",
            "user_last_name": "Joinup"
        },
        "product": {
            "product": "ONEPASS 1 d\u00eda",
            "product_instructions": "EL D\u00cdA ANTES DE LA SALIDA: Recibir\u00e1s un email y un SMS con el nombre y el n\u00famero de tel\u00e9fono de la persona que recoger\u00e1 tu veh\u00edculo en la estaci\u00f3n. En caso contrario, llama al n\u00famero que aparece en el voucher de Parclick una hora antes de la recogida del veh\u00edculo. Tienes que llamar desde el n\u00famero de tel\u00e9fono que hab\u00edas indicado durante tu reserva, para que te pongan en contacto directamente con el agente.\r\n\r\nA TU LLEGADA: Avisa por tel\u00e9fono 15 minutos antes de llegar a la estaci\u00f3n, donde te estar\u00e1n esperando para recoger tu veh\u00edculo. Antes de entregarle las llaves al conductor mu\u00e9strale el justificante de tu reserva de Parclick. \r\n \r\nEL D\u00cdA ANTES DE REGRESAR: Recibir\u00e1s un email y un SMS con el nombre y el n\u00famero de tel\u00e9fono de la persona que recoger\u00e1 tu veh\u00edculo en la estaci\u00f3n. \r\n \r\nA TU VUELTA: Avisa por tel\u00e9fono una vez hayas llegado. Te estar\u00e1n esperando con tu veh\u00edculo en el mismo punto de la estaci\u00f3n que a la llegada.",
            "product_category": "ONEPASS",
            "product_warning": "En este aparcamiento es necesario dejar las llaves del coche.\r\nEs importante llegar a la hora indicada en tu reserva, ya que nuestro personal estar\u00e1 all\u00ed esper\u00e1ndote para recibirte.\r\nPuedes contactar con el parking para cualquier duda o incidencia que te surja en el aparcamiento: +339 73 72 88 55 .",
            "product_net_price": 29.17,
            "product_vat_price": 5.83,
            "product_vat_percent": "20%",
            "product_gross_price": 35
        },
        "voucher": {
            "voucher_code": "Q572PPX",
            "voucher_id": 935445,
            "voucher_description": {
                "0": "EL D\u00cdA ANTES DE LA SALIDA: Recibir\u00e1s un email y un SMS con el nombre y el n\u00famero de tel\u00e9fono de la persona que recoger\u00e1 tu veh\u00edculo en la estaci\u00f3n. En caso contrario, llama al n\u00famero que aparece en el voucher de Parclick una hora antes de la recogida del veh\u00edculo. Tienes que llamar desde el n\u00famero de tel\u00e9fono que hab\u00edas indicado durante tu reserva, para que te pongan en contacto directamente con el agente.\r",
                "2": "A TU LLEGADA: Avisa por tel\u00e9fono 15 minutos antes de llegar a la estaci\u00f3n, donde te estar\u00e1n esperando para recoger tu veh\u00edculo. Antes de entregarle las llaves al conductor mu\u00e9strale el justificante de tu reserva de Parclick. \r",
                "4": "EL D\u00cdA ANTES DE REGRESAR: Recibir\u00e1s un email y un SMS con el nombre y el n\u00famero de tel\u00e9fono de la persona que recoger\u00e1 tu veh\u00edculo en la estaci\u00f3n. \r",
                "6": "A TU VUELTA: Avisa por tel\u00e9fono una vez hayas llegado. Te estar\u00e1n esperando con tu veh\u00edculo en el mismo punto de la estaci\u00f3n que a la llegada."
            }
        },
        "booking": {
            "booking_id": 1055120,
            "booking_state": "CONFIRMED",
            "booking_is_cancellable": true,
            "booking_cancellation_type": 1,
            "booking_from": "31\/12\/2019 10:00",
            "booking_to": "31\/12\/2019 12:00",
            "total_net_price": 29.17,
            "total_vat": 5.83,
            "total": 35,
            "vehicle_type": "CAR",
            "booking_created_at": "13\/11\/2019 11:31",
            "extra_fields": {
                "1": {
                    "label": "brand",
                    "value": "saab"
                },
                "2": {
                    "label": "model",
                    "value": "93"
                },
                "3": {
                    "label": "license_plate",
                    "value": "1788DZN"
                },
                "4": {
                    "label": "arrival_flight",
                    "value": "1667787"
                },
                "5": {
                    "label": "departure_flight",
                    "value": "666999"
                }
            }
        },
        "parking": {
            "parking_id": 1046,
            "parking_name": "A\u00e9roport Nantes ECTOR - Service Voiturier - Ext\u00e9rieur",
            "parking_phone": "+33 01 76 40 00 32",
            "parking_address": "A\u00e9roport Nantes Atlantique,",
            "parking_zip": "44340",
            "parking_city": "Nantes",
            "parking_province": "Loire-Atlantique",
            "parking_country": "FRANCE",
            "parking_maximum_height": "1.9 m.",
            "parking_description": "\u00bfBuscas un parking cerca del Aeropuerto de Nantes Atlantique? Entonces est\u00e1s de suerte... Conocemos uno a escasos metros que te propone un servicio de aparcacoches para aparcar en Bouguenais. Reserva tu plaza con el parking Valet A\u00e9roport Nantes ECTOR y deja que un aparcacoches conduzca tu veh\u00edculo a un parking vigilado cerca del aeropuerto de Nantes Atlantique. \r\n\r\nEl parking A\u00e9roport Nantes ECTOR dispone de un servicio valet, es decir, no tendr\u00e1s que conducir hasta el parking. Simplemente, dir\u00edgete a la terminal del Aeropuerto de Nantes Atlantique. El d\u00eda antes de tu salida, recibir\u00e1s un mensaje con los datos del operario a quien tendr\u00e1s que llamar con 15 minutos de antelaci\u00f3n para avisarle de tu llegada. All\u00ed, el conductor te estar\u00e1 esperando para recoger tu veh\u00edculo y llevarlo al aparcamiento. A tu regreso, va a pasar lo mismo: el personal de A\u00e9roport Nantes ECTOR te estar\u00e1 esperando en la terminal para entregarte, de nuevo, tu veh\u00edculo. Al fin y al cabo, \u00a1mereces que te traten como si fueras de la realeza aun cuando te vas de la ciudad del Castillo de los Duques de Breta\u00f1a!\r\n\r\nEl parking A\u00e9roport Nantes ECTOR te permite estacionar cerca del Aeropuerto Nantes Atlantique durante el tiempo que dure tu estancia. Dejar\u00e1s tu coche a un buen recaudo en este parking vigilado las 24h \u00a1Viaja sin tener que estar dando vueltas al aeropuerto buscando la entrada del parking gracias al servicio valet del parking A\u00e9roport Nantes ECTOR!",
            "parking_instructions": {
                "begin": "EL D\u00cdA ANTES DE LA SALIDA: Recibir\u00e1s un email y un SMS con el nombre y el n\u00famero de tel\u00e9fono de la persona que recoger\u00e1 tu veh\u00edculo en la terminal. En caso contrario, llama al n\u00famero que aparece en el voucher de Parclick una hora antes de la recogida del veh\u00edculo. Tienes que llamar desde el n\u00famero de tel\u00e9fono que hab\u00edas indicado durante tu reserva, para que te pongan en contacto directamente con el agente.",
                "return": "El parking sigue en todo momento el estado de tu vuelo, para garantizar la presencia de un agente sin que tengas que esperar si tu vuelo llega con antelaci\u00f3n o retraso.\r\n\r\nA tu vuelta, te dejar\u00e1n una botella de agua en tu veh\u00edculo.",
                "info": ""
            },
            "parking_latitude": 47.157590455483,
            "parking_longitude": -1.6011363273911,
            "parking_category": 2,
            "parking_shuttle_schedule": [],
            "parking_airport_terminals": [
                {
                    "id": 30
                }
            ],
            "spot_name": 100,
            "is_box": false
        },
        "fee": [],
        "third_party_discriminator": "JOINUP",
        "currency": "EUR",
        "has_voucher_notification": true,
        "has_receipt_notification": false
    },
    "email": {
        "from": "miki.petrovic@parclick.com",
        "from_name": "Parclick by JOINUP",
        "to": "hello@ectorparking.com",
        "to_name": "A\u00e9roport Nantes ECTOR - Service Voiturier - Ext\u00e9rieur",
        "subject": "Parclick by JOINUP Reservar Q572PPX | Parking A\u00e9roport Nantes ECTOR - Service Voiturier - Ext\u00e9rieur, ONEPASS"
    },
    "locale": "es_ES",
    "application_name": "Parclick",
    "host": "parclick.es",
    "logo_image": "https:\/\/s3-eu-west-1.amazonaws.com\/static.parclick.com\/assets\/img\/Logo-Parclick.png",
    "url_faqs": "https:\/\/parclick.com\/faqs",
    "email_contact": "parclick-info@parclick.com"
}
</pre>
</p>
</details>

<br>

**401 Unauthorized**

| Parameters      | Type            | Description                  |
| --------------- |:---------------:| -----------------------------|
| code            | integer         | http response code value     |
| message         | string          | error description            |

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

As a first step and part of the development by the integrator it is necessary to select the vehicles available **Vehicle type /v1/vehicle-type**, prior to select the list of car parks for certain coordinates and a specific type of vehicle and dates. For this task it is necessary to call the endpoint **Get parkings /v1/parking/** to get the parking id. With this data it is possible to show a map with the available car parks. Once a specific car park has been selected, it is necessary to obtain the best pass (product token `2338bb72051ae11083a20cd94f3b3183ede3333708b1bc7b50e6af509100ef14`) **list products /v1/pass** for the selected period and the additional fields for that car park **get parking /v1/parking/{parking_id}** located in response fieldsRequested array.

### <a name="workflow_reservation"></a><span style="color:#FF6600;"> 1- Workflow to create a booking reservation</span>


1. Get the parking id and timespan (from, to)
2. Get the required fields for the selected parking using the parking endpoint  `/v1/parking/{parking_id}/details?locale=en_GB&group=detail` In the response, the _embedded node display the required fields for the seleted parking. 
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


| Parameters      | Type                | Description                                                 |
| --------------- |:------------------- | ----------------------------------------------------------- |
| group           | string              | **list** to show details                                    |
| email           | string              | search by useremail in whole or in part                     |
| status          | string              | booking status available: not_modified, confirmed, canceled |
| firstName       | string              | string to find                                              |
| lastName        | string              | string to find                                              |
| from            | date YY-mm-dd       | booking start date                                          |
| to              | date YY-mm-dd       | booking end date                                            |

<br>

## <a name="entry_code"></a><span style="color:#FF6600;">Generate entry codes</span>

Some car parks require the generation of access codes (QR Code, Barcode...) to gain access. If the selected car park requires a QRCode, barcode or some kind of specific code, the _get voucher data_ endpoint response specifies, if necessary, the type of code in the **external\_code\_tech** field.  Parclick handles the following types of access codes:



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

<p style="font-size:11px;color:#999999;text-align:center;">2019 Copyright Parclick S.L. All rights reserved.</p>
