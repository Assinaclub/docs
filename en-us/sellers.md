# Seller <!-- {docsify-ignore-all} -->

## Overview

> Service responsible for registering new sellers in your Marketplace

!> Allowed only in Enterprise Plan and Master Plan accounts

Sellers can be an Individual Person or a Legal Entity, and they are Marketplace partners.

## Register New Seller
---
<span class="verb httpPOST">POST</span> ***/service/resources/sellers***

---

### Headers

| Field | Value |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|

### Parameters
> The parameters below must be sent in JSON format

|   Attribute    |   Type   |   Description                                       |  Mandatory  |  Size  |
|-----------------------|-----------|-------------------------------------------------------------|----------------|------------|
|   login               |   string  |   Login Access for Seller Account   |   yes        |   50       |
|   password            |   string  |   Password Access for Seller Account   |   yes       |   20       |
|   name                |   string  |   Individual Person's name or Business name   |   yes        |   80       |
|   cpf_cnpj            |   string  |   Seller's CPF or CNPJ                        |   yes        |   -        |
|   email               |   string  |   Email address. It will be used as account's login   |   yes       |   50       |
|   phone               |   string  |   Phone or Cellphone number          |   yes        |   -        |
|   description         |   string  |   Individual's description or Company's description   |   no         |   255      |
|   **[address]**         |   **Object**  |   **Address data**                      |   -          |   -        |
|   street              |   string  |   Public Place (Street, Avenue, Etc).        |   no         |   70       |
|   number              |   string  |   Address number                           |   no         |   10       |
|   district            |   string  |   Address district                 |   no         |   100      |
|   complement          |   string  |   Address complement                  |   no         |   100      |
|   city                |   string  |   Address city                                |   no         |   50       |
|   state               |   string  |   Address state                              |   no         |   2        |
|   zipcode             |   string  |   Address zip code                            |   no         |   8        |
|   **[owner]**           |   **Object**  | **Seller or business manager data** |   -          |   -        |
|   name                |   string  | Seller/business manager name |   no         |   80       |
|   email               |   string  | Seller/business manager email address |   no         |   50       |
|   cpf                 |   string  | Seller/business manager CPF document |   no         |   -        |
|   phone               |   string  | Seller/business manager phone number |   no         |   -        |
|   **[bank]**            |   **Object**  |   **Company's Bank or Seller Bank data**   |   -          |   -        |
|   code                |   string  |   Bank code                                  |   no         |   3        |
|   agency              |   string  |   Agency number                          |   no        |   4        |
|   account             |   string  |   Account number                     |   no         |   10       |
|   type                |   string  |   Account type                                 |   no         |  'checkings' or 'savings' |
|   external_id         |   string  | External account ID (Example: email registered on PagSeguro)  |   no         |   120   |

### Example of Content to be sent
Check out the example below the content that will be sent on request's body.

