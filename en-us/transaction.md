# Transaction <!-- {docsify-ignore-all} -->

## Overview

> Service responsible for listing and query transactions.

## List Transactions
---
<span class="verb httpGET">GET</span> ***/service/resources/transactions***

---

### Headers

| Field | Value |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|

### Query Params (Filters)

| Attribute      | Type   | Description                        |
|----------------|--------|------------------------------------|
| transaction_id | number | Transaction Unique ID              |
| order_id       | string | Transaction Order ID               |
| status         | number | Transaction Status (de `1` a `10`) |
| customer_name  | string | Customer Name                      |
| method         | string | Payment Method                     |
| from_date      | date   | Transaction Initial Date filter (`Y-m-d`)  |
| to_date        | date   | Transaction Final Date filter (`Y-m-d`)   |
| from_amount    | number | Transaction Inital Range Amount    |
| to_amount      | number | Transaction Final Range Amount     |
| order          | string | Result Ordering                    |

### Return Status
| Code | Descrição                                                    |
| ------ | ------------------------------------------------------------ |
| 401    | Not authenticated. Check if Basic Authentication keys had been correctly informed. |
| 403    | Not authorized. Your account has no permission to complete this action. |
| 201    | Resource created.                                            |

### Response Content Structure
Check out in the example below this service's response content structure

|   Attribute    |   Type   |   Description                           |
|-----------------------|------------|-----------------------------------------------|
|   data            |   Array     |   Transactions List   |
|   transação       |   Object    |   Transaction Object     |

### Query Params (Filters)

|  Attribute  |   Type   |   Description           |
|-------------|----------|----------------------------|
|   from_date    |   date  |  Date Filter (starts) (yyyy-mm-dd)  |
|   to_date      |   date  | Date Filter (end) (yyyy-mm-dd) |
|   status         |   number  |  Status Filter (1 ~ 10)  |
|   limit        |   number  |  Limit of returned Transactions [standard: 15]  |
|   order        |   string  | Result Sort ('asc') |
|   page         |   number  |  Selected Page  |

### Response Content
Check ot in the example below this service's response content 

