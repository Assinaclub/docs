# Voucher <!-- {docsify-ignore-all} -->

## Overview

> Service responsible for register and query vouchers.
> Vouchers are Transactions that use the Voucher Payment Method.


## NewVoucher
---
<span class="verb httpPOST">POST</span> ***/service/resources/vouchers***

---

### Headers

| Field | Value |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|

### Parameters 
> The parameters below must be sent in JSON format

| Attribute | Type    | Description                                    | Madatory | Size |
|---------------|------------|--------------------------------------------------------|-------------|---------|
| [**order**]   | **Object** | Order data                               | yes       |  -  |
| order_id      | string     | Order ID                                | yes     | 16 |
| amount        | numeric    | Order value (Voucher)   | yes       | -    |
| created_at    | datetime   | Date when Voucher was created | yes       | `Y-m-d H:i:s` |
| callback_url  | string     | Callback URL                   | yes       | 255 |
| [**seller**]  | **Object** | Seller data                            | yes       |  -   |
| cpf_cnpj      | string     | *Seller's* CPF or CNPJ document  | yes       | valid CPF or CNPJ |
| [**customer**]| **Object** | Customer data (purchaser) | yes       |  -   |
| name          | string     | Business name or Individual Person's name | yes       | 80      |
| cpf_cnpj      | string     | Customer's CPF or CNPJ document | no        | valid CPF or CNPJ |
| email         | string     | Customer's email address   | no        | 50      |
| phone         | string     | Customer's phone or cellphone number | no        | -       |
| birthdate     | date       | Customer's birthdate                 | no        | -       |
| [**customer**][**address**] | **Object** | Customer Address data | no        | -       |
| street        | string     | Public Place (Street, Avenue, etc...)     | no        | 70      |
| number        | string     | Address number                        | no        | 10      |
| district      | string     | Address district              | no        | 100     |
| complement    | string     | Address complement               | no        | 100     |
| city          | string     | Address city                              | no        | 50      |
| state         | string     | Address state                            | no        | 2       |
| zipcode       | string     | Address zip code                         | no        | 8       |


### Example of Content to be Sent
Check out in the example below the content that will be sent on request's body.

```json
{
    "order": {
        "order_id": "1000077",
        "amount": 499.99,
        "created_at": "2020-08-03 21:45:10",
        "callback_url": "https://meusite.com.br/retorno"
    },
    "seller": {
        "cpf_cnpj": "799.998.833-01"
    },
    "customer": {
        "name": "FULANO DA SILVA",
        "cpf_cnpj": "949.373.210-05",
        "email": "fulano@mail.me",
        "phone": "(11) 99780-1000",
        "birthdate": "1990-10-10",
        "address": {
            "street": "Av. Brasil",
            "number": "1000",
            "district": "Centro",
            "complement": "Ap 451",
            "city": "São Paulo",
            "state": "SP"
        }
    }
}
```

### Return Status
| Code | Description                                                  |
| ---- | ------------------------------------------------------------ |
| 401  | Not authenticated. Check if Basic Authentication keys had been correctly informed. |
| 403  | Not authorized. Your account has no permission to complete this action. |
| 406  | Not accepted. Some sent data can be invalid, verify the data sent on request body. |
| 201  | Resource created.                                            |


#### In case of Success

```json
{
  "id": 1011957,
  "uuid": "166a3751923064f757e01091a9659fe9",
  "resource": "vouchers",
  "attributes": {
    "seller": {
      "id": 23642,
      "uuid": "25044c8f6892d2f084f72025c377023d",
      "resource": "sellers",
      "attributes": {
        "is_active": true,
        "name": "Joseferson Mattias",
        "cpf_cnpj": "799.993.388-01"
      }
    },
    "order_id": "1000077",
    "amount": 499.99,
    "installments": 1,
    "tid": "a40f90f9-029f-40b1-9d07-9bf36001a447",
    "authorization_id": "",
    "status": {
      "code": 8,
      "message": "Aprovado e Capturado"
    },
    "method": "voucher",
    "captured_amount": 499.99,
    "captured_at": "2020-08-07 13:57:45",
    "callback_url": "https://meusite.com.br/retorno",
    "customer": {
      "name": "FULANO DA SILVA",
      "cpf_cnpj": "949.373.210-05",
      "email": "fulano@mail.me",
      "phone": "(11) 99780-1000",
      "address": {
        "street": "",
        "number": "",
        "district": "",
        "complement": "",
        "city": "",
        "state": "",
        "zip_code": ""
      }
    },
    "products": [],
    "history": [
      {
        "amount": 499.99,
        "type": "created",
        "status": "succeeded",
        "response_code": "",
        "response_message": "",
        "authorization_code": "",
        "authorization_id": "a40f90f9-029f-40b1-9d07-9bf36001a447",
        "authorization_nsu": "20200807135745256",
        "created_at": "2020-08-07 13:57:45"
      },
      {
        "amount": 499.99,
        "type": "captured",
        "status": "succeeded",
        "response_code": "",
        "response_message": "",
        "authorization_code": "",
        "authorization_id": "a40f90f9-029f-40b1-9d07-9bf36001a447",
        "authorization_nsu": "20200807135745256",
        "created_at": "2020-08-07 13:57:45"
      }
    ],
    "created_at": "2020-08-07 13:57:45",
    "updated_at": "2020-08-07 13:57:45"
  }
}
```

#### In case of Failure

```json
{
  "code": "406",
  "message": {
    "seller.cpf_cnpj": [
      "Seller.cpf Cnpj is not a valid CPF/CNPJ."
    ]
  },
  "resource": "store.voucher"
}
```

## Cancel Voucher
Soon...

## Query Voucher
Soon...

## List Vouchers
Soon...

## List Vouchers by Seller (CPF/CNPJ)
Soon...