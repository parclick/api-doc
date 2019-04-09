![alt text](https://static.parclick.com/assets/img/logotipo-parclick.svg)

<p style="color:#999999;text-align:right;">Document version 1.0.0</div>
</p>

# <span style="color:#FF6600;">Parclick - API Reference v3.2</span>
#### <span style="color:#999;">TECNICAL DOCUMENTATION</a>




###<span style="color:#FF6600;">Content table</span>

1. API description
2. Login
3. Refresh token
4. List parkings
5. List products
6. Create reservation
7. Cancel reservation
8. List reservations
9. Worflows
 	- Workflow to create a booking reservation
	- 	Workflow to cancel a booking
	- Workflow to list booking
10. Generate entry code

<br><br>



###<span style="color:#FF6600;">API Description</a>

Parclick has a rest API with two environments: development (https://pre.api.parclick.com) and production (https://api.parclick.com). Endpoints requiring authentication must have a JWT token in the header. This tutorial help to unsderstand the available endpoints and the flow to create, delete and list bookings. 


<br>
##<span style="color:#FF6600;">Login</span>

###<span style="color:#10a54a;">`POST`</span> `/v1/login`


This method allows third party systems to be authenticated and receive a key (token) to pass when calling the e-commerce web service methods.

### _Request:_

| Parameters      | Type            | Required |
| --------------- |:--------------- | -------- |
| \_username      | string          | true     |
| \_password      | string          | true     |



### _Response:_

**200 Ok**

| Parameters      | Type           | Required                             |
| --------------- |:-------------- | :----------------------------------- |
| token           | string         | token (expires in 1 hour)            |
| refresh_token   | string         | token to refresh current valid token |                       
| refresh_token   | string         | user id authenticated                |

```javascript
{
    "token": "eyJhbGciOiJSUzI1NiJ9.eyJyb2xlcyI6WyJST0xFX1NVUEVSX0FETUlOIiwiUk9MRV9VU0VSIl0sInVzZXJuYW1lIjoibWlraS5wZXRyb3ZpY0BwYXJjbGljay5jb20iLCJleHAiOjE1NTIzMjAyODgsImlhdCI6MTU1MjMxNjY4OH0.jXllvfZWoXvw-1QGkB4kQGccKVJOUBtFP5VF37Oq8GosLhO2xwqeUVEs3vIj4C030SaZBsmvDNgGY3os4_qMWFgw5EPtPF_n5bLRtQ-BcKAuvqnQmNA9dk5ExxTQEUI08opCDFTxp4nmd6tU97TtCUpKQaOcqR-KP1XEgKaZf2hQNykUPP1k_9Z4NEHdVTy5tSnroqP1hyoF_S7AaxM-XNlKmwnIhmbWgeYmOD41ogBXeRRgiUuP8SelgJchWd2rLcAl7MTDMnW_8EvrpfjaY8nVtENsMZu5_CZhdhUzSBKcWSJHMBGhWf7NWINBib23YNc0ut6sYdNCQ-BagXYq2Riy6gn1SbocUAe9Yt6nmL_9bTzqKhku85S6tZcr9IyRiGOlyU0vZfAOBt98FZ9SUemaDufyHOfY-fvyP7iq-efSyE1IY-FZnabwHQtDl3Z5j_VvMqrJs2tPJX27HI6gNIgE7rKXL-YFHK__nOofR2H9cUp6BSEcmAGjD7zVDilWgSUPZGOp6yLr4w-Fu1NgpPIJt6KZxqcpxCU-nhvbn3wIpGm6b-9mNDZy9yhJftJMuLGeqLMcO2IRnDTQK8fYEodTsfGmqU-b7bj_HEAdxgTFw0CeFT6cmMnVo-wUZHcrKWKgiJjBRUlHTM6wD6JBClHb0iTkjtQg7oqb9iHxzWA",
    "refresh_token": "81a2467f1093aeb96d83b1c46dfe3ad0e0f341f4190f73c17a37351e41693c546d70a01b01671f80962112a7a4090c37caa397161da87f69cd334bc460cae424",
    "id": 600116
}
```

**401 Unauthorized**

| Parameters      | Type            | Required                     |
| --------------- |:--------------- | ---------------------------- |
| code            | string          | http response code value     |
| message         | string          | error description            |


```javascript
{
	"code": 401,
	"message": "Bad credentials"
}
```

<br>
##<span style="color:#FF6600;">Refresh token</span>

###<span style="color:#10a54a;">`POST`</span> `/v1/token/refresh`

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

```javascript
{
    "token": "eyJhbGciOiJSUzI1NiJ9.eyJyb2xlcyI6WyJST0xFX1VTRVIiXSwidXNlcm5hbWUiOiJtaWtpLnBldHJvdmljQHBhcmNsaWNrLmNvbSIsImV4cCI6MTU1MjQ5MDgxNiwiaWF0IjoxNTUyNDg3MjE2fQ.dwp3pbUF4vktlX_vIhuiYJQ3XwXmJHJe_gl8LrZUyJ3VZ1pEsFsvtbl8Q3C82tJlxslO9SJWI1rDcHGsGni0FIUxHbKEuuTNwzM2n4EfS25HSTa-0FQJckBhQBa8CRDBTCbfNrNUxPM2tie0ILrTnZaGIMHjtL9px08M2fEqYkC0SF3haYnhvjg40DfxCIcba-_SkXJyQGDsJEQeMiCeNWQNe4gap-Vg_lLFmhKzuWvp5PHRMDqPZ1IbFXfqbZEn79dobwr93lBNce_tBmeOwBhOI-OtIeVcwhhy5998wV0V_KxlF-LgxBKsr6cyWwS1JGStNU2BbqQcjGh3wGqhU42g-29zhI0Pn2fLuUAqdwL73c4wVJ-iZx4UMIY1XI0poBBX6cjImuN8JzPFZbwjW4VHGZ6G7VNvBZsngrQ7QQngM3OFkQ19vTCo0O9kXJqjBkR53WjFxuHCiT7AaQd18DoR51L7HejthunO0sqQGNZoPhWtrLZMcnc9vq3DFJI5YxeFWuB5sAIwMUi-xLk3l8ijReSQ63sbUrVEOX3mrDE8N1zOfHt_OoniniVdYY7pUDczDmsybye_JQjxcu9o96Em0Pd37ABcf_-y8LjDgIgjkceFVJiBArAhcZZGtoxexPaRcRyzE698TARXsC6EmLih0TEaA8gZNsAOJL5U2e4",
    "refresh_token": "7635bc6f8970fa9042297a692dfba629a346dff4873fd34ddcea82c4be74e738414164e69d455ecafc29b5d5082bb2bff08082ec15f62dfc2a2bd47701eb7976"
}
```

**401 Unauthorized**

| Parameters      | Type            | Required                     |
| --------------- |:--------------- | ---------------------------- |
| code            | string          | http response code value     |
| message         | string          | error description            |


```javascript
{
	"code": 401,
	"message": "Bad credentials"
}
```

<br>
##<span style="color:#FF6600;">List parkings</span>

###<span style="color:#0f6ab4;">`GET`</span> `/v1/parking`


This method returns a list of car parks close to a location and based on the parameters used.

### _Request:_

| Parameters      | Type                   | Required | Description                                                                       | Default             |
| --------------- |:---------------------- | -------- | --------------------------------------------------------------------------------- |-------------------- |
| locale          | string                 | true     | language                                                                          | en_GB               |
| group           | string                 | true     | returned data [list]                                                              | null                |
| limit           | integer                | true     | total number of records [1-200]                                                   | 200                 |
| from            | date yyyy-MM-dd HH:mm  | true     | booking start date                                                                | null                |
| to              | date yyyy-MM-dd HH:mm  | true     | booking end date                                                                  | null                |
| latitude        | float                  | true     | valid latitude in which to look for                                               | null                |
| longitude       | float                  | true     | valid longitude in which to look for                                              | null                |
| radius          | integer                | false    | search radius                                                                     | null                |
| vehicleType     | integer                | true     | vehicle type [1 car, 2 van, 3 caravan, 4 bus, 5 truck, 6 motorbike, 7 small truck]| null                |
| freemium        | bool                   | false    | is freemium                                                                       | null                |
| arrival_flight  | string                 | false    | arrival flight voucher                                                            | null                |



### _Response:_

**200 Ok**

| Parameters      | Type           | Required                             |
| --------------- |:-------------- | :----------------------------------- |
| token           | string         | Token (expires in 1 hour)            |
| refresh_token   | string         | Token to refresh current valid token |                       
| refresh_token   | string         | user id authenticated                |

```javascript
{
    "token": "eyJhbGciOiJSUzI1NiJ9.eyJyb2xlcyI6WyJST0xFX1NVUEVSX0FETUlOIiwiUk9MRV9VU0VSIl0sInVzZXJuYW1lIjoibWlraS5wZXRyb3ZpY0BwYXJjbGljay5jb20iLCJleHAiOjE1NTIzMjAyODgsImlhdCI6MTU1MjMxNjY4OH0.jXllvfZWoXvw-1QGkB4kQGccKVJOUBtFP5VF37Oq8GosLhO2xwqeUVEs3vIj4C030SaZBsmvDNgGY3os4_qMWFgw5EPtPF_n5bLRtQ-BcKAuvqnQmNA9dk5ExxTQEUI08opCDFTxp4nmd6tU97TtCUpKQaOcqR-KP1XEgKaZf2hQNykUPP1k_9Z4NEHdVTy5tSnroqP1hyoF_S7AaxM-XNlKmwnIhmbWgeYmOD41ogBXeRRgiUuP8SelgJchWd2rLcAl7MTDMnW_8EvrpfjaY8nVtENsMZu5_CZhdhUzSBKcWSJHMBGhWf7NWINBib23YNc0ut6sYdNCQ-BagXYq2Riy6gn1SbocUAe9Yt6nmL_9bTzqKhku85S6tZcr9IyRiGOlyU0vZfAOBt98FZ9SUemaDufyHOfY-fvyP7iq-efSyE1IY-FZnabwHQtDl3Z5j_VvMqrJs2tPJX27HI6gNIgE7rKXL-YFHK__nOofR2H9cUp6BSEcmAGjD7zVDilWgSUPZGOp6yLr4w-Fu1NgpPIJt6KZxqcpxCU-nhvbn3wIpGm6b-9mNDZy9yhJftJMuLGeqLMcO2IRnDTQK8fYEodTsfGmqU-b7bj_HEAdxgTFw0CeFT6cmMnVo-wUZHcrKWKgiJjBRUlHTM6wD6JBClHb0iTkjtQg7oqb9iHxzWA",
    "refresh_token": "81a2467f1093aeb96d83b1c46dfe3ad0e0f341f4190f73c17a37351e41693c546d70a01b01671f80962112a7a4090c37caa397161da87f69cd334bc460cae424",
    "id": 600116
}
```

**401 Unauthorized**

| Parameters      | Type            | Required                     |
| --------------- |:--------------- | ---------------------------- |
| code            | string          | http response code value     |
| message         | string          | error description            |


```javascript
{
	"code": 401,
	"message": "Bad credentials"
}
```

<br>
##<span style="color:#FF6600;">Get products</span>

###<span style="color:#0f6ab4;">`GET`</span> `/v1/pass`

This method returns the best product available on selected car park based on the stay provided.

### _Request:_

| Parameters      | Type                  | Required | Description                                        | Default        |
| --------------- |:--------------------- | :------- |--------------------------------------------------- |--------------- |
| group           | string                | true     | Property groups to be returned [bestpass, list]    |bestpass        |
| locale          | string                | true     | Language in which the information will be returned |en_GB           | 
| parking         | integer               | true     | Parking identificator                              |null            |
| vehicleType     | integer               | true     | Type of vehicle that the customer wants to park    |1               |
| from            | date yyyy-MM-dd HH:mm | true     | Booking start date                                 |null            |
| to              | date yyyy-MM-dd HH:mm | true     | Booking end date                                   |null            |


### _Response:_

**200 Ok**

```javascript
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
```

**200 Ok - No results**

```javascript

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
```

**401 Bad request -  when parameters are missing or incorrect**


```javascript
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
```

<br>
##<span style="color:#FF6600;">Create reservation</span>

###<span style="color:#10a54a;">`POST`</span> `/v1/booking/new`

This method creates a new reservation.

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
| from            | date yyyy-MM-dd HH:mm | true     | booking art date         | null                   |
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

```javascript
{
    "booking_id": 805097,
    "voucher_id": 685447,
    "voucher_code": "L5N9675",
}
```

**401 Unauthorized**

| Parameters      | Type            | Required                     |
| --------------- |:--------------- | ---------------------------- |
| code            | string          | http response code value     |
| message         | string          | error description            |


```javascript
{
	"code": 401,
	"message": "Bad credentials"
}
```

<br>
##<span style="color:#FF6600;">Cancel reservation</span>

###<span style="color:#10a54a;">`POST`</span> `/v1/booking/{id}/cancelex`

This method allows cancel a reservation. The id required to cancel reservation is done in the new reservation response.

### _Header:_
Authorization: Bearer {JWT_TOKEN}

### _Request:_

| Parameters      | Type                | Required | Default  | Description       |
| --------------- |:--------------------| ---------| ---------| ----------------- |
| id              | integer             | true     | null     | voucher id        |


### _Response:_

**200 Ok**


**401 Unauthorized**

| Parameters      | Type            | Required                     |
| --------------- |:---------------:| -----------------------------|
| code            | string          | http response code value     |
| message         | string          | error description            |


```javascript
{
	"code": 401,
	"message": "Bad credentials"
}
```

<br>
##<span style="color:#FF6600;">List reservations</span>

###<span style="color:#0f6ab4;">`GET`</span> `/v1/booking/list`

List all bookings that match filter criteria.

### _Header:_
Authorization: Bearer {JWT_TOKEN}

### _Request:_

| Parameters      | Type                  | Required | Default  |
| --------------- |:--------------------- | -------- | -------- |
| group           | string                | true     | null     |
| email           | string                | false    | null     |
| product         | integer               | false    | null     |
| firstName       | string                | false    | null     |
| lastName        | string                | false    | null     |
| from            | date yyyy-MM-dd HH:mm | false    | null     |
| to              | date yyyy-MM-dd HH:mm | false    | null     |


### _Response:_

**200 Ok**

```javascript
{
	"results": [
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
```

**401 Unauthorized**

| Parameters      | Type            | Required                     |
| --------------- |:---------------:| -----------------------------|
| code            | string          | http response code value     |
| message         | string          | error description            |


```javascript
{
	"code": 401,
	"message": "Bad credentials"
}
```
<br>
##<span style="color:#FF6600;">Get voucher data</span>

###<span style="color:#0f6ab4;">`GET`</span> `/v1/voucher/{voucher_id}/details`

This method allows to get the voucher data for an specific reservation. The voucher_id required to get reservation is done in the new reservation response.

### _Header:_
Authorization: Bearer {JWT_TOKEN}

### _Request:_

| Parameters      | Type                | Required | Default  | Description              |
| --------------- |:--------------------| ---------| ---------| ------------------------ |
| voucher_id      | integer             | true     | null     | voucher id               |
| locale          | string              | false    | es_ES    | language to receive data |


### _Response:_

**200 Ok**

| Parameters               | Type             | Description                                  |
| ------------------------ | ---------------- | -------------------------------------------- |
| user_first_name          | string           | User first name                              |
| user_last_name           | string           | User last name                               |
| voucher_code             | string           | voucher code                                 |
| voucher_id               | integer          | voucher id (id used to cancel a reservation) |
| product                  | string           | product description i.e. "MULTIPASS 2 horas" |
| product_warning          | string           | warning description                          | 
| booking_state            | string           | status (CONFIRMED,CANCELLED,PENDING)         |
| from                     | string d/m/Y H:i | start date                                   |
| to                       | string d/m/Y H:i | end date                                     |
| parking_name             | string           | parking name                                 |
| parking_address          | string           | parking address                              |
| parking_zip              | string           | parking zip                                  |
| parking_city             | string           | parking city                                 |
| parking_province         | string           | parking province                             |
| parking_country          | string           | parking country                              |
| parking\_maximum\_height | string           | parking maximum height                       |
| parking_description      | string           | parking description                          |
| parking_instructions     | string           | parking instructions                         |
| parking_latitude         | float            | parking latitude                             |
| parking_longitude        | float            | parking longitude                            |
| voucher_description      | array | string   | voucher_description                          |
| amount                   | float            | total amount                                 |
| gross_price              | float            | gross price                                  |
| vat_price                | float            | vat price                                    |
| vehicle_type             | string           | vehicle type                                 |
| external_code            | string | integer | booking external code                        |
| external\_code\_type     | string           | external code type                           |
| external\_code\_tech     | string           | external code tech                           |
| extra_fields             | array            | all extra fields in booking                  |


```javascript
{
    "user_first_name": "test13",
    "user_last_name": "Uno13",
    "voucher_code": "L015DYZ",
    "voucher_id": 685443,
    "product": "MULTIPASS 2 horas",
    "product_warning": "Si la puerta no se abre, puedes utilizar el interfono para ponerte en contacto con los operarios del parking, te explicarán los pasos a seguir.",
    "booking_state": "CONFIRMED",
    "from": "01/04/2019 10:00",
    "to": "01/04/2019 12:00",
    "parking_name": "Hôtel de Ville - Paris",
    "parking_address": "6, quai de Gesvres",
    "parking_zip": "75004",
    "parking_city": "París",
    "parking_province": "Île-de-France",
    "parking_country": "FRANCE",
    "parking_maximum_height": "1.8 m.",
    "parking_description": "El parking Hôtel de Ville se encuentra en Paris, frente al puente de Notre-Dame, además está muy bien ubicado para visitar la Catedral y la Conciergerie de la famosa Isla de la Cité. También puedes ir al Centro Pompidou o a la Plaza Saint-Michel por el otro lado de la Isla. El parking Hôtel de Ville te permite aparcar cerca del Ayuntamiento de Paris y cerca del teatro del Châtelet. Este parking Hôtel de Ville representa un buen punto de partida para pasear a orillas del Sena hacia el Louvre y la Torre Eiffel. Regresar por metro resulta muy fácil, ya que solo tendrás que bajar a Châtelet (líneas 1, 4, 7, 11 y 14) o al Hôtel de Ville (líneas 1 y 11).",
    "parking_instructions": "A TU LLEGADA: coge el ticket. Aparca en cualquier plaza libre. Ve a la cabina de control con tu reserva Parclick y el ticket.\r\n\r\nPARA SALIR: utiliza el ticket que recogiste al entrar.",
    "parking_latitude": 48.8569651,
    "parking_longitude": 2.3496573,
    "voucher_description": {
        "0": "A TU LLEGADA: coge el ticket. Aparca en cualquier plaza libre. Ve a la cabina de control con tu reserva Parclick y el ticket.\r",
        "2": "PARA SALIR: utiliza el ticket que recogiste al entrar."
    },
    "amount": 6,
    "gross_price": "5.00",
    "vat_price": "1.00",
    "vehicle_type": "CAR",
    "external_code": "47000000000000081147",
    "external_code_type": "QR",
    "parking_access_point": {
        "id": 806
    },
    "external_code_tech": "qrcode_1",
    "extra_fields": {
        "1": {
            "label": "Marca del vehículo",
            "value": "saab"
        },
        "2": {
            "label": "Modelo del vehículo",
            "value": "93"
        }
    }
}
```


**401 Unauthorized**

| Parameters      | Type            | Required                     |
| --------------- |:---------------:| -----------------------------|
| code            | string          | http response code value     |
| message         | string          | error description            |


```javascript
{
	"code": 401,
	"message": "Bad credentials"
}
```

<br>

## <span style="color:#FF6600;">Workflows for booking reservation, cancel and list</span>


![alt text](https://static.parclick.com/docs/external_integration_grap.png)

###<span style="color:#FF6600;"> 1- Workflow to create a booking reservation</span>

1. Get the parking id and timespan (from, to)
2. Get the required fields for the selected parking using the parking endpoint  `/v1/parking/{parking_id}?locale=en_GB&group=detail` In the response, the _embedded node display the required fields for the seleted parking. 
3. Generate the token by calling the bestpass endpoint `/v1/parking/bestpass`. In the response get the provided token `2338bb72051ae11083a20cd94f3b3183ede3333708b1bc7b50e6af509100ef14` for each product available
4. Collect all required fields (email, token, product, firstName, lastName, from, to) to fill the New endpoint `v1/booking/new`

*Parking response, required fields example*

```javascript
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
```

*Get token via API pass*

```javascript
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
```

###<span style="color:#FF6600;">2. Workflow to cancel a booking</span>

1. To cancel a booking is mandatory to know the booking id (E.g. 685435) and then proceed.

<br>
###<span style="color:#FF6600;">3. Workflow to list bookings</a>

1. To list bookings you can filter with this available fields:


| Parameters      | Type                | Description                                                 |
| --------------- |:------------------- | ----------------------------------------------------------- |
| group           | string              | **list** to show details                                    |
| email           | string              | search by email in whole or in part                         |
| status          | string              | booking status available: not_modified, confirmed, canceled |
| firstName       | string              | string to find                                              |
| lastName        | string              | string to find                                              |
| from            | date YY-mm-dd       | from                                                        |
| to              | date YY-mm-dd       | to                                                          |

<br>
## <span style="color:#FF6600;">Generate entry codes</span>

Some car parks require the generation of access codes (QR Code, Barcode...) to gain access. Parclick handles the following types of access codes:

####<span style="color:#FF6600;">`qrcode_1`</a>

This QR code is used in SAEMES car parks and uses the following syntax to generate it.

<br>
####<span style="color:#FF6600;">`barcode_1`</a>

This barcode (type code39) is used in Parkia car parks and uses the following syntax to generate it.

<br>
####<span style="color:#FF6600;">`qrcode_2`</a>

This QR code is used in San Marcos car parks and uses the following syntax to generate it.

<br>
####<span style="color:#FF6600;">`barcode_2`</a>

This barcode (type code128) is used in Marco Polo car parks and uses the following syntax to generate it.

<br>
####<span style="color:#FF6600;">`code_1`</a>

This code is used in AENA car parks and uses the following syntax to generate it. This provider require the licence plate to identify and gain access.

<br>
####<span style="color:#FF6600;">`code_2`</a>

This code is used in Firenze Parcheggi car parks and uses the following syntax to generate it. This provider require the booking ID to identify and gain access.

<br>
####<span style="color:#FF6600;">`code_3`</a>

This code is used in Copark car parks and uses the following syntax to generate it. This provider require the external code to identify and gain access.

<br>
<p style="color:#999999;text-align:center;">2019 Copyright Parclick S.L. All rights reserved.</p>