In case of success:
```json
{
    "data": [
        {
            "id": 1011823,
            "uuid": "5851de4d1b821953454733521bf09700",
            "resource": "transactions",
            "attributes": {
                "order_id": "1011823",
                "amount": 529.19,
                "installments": 3,
                "tid": "20200629174147590",
                "authorization_id": "",
                "status": {
                    "code": 5,
                    "message": "Autorizado"
                },
                "method": "visa",
                "captured_amount": 0,
                "captured_at": "0000-00-00 00:00:00",
                "acquirer": "simulated",
                "acquirer_message": "transaction approved",
                "acquirer_code": "",
                "url_authentication": "",
                "callback_url": "",
                "card": {
                    "holder": "FULANO",
                    "number": "411111******1111",
                    "expiry_month": "12",
                    "expiry_year": "2022",
                    "brand": "visa"
                },
                "customer": {
                    "name": "Fulano da Silva",
                    "cpf_cnpj": "799.993.388-01",
                    "email": "fulano@mail.me",
                    "phone": "(12) 31232-3121",
                    "address": {
                        "street": "RUA JULIO GONZALEZ",
                        "number": "100",
                        "district": "BARRA FUNDA",
                        "complement": "",
                        "city": "SAO PAULO",
                        "state": "SP",
                        "zip_code": "01156060"
                    }
                },
                "products": [],
                "antifraud": null,
                "history": [
                    {
                        "amount": 529.19,
                        "type": "created",
                        "status": "succeeded",
                        "response_code": "",
                        "response_message": "",
                        "authorization_code": "",
                        "authorization_id": "",
                        "authorization_nsu": "",
                        "created_at": "2020-06-29 17:41:47"
                    },
                    {
                        "amount": 529.19,
                        "type": "authorized",
                        "status": "succeeded",
                        "response_code": "0",
                        "response_message": "transaction approved",
                        "authorization_code": "",
                        "authorization_id": "20200629174147590",
                        "authorization_nsu": "3cfafe98-15fd-4122-a248-d73ef486ccf2",
                        "created_at": "2020-06-29 17:41:47"
                    }
                ],
                "created_at": "2020-06-29 17:41:47",
                "updated_at": "2020-06-29 17:41:47"
            }
        },
        {
            "id": 1011822,
            "uuid": "bb2f33940ce8183e454f61099a882814",
            "resource": "transactions",
            "attributes": {
                "order_id": "1011822",
                "amount": 582.96,
                "installments": 3,
                "tid": "20200629173856552",
                "authorization_id": "",
                "status": {
                    "code": 5,
                    "message": "Autorizado"
                },
                "method": "visa",
                "captured_amount": 0,
                "captured_at": "0000-00-00 00:00:00",
                "acquirer": "simulated",
                "acquirer_message": "transaction approved",
                "acquirer_code": "",
                "url_authentication": "",
                "callback_url": "",
                "card": {
                    "holder": "FULANO",
                    "number": "411111******1111",
                    "expiry_month": "02",
                    "expiry_year": "2022",
                    "brand": "visa"
                },
                "customer": {
                    "name": "Fulano da Silva",
                    "cpf_cnpj": "799.993.388-01",
                    "email": "fulano@mail.me",
                    "phone": "(12) 31232-3121",
                    "address": {
                        "street": "RUA JULIO GONZALEZ",
                        "number": "100",
                        "district": "BARRA FUNDA",
                        "complement": "",
                        "city": "SAO PAULO",
                        "state": "SP",
                        "zip_code": "01156060"
                    }
                },
                "products": [
                    {
                        "name": "TESTE",
                        "unit_price": 529,
                        "quantity": 1,
                        "sku": "20200629173750"
                    }
                ],
                "antifraud": null,
                "history": [
                    {
                        "amount": 582.96,
                        "type": "created",
                        "status": "succeeded",
                        "response_code": "",
                        "response_message": "",
                        "authorization_code": "",
                        "authorization_id": "",
                        "authorization_nsu": "",
                        "created_at": "2020-06-29 17:38:55"
                    },
                    {
                        "amount": 582.96,
                        "type": "authorized",
                        "status": "succeeded",
                        "response_code": "0",
                        "response_message": "transaction approved",
                        "authorization_code": "",
                        "authorization_id": "20200629173856552",
                        "authorization_nsu": "421b05c8-9675-424b-9b56-b8e1a0858d9b",
                        "created_at": "2020-06-29 17:38:56"
                    }
                ],
                "created_at": "2020-06-29 17:38:55",
                "updated_at": "2020-06-29 17:38:56"
            }
        }
    ],
    "links": {
        "first": "https://api.pluscommercebr.com.br/service/resources/transactions?page=1",
        "last": "https://api.pluscommercebr.com.br/service/resources/transactions?page=30",
        "prev": null,
        "next": "https://api.pluscommercebr.com.br/service/resources/transactions?page=2"
    },
    "meta": {
        "current_page": 1,
        "last_page": 30,
        "from": 1,
        "to": 2,
        "per_page": "2",
        "total": 130
    }
}
```
### In case of any transaction found:

```json
{
    "data": []
}
```

## Query Transaction

---
<span class="verb httpGET">GET</span> ***/service/resources/transactions?id={id}***

---

### Query Params

|  Attribute  |   Type   |   Description         |
|-------------|----------|----------------------------|
|   id        |   string  |  Transaction ID  |
|   uuid      |   string  |  Unique Transaciton ID  |

### Response Content Structure
Check in the example below this service's response content structure

