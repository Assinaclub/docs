# Checkout <!-- {docsify-ignore-all} -->

## Overview

> Service responsible for creating Checkouts

## Create Checkout
---
<span class="verb httpPOST">POST</span> ***/service/resources/checkout***

---

### Headers

| Field | Value |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|

### Parameters
> The parameters below must be sent in JSON format

| Attribute | type | Description | Mandatory | Size |
|-|-|-|-|-|
| [**customer**]&#9660; | **Object** | Customer data (purchaser) | yes | - |
| customer.name | string | Customer name | yes | 80 |
| customer.tax_receipt | string | Customer's CPF or CNPJ | yes | `cpf` or `cnpj` |
| customer.email | string | Customer email | no | 80 |
| customer.phone | string | Customer phone or cellphone number | no | 10 or 11 |
| customer.birthdate | string | Customer birth date | no | `Y-m-d` or `d/m/Y` |
| [**address**]&#9660; | **Object** | Customer's Address data | no | - |
| address.street | string | Public Place (Street,   Avenue, etc) | no | 100 |
| address.number | string | Address number | no | 10 |
| address.district | string | Address district | no | 80 |
| address.complement | string | Address complement | no | 50 |
| address.city | string | Address city | no | 80 |
| address.state | string | Address state | no | 2 |
| address.zip_code | string | Address zip code | no | 8 |
| [**installment_setting**]&#9660; | **Object** | Installment settings | no | - |
| installment_setting.max_installments | numeric | Max number of Installments | no | 1 ~ 12 |
| installment_setting.min_installment_value | numeric | Minimum Installment value | no | - |
| installment_setting.interest | numeric | Interest fee (in percentage) | no | - |
| installment_setting.interest_free_installments | numeric | Installment quantity without interest fee | no | 1 ~ 12 |
| [**order**]&#9660; | **Object** | Order data | yes | - |
| order.order_id | string | Order ID (defined by the Store) | no | 16 |
| order.amount | numeric | Order value (price) | yes | - |
| order.return_url | string | Callback URL | no | 255 |
| order.return_type | string | Callback type | yes | `json` or `xml` |
| [**products**]&#9660; | **List** | Product list | no | - |
| products.*.name | string | Product name | no | 255 |
| products.*.unit_price | numeric | Product's unit price | no | - |
| products.*.quantity | numeric | Product quantity | no | - |
| products.*.sku | string | Product code | no | 50 |
| products.*.description | string | Product description | no | 255 |
| [**split_rules**]&#9660; | **List** | Split rules list | no | - |
| split_rules.*.receiver | string | Seller's unique ID (Marketplace) | yes, if you want to create rule | 50 |
| split_rules.*.percentage | numeric | Percentage that will be discounted form Transaction value | no | - |
| split_rules.*.amount | numeric | Fixed value that will be discounted in Transaction Value | no | - |
| seller_id | string | Seller ID (used to make Transactions in seller's name) | no | 50 |
| expires_in | numeric | Quantity of minutes that the Link will take to expire. [`Standard 1440 minutes`] | no | - |

### Example of Content to be Sent
Check out in the examples below the content that can be sent in request's body.

```json
{
    "customer": {
        "name": "Fulano da Silva",
        "tax_receipt": "212.348.796-11",
        "email": "teste@email.com",
        "phone": "(11) 2222-3333",
        "birthdate": "1987-11-21",
        "address": {
            "zip_code": "01156-060",
            "street": "Rua Júlio Gonzalez",
            "number": "1023",
            "district": "Barra Funda",
            "complement": "Sala 02",
            "city": "São Paulo",
            "state": "SP"
        }
    },
    "installment_setting": {
        "max_installments": 12,
        "min_installment_value": 5,
        "interest": 0,
        "interest_free_installments": 12
    },
    "order": {
        "order_id": "100001",
        "amount": "15.00",
        "return_url": "https:\/\/www.loja.com.br\/callback",
        "return_type": "json"
    },
    "products": [
        {
            "name": "Caneca",
            "unit_price": "15.00",
            "quantity": 1,
            "sku": "CAN123",
            "description": "Caneca verde"
        },
        {
            "name": "Camiseta",
            "unit_price": "30.00",
            "quantity": 1,
            "sku": "CAM145G"
        }
    ],
    "split_rules": [
        {
            "receiver": "qwertyuiopasdfghjklzxcvbnm123456",
            "percentage": "50"
        },
        {
            "receiver": "654321mnbvcxzlkjhgfdsapoiuytrewq",
            "percentage": "20"
        }
    ],
    "expires_in": 60
}
```


### Structure of Response Content
Check out in the example below this service's response content structure.

| Attribute | Type | Description |
|:-:|:-:|:-:|
| id | numeric | Checkout ID |
| resource | string | Resource Type |
| [**attributes**]&#9660; | **Object** | Checkout data |
| token | string | Checkout Token |
| link | string | Checkout URL |
| expires_at | date | Checkout expire date |
| created_at | date | Checkout creation date |
| updated_at | date | Date when Checkout was last updated |

#### In case of success

```json
{
  "id": 154,
  "resource": "checkout",
  "attributes": {
    "token": "v4b7043bv01938n1498n0809bh0a9834",
    "link": "https://api.ipag.com.br/vpos?checkout=v4b7043bv01938n1498n0809bh0a9834",
    "created_at": "2020-11-05 15:59:13",
    "created_at": "2020-11-05 14:59:13",
    "updated_at": "2020-11-05 14:59:13"
  }
}
```