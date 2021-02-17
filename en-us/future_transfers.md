# Future Transfers <!-- {docsify-ignore-all} -->

## Overview

> Service responsible for listing the future receivables

## List Future Transfers
---
<span class="verb httpGET">GET</span> ***/service/resources/future_transfers***

---

### Headers

| Field | Value |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|

### Response Content Structure
Check out in the example below this service's response content structure.

|   Attribute   |   Type   |   Description                        |
|-----------------------|------------|-----------------------------------------------|
|   **data**                |   **Array**    |   **List sorted by Receivables Date**   |
|   *data*                  |   date   |   Expected Payment date          |
|   sub_total            |   number   |   Transaction amount expected on specified date   |
|   **[receivables]**        |   **Array**   |  **Object with the resource data**  |
|   expected_total_amount        |   number   |   Total amount expected for all the future payments.   |

### Response Content
Check out in the examples below this service's response content.

In case of success:
```json
{
  "data": {
    "2020-09-04": {
      "sub_total": 3.13,
      "receivables": [
        {
          "id": 917,
          "resource": "receivables",
          "attributes": {
            "receiver_id": "4c63a38de84c8eb2ad7584bbb274af2d",
            "transaction": "1011826",
            "status": "pending",
            "amount": 1.59,
            "gross_amount": 1.79,
            "installment": 9,
            "description": "",
            "paid_at": "",
            "canceled_at": "",
            "expected_on": "2020-09-04 10:00:00",
            "created_at": "2020-07-03 17:15:20",
            "updated_at": "2020-07-03 17:15:20"
          }
        },
        {
          "id": 918,
          "resource": "receivables",
          "attributes": {
            "receiver_id": "4c63a38de84c8eb2ad7584bbb274af2d",
            "transaction": "1011826",
            "status": "pending",
            "amount": 1.54,
            "gross_amount": 1.78,
            "installment": 10,
            "description": "",
            "paid_at": "",
            "canceled_at": "",
            "expected_on": "2020-09-04 10:00:00",
            "created_at": "2020-07-03 17:15:20",
            "updated_at": "2020-07-03 17:15:20"
          }
        }
      ]
    }
  },
  "links": {
    "first": "https://api.ipag.com.br/service/resources/future_transfers?page=1",
    "last": "https://api.ipag.com.br/service/resources/future_transfers?page=1",
    "prev": null,
    "next": null
  },
  "meta": {
    "current_page": 1,
    "last_page": 1,
    "from": 1,
    "to": 1,
    "per_page": 30,
    "total": 1
  },
  "expected_total_amount": 3.13
}
```

### In case there is no Future Transferences:

```json
{
    "data": []
}
```

## List Future Seller's Transfers
---
<span class="verb httpGET">GET</span> ***/service/resources/future_transfers?seller_id={id}***

---

Or

---
<span class="verb httpGET">GET</span> ***/service/resources/future_transfers?cpf_cnpj={cpf_cnpj}***

---

### Response Content Structure
The structure follows the same pattern of [List Future Transferences](en-us/future_transfers?id=listar-lançamentos-futuros).