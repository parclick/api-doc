![alt text](https://static.parclick.com/assets/img/logotipo-parclick.svg)

<p style="color:#999999;text-align:right;">Document version 1.3.0</p>

# <span style="color:#FF6600;">Parclick - API Reference</span>
#### <span style="color:#999;">TECHNICAL DOCUMENTATION</span>



### <span style="color:#FF6600;">Content table</span>

1. [API description](#description)
2. [Login](#login)
3. [Refresh token](#refresh)
4. [List parkings](#list_parkings)
5. [Get parking](#get_parking)
6. [List products](#list_products)
7. [Vehicle type](#vehicle_type)
8. [Create reservation](#create_reservation)
9. [Cancel reservation](#cancel_reservation)
10. [List reservations](#list_reservation)
11. [Get voucher details](#voucher_details)
11. [Worflows](#workflows)
 	- [Workflow to create a booking reservation](#workflow_reservation)
	- [Workflow to cancel a booking](#workflow_cancel)
	- [Workflow to list booking](#workflow_list)
12. [Generate entry code](#entry_code)

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

### _Header:_
Authorization: Bearer {JWT_TOKEN}

### _Request:_

| Parameters      | Type                   | Required | Description                                                                       | Default             |
| --------------- |:---------------------- | -------- | --------------------------------------------------------------------------------- |-------------------- |
| locale          | string                 | true     | language in which the information will be returned                                | en_GB               |
| group           | string                 | true     | **search**                                                                        | null                |
| limit           | integer                | true     | total number of records [1-200]                                                   | 200                 |
| from            | date yyyy-MM-dd HH:mm  | true     | booking start date                                                                | null                |
| to              | date yyyy-MM-dd HH:mm  | true     | booking end date                                                                  | null                |
| latitude        | float                  | true     | valid latitude in which to look for                                               | null                |
| longitude       | float                  | true     | valid longitude in which to look for                                              | null                |
| radius          | integer                | false    | search radius                                                                     | null                |
| vehicleType     | integer                | true     | type of vehicle you wish to park [1 car, 2 van, 3 caravan, 4 bus, 5 truck, 6 motorbike, 7 small truck]| null                |
| freemium        | bool                   | false    | car parks where reservations cannot be booked                                     | false                |



### _Response:_

**200 Ok**

<details><summary style="color:#FF6600;">Show response 200 Ok</summary>
<p>
<pre>
{
    "page": 1,
    "limit": 10,
    "pages": 4,
    "items": [
        {
            "id": 263,
            "covered": true,
            "flexible_entry": false,
            "guarded": true,
            "latitude": 40.413240823385,
            "image_list": "//static.parclick.com/parking/2017/07/f72/be4/6b/f72be46b-bca8-53d0-b054-4ca8bd4f0f2b.jpeg",
            "longitude": -3.7330859504143,
            "image": {
                "id": 542,
                "extension": "jpg"
            },
            "is_cancellable": true,
            "address": "Avda. de Portugal, 51, Madrid",
            "city": "Madrid",
            "country": "España",
            "province": "Madrid",
            "zip": "28011",
            "provider": {
                "id": 19,
                "name": "EMT"
            },
            "handicapped_access": true,
            "security": true,
            "open_24h": true,
            "giving_keys": false,
            "exclude_checking": false,
            "name": "EMT Avenida de Portugal",
            "max_height": 210,
            "passes": [
                {
                    "id": 45412,
                    "name": "ONEPASS 12 hours",
                    "vehicle_type": {
                        "id": 1,
                        "type": "CAR",
                        "max_height": 190,
                        "max_length": 500
                    },
                    "warning_message": "You must enter through an entrance, which says \"Parking Público\" and not \"Residentes\". Before exiting please get to the control cabin with your reservation locator to get an exit ticket from the clerk on duty",
                    "price": 5,
                    "type": "pass",
                    "internal_name": "ONEPASS 12h (5.00€)",
                    "duration": 12,
                    "multiparking": false,
                    "multipass": false,
                    "frequency": "HOURLY",
                    "_links": {
                        "self": [
                            {
                                "href": "https://loc.api.parclick.com/v1/product/45412"
                            },
                            {
                                "href": "https://loc.api.parclick.com/v1/pass/45412"
                            }
                        ]
                    }
                }
            ],
            "subscriptions": [
                {
                    "id": 1527,
                    "name": "Weekend Parking Pass",
                    "vehicle_type": {
                        "id": 1,
                        "type": "CAR",
                        "max_height": 190,
                        "max_length": 500
                    },
                    "warning_message": "This product is valid only for the Parking A Rosa.",
                    "price": 25,
                    "type": "subscription",
                    "subscription_type": "MONTHLY",
                    "duration": 12,
                    "_links": {
                        "self": {
                            "href": "https://loc.api.parclick.com/v1/product/1527"
                        }
                    }
                },
                {
                    "id": 1970,
                    "name": "Weekend Parking Pass",
                    "vehicle_type": {
                        "id": 1,
                        "type": "CAR",
                        "max_height": 190,
                        "max_length": 500
                    },
                    "warning_message": "Before exiting please get to the control cabin with your reservation locator to get an exit ticket from the clerk on duty",
                    "price": 25,
                    "type": "subscription",
                    "subscription_type": "MONTHLY",
                    "duration": 12,
                    "_links": {
                        "self": {
                            "href": "https://loc.api.parclick.com/v1/product/1970"
                        }
                    }
                },
                {
                    "id": 69967,
                    "name": "PASS 31 DÍAS",
                    "vehicle_type": {
                        "id": 1,
                        "type": "CAR",
                        "max_height": 190,
                        "max_length": 500
                    },
                    "warning_message": "You must enter through an entrance, which says \"Parking Público\" and not \"Residentes\". Before exiting please get to the control cabin with your reservation locator to get an exit ticket from the clerk on duty",
                    "price": 139,
                    "type": "subscription",
                    "subscription_type": "MONTHLY",
                    "duration": 12,
                    "_links": {
                        "self": {
                            "href": "https://loc.api.parclick.com/v1/product/69967"
                        }
                    }
                }
            ],
            "access": [
                {
                    "id": 1769,
                    "type": "vehicle",
                    "latitude": 40.413240823385,
                    "longitude": -3.7330859504143
                }
            ],
            "slug": "mm_avenida_de_portugal",
            "multiparking": true,
            "reviews_summary": {
                "reviews": {
                    "96": {
                        "review": [],
                        "average": 3.3333333333333335
                    },
                    "105": {
                        "review": [],
                        "opinion": "Cada vez que quería salir del pk tenía que pasar por ventanilla para que me validaran el ticket... y eso era un poco engorroso. Por lo demás todo perfecto.",
                        "average": 4.333333333333333
                    },
                    "145": {
                        "review": [],
                        "opinion": "Espacio, amabilidad y eficacia del personal. Accesibilidad entradas y salidas, multiples",
                        "average": 4.666666666666667
                    },
                    "249": {
                        "review": [],
                        "opinion": "Parking amplio. Personal amable. Recomendado.",
                        "average": 5
                    },
                    "1231": {
                        "review": [],
                        "average": 3
                    },
                    "1532": {
                        "review": [],
                        "opinion": "el sistema de acudir a la garita, y tienen las fichas en papel, mientras buscan reserva y llaman para comprobar es muy lento.",
                        "average": 4.333333333333333
                    },
                    "1668": {
                        "review": [],
                        "opinion": "La entrada al parkin que yo cogi estaba super alejada de la cabina,para la proxima no me pasa!",
                        "average": 5
                    },
                    "1975": {
                        "review": [],
                        "average": 5
                    },
                    "2206": {
                        "review": [],
                        "average": 4.333333333333333
                    },
                    "2214": {
                        "review": [],
                        "opinion": "Puede mejorar el precio. Más barato, mucho más lleno",
                        "average": 5
                    },
                    "2869": {
                        "review": [],
                        "average": 5
                    },
                    "2918": {
                        "review": [],
                        "average": 4.666666666666667
                    },
                    "3176": {
                        "review": [],
                        "average": 5
                    },
                    "3240": {
                        "review": [],
                        "opinion": "en los tiempos actuales el sistema de entrada y salida esta obsoleto. no tiene sentido tener que validar el tiquet cuando entras o sales del parking. al menos realizada la entrada el ticket debería de habilitar para entrar o salir hasta la finalización",
                        "average": 3.6666666666666665
                    },
                    "3670": {
                        "review": [],
                        "average": 4
                    },
                    "3789": {
                        "review": [],
                        "opinion": "Parking enorme, muchísimas plazas disponibles. Buenas instalaciones, limpio, y sobre todo, barato y bien comunicado con línea de bus.",
                        "average": 5
                    },
                    "4394": {
                        "review": [],
                        "opinion": "el parking es muy largo con bastantes salidas a la calle, pero sólo tiene cabina de pago en la última salida a la calle con lo cual si entras desde la calle por una salida que está en medio del parking tienes que andar muchísimo, añadiría más cabinas",
                        "average": 2.6666666666666665
                    },
                    "4571": {
                        "review": [],
                        "opinion": "Rápido, los empleados muy agradables, muy limplio, plazas grandes... muy bueno. Eso sí, aconsejo aparcar lo más arriba posible, que es donde está la cabina, y además la parada Alto de Extremadura está mucho más cerca desde ahí que Puerta del Ángel.",
                        "average": 5
                    },
                    "4933": {
                        "review": [],
                        "average": 5
                    },
                    "5071": {
                        "review": [],
                        "average": 4.333333333333333
                    },
                    "5184": {
                        "review": [],
                        "average": 3.6666666666666665
                    },
                    "5316": {
                        "review": [],
                        "average": 4.666666666666667
                    },
                    "5338": {
                        "review": [],
                        "average": 3
                    },
                    "5367": {
                        "review": [],
                        "average": 5
                    }
                },
                "questions": {
                    "4": {
                        "score": 101,
                        "number": 24,
                        "average": 4.208333333333333
                    },
                    "5": {
                        "score": 106,
                        "number": 24,
                        "average": 4.416666666666667
                    },
                    "6": {
                        "score": 107,
                        "number": 24,
                        "average": 4.458333333333333
                    }
                },
                "totalScore": 4.361111111111112
            },
            "_links": {
                "self": {
                    "href": "https://loc.api.parclick.com/v1/parking/263"
                }
            },
            "_embedded": {
                "city": {
                    "id": 37,
                    "name": "Madrid",
                    "slug": "madrid",
                    "_links": {
                        "self": {
                            "href": "https://loc.api.parclick.com/v1/city/37"
                        }
                    }
                }
            }
        },
        {
            "id": 955,
            "covered": true,
            "flexible_entry": true,
            "guarded": true,
            "latitude": 40.390566050823,
            "image_list": "//static.parclick.com/parking/2016/06/parking-2041-large.jpg",
            "longitude": -3.7439535991564,
            "image": {
                "id": 2041,
                "extension": "jpg"
            },
            "is_cancellable": true,
            "address": "Calle Carlos Domingo, 5",
            "city": "Madrid",
            "country": "España",
            "province": "Madrid",
            "zip": "28047",
            "provider": {
                "id": 81,
                "name": "DM - Grupo Bolton"
            },
            "handicapped_access": true,
            "security": true,
            "open_24h": true,
            "giving_keys": true,
            "exclude_checking": false,
            "name": "DM Gómez Ulla",
            "max_height": 200,
            "passes": [
                {
                    "id": 39836,
                    "name": "MULTIPASS 3 hours",
                    "vehicle_type": {
                        "id": 1,
                        "type": "CAR",
                        "max_height": 190,
                        "max_length": 500
                    },
                    "warning_message": "At this car park, you must leave your car keys with us.",
                    "price": 3,
                    "type": "pass",
                    "internal_name": "MULTIPASS 3h (3.00€)",
                    "duration": 3,
                    "multiparking": false,
                    "multipass": true,
                    "frequency": "HOURLY",
                    "_links": {
                        "self": [
                            {
                                "href": "https://loc.api.parclick.com/v1/product/39836"
                            },
                            {
                                "href": "https://loc.api.parclick.com/v1/pass/39836"
                            }
                        ]
                    }
                }
            ],
            "subscriptions": [],
            "access": [
                {
                    "id": 743,
                    "type": "vehicle",
                    "latitude": 40.390566050823,
                    "longitude": -3.7439535991564
                }
            ],
            "slug": "dm_gomez_ulla",
            "multiparking": false,
            "reviews_summary": {
                "reviews": {
                    "2299": {
                        "review": [],
                        "average": 4
                    },
                    "2382": {
                        "review": [],
                        "opinion": "lo que más me importa es que estuviera cerca de mi destino y así era. es un parking de barrio. normal.",
                        "average": 4
                    },
                    "2535": {
                        "review": [],
                        "opinion": "fácil llegar y el personal amable y profesional.",
                        "average": 4.666666666666667
                    },
                    "5155": {
                        "review": [],
                        "average": 5
                    }
                },
                "questions": {
                    "4": {
                        "score": 18,
                        "number": 4,
                        "average": 4.5
                    },
                    "5": {
                        "score": 16,
                        "number": 4,
                        "average": 4
                    },
                    "6": {
                        "score": 19,
                        "number": 4,
                        "average": 4.75
                    }
                },
                "totalScore": 4.416666666666667
            },
            "_links": {
                "self": {
                    "href": "https://loc.api.parclick.com/v1/parking/955"
                }
            },
            "_embedded": {
                "city": {
                    "id": 37,
                    "name": "Madrid",
                    "slug": "madrid",
                    "_links": {
                        "self": {
                            "href": "https://loc.api.parclick.com/v1/city/37"
                        }
                    }
                }
            }
        },
        {
            "id": 441,
            "covered": true,
            "flexible_entry": true,
            "guarded": true,
            "latitude": 40.386205849278,
            "image_list": "//static.parclick.com/parking/2016/06/parking-851-large.jpg",
            "longitude": -3.7415589299796,
            "image": {
                "id": 851,
                "extension": "jpg"
            },
            "is_cancellable": true,
            "address": "Calle Batalla de Torrijos 4",
            "city": "Madrid",
            "country": "España",
            "province": "Madrid",
            "zip": "28025",
            "provider": {
                "id": 83,
                "name": "La Madrileña"
            },
            "handicapped_access": true,
            "security": true,
            "open_24h": true,
            "giving_keys": true,
            "exclude_checking": false,
            "name": "La Madrileña",
            "max_height": 425,
            "passes": [
                {
                    "id": 5420,
                    "name": "MULTIPASS 1 day (24 hours) - CARS",
                    "vehicle_type": {
                        "id": 1,
                        "type": "CAR",
                        "max_height": 190,
                        "max_length": 500
                    },
                    "warning_message": "At this car park, you must leave your car keys with us.",
                    "price": 15,
                    "type": "pass",
                    "internal_name": "MULTIPASS 1d (15.00€)",
                    "duration": 24,
                    "multiparking": false,
                    "multipass": true,
                    "frequency": "HOURLY",
                    "_links": {
                        "self": [
                            {
                                "href": "https://loc.api.parclick.com/v1/product/5420"
                            },
                            {
                                "href": "https://loc.api.parclick.com/v1/pass/5420"
                            }
                        ]
                    }
                }
            ],
            "subscriptions": [],
            "access": [
                {
                    "id": 572,
                    "type": "vehicle",
                    "latitude": 40.386205849278,
                    "longitude": -3.7415589299796
                }
            ],
            "slug": "la_madrilena",
            "multiparking": false,
            "reviews_summary": {
                "reviews": {
                    "203": {
                        "review": [],
                        "opinion": "Parking low cost. Hay que dejar las llaves por si el empleado tiene que moverlo. Aparcamiento tipo tetris. Al recoger el coche estaba bien (sin  roces), y los mismos kilómetros. Recomendable hacer unas fotos del coche y cuentakilómetros al dejarlo.",
                        "average": 3.3333333333333335
                    },
                    "4006": {
                        "review": [],
                        "average": 2
                    }
                },
                "questions": {
                    "4": {
                        "score": 6,
                        "number": 2,
                        "average": 3
                    },
                    "5": {
                        "score": 5,
                        "number": 2,
                        "average": 2.5
                    },
                    "6": {
                        "score": 5,
                        "number": 2,
                        "average": 2.5
                    }
                },
                "totalScore": 2.666666666666667
            },
            "_links": {
                "self": {
                    "href": "https://loc.api.parclick.com/v1/parking/441"
                }
            },
            "_embedded": {
                "city": {
                    "id": 37,
                    "name": "Madrid",
                    "slug": "madrid",
                    "_links": {
                        "self": {
                            "href": "https://loc.api.parclick.com/v1/city/37"
                        }
                    }
                }
            }
        },
        {
            "id": 925,
            "covered": true,
            "flexible_entry": true,
            "guarded": true,
            "latitude": 40.408647065765,
            "image_list": "//static.parclick.com/parking/2017/07/3ad/887/f8/3ad887f8-ab01-5df1-9d18-177a7d646282.jpeg",
            "longitude": -3.7147103357474,
            "image": {
                "id": 2010,
                "extension": "jpg"
            },
            "is_cancellable": true,
            "address": "Calle de la Ventosa",
            "city": "Madrid",
            "country": "España",
            "province": "Madrid",
            "zip": "28005",
            "provider": {
                "id": 230,
                "name": "PROMOPARC"
            },
            "handicapped_access": true,
            "security": true,
            "open_24h": false,
            "giving_keys": false,
            "exclude_checking": false,
            "name": "PROMOPARC Hospital VOT",
            "max_height": 210,
            "passes": [
                {
                    "id": 24822,
                    "name": "ONEPASS 3 horas",
                    "vehicle_type": {
                        "id": 1,
                        "type": "CAR",
                        "max_height": 190,
                        "max_length": 500
                    },
                    "warning_message": "",
                    "price": 5.5,
                    "type": "pass",
                    "internal_name": "ONEPASS 3h (5.50€)",
                    "duration": 3,
                    "multiparking": false,
                    "multipass": false,
                    "frequency": "HOURLY",
                    "_links": {
                        "self": [
                            {
                                "href": "https://loc.api.parclick.com/v1/product/24822"
                            },
                            {
                                "href": "https://loc.api.parclick.com/v1/pass/24822"
                            }
                        ]
                    }
                }
            ],
            "subscriptions": [
                {
                    "id": 21791,
                    "name": "YOUR PARKING SPACE Ticket - 24h",
                    "vehicle_type": {
                        "id": 1,
                        "type": "CAR",
                        "max_height": 190,
                        "max_length": 500
                    },
                    "warning_message": "",
                    "price": 125,
                    "type": "subscription",
                    "subscription_type": "MONTHLY",
                    "duration": 12,
                    "_links": {
                        "self": {
                            "href": "https://loc.api.parclick.com/v1/product/21791"
                        }
                    }
                },
                {
                    "id": 21797,
                    "name": "YOUR PARKING SPACE Ticket - 24h",
                    "vehicle_type": {
                        "id": 6,
                        "type": "MOTORBIKE",
                        "max_length": 220
                    },
                    "warning_message": "",
                    "price": 50,
                    "type": "subscription",
                    "subscription_type": "MONTHLY",
                    "duration": 12,
                    "_links": {
                        "self": {
                            "href": "https://loc.api.parclick.com/v1/product/21797"
                        }
                    }
                }
            ],
            "access": [
                {
                    "id": 724,
                    "type": "vehicle",
                    "latitude": 40.408647065765,
                    "longitude": -3.7147103357474
                }
            ],
            "slug": "promoparc_hospital_vot",
            "multiparking": false,
            "reviews_summary": {
                "reviews": {
                    "326": {
                        "review": [],
                        "opinion": "Parking muy limpio, no es necesario dejar las llaves y el personal es muy amable.",
                        "average": 5
                    },
                    "1454": {
                        "review": [],
                        "average": 4
                    },
                    "1866": {
                        "review": [],
                        "opinion": "cerca, limpio pero no había nadie en la garita ni para entrar ni para salir, seguí las instrucciones y llamé pero no válido la tarjeta así q para sacar el coche tuve q llamar otra vez. aparte de eso todo bien",
                        "average": 4.666666666666667
                    },
                    "2544": {
                        "review": [],
                        "average": 4
                    },
                    "2615": {
                        "review": [],
                        "opinion": "The car park in itself is well maintained. Big space for a van to park. On the contrary, the man who checked us in at the time we arrived for our reservation failed to tell us that there will be nobody on hand the day we leave the car park on a saturday a",
                        "average": 2.6666666666666665
                    },
                    "3140": {
                        "review": [],
                        "average": 4.333333333333333
                    },
                    "4076": {
                        "review": [],
                        "opinion": "Todo perfecto y a buen precio",
                        "average": 5
                    },
                    "4253": {
                        "review": [],
                        "average": 5
                    },
                    "4500": {
                        "review": [],
                        "average": 4
                    },
                    "4659": {
                        "review": [],
                        "opinion": "Una buena opción para aparcar en Madrid a un precio asequible. El parking es seguro y el personal es atento,",
                        "average": 4.666666666666667
                    },
                    "5270": {
                        "review": [],
                        "average": 4.666666666666667
                    },
                    "5277": {
                        "review": [],
                        "average": 4.666666666666667
                    }
                },
                "questions": {
                    "4": {
                        "score": 53,
                        "number": 12,
                        "average": 4.416666666666667
                    },
                    "5": {
                        "score": 54,
                        "number": 12,
                        "average": 4.5
                    },
                    "6": {
                        "score": 51,
                        "number": 12,
                        "average": 4.25
                    }
                },
                "totalScore": 4.388888888888888
            },
            "_links": {
                "self": {
                    "href": "https://loc.api.parclick.com/v1/parking/925"
                }
            },
            "_embedded": {
                "city": {
                    "id": 37,
                    "name": "Madrid",
                    "slug": "madrid",
                    "_links": {
                        "self": {
                            "href": "https://loc.api.parclick.com/v1/city/37"
                        }
                    }
                }
            }
        },
        {
            "id": 444,
            "covered": false,
            "flexible_entry": false,
            "guarded": false,
            "latitude": 40.4087362,
            "image_list": "//static.parclick.com/parking/2018/12/036/4e4/bd/0364e4bd-497c-5a49-b122-62575ab5bc22.jpeg",
            "longitude": -3.7106774,
            "image": {
                "id": 875,
                "extension": "png"
            },
            "is_cancellable": true,
            "address": "Calle de Toledo, 88",
            "city": "Madrid",
            "country": "España",
            "province": "Madrid",
            "zip": "28005",
            "provider": {
                "id": 81,
                "name": "DM - Grupo Bolton"
            },
            "handicapped_access": false,
            "security": false,
            "open_24h": false,
            "giving_keys": false,
            "exclude_checking": false,
            "name": "DM Latina",
            "max_height": 220,
            "passes": [
                {
                    "id": 39840,
                    "name": "MULTIPASS 3 hours",
                    "vehicle_type": {
                        "id": 1,
                        "type": "CAR",
                        "max_height": 190,
                        "max_length": 500
                    },
                    "warning_message": "At this car park, you must leave your car keys with us.",
                    "price": 8.1,
                    "type": "pass",
                    "internal_name": "MULTIPASS 3h (8.10€)",
                    "duration": 3,
                    "multiparking": false,
                    "multipass": true,
                    "frequency": "HOURLY",
                    "_links": {
                        "self": [
                            {
                                "href": "https://loc.api.parclick.com/v1/product/39840"
                            },
                            {
                                "href": "https://loc.api.parclick.com/v1/pass/39840"
                            }
                        ]
                    }
                }
            ],
            "subscriptions": [],
            "access": [
                {
                    "id": 294,
                    "type": "vehicle",
                    "latitude": 40.4087362,
                    "longitude": -3.7106774
                }
            ],
            "slug": "dm_latina",
            "multiparking": false,
            "reviews_summary": {
                "reviews": {
                    "270": {
                        "review": [],
                        "average": 5
                    },
                    "769": {
                        "review": [],
                        "average": 3.6666666666666665
                    },
                    "1113": {
                        "review": [],
                        "average": 4
                    },
                    "1298": {
                        "review": [],
                        "opinion": "Mejorar la comunicación entre el personal ya que cuando llegamos para sacar el coche al finalizar la reserva no se ponían de acuerdo entre ellos para saber quién tenía que sacar el coche. Además no estaban seguros de donde estaba, ni tampoco las llaves.",
                        "average": 2
                    },
                    "1319": {
                        "review": [],
                        "average": 3.6666666666666665
                    },
                    "1553": {
                        "review": [],
                        "opinion": "muy buen trato y parking muy cerca para alojarte en la latina muy contentos",
                        "average": 4.333333333333333
                    },
                    "1610": {
                        "review": [],
                        "opinion": "barato y en el mismo centro",
                        "average": 3.3333333333333335
                    },
                    "1660": {
                        "review": [],
                        "average": 5
                    },
                    "2621": {
                        "review": [],
                        "average": 3.6666666666666665
                    },
                    "3548": {
                        "review": [],
                        "opinion": "RAS",
                        "average": 4.666666666666667
                    },
                    "4187": {
                        "review": [],
                        "average": 5
                    },
                    "4442": {
                        "review": [],
                        "opinion": "Conocían perfectamente el modo de actuar con la App y todo funcionó bien",
                        "average": 5
                    },
                    "4940": {
                        "review": [],
                        "opinion": "llegas y ellos se encargan de aparcarte el coche, ya que las llaves se las quedan para si tienen que mover el coche. si quieres sacarlo enseñas el ticket y te sacan el coche.",
                        "average": 4
                    },
                    "4945": {
                        "review": [],
                        "average": 4.333333333333333
                    }
                },
                "questions": {
                    "4": {
                        "score": 56,
                        "number": 14,
                        "average": 4
                    },
                    "5": {
                        "score": 55,
                        "number": 14,
                        "average": 3.9285714285714284
                    },
                    "6": {
                        "score": 62,
                        "number": 14,
                        "average": 4.428571428571429
                    }
                },
                "totalScore": 4.119047619047619
            },
            "_links": {
                "self": {
                    "href": "https://loc.api.parclick.com/v1/parking/444"
                }
            },
            "_embedded": {
                "city": {
                    "id": 37,
                    "name": "Madrid",
                    "slug": "madrid",
                    "_links": {
                        "self": {
                            "href": "https://loc.api.parclick.com/v1/city/37"
                        }
                    }
                }
            }
        },
        {
            "id": 407,
            "covered": true,
            "flexible_entry": true,
            "guarded": true,
            "latitude": 40.429977094199,
            "image_list": "//static.parclick.com/parking/2017/07/c75/9f4/97/c759f497-e3f0-51e5-b0e7-41aa0e4782c5.jpeg",
            "longitude": -3.7187371270549,
            "is_cancellable": true,
            "address": "Marqués de Urquijo, s/n",
            "city": "Madrid",
            "country": "España",
            "province": "Madrid",
            "zip": "28008",
            "provider": {
                "id": 27,
                "name": "Mutuapark"
            },
            "handicapped_access": true,
            "security": true,
            "open_24h": true,
            "giving_keys": false,
            "exclude_checking": false,
            "name": "PARKIA Marqués De Urquijo",
            "max_height": 190,
            "passes": [
                {
                    "id": 4581,
                    "name": "MULTIPASS 1 day",
                    "vehicle_type": {
                        "id": 1,
                        "type": "CAR",
                        "max_height": 190,
                        "max_length": 500
                    },
                    "warning_message": "Although the car park is open 24 hours, you must redeem the receipt within Customer Service Hours:\r\na) Monday to Saturday from 7.00h to 23.00h.\r\nb) Sunday and holiday from 11.00h to 19.00h",
                    "price": 31.85,
                    "type": "pass",
                    "internal_name": "MULTIPASS 1d (31.85€)",
                    "duration": 24,
                    "multiparking": false,
                    "multipass": true,
                    "frequency": "HOURLY",
                    "_links": {
                        "self": [
                            {
                                "href": "https://loc.api.parclick.com/v1/product/4581"
                            },
                            {
                                "href": "https://loc.api.parclick.com/v1/pass/4581"
                            }
                        ]
                    }
                }
            ],
            "subscriptions": [
                {
                    "id": 70497,
                    "name": "PASS",
                    "vehicle_type": {
                        "id": 1,
                        "type": "CAR",
                        "max_height": 190,
                        "max_length": 500
                    },
                    "warning_message": "",
                    "price": 270.1,
                    "type": "subscription",
                    "subscription_type": "MONTHLY",
                    "duration": 1,
                    "_links": {
                        "self": {
                            "href": "https://loc.api.parclick.com/v1/product/70497"
                        }
                    }
                }
            ],
            "access": [
                {
                    "id": 846,
                    "type": "vehicle",
                    "latitude": 40.429977094199,
                    "longitude": -3.7187371270549
                }
            ],
            "slug": "parkia_marques_de_urquijo",
            "multiparking": false,
            "reviews_summary": {
                "reviews": {
                    "731": {
                        "review": [],
                        "average": 3.6666666666666665
                    }
                },
                "questions": {
                    "4": {
                        "score": 3,
                        "number": 1,
                        "average": 3
                    },
                    "5": {
                        "score": 4,
                        "number": 1,
                        "average": 4
                    },
                    "6": {
                        "score": 4,
                        "number": 1,
                        "average": 4
                    }
                },
                "totalScore": 3.6666666666666665
            },
            "_links": {
                "self": {
                    "href": "https://loc.api.parclick.com/v1/parking/407"
                }
            },
            "_embedded": {
                "city": {
                    "id": 37,
                    "name": "Madrid",
                    "slug": "madrid",
                    "_links": {
                        "self": {
                            "href": "https://loc.api.parclick.com/v1/city/37"
                        }
                    }
                }
            }
        },
        {
            "id": 1666,
            "covered": true,
            "flexible_entry": false,
            "guarded": true,
            "latitude": 40.43232507531,
            "image_list": "//static.parclick.com/parking/2017/07/136/6cf/31/1366cf31-9f20-593c-a3d5-09fd377bc9fe.jpeg",
            "longitude": -3.7194070438047,
            "is_cancellable": true,
            "address": "Calle Romero Robledo, 9",
            "city": "Madrid",
            "country": "Spain",
            "province": "Madrid",
            "zip": "28008",
            "provider": {
                "id": 81,
                "name": "DM - Grupo Bolton"
            },
            "handicapped_access": true,
            "security": true,
            "open_24h": false,
            "giving_keys": true,
            "exclude_checking": false,
            "name": "DM Argüelles",
            "max_height": 210,
            "passes": [
                {
                    "id": 563,
                    "name": "MULTIPASS 3 hours",
                    "vehicle_type": {
                        "id": 1,
                        "type": "CAR",
                        "max_height": 190,
                        "max_length": 500
                    },
                    "warning_message": "At this car park, you must leave your car keys with us.",
                    "list_price": 9,
                    "price": 5,
                    "type": "pass",
                    "internal_name": "MULTIPASS 3h (5.00€)",
                    "duration": 3,
                    "multiparking": false,
                    "multipass": true,
                    "frequency": "HOURLY",
                    "_links": {
                        "self": [
                            {
                                "href": "https://loc.api.parclick.com/v1/product/563"
                            },
                            {
                                "href": "https://loc.api.parclick.com/v1/pass/563"
                            }
                        ]
                    }
                }
            ],
            "subscriptions": [],
            "access": [
                {
                    "id": 1613,
                    "type": "vehicle",
                    "latitude": 40.43232507531,
                    "longitude": -3.7194070438047
                }
            ],
            "slug": "dm-arguelles",
            "multiparking": false,
            "reviews_summary": {
                "reviews": {
                    "12": {
                        "review": [],
                        "average": 3.6666666666666665
                    },
                    "988": {
                        "review": [],
                        "opinion": "El personal es muy amable",
                        "average": 3.6666666666666665
                    },
                    "2194": {
                        "review": [],
                        "average": 4
                    },
                    "2747": {
                        "review": [],
                        "average": 4
                    },
                    "3219": {
                        "review": [],
                        "average": 3.6666666666666665
                    },
                    "3220": {
                        "review": [],
                        "average": 3.6666666666666665
                    },
                    "3909": {
                        "review": [],
                        "average": 4
                    },
                    "3910": {
                        "review": [],
                        "average": 4
                    },
                    "4296": {
                        "review": [],
                        "average": 5
                    },
                    "4331": {
                        "review": [],
                        "average": 4
                    },
                    "4456": {
                        "review": [],
                        "opinion": "Excelent price/performance ratio. Offers what you need by half the price of other parkings.",
                        "average": 3.3333333333333335
                    },
                    "4907": {
                        "review": [],
                        "average": 3
                    }
                },
                "questions": {
                    "4": {
                        "score": 45,
                        "number": 12,
                        "average": 3.75
                    },
                    "5": {
                        "score": 44,
                        "number": 12,
                        "average": 3.6666666666666665
                    },
                    "6": {
                        "score": 49,
                        "number": 12,
                        "average": 4.083333333333333
                    }
                },
                "totalScore": 3.833333333333334
            },
            "_links": {
                "self": {
                    "href": "https://loc.api.parclick.com/v1/parking/1666"
                }
            },
            "_embedded": {
                "city": {
                    "id": 37,
                    "name": "Madrid",
                    "slug": "madrid",
                    "_links": {
                        "self": {
                            "href": "https://loc.api.parclick.com/v1/city/37"
                        }
                    }
                }
            }
        },
        {
            "id": 2254,
            "covered": true,
            "flexible_entry": true,
            "guarded": true,
            "latitude": 40.42679480842,
            "image_list": "//static.parclick.com/parking/2018/09/36a/d3c/e4/36ad3ce4-7da0-515a-ac30-62cc41cea7a8.jpeg",
            "longitude": -3.7142887754449,
            "is_cancellable": true,
            "address": "Calle de la Princesa, 25",
            "city": "Madrid",
            "country": "Spain",
            "province": "Madrid",
            "zip": "28008",
            "provider": {
                "id": 227,
                "name": "CITY PARKING MADRID"
            },
            "handicapped_access": false,
            "security": true,
            "open_24h": true,
            "giving_keys": false,
            "exclude_checking": false,
            "name": "Princesa 25",
            "max_height": 190,
            "passes": [
                {
                    "id": 68831,
                    "name": "MULTIPASS 5 hours",
                    "vehicle_type": {
                        "id": 1,
                        "type": "CAR",
                        "max_height": 190,
                        "max_length": 500
                    },
                    "warning_message": "In order to enjoy the reduced price of this pass you must bring a printed-out receipt. Without the receipt you will be charged the full price.",
                    "price": 18,
                    "type": "pass",
                    "internal_name": "ONEPASS 5h (18.00€)",
                    "duration": 5,
                    "multiparking": false,
                    "multipass": false,
                    "frequency": "HOURLY",
                    "_links": {
                        "self": [
                            {
                                "href": "https://loc.api.parclick.com/v1/product/68831"
                            },
                            {
                                "href": "https://loc.api.parclick.com/v1/pass/68831"
                            }
                        ]
                    }
                }
            ],
            "subscriptions": [
                {
                    "id": 68784,
                    "name": "NIGHT MONTHLY TICKET",
                    "vehicle_type": {
                        "id": 1,
                        "type": "CAR",
                        "max_height": 190,
                        "max_length": 500
                    },
                    "warning_message": "",
                    "price": 120,
                    "type": "subscription",
                    "subscription_type": "NIGHTLY",
                    "duration": 12,
                    "_links": {
                        "self": {
                            "href": "https://loc.api.parclick.com/v1/product/68784"
                        }
                    }
                }
            ],
            "access": [
                {
                    "id": 2794,
                    "type": "vehicle",
                    "latitude": 40.42679480842,
                    "longitude": -3.7142887754449
                }
            ],
            "slug": "princesa-25",
            "multiparking": false,
            "reviews_summary": {
                "reviews": {
                    "4864": {
                        "review": [],
                        "opinion": "Muy pequeño y poco cuidado \nLo único al lado del Meliá",
                        "average": 2.6666666666666665
                    }
                },
                "questions": {
                    "4": {
                        "score": 2,
                        "number": 1,
                        "average": 2
                    },
                    "5": {
                        "score": 2,
                        "number": 1,
                        "average": 2
                    },
                    "6": {
                        "score": 4,
                        "number": 1,
                        "average": 4
                    }
                },
                "totalScore": 2.6666666666666665
            },
            "_links": {
                "self": {
                    "href": "https://loc.api.parclick.com/v1/parking/2254"
                }
            },
            "_embedded": {
                "city": {
                    "id": 37,
                    "name": "Madrid",
                    "slug": "madrid",
                    "_links": {
                        "self": {
                            "href": "https://loc.api.parclick.com/v1/city/37"
                        }
                    }
                }
            }
        },
        {
            "id": 577,
            "covered": true,
            "flexible_entry": true,
            "guarded": true,
            "latitude": 40.416672224936,
            "image_list": "//static.parclick.com/parking/2017/07/b07/809/fa/b07809fa-1e69-52f7-ae21-59e0221213ec.jpeg",
            "longitude": -3.7093525422372,
            "image": {
                "id": 1109,
                "extension": "jpg"
            },
            "is_cancellable": true,
            "address": "Mesón de Paños, 11",
            "city": "Madrid",
            "country": "España",
            "province": "Madrid",
            "zip": "28013",
            "provider": {
                "id": 124,
                "name": "GARAJE FERMAR"
            },
            "handicapped_access": true,
            "security": false,
            "open_24h": false,
            "giving_keys": false,
            "exclude_checking": false,
            "name": "Garaje Fermar",
            "max_height": 250,
            "passes": [
                {
                    "id": 61986,
                    "name": "ONEPASS 3 hours",
                    "vehicle_type": {
                        "id": 1,
                        "type": "CAR",
                        "max_height": 190,
                        "max_length": 500
                    },
                    "warning_message": "",
                    "price": 9,
                    "type": "pass",
                    "internal_name": "ONEPASS 3h (9.00€)",
                    "duration": 3,
                    "multiparking": false,
                    "multipass": false,
                    "frequency": "HOURLY",
                    "_links": {
                        "self": [
                            {
                                "href": "https://loc.api.parclick.com/v1/product/61986"
                            },
                            {
                                "href": "https://loc.api.parclick.com/v1/pass/61986"
                            }
                        ]
                    }
                }
            ],
            "subscriptions": [],
            "access": [
                {
                    "id": 662,
                    "type": "vehicle",
                    "latitude": 40.416672224936,
                    "longitude": -3.7093525422372
                }
            ],
            "slug": "garaje_fermar",
            "multiparking": false,
            "reviews_summary": {
                "reviews": {
                    "43": {
                        "review": [],
                        "opinion": "Entrada con gps bastante complicada. Seguir indicaciones del personal del parking",
                        "average": 4
                    },
                    "2549": {
                        "review": [],
                        "average": 2.6666666666666665
                    },
                    "3153": {
                        "review": [],
                        "opinion": "Excelente ubicación y el personal muy simpático y atento.",
                        "average": 4.666666666666667
                    },
                    "3816": {
                        "review": [],
                        "average": 4.333333333333333
                    },
                    "5276": {
                        "review": [],
                        "average": 4
                    }
                },
                "questions": {
                    "4": {
                        "score": 15,
                        "number": 5,
                        "average": 3
                    },
                    "5": {
                        "score": 20,
                        "number": 5,
                        "average": 4
                    },
                    "6": {
                        "score": 24,
                        "number": 5,
                        "average": 4.8
                    }
                },
                "totalScore": 3.9333333333333327
            },
            "_links": {
                "self": {
                    "href": "https://loc.api.parclick.com/v1/parking/577"
                }
            },
            "_embedded": {
                "city": {
                    "id": 37,
                    "name": "Madrid",
                    "slug": "madrid",
                    "_links": {
                        "self": {
                            "href": "https://loc.api.parclick.com/v1/city/37"
                        }
                    }
                }
            }
        },
        {
            "id": 1811,
            "covered": true,
            "flexible_entry": false,
            "guarded": true,
            "latitude": 40.424163418398,
            "image_list": "//static.parclick.com/parking/2017/07/ec0/14d/a8/ec014da8-13f6-5b82-bb91-bbd0c0722579.jpeg",
            "longitude": -3.7120694318933,
            "is_cancellable": true,
            "address": "Plaza de España, 18",
            "city": "Madrid",
            "country": "España",
            "province": "Madrid",
            "zip": "28008",
            "provider": {
                "id": 19,
                "name": "EMT"
            },
            "handicapped_access": true,
            "security": true,
            "open_24h": true,
            "giving_keys": false,
            "exclude_checking": false,
            "name": "EMT Plaza España",
            "max_height": 190,
            "passes": [
                {
                    "id": 45017,
                    "name": "EVENING",
                    "vehicle_type": {
                        "id": 1,
                        "type": "CAR",
                        "max_height": 190,
                        "max_length": 500
                    },
                    "warning_message": "Before exiting please get to the control cabin with your reservation locator to get an exit ticket from the clerk on duty",
                    "price": 15,
                    "type": "pass",
                    "internal_name": "ONEPASS 6h (15.00€)",
                    "duration": 6,
                    "multiparking": false,
                    "multipass": false,
                    "frequency": "HOURLY_RANGE",
                    "_links": {
                        "self": [
                            {
                                "href": "https://loc.api.parclick.com/v1/product/45017"
                            },
                            {
                                "href": "https://loc.api.parclick.com/v1/pass/45017"
                            }
                        ]
                    }
                }
            ],
            "subscriptions": [],
            "access": [
                {
                    "id": 2448,
                    "type": "vehicle",
                    "latitude": 40.424163418398,
                    "longitude": -3.7120694318933
                }
            ],
            "slug": "emt-plaza-espana",
            "multiparking": false,
            "reviews_summary": {
                "reviews": {
                    "396": {
                        "review": [],
                        "opinion": "El parking muy bien pero el proceso de validación de la reserva muy lento y rudimentario.",
                        "average": 4
                    },
                    "4563": {
                        "review": [],
                        "average": 3
                    },
                    "5039": {
                        "review": [],
                        "average": 4
                    },
                    "5301": {
                        "review": [],
                        "average": 4.666666666666667
                    },
                    "5304": {
                        "review": [],
                        "opinion": "Agradecer la excelente atención prestada por los trabajadores ¡repetiremos!",
                        "average": 5
                    },
                    "5358": {
                        "review": [],
                        "opinion": "Sistema de certificación de reserva vía Parkclick muy lento en el aparcamiento",
                        "average": 2.6666666666666665
                    }
                },
                "questions": {
                    "4": {
                        "score": 25,
                        "number": 6,
                        "average": 4.166666666666667
                    },
                    "5": {
                        "score": 21,
                        "number": 6,
                        "average": 3.5
                    },
                    "6": {
                        "score": 24,
                        "number": 6,
                        "average": 4
                    }
                },
                "totalScore": 3.8888888888888893
            },
            "_links": {
                "self": {
                    "href": "https://loc.api.parclick.com/v1/parking/1811"
                }
            },
            "_embedded": {
                "city": {
                    "id": 37,
                    "name": "Madrid",
                    "slug": "madrid",
                    "_links": {
                        "self": {
                            "href": "https://loc.api.parclick.com/v1/city/37"
                        }
                    }
                }
            }
        }
    ],
    "total": 38,
    "params": {
        "locale": "en_GB",
        "group": "search",
        "latitude": "40.4089785",
        "longitude": "-3.761460599999964",
        "radius": "5",
        "limit": "10",
        "from": "2019-12-31 20:00",
        "to": "2019-12-31 23:00",
        "vehicleType": "1"
    },
    "_links": {
        "self": {
            "href": "/v1/parking?page=1&limit=10"
        },
        "first": {
            "href": "/v1/parking?page=1&limit=10"
        },
        "last": {
            "href": "/v1/parking?page=4&limit=10"
        },
        "next": {
            "href": "/v1/parking?page=2&limit=10"
        }
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

## <a name="list_products"></a><span style="color:#FF6600;">List products</span>

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
| from            | date yyyy-MM-dd HH:mm | true     | booking start date                                 |null            |
| to              | date yyyy-MM-dd HH:mm | true     | booking end date                                   |null            |


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
            "id": 27032,
            "category": "ONEPASS",
            "name": "ONEPASS 1 day",
            "parking": {
                "id": 1047,
                "covered": false,
                "flexible_entry": false,
                "guarded": true,
                "is_cancellable": true,
                "airport": {
                    "id": 19,
                    "terminal": [
                        {
                            "id": 25,
                            "terminal": {
                                "id": 8
                            }
                        }
                    ],
                    "category_name": "Valet",
                    "category": 2,
                    "has_shuttle": false
                },
                "address": "T2 Aéroport de Nice-Côte d'Azur, Nice, France",
                "city": "Nice",
                "name": "T2 ECTOR Aéroport - Service Voiturier - Nice",
                "_links": {
                    "self": {
                        "href": "https://loc.api.parclick.com/v1/parking/1047"
                    }
                },
                "_embedded": {
                    "city": {
                        "id": 115,
                        "_links": {
                            "self": {
                                "href": "https://loc.api.parclick.com/v1/city/115"
                            }
                        }
                    }
                }
            },
            "vehicle_type": {
                "id": 1,
                "type": "CAR"
            },
            "warning_message": "Please mind you need to leave the car keys with us.",
            "administration_fee": 0,
            "paypal_fee": 4.62,
            "price": 49,
            "type": "pass",
            "internal_name": "ONEPASS 8h (49€)",
            "token": "2338bb72051ae11083a20cd94f3b3183ede3333708b1bc7b50e6af509100ef14",
            "duration": 8,
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
        }
    ],
    "total": 1,
    "params": {
        "locale": "en_GB",
        "group": "bestpass",
        "from": "2019-07-22 10:00",
        "to": "2019-07-22 18:00",
        "parking": "1047",
        "vehicleType": "1"
    }
}
</pre>
</p>
</details>

<br>

**200 Ok - No results**

<details><summary style="color:#FF6600;">Show response 200 No results</summary>
<p>
<pre>
{  
   "page":1,
   "limit":10,
   "pages":0,
   "items":[  

   ],
   "total":0,
   "params":{  
      "locale":"en_GB",
      "group":"bestpass",
      "parking":"785",
      "from":"2019-04-13 10:00",
      "to":"2019-04-13 12:00",
      "vehicleType":"6"
   },
   "_links":{  
      "self":{  
         "href":"/v1/pass?page=1&limit=10"
      },
      "first":{  
         "href":"/v1/pass?page=1&limit=10"
      },
      "last":{  
         "href":"/v1/pass?page=0&limit=10"
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
| from            | date yyyy-MM-dd HH:mm | true     | booking start date       | null                   |
| to              | date yyyy-MM-dd HH:mm | true     | booking end date         | null                   |
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
| from            | date yyyy-MM-dd HH:mm | false    | null             | booking start date      |
| to              | date yyyy-MM-dd HH:mm | false    | null             | booking end date        |


### _Response:_

**200 Ok**

<details><summary style="color:#FF6600;">Show response 200 Ok group list</summary>
<p>
<pre>
{
	"items": [
        {
            "id": 805081,
            "createdAt": "2019-03-11T10:06:01+0000",
            "end_booking_date": "2019-04-01T12:00:00+0000",
            "start_booking_date": "2019-04-01T10:00:00+0000",
            "total": 6,
            "booking_code": "L3KNEKY",
            "first_name": "test6",
            "last_name": "Uno6",
            "state": "PENDING",
            "total_net_price": 5,
            "total_vat": 1
        },
    ]
}
</pre>
</p>
</details>

<details><summary style="color:#FF6600;">Show response 200 Ok group booking</summary>
<p>
<pre>
{
	"items": [
	{
            "id": 962978,
            "createdAt": "2019-05-21T13:32:55+0000",
            "end_booking_date": "2019-05-30T12:00:00+0000",
            "start_booking_date": "2019-05-30T10:00:00+0000",
            "total": 6,
            "booking_code": "LKKX873",
            "first_name": "miki1",
            "last_name": "moto1",
            "username": "TEST_ITmikimoto1@parclick.comTEST_IT",
            "items": [
                {
                    "id": 964157,
                    "parking": {
                        "id": 792,
                        "latitude": 48.8569651,
                        "image_list": "//static.parclick.com/parking/2017/08/627/80d/d0/62780dd0-27cd-579b-86a5-714a4f44b72c.jpeg",
                        "longitude": 2.3496573,
                        "is_cancellable": true,
                        "address": "6, quai de Gesvres",
                        "city": "París",
                        "country": "Francia",
                        "zip": "75004",
                        "name": "Hôtel de Ville - Paris",
                        "freemium": false,
                        "cancellation_type": 2,
                        "_links": {
                            "self": {
                                "href": "https://loc.api.parclick.com/v1/parking/792"
                            }
                        }
                    },
                    "product": {
                        "id": 60934,
                        "instructions": "ARRIVAL: scan your QR code. Park in any free space. Go to the office with your reservation and the ticket. If there is no-one there, ring the intercom.\r\n\r\nDEPARTURE: Use the card/device the staff gave you.\r\n\r\nIF YOUR BOOKING ALLOWS UNLIMITED ENTRANCE AND EXIT: Use the multipass card/device the staff gave you.",
                        "name": "MULTIPASS 2 hours",
                        "vehicle_type": {
                            "id": 1
                        },
                        "warning_message": "In case of problems while accessing to the parkings, ring the intercom.\r\nThe reception is located at level: -1",
                        "list_price": 8.4,
                        "price": 6,
                        "type": "pass",
                        "_links": {
                            "self": [
                                {
                                    "href": "https://loc.api.parclick.com/v1/product/60934"
                                },
                                {
                                    "href": "https://loc.api.parclick.com/v1/pass/60934"
                                }
                            ]
                        }
                    }
                }
            ],
            "fees": [],
            "state": "PENDING",
            "total_net_price": 5,
            "total_vat": 1
        }
    ]
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
| parking_instructions      | array            | parking instructions ()                         |
| parking_latitude          | float            | parking latitude                             |
| parking_longitude         | float            | parking longitude                            |
| parking_category          | array            | parking category                             |
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
| from                      | string           | email from                                   |
| from_name                 | string           | email from name                              |
| to                        | string           | email to                                     |
| to_name                   | string           | email to name                                |
| cco                       | string           | email carbon copy                            |
| subject                   | string           | email subject                                | 
|---------------------------------------------------------------------------------------------|

<details><summary style="color:#FF6600;">Show response 200 Ok</summary>
<p>
<pre>
{
    "data": {
        "user": {
            "user_first_name": "John",
            "user_last_name": "DoE"
        },
        "product": {
            "product": "ONEPASS 2 hours",
	    "product_instructions": "Here are the product instrucctions",
            "product_category": "ONEPASS",
            "product_warning": "For the QR reader to work properly, you must have the latest version of the app installed.",
            "product_net_price": 2.48,
            "product_vat_price": 0.52,
            "product_vat_percent": "21%",
            "product_gross_price": 3
        },
        "voucher": {
            "voucher_code": "VZ87NG9",
            "voucher_id": 920643,
            "voucher_description": [
                "ARRIVAL:\r",
                "\u003Cbr\/\u003EPresent the QR code on your reservation (on your phone or a printed version) to the orange Parclick reader, that you will find next to the entrance barrier. The light will turn green and the barrier will open (in 3-4 seconds). Park in any of the identified spaces for Parclick customers:\r",
                "\u003Cbr\/\u003E- Outdoor spaces: no 3, 7, 8, 9, 11.\r",
                "\u003Cbr\/\u003E- Underground spaces (floor -2): no 24, 25, 26, 40, 41.\r",
                "\u003Cbr\/\u003E\r",
                "\u003Cbr\/\u003EDEPARTURE:\r",
                "\u003Cbr\/\u003EShow your QR code to the reader. If the light turns red, contact the operator through the barrier interphone.\r",
                "\u003Cbr\/\u003E\r",
                "\u003Cbr\/\u003EIF YOUR BOOKING ALLOWS UNLIMITED ENTRANCE AND EXIT:\r",
                "\u003Cbr\/\u003EFollow the same process, indicated before, to enter and exit.\r",
                "\u003Cbr\/\u003E\r",
                "\u003Cbr\/\u003ECOURTESY TIME AND EXCEEDED TIME:\r",
                "\u003Cbr\/\u003EYou have 30 minutes\u2019 courtesy time to enter the car park before the time indicated on your booking and 30 minutes of courtesy time to leave after. If your stay exceeds the courtesy time, it will automatically charge you the difference on your card which you used to make your booking  (+0.38\u20ac \/ 15 mins)."
            ]
        },
        "booking": {
            "booking_state": "CONFIRMED",
            "booking_is_cancellable": true,
            "booking_cancellation_type": 2,
            "booking_from": "31\/12\/2019 10:00",
            "booking_to": "31\/12\/2019 12:00",
            "total_net_price": 2.48,
            "total_vat": 0.52,
            "total": 3,
            "vehicle_type": "CAR",
            "external_code_tech": "qrcode_1",
            "qr_hash": "@VZ87NG9#",
            "extra_fields": [
                {
                    "3": {
                        "label": "Vehicle registration number",
                        "value": "CB797XH"
                    }
                }
            ]
        },
        "parking": {
            "parking_id": 1988,
            "parking_name": "Fray Luis de Le\u00f3n - Atocha",
            "parking_address": "Calle de Fray Luis de Le\u00f3n, 11",
            "parking_zip": "28012",
            "parking_city": "Madrid",
            "parking_province": "Madrid",
            "parking_country": "SPAIN",
            "parking_maximum_height": "2.2 m.",
            "parking_description": "Parking in the centre of Madrid can seem an absolute madness, but it won\u2019t be if you know how. In Parclick we\u2019ve already thought of a solution for you, whether you\u2019re just going to the centre for a few hours or want to park your car for a few days. You can leave your car in the Fray Luis de Le\u00f3n 11 Car Park and forget all your worries!\r\n\r\nLocated less than 5 minutes walking from the Embajadores Roundabout, a very centric point of the city of Madrid, the Fray Luis de Le\u00f3n 11 Car Park is as great option if what you want is to leave your car in a zone that\u2019s totally connected with most of the interesting points in the city without having to go through busy streets like Gran V\u00eda or the historic centre of Madrid. Ultimately, it\u2019s the perfect place to leave your car during your stay in the city. \r\n\r\nIn the end, the Fray Luis de Le\u00f3n 11 Car Park would be the perfect place to park your car. Covered, guarded and economic. What are you waiting for? Book your parking space with Parclick forget about your car for now!",
            "parking_instructions": "ARRIVAL:\r\n\u003Cbr\/\u003EPresent the QR code on your reservation (on your phone or a printed version) to the orange Parclick reader, that you will find next to the entrance barrier. The light will turn green and the barrier will open (in 3-4 seconds). Park in any of the identified spaces for Parclick customers:\r\n\u003Cbr\/\u003E- Outdoor spaces: no 3, 7, 8, 9, 11.\r\n\u003Cbr\/\u003E- Underground spaces (floor -2): no 24, 25, 26, 40, 41.\r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003EDEPARTURE:\r\n\u003Cbr\/\u003EShow your QR code to the reader. If the light turns red, contact the operator through the barrier interphone.\r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003EIF YOUR BOOKING ALLOWS UNLIMITED ENTRANCE AND EXIT:\r\n\u003Cbr\/\u003EFollow the same process, indicated before, to enter and exit.\r\n\u003Cbr\/\u003E\r\n\u003Cbr\/\u003ECOURTESY TIME AND EXCEEDED TIME:\r\n\u003Cbr\/\u003EYou have 30 minutes\u2019 courtesy time to enter the car park before the time indicated on your booking and 30 minutes of courtesy time to leave after. If your stay exceeds the courtesy time, it will automatically charge you the difference on your card which you used to make your booking  (+0.38\u20ac \/ 15 mins).",
            "parking_latitude": 40.404219150793,
            "parking_longitude": -3.6981113117529,
            "parking_provider_id": 439,
            "parking_provider_name": "Altamira",
            "parking_type": false,
            "spot_name": 160,
            "is_box": false
        },
        "fee": [],
        "third_party_discriminator": "TEST_IT",
        "currency": "EUR",
        "has_voucher_notification": true,
        "has_receipt_notification": false
    },
    "email": {
        "from": "miki.petrovic@parclick.com",
        "from_name": "Parclick | IT",
        "to": "natalia.gonzalez@parclick.com",
        "to_name": "miki.petrovic@parclick.com",
        "cco": "archive@parclick.com",
        "subject": "Parclick | IT | IT"
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