|   Attribute     |   Type   |   Description                               |
|-----------------------|------------|-----------------------------------------------------|
|   id                  |   string   |   System Transaction ID   |
|   uuid                |   string   | Unique System Transaction ID (UUID). |
|   resource            |   string   |   Resource Type   |
|   **[attributes]**        |   **Object**   |   **Object with the Resource's Attributes**   |
|   order_id            |   string   |   Order ID                  |
|   amount            |   number   |  Transaction value           |
|   installments            |   number   |   Installments number   |
|   tid            |   string   |   Transaction TID                  |
|   authorization_id            |   string   |   Authorization ID   |
|   **status**            |   **Object**   |     Transaction Status     |
|   method            |   string   |   Transaction method used   |
|   captured_amount            |   number   |   Captured value       |
|   captured_at            |   date   |   Capture date              |
|   acquirer            |   string   |   Acquirer          |
|   acquirer_message            |   string   |  Acquirer response message  |
|   acquirer_code            |   string   |   Acquirer response code   |
|   url_authentication            |   string   | External URL to Authentication or Bank Slip Link |
|   callback_url            |   string   |   Callback return URL   |
|   card            |   string   |   Resource Type                 |
|   **customer**            |   **Object**   |   Customer Data   |
|   **products**            |   **Object**   |   Products data   |
|   **antifraud**            |   **Object**   |   Anti fraud system data   |
|   **history**            |   **Object**   |  Transaction History data  |
|   created_at            |   date   |   Resource creation date   |
|   updated_at            |   date   |   Resource update date   |


### Response content
Check out in the example below this service's response content

In case of success:

```json
{
    "id": 1011823,
    "uuid": "5851de4d1b821953454733521bf09700",
    "resource": "transactions",
    "attributes": {
        "order_id": "1011823",
        "amount": 529.19,
        "installments": 3,
        "tid": "20200629174147590",
        "authorization_id": "",
        "status": {
            "code": 5,
            "message": "Autorizado"
        },
        "method": "visa",
        "captured_amount": 0,
        "captured_at": "0000-00-00 00:00:00",
        "acquirer": "simulated",
        "acquirer_message": "transaction approved",
        "acquirer_code": "",
        "url_authentication": "",
        "callback_url": "",
        "card": {
            "holder": "FULANO",
            "number": "411111******1111",
            "expiry_month": "12",
            "expiry_year": "2022",
            "brand": "visa"
        },
        "customer": {
            "name": "Fulano da Silva",
            "cpf_cnpj": "799.993.388-01",
            "email": "fulano@mail.me",
            "phone": "(12) 31232-3121",
            "address": {
                "street": "RUA JULIO GONZALEZ",
                "number": "100",
                "district": "BARRA FUNDA",
                "complement": "",
                "city": "SAO PAULO",
                "state": "SP",
                "zip_code": "01156060"
            }
        },
        "products": [
            {
                "name": "TESTE",
                "unit_price": 529.19,
                "quantity": 1,
                "sku": "20200629174110"
            }
        ],
        "antifraud": null,
        "history": [
            {
                "amount": 529.19,
                "type": "created",
                "status": "succeeded",
                "response_code": "",
                "response_message": "",
                "authorization_code": "",
                "authorization_id": "",
                "authorization_nsu": "",
                "created_at": "2020-06-29 17:41:47"
            },
            {
                "amount": 529.19,
                "type": "authorized",
                "status": "succeeded",
                "response_code": "0",
                "response_message": "transaction approved",
                "authorization_code": "",
                "authorization_id": "20200629174147590",
                "authorization_nsu": "3cfafe98-15fd-4122-a248-d73ef486ccf2",
                "created_at": "2020-06-29 17:41:47"
            }
        ],
        "created_at": "2020-06-29 17:41:47",
        "updated_at": "2020-06-29 17:41:47"
    }
}
```

In case of Transaction not found:

```json
{
    "code": "404",
    "message": "Transaction Not Found",
    "resource": "resource.transaction.view"
}
```

## Release Transaction Receivables
---
<span class="verb httpPOST">POST</span> ***/service/transaction/release_receivables***

---

### Example of Content to be sent
Check out the example below the content that will be sent on request's body.

```json
{
	"seller_id": 123456,
	"transaction_id": 1011000
}
```

### Return Status
| Code | Descrição                                                    |
| ------ | ------------------------------------------------------------ |
| 401    | Not authenticated. Check if Basic Authentication keys had been correctly informed. |
| 403    | Not authorized. Your account has no permission to complete this action. |
| 404    | Resource Not Found.
| 200    | Receivables Released.