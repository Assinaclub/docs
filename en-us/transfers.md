# Transference<!-- {docsify-ignore-all} -->

## Overview

> Service responsible for listing and consulting transferences.

## List Transferences
---
<span class="verb httpGET">GET</span> ***/service/resources/transfers***

---

### Headers

| Field | Value |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|

### Return Status
| Code | Description                                                  |
| ---- | ------------------------------------------------------------ |
| 401  | Not authenticated. Check if Basic Authentication keys had been correctly informed. |
| 403  | Not authorized. Your account has no permission to complete this action. |
| 406  | Not accepted. Some sent data can be invalid, verify the data sent on request body. |
| 201  | Resource Created.                                            |

### Response Content Structure
Check in the example below this service's response content structure

|   Attribute     |   Type   |   Description                         |
|-----------------------|------------|-----------------------------------------------|
|   **data**                |   **Array**    |   **Transferences List**   |
|   id                  |   string   |  Transference ID on System  |
|   resource            |   string   |   Resource type                 |
|   **[attributes]**        |   **Object**   |   **Object with resource attributes**   |
|   transfer_number     |   boolean  |   Transference ID code   |
|   transfer_source     |   string   |   Where the resource has been transfered   |
|   status              |   string   |   Transference Status   |
|   amount              |   string   |   Liquid Transference Value     |
|   original_amount     |   string   |   Original Transference Valueq   |
|   currency            |   string   |   Transference Currency                   |
|   description         |   string   |   Transference Description   |
|   created_at          |   string   |   Date when Transference was created   |
|   updated_at          |   string   |   Date when Transference was last updated   |

### Response Content Structure
Check in the example below this service's response content structure

In case of success:
```json
{
    "data": [
        {
            "id": 4,
            "resource": "transfers",
            "attributes": {
                "transfer_number": "5ab6987a6ee1970dacadc7fc1909dd8c",
                "transfer_source": “bank account”,
		    "receiver_id": “308a4f1079d3c50653dfeca80d85ea6a",
                "status": "failed",
                "amount": 25.48,
                "original_amount": 25.48,
                "currency": “BRL",
                "description": "",
                "created_at": "2018-12-20 16:54:44",
                "updated_at": "2018-12-20 16:54:44"
            }
        }
    ],
    "links": {
        "first": “https://api.ipag.com.br/service/resources/transfers?page=1”,
        "last": “https://api.ipag.com.brservice/resources/transfers?page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "last_page": 1,
        "from": 1,
        "to": 3,
        "per_page": 15,
        "total": 3
    }
}
```
### In case of any Transference found:

```json
{
    "data": []
}
```

## Query Transference

---
<span class="verb httpGET">GET</span> ***/service/resources/transfers?id={id}***

---

### Query Params

|  Attribute  |   Type   |   Description      |
|-------------|----------|----------------------------|
|   id      |   string  |  Transference unique ID  |

### Response content structure
Check in the example below this service's response content structure

| Attribute        |   Type   |   Description                               |
|-----------------------|------------|-----------------------------------------------------|
|   id                  |   string   |   Transference ID in the system   |
|   resource            |   string   |   Resource Type                     |
|   **[attributes]**        |   **Object**   |   **Object with the resource attributes **   |
|   transfer_number     |   boolean  |   Transference ID code   |
|   transfer_source     |   string   | Where the resource has been transfered |
|   status              |   string   |   Transference Status        |
|   amount              |   string   |   Liquid Amount                      |
|   original_amount     |   string   |   Original Amount                      |
|   currency            |   string   |   Transference Currency                         |
|   description         |   string   |   Transference Description   |
|   created_at          |   string   |   Date when the Transference was  created   |
|   updated_at          |   string   |   Date when the Transference was last updated   |
|   **receivables**         |   **Object**   |  **Object with the receivables linked to the Transference**  |

### Response Content Structure
Check in the example below this service's response content structure

In case of success:

```json
{
    "id": 4,
    "resource": "transfers",
    "attributes": {
        "transfer_number": "5ab6987a6ee1970dacadc7fc1909dd8c",
        "transfer_source": “bank account",
        "receiver_id": "308a4f1079d3c50653dfeca80d85ea6a",
        "status": "failed",
        "amount": 25.48,
        "original_amount": 25.48,
        "currency": “BRL",
        "description": "",
        "created_at": "2018-12-20 16:54:44",
        "updated_at": "2018-12-20 16:54:44”,
        "receivables": {
            "data": [
                {
                    "id": 8,
                    "resource": "receivables",
                    "attributes": {
                        "transaction": "1005534",
                        "status": "pending",
                        "amount": 9.2,
                        "gross_amount": 10,
                        "installment": 1,
                        "description": "",
                        "paid_at": "",
                        "canceled_at": "",
                        "expected_on": "2018-11-20 15:28:02",
                        "created_at": "2018-11-19 15:28:02",
                        "updated_at": "2018-11-19 15:28:02"
                    }
                },
                {
                    "id": 9,
                    "resource": "receivables",
                    "attributes": {
                        "transaction": "1005877",
                        "status": "pending",
                        "amount": 8.14,
                        "gross_amount": 9,
                        "installment": 1,
                        "description": "",
                        "paid_at": "",
                        "canceled_at": "",
                        "expected_on": "2018-12-07 10:54:40",
                        "created_at": "2018-12-06 10:54:40",
                        "updated_at": "2018-12-06 10:54:40"
                    }
                },
                {
                    "id": 10,
                    "resource": "receivables",
                    "attributes": {
                        "transaction": "1005877",
                        "status": "pending",
                        "amount": 8.14,
                        "gross_amount": 9,
                        "installment": 2,
                        "description": "",
                        "paid_at": "",
                        "canceled_at": "",
                        "expected_on": "2018-12-07 10:54:40",
                        "created_at": "2018-12-06 10:54:40",
                        "updated_at": "2018-12-06 10:54:40"
                    }
                }
            ]
        }
    }
}

```

In case of any Transference found:

```json
{
    "code": "404",
    "message": "Transfer Not Found",
    "resource": "resource.transfers.view"
}
```

## List Sellers' Transferences

---
<span class="verb httpGET">GET</span> ***/service/resources/sellers_transfers***

---

### 
The structure follows the same pattern as [List Transferences](pt-br/transfers?id=listar-transferências).

## Query Seller's Transference
---
<span class="verb httpGET">GET</span> ***/service/resources/sellers_transfers?id={id}***

---

### Query Params

|  Attribute  |   Type   |   Description      |
|-------------|----------|----------------------------|
|   id      |   string  |  Transference unique ID  |

### Response Content Structure
The structure follows the same pattern as [Query Transference](pt-br/transfers?id=consultar-transferência).