```json
{
    "login": "josefrancisco",
    "password": "123123",
    "name":"José Francisco Silva",
    "cpf_cnpj": "854.508.440-42",
    "email": "jose@mail.me",
    "phone": "(11) 98712-1234",
    "description": "",
    "address": {
        "street": "Rua Júlio Gonzalez”,
        "number": "1000",
        "district": "Barra Funda",
        "complement": "10 Andar Sala 3",
        "city": "São Paulo",
        "state": "SP",
        "zipcode": "01156060"
    },
    "owner": {
        "name": "Giosepe",
        "email": "giosepe@teste.com",
        "cpf": "799.993.388-01",
        "phone": "(11) 91333-9876",
        "birthdate": "1988-03-29"
    },
    "bank": {
        "code": "290",
        "agency": "0001",
        "account": "100500",
        "type": "checkings",
        "external_id": "teste@mail.me"
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

### Response Content Structure
Check out in the example below this service's response content structure.

|   Attribute   |   Type   |   Description                                               |
|-----------------|------------|---------------------------------------------------------------------|
|   id            |   string   |   Seller's ID on system    |
|   resource      |   string   |   Resource type                                      |
|   [**attributes**]  |   **Object**   |   **Object with the resource attributes**   |
|   is_active     |   boolean  |   Tells if this resource is active or not in the system   |
|   login         |   string   |   Account's Login Access                     |
|   name          |   string   | Seller (Individual Person) name or Company's Trade name |
|   cpf_cnpj      |   string   |   Seller CPF or CNPJ number                             |
|   type          |   string   |   Seller type (individual person or Legal Entity)   |
|   email         |   string   |   Email address                                    |
|   phone         |   string   |   Phone number                          |
|   description   |   string   |   Seller Description                            |
|   created_at    |   date     |   Date when the resource was created   |
|   updated_at    |   date     |   Last date when the resource was updated   |
|   [**address**]     |   **Object**   |   **Object with the address attributes**   |
|   street        |   string   |   Public Place (Street, Avenue, Etc).               |
|   number        |   string   |   Address Number                                  |
|   district      |   string   |   Address District                      |
|   complement    |   string   |   Address Complement                          |
|   city          |   string   |   Address City                                     |
|   state         |   string   |   Address State (Initials)                            |
|   zipcode       |   string   |   Address zip code                                   |
|   [**owner**]       |   **Object**   |  **Object with the seller's attributes or with the company manager in case of Being a Business seller**  |
|   name          |   string   |   Seller/business manager name  |
|   cpf           |   string   |   Seller/business manager CPF number   |
|   phone         |   string   | Seller/business manager phone number |
|   [**bank**]        |   **Object**   | **Company's Bank or Seller Bank data** |
|   code          |   string   |   Bank code                                           |
|   agency        |   string   |   Agency number                                     |
|   account       |   string   |   Account number                                      |
|   type          |   string   |   Account type                                             |
|   external_id   |   string   | External account ID (Example: email registered on PagSeguro)  |


### Response Content
Check out in the example below this service's response content.

#### In case of Success:
```json
{
    "id": "4c63a38de84c8eb2ad7584bbb274af2d",
    "resource": "sellers",
    "attributes": {
        "is_active": true,
        "login": "josefrancisco",
        "name": "José Francisco Silva",
        "cpf_cnpj": "49.703.443/0001-38",
        "type": "Pessoa Jurídica",
        "email": "jose@mail.me",
        "phone": "(11) 98712-1234",
        "description": "",
        "address": {
            "street": "Rua Júlio Gonzalez",
            "number": "1000",
            "district": "Barra Funda",
            "complement": "10 Andar Sala 3",
            "city": "São Paulo",
            "state": "SP",
            "zipcode": "01156060"
        },
        "owner": {
            "name": "Giosepe",
            "cpf": "799.993.388-01",
            "phone": "(11) 91333-9876",
            "birthdate": "1988-03-29"
        },
        "bank": {
            "code": "033",
            "agency": "1000",
            "account": "100333",
            "type": "checkings",
            "external_id": "teste@mail.me"
        },
        "created_at": "2020-06-17 00:00:00",
        "updated_at": "2020-06-17 00:00:00"
    }
}
```

#### In case of failure:
```json
{
  "code": "406",
  "message": {
    "document": [
      "Document is not a valid CPF\/CNPJ."
    ]
  },
  "resource": “resource.sellers.store"
}
```

## Seller Update
---
<span class="verb httpPUT">PUT</span> ***/service/resources/sellers?id={id}***

---

!> To update Seller's data, use the same fields described in [Register new Seller's](en-us/sellers?id=registrar-novo-vendedor)  `body`.<br>
In this case all the fields are optional.

### Example of Content to be Sent
Check out in the example below the content to be sent on request's body. Updating only the seller's email contact:

```json
{
    "email": "contato@mail.me"
}
```

## Seller Query
---
<span class="verb httpGET">GET</span> ***/service/resources/sellers?id={id}***

---

## List Sellers
---
<span class="verb httpGET">GET</span> ***/service/resources/sellers***

---
