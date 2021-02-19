# Split Rules<!-- {docsify-ignore-all} -->

## Overview

> Service responsible for managing Split Rules from a **pre-authorized(5)** Transaction.

## New Split Rule
---
<span class="verb httpPOST">POST</span> ***/service/resources/split_rules?transaction={id}***

---

### Headers

| Field | Value |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|

### Parameters
> The parameters below must be sent on request's body in JSON format. 

|   Attribute   |   Type   |   Description     |   Mandatory                   |  Size  |
|----------------|------------|---------------------------|-----------------------------------------|------------|
|   receiver_id  |   string   |  Seller's *id* or *uuid*  |   yes                             |   36       |
|   amount       |   numeric  |   Split's fixed value   |  yes, if `percentage` won't be sent  |   -        |
|   percentage   |   numeric  |   Percentage     |   yes, if `amount` won't be sent.   |   -        |

### Example of Content to be Sent
Check out in the example below the content will be sent in request's body.

```json
{
    "receiver_id": "1000000",
    "percentage": 10.00
}

ou

{
    "receiver_id": "1000000",
    "amount": 50.00
}
```

### Return Status
| Código | Descrição                                                    |
| ------ | ------------------------------------------------------------ |
| 401    | Not authenticated. Check if Basic Authentication keys had been correctly informed. |
| 403    | Not authorized. Your account has no permission to complete this action. |
| 406    | Not accepted. Some sent data can be invalid, verify the data sent on request body. |
| 200    | Resource Created.                                            |

### Response Content Structure
Check out in the example below the Structure of the response content from this service.

|   Attribute        |   Type   |   Description                                           |
|--------------------------|-------------|-----------------------------------------------------------------|
|   id                     |   int       |   Split Rule ID                 |
|   resources              |   string    |   Resource type                                |
|   **[attributes]**       |   **Object**    |   Object with the resource attributes   |
|   receiver_id            |   string    |   Unique Seller's ID (who will receive)   |
|   percentage             |   numeric   |   Split Percentage                           |
|   amount                 |   numeric   |  Fixed Split Value (ignored when percentage is informed)  |
|   liable                 |   boolean   |   Responsible in Chargebacks           |
|   charge_processing_fee  |   boolean   |   Charge partial fee value        |
|   created_at             |   datetime  |   Date when resource was created    |
|   updated_at             |   datetime  |   Date when resource was last updated   |


### Response Content
Check out in the example below this service's response content  

#### In case of Success:
```json
{
  "id": 415,
  "resource": "split_rules",
  "attributes": {
    "receiver_id": "939a391a1ac9a3431f2d78e83bd8b856",
    "percentage": 0,
    "amount": 40,
    "liable": false,
    "charge_processing_fee": false,
    "created_at": "2020-08-14 15:50:13",
    "updated_at": "2020-08-14 15:50:13"
  }
}
```

#### In case of Failure:
```json
{
  "code": "400",
  "message": "This transaction has already been captured and it is not possible to modify its split rules.",
  "resource": "service.split_rules.store"
}
```

## Split Rule Query

> You can query a specific Split Rule from a specific Transaction

---
<span class="verb httpGET">GET</span> ***/service/resources/split_rules?transaction={transaction_id}&id={split_rule_id}***

---

### Query Params

| Attribute   | Value          | Type   | Mandatory |
| ----------- | -------------- | ------ | --------- |
| transaction | Transaction ID | string | yes       |
| id          | Split Rule ID  | string | yes       |

## List Split Rules
---

> You can list all the Split Rules form an specific transaction.

<span class="verb httpGET">GET</span> ***/service/resources/split_rules?transaction={transaction_id}***

---

### Query Params

| Attribute   | Value          | Type   | Mandatory |
| ----------- | -------------- | ------ | --------- |
| transaction | Transaciton ID | string | Yes       |

## Delete Split Rule
---

!> You can delete a Split Rule from a specific Transaction only if the Transaction wasn't captured yet

<span class="verb httpDELETE">DELETE</span> ***/service/resources/split_rules?transaction={transaction_id}&id={split_rule_id}***

---

### Query Params

| Attribute   | Value          | Type   | Mandatory |
| ----------- | -------------- | ------ | --------- |
| transaction | Transaciton ID | string | yes       |
| id          | Split Rule ID  | string | yes       |

### Return Status
| Code | Description                                                  |
| ---- | ------------------------------------------------------------ |
| 400  | Resource already removed or Transaction was already captured |
| 200  | Resource removed successfuly.                                |
