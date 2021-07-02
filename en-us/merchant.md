# Establishment <!-- {docsify-ignore-all} -->

## Overview

> Service responsible for registering new Establishments

!>	Allowed only on Master Plan Accounts

This service performs the accreditation process of a Commercial Establishment.

## New Establishment

---
<span class="verb httpPOST">POST</span> ***/service/resources/establishments***

---

### Headers

| Content-Type  | application/json |
| ------------- | ---------------- |
| **Authorization** | **Basic** `token` |

### Parameters
> The parameters below must be sent in JSON format

| Attribute     | Type       | Description                                                  | Mandatory | Size                     |
| ------------- | ---------- | ------------------------------------------------------------ | --------- | ------------------------ |
| name          | string     | Name of the Legal Entity or Individual Person                | yes       | 80                       |
| email         | string     | Email adress. It will be used as account login if no `login` is informed | yes       | 50                       |
| login         | string     | It will be used as account login (Alphanumeric)               | no       | 50                       |
| password      | string     | Password that will be used on iPag's Panel access            | yes       | 6-20                     |
| document      | string     | CPF or CNPJ number                                           | yes       | -                        |
| phone         | string     | Phone or Cellphone number                                    | yes       | -                        |
| **[address]** | **Object** | Address data                                                 | yes       | -                        |
| street        | string     | Public Place (Street, Avenue, etc...)                        | yes       | 70                       |
| number        | string     | Address number                                               | yes       | 10                       |
| district      | string     | Address district                                             | yes       | 100                      |
| complement    | string     | Address complement                                           | yes       | 100                      |
| city          | string     | Address city                                                 | yes       | 50                       |
| state         | string     | Address state                                                | yes       | 2                        |
| zipcode       | string     | Address zip code                                             | yes       | 8                        |
| **[owner]**   | **Object** | **Owner or business manager data**                           | -         | -                        |
| name          | string     | Owner/business manager name                                  | no        | 80                       |
| email         | string     | Owner/business manager email                                 | no        | 50                       |
| cpf           | string     | Owner/business manager CPF document                          | no        | -                        |
| phone         | string     | Owner/business manager phone                                 | no        | -                        |
| **[bank]**    | **Object** | **Company's Bank data **                                     | -         | -                        |
| code          | string     | Bank code                                                    | no        | 3                        |
| agency        | string     | Agency number                                                | no        | 4                        |
| account       | string     | Account number                                               | no        | 10                       |
| type          | string     | Account type                                                 | no        | 'checkings' or 'savings' |
| external_id   | string     | External account ID (Example: email registered on PagSeguro) | no        | 120                      |

### Example of Content to be sent
Check out the example below the content that will be sent on request's body.

```json
{
    "name": "Academia Interestelar 47",
    "email": "adm@academia.me",
    "password": "123456",
    "document": "72.032.656/0001-05",
    "phone": "(11) 5555-5555",
    "address": {
        "street": "Rua Mariano Lunar",
        "number": "100",
        "district": "Centro",
        "complement": "APTO 412",
        "city": "São Paulo",
        "state": "SP",
        "zipcode": "01156000"
    }
}
```

### Return Status

| Code | Description |
| -------| ------ |
| 401 | Not authenticated. Check if Basic Authentication keys had been correctly informed. |
| 403 |	Not authorized. Your account has no permission to complete this action. |
| 406 |	Not accepted. Some sent data can be invalid, verify the data sent on request body. |
| 201 |	Resource created. |

### Response Content Structure
Check out in the example below this service's response content structure

| Attribute        | Type       | Description                                                  |
| ---------------- | ---------- | ------------------------------------------------------------ |
| id               | string     | Establishment ID on system                                   |
| resource         | string     | Resource type                                                |
| **[attributes]** | **Object** | **Object with the resources attributes**                     |
| is_active        | boolean    | Active (true) or inactive (false) resource in system         |
| name             | string     | Individual Person name or Trade name                         |
| business_name    | string     | Business name                                                |
| document         | string     | CPF or CNPJ                                                  |
| email            | string     | Email address                                                |
| phone            | string     | Phone or Cellphone number                                    |
| created_at       | date       | Date when the resource was created                           |
| updated_at       | date       | Last date when the resource was updated                      |
| **[address]**    | **Object** | **Object with the address attributes**                       |
| street           | string     | Public Place (street, Avenue, Etc).                          |
| number           | string     | Address number                                               |
| district         | string     | Address district                                             |
| complement       | string     | Address Complement                                           |
| city             | string     | Address city                                                 |
| state            | string     | Address state                                                |
| zipcode          | string     | Address zip code                                             |
| **[owner]**      | **Object** | **Object with the business manager  attributes or the owner of the bussiness** |
| name             | string     | Owner/business manager name                                  |
| cpf              | string     | Owner/business manager CPF number                            |
| phone            | string     | Owner/business manager phone                                 |
| **[bank]**       | **Object** | **Company's Bank Data**                                      |
| code             | string     | Bank code                                                    |
| agency           | string     | Agency number                                                |
| account          | string     | Account number                                               |
| type             | string     | Account type                                                 |
| external_id      | string     | External account ID (Example: email registered on PagSeguro) |

### Response Content
Check out in the examples below the response content in this service.

#### In case of success

```json
{
    "id": "9bd90fed98b9f7f1e9024b13e758c45a",
    "resource": "establishments",
    "attributes": {
    "is_active": true,
    "name": "Academia Interestelar 47",
    "bussiness_name": "Academia Interestelar 47",
    "document": "72.032.656/0001-05",
    "email": "adm@academia.me",
    "phone": "(11) 5555-5555",
        "address": {
        "street": "Rua Mariano Lunar",
        "number": "100",
        "district": "Centro",
        "complement": "APTO 412",
        "city": "São Paulo",
        "state": "SP",
        "zipcode": "01156000"
    },
    "owner": [],
    "created_at": "2020-01-28",
    "updated_at": "2020-01-28"
    }
}
```

#### In case of failure

```json
{
    "code": "406",
    "message": {
        "document": [
            "Document is not a valid CPF\/CNPJ."
        ]
    },
    "resource": "resource.establishments.store"
}
```

## Update Establishment 
<span class="verb httpPUT">PUT</span> ***/service/resources/establishments?id={id}***

Recebe os mesmos atributos do credenciamento, com excessão da senha e e-mail.

## Query Establishment 
<span class="verb httpGET">GET</span> ***/service/resources/establishments?id={id}***

## List Establishments
<span class="verb httpGET">GET</span> ***/service/resources/establishments***