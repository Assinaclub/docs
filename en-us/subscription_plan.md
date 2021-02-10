# Subscription Plan <!-- {docsify-ignore-all} -->

## Overview

> Service responsible for managing Subscription Plans

## New Subscription Plan
---
<span class="verb httpPOST">POST</span> ***/service/resources/plans***

---

### Headers

| Field | Value |
| ------------ | ------ |
| Content-Type | application/json |
| Accept | application/json |

### Parameters
> The parameters below must be sent in JSON format

|   Attribute   |  Type  |                Description                | Mandatory |            Size            |
|:------------:|:-------:|:----------------------------------------:|:-----------:|:------------------------------:|
|     name     |  string |              Plan name              |     Yes     |               255              |
|  description |  string |            Plan description            |     Yes     |               255              |
|    amount    | numeric |              Plan price              |     Yes     |                -               |
|   frequency  |  string |     Plan's payment frequency     |     Yes     | `monthly`, `weekly` or `daily` |
|   interval   | numeric |     Plan's payment interval     |     Yes     |             1 ~ 12             |
|    cycles    | numeric |       Plan's charge cycles       |     No     |                -               |
|     [**trial**]&#9660;  |  **Object** |  Plan's Trial period data  |     No     |       -         |
| trial.amount | numeric |        Plan's trial price        |     No     |                -               |
| trial.cycles | numeric | Plan's trial cycles |     No     |                -               |


### Example of Content to be Sent
Check the example below the content that will be sent on request body.

```json
{
    "name": "PLANO GOLD",
  "description": "Plano Gold com até 4 treinos por semana",
  "amount": 99.00,
  "frequency": "monthly",
  "interval": 1,
  "cycles": 0,
  "best_day": true,
  "pro_rated_charge": true,
  "grace_period": 0,
  "trial": {
    "amount": 0,
    "cycles": 0
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

### Response content structure
Check out the example below the service's response content structure.

|        Attribute        |    Type    |                Description                |
|:-----------------------:|:----------:|:---------------------------------------:|
|           type          |   string   |             Resource type             |
|            id           |   numeric  |               Plan ID               |
|    [**attributes**]&#9660;    | **Object** |             Plan data             |
|           name          |   string   |              Plan name              |
|       description       |   string   |           Plan description           |
|          amount         |   numeric  |             Plan price             |
|        frequency        |   string   |    Plan payment frequency    |
|         interval        |   numeric  |     Plan payment interval     |
|          cycles         |   numeric  |       Plan charge cycles       |
|        created_at       |    date    |        Plan register date        |
|        updated_at       |    date    |      Plan update date      |
|        [**links**]&#9660;     | **Object** |  Plan links data  |
|       links.payment      |   string  |       Payment link for the Creation of Subscription       |
|        [**trial**]&#9660;     | **Object** |  Plan Trial period data  |
|       trial.amount      |   numeric  |       Plan Trial price       |
|       trial.cycles      |   numeric  | Plan Trial charge cycles |


### In case of Success

```json
{
  "type": "plans",
  "id": 497,
  "attributes": {
    "name": "PLANO GOLD",
    "description": "Plano Gold com até 4 treinos por semana",
    "amount": 99,
    "frequency": "monthly",
    "interval": 1,
    "cycles": 0,
    "best_day": true,
    "pro_rated_charge": true,
    "grace_period": 0,
    "trial": {
      "amount": 0,
      "cycles": 0
    },
    "links": {
      "payment": "https://api.ipag.com.br/subscriptions?id=7380ad8a673226ae47fce7bff88e9c33c69b66b3f569c61c97d58aa9b31f473bf6799881"
    },
    "created_at": "2020-10-09 16:24:08",
    "updated_at": "2020-10-09 16:24:08"
  }
}
```

## Update Subscription Plan
---
<span class="verb httpPUT">PUT</span> ***/service/resources/plans?id={id}***

---

### Headers

| Field | Value |
| ------------ | ------ |
| Content-Type | application/json |

!> To update a Plan data use the same fields described in [Register New Plan](en-us/subscription_plan?id=novo-plano) in the `body`.<br>In this case all the fields are optional
.

### Example of Content to be sent
Check it out on the example below the content that you can send in the request body.

```json
{
    "amount" : 100.00
}
```


## Subscription Plan Query
---
<span class="verb httpGET">GET</span> ***/service/resources/plans?id={id}***

---

### Query Params

|  Attribute  |   Type   |   Description           |
|-------------|----------|----------------------------|
|   id        |   string | Subscription Plan ID |



> [Query return](en-us/subscription_plan?id=em-caso-de-sucesso)

## List Subscription Plan 
---
<span class="verb httpGET">GET</span> ***/service/resources/plans***

---

## Delete Subscription Plan
---
<span class="verb httpDELETE">DELETE</span> ***/service/resources/plans?id={id}***

---

> In case of success the **Response** will be `200 OK`

!> A Subscription Plan only will be able to be deleted if there isn't any Subscription using it.