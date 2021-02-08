# Customer <!-- {docsify-ignore-all} -->

## Overview

> Service responsible for managing Customers

## New Customer
---
<span class="verb httpPOST">POST</span> ***/service/resources/customers***

---
### Header

| Field | Value |
| ------------ | ------ |
| Content-Type | application/json |

### Parameters
> The parameters below must be sent in JSON format

| Attribute | Type | Description | Mandatory | Size |
|:-:|:-:|:-:|:-:|:-:|
| nome | string | Customer name | Yes | 100 |
| cpf_cnpj | string | Customer's CPF or CNPJ | No | `cpf`or `cnpj` |
| email | string | Customer email | No | 80 |
| phone | string | Customer phone | No | 10 or 11 |
| [**address**]&#9660; | **Object** | Customer address data | No | - |
| address.street | string | Customer address street | No | 100 |
| address.number | string | Customer address number | No | 10 |
| address.district | string | Customer address district | No | 80 |
| address.complement | string | Customer address complement | No | 50 |
| address.city | string | Customer address city | No | 80 |
| address.state | string | Customer address state | No | 2 |
| address.zipcode | string | Customer address zip code | No | 8 |

### Example of content to be sent
Check out the Content that you can send on the body of the request below.

```json
{
  "name": "fulano da silva",
  "cpf_cnpj": "289.610.390-24",
  "email": "fulano@mail.me",
  "phone": "(11) 91234-1200",
  "address": {
    "street": "Rua Marajá",
    "number": "123",
    "district": "Jd. Filadelfia",
    "complement": "",
    "city": "Presidente Prudente",
    "state": "SP",
    "zipcode": "19030-130"
  }
}
```

### Return Status
| Code | Description                                                  |
| ---- | ------------------------------------------------------------ |
| 401  | Not authenticated. Check if Basic Authentication keys had been correctly informed. |
| 403  | Not authorized. Your account has no permission to complete this action. |
| 406  | Not accepted. Some sent data can be invalid, verify the data sent on request body. |
| 201  | Resource Created.                                            |

### Response content structure
Check in the example below this service's response content structure

| Attribute | Type | Description |
|:-:|:-:|:-:|
| id | numeric | Customer ID |
| uuid | string | Unique Customer ID |
| resource | string | Resource type |
| [**attributes**]&#9660; | **Object** | Customer data |
| is_active | boolean | Active (true) or deactive (false) Customer |
| nome | string | Customer name |
| cpf_cnpj | string | Customer's CPF or CNPJ |
| email | string | Customer's email |
| phone | string | Customer's phone |
| created_at | date | Customer Registration date |
| updated_at | date | Last Customer data updated date |
| [**address**]&#9660; | **Object** | Customer Address data |
| address.street | string | Customer Address street |
| address.number | string | Customer Address number |
| address.district | string | Customer Address district |
| address.complement | string | Customer Address complement |
| address.city | string | Customer Address city |
| address.state | string | Customer Address state |
| address.zipcode | string | Customer Address zip code |

#### In case of success

```json
{
  "id": 23809,
  "uuid": "bc59e38bc67f18b4ab36cd450302b8c6",
  "resource": "customers",
  "attributes": {
    "is_active": true,
    "name": "fulano da silva",
    "cpf_cnpj": "289.610.390-24",
    "email": "fulano@mail.me",
    "phone": "(11) 91234-1200",
    "address": {
      "street": "Rua Marajá",
      "number": "123",
      "district": "Jd. Filadelfia",
      "complement": "",
      "city": "Presidente Prudente",
      "state": "SP",
      "zipcode": "19030-130"
    },
    "created_at": "2020-10-08",
    "updated_at": "2020-10-08"
  }
}
```

#### In case of failure

```json
{
  "code": "406",
  "message": {
    "address.zipcode": [
      "Address.zip Code is required"
    ]
  },
  "resource": "service.customers.create"
}
```
## Update Customer
<span class="verb httpPUT">PUT</span> ***/service/resources/customers?id={id}***

### Header

| Field | Value |
| ------------ | ------ |
| Content-Type | application/json |

### Example of Content to be sent
Check out the Content that you can send on the body of the request below.

```json
{
    "name" : "Fulano de Oliveira"
}
```

### In case of success

```json
{
  "id": 23809,
  "uuid": "bc59e38bc67f18b4ab36cd450302b8c6",
  "resource": "customers",
  "attributes": {
    "is_active": true,
    "name": "Fulano de Oliveira",
    "cpf_cnpj": "289.610.390-24",
    "email": "fulano@mail.me",
    "phone": "(11) 91234-1200",
    "address": {
      "street": "Rua Marajá",
      "number": "123",
      "district": "Jd. Filadelfia",
      "complement": "",
      "city": "Presidente Prudente",
      "state": "SP",
      "zipcode": "19030-130"
    },
    "created_at": "2020-10-08",
    "updated_at": "2020-10-08"
  }
}
```

## Customer Query
---
<span class="verb httpGET">GET</span> ***/service/resources/customers?id={id}***

---

> [Request Response](pt-br/customer?id=em-caso-de-sucesso)

## List Customers
---
<span class="verb httpGET">GET</span> ***/service/resources/customers***

---

## Delete Customer
---
<span class="verb httpDELETE">DELETE</span> ***/service/resources/customers?id={id}***

---

> In case of success the **Response** will be `200 OK`

!> A customer only will be able to be deleted if there isn't any resource using it.
