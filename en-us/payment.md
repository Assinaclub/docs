# Payment <!-- {docsify-ignore-all} -->

## Overview

> This service is responsible for create, query, cancel and capture Payments (transactions).

## Create Payment
---
<span class="verb httpPOST">POST</span> ***/service/payment***

---

### Headers

| Field | Value |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|
| x-api-version | 2 |

### Parameters
> The parameters below must be sent in JSON format.

| Attribute | Type | Description | Mandatory | Size |
|:-:|:-:|:-:|:-:|:-:|
| amount | numeric | Payment amount value | Yes | - |
| order_id | string | Order ID Number (defined by the store) | no | 16 |
| callback_url | string | Callback URL | yes | 255 |
| **[payment]** | **Object** | Payment data | yes | - |
| payment.type | string | Payment type | yes | `card`, `boleto` or `pix` |
| payment.method | string | Payment method | yes | 20 |
| payment.installments | numeric | Number (quantity) of Installments | yes | 1   ~ 12 |
| payment.capture | boolean | Payment should (true) or not (false) be automatically captured. | no | - |
| pament.fraud_analysis | boolean | Payment should (true) or not (false) be queried by Anti Fraud system. | no | - |
| payment.softdescriptor | string | Name that will appear on invoice card. This feature depends on being enabled on the acquirer. | no | 16 |
| payment.pix_expires_in | numeric |  Quantity of minutes that the Pix will take to expire. [`Standard 1440 minutes`] | no | - |
| **[payment.card]** | **Object** | Credit or Debit card data. | yes, if payment.type = `card` | - |
| payment.card.holder | string | Card owner's name. | yes, if payment.type = `card` | 50 |
| payment.card.number | string | Card's number | yes, if payment.type = `card` | 19 |
| payment.card.expiry_month | string | Good Thru month | yes, if payment.type = `card` | 2 |
| payment.card.expiry_year | string | Good Thru year | yes, if payment.type = `card` | 4 |
| payment.card.cvv | string | Card's security code | no | 3 ou 4 |
| payment.card.token | string | Card's Tokened code | no | 36 |
| payment.card.tokenize | boolean | Card should (true) or not (false) be tokened for future payments. | no | - |
| **[payment.boleto]** | **Object** | Bank slip data | no | - |
| payment.boleto.due_date | date | Bank slip due date. | yes, if payment.type = `boleto` | `Y-m-d` ou `d/m/Y` |
| payment.boleto.instructions | List | Instruction list that will be able to be defined by the Bank slip (depends on the Bank slip) | no | - |
| payment.boleto.instructions.*.instruction | string | Bank slip instruciton | no | 70 |
| **[customer]** | **Object** | Customer's data (acquirer) | yes | - |
| customer.name | string | Customer's name | yes | 80 |
| customer.cpf_cnpj | string | Customer's CPF or CNPJ (CPF if it is personal account, CNPJ if it is business account) | no | `cpf`   ou `cnpj` |
| customer.email | string | Customer's email. | no | 80 |
| customer.phone | string | Customer's phone | no | 10   ou 11 |
| customer.birthdate | string | Customer's birth date | no | `Y-m-d` ou `d/m/Y` |
| customer.ip | string | Customer's IP (Internet Protocol) | no | `ip` |
| **[customer.billing_address]** | **Object** | Customer's billing address. | no | - |
| customer.billing_address.street | string | Billing Address Public Place (street, avenue, etc) | no | 100 |
| customer.billing_address.number | string |                    Billing Address number                    | no | 10 |
| customer.billing_address.district | string | Billing Address district | no | 80 |
| customer.billing_address.complement | string | Billing Address complement | no | 50 |
| customer.billing_address.city | string | Billing Address city | no | 80 |
| customer.billing_address.state | string | Billing Address state | no | 2 |
| customer.billing_address.zipcode | string | Billing Address zip code | no | 8 |
| **[customer.shipping_address]** | **Object** | Customer's Shipping Address data | no | - |
| **[products]** | **List** | Products list | no | - |
| products.*.name | string | Product name | no | 100 |
| products.*.description | string | Product description | no | 255 |
| products.*.unit_price | numeric | Product unit price | no | - |
| products.*.quantity | numeric | Product quantity | no | - |
| products.*.sku | string | Product code | no | 50 |
| **[subscription]** | **Object** | Current Subscription\|Charge data | no | - |
| subscription.frequency | numeric | Interval frequency that will be charged the Subscription amount | yes, in case you wish create a Subscription | 1 ~ 12 |
| subscription.interval | string | Time interval that the Subscription will be charged | yes, in case you wish create a Subscription | `day`,   `week`, `month` |
| subscription.start_date | date | Subscription start date since the first charge | yes, in case you wish create a Subscription | `Y-m-d` ou `d/m/Y` |
| subscription.amount | numeric | Subscription Value | no | - |
| subscription.installments | numeric | Installment (card) that will be applyed on the Subscription Value. | no | - |
| subscription.cycles | numeric | Subscription Charges Cycles | no | 0   ~ 120 |
| **[subscription.trial]** | **Object** | Trial period data | no | - |
| subscription.trial.amount | numeric | Subscription Trial value | no |  |
| subscription.trial.cycles | numeric |              Charge Cycles of the Trial Period               | no | 0 ~ 120 |
| subscription.trial.frequency | numeric | Interval Frequency that the Trial Value will be charged. | no | 1   ~ 12 |
| **[split_rules]** | **List** | Split Rule List | no | - |
| split_rules.seller_id | string | Seller Unique ID (Marketplace) | yes, in case you wish create a Rule | 50 |
| split_rules.percentage | numeric | Percentage that will be discounted from transaction value | no | - |
| split_rules.amount | numeric | Fixed value defined that will be discounted from the Transaction Value | no | - |
| split_rules.liable | boolean | Defines if the value will need to be discounted in case of Chargeback | no | - |
| split_rules.charge_processing_fee | boolean | Defines if it will be needed to calculate the transaction fee that refers to the split value | no | - |
| split_rules.hold_receivables | boolean | Defines if the Receivable will be holded until Release request (ReceivableReleaseRequest)  | no | Default: `false` |

### Example of Sent Content
Check out the Content that you can send on the body of the request below

#### Example of Credit Card Payment (Simple)

```json
{
    "amount": 97.86,
    "callback_url": "https://99mystore.com.br/pluscommercebr/callback",
    "order_id": "1234567",
    "payment": {
        "type": "card",
        "method": "visa",
        "installments": 1,
        "card": {
            "holder": "FULANO DA SILVA",
            "number": "4111 1111 1111 1111",
            "expiry_month": "03",
            "expiry_year": "2021",
            "cvv": "123"
        }
    },
    "customer": {
        "name": "Jack Jins",
        "cpf_cnpj": "799.993.388-01"
    }
}
```

#### Example of Credit Card Payment (Complete)

```json
{
    "amount": 100.00,
    "callback_url": "https://99mystore.com.br/pluscommercebr/callback",
    "payment": {
        "type": "card",
        "method": "visa",
        "installments": 1,
        "capture": false,
        "softdescriptor": "MYSTORE",
        "fraud_analysis": true,
        "card": {
            "holder": "FULANO DA SILVA",
            "number": "4111 1111 1111 1111",
            "expiry_month": "03",
            "expiry_year": "2021",
            "cvv": "123"
        }
    },
    "customer": {
        "name": "Fulano da Silva",
        "cpf_cnpj": "79999338801",
        "phone": "(11) 99719-2099",
        "email": "fulano@mail.me",
        "birthdate": "1989-03-28",
        "ip": "192.168.0.1",
        "billing_address": {
            "street": "Rua Júlio Gonzalez",
            "number": "1023",
            "district": "Barra Funda",
            "complement": "Sala 02",
            "city": "São Paulo",
            "state": "SP",
            "zipcode": "01156-060"
        }
    },
    "products": [
        {
            "name": "Darth Vader - Toy",
            "description": "Darth Vader Toy in Real Size Model",
            "unit_price": 50.00,
            "quantity": 2,
            "sku": "DARKSIDE"
        }
    ]
}
```

#### Example of Credit Card Payment for Customers outside of Brazil (Foreigners) (Simple)

```json
{
    "amount": 100.00,
    "callback_url": "https://99mystore.com.br/pluscommercebr/callback",
    "order_id": "1234567",
    "payment": {
        "type": "card",
        "method": "visa",
        "installments": 1,
        "card": {
            "holder": "JACK JONES",
            "number": "4111111111111111",
            "expiry_month": "03",
            "expiry_year": "2021",
            "cvv": "123"
        }
    },
    "customer": {
        "name": "Jack Jones"
    }
}

> Example of Bank slip payment (Complete)

```json
{
    "amount": 100.00,
    "callback_url": "https://99mystore.com.br/pluscommercebr/callback",
    "payment": {
        "type": "boleto",
        "method": "boletopagseguro",
        "boleto": {
            "due_date": "2020-10-20",
            "instructions": [
                "Sr. Caixa não receber após o vencimento",
                "Boleto referente ao pedido 777 na Loja MYSTORE"
            ]
        }
    },
    "customer": {
        "name": "Fulano da Silva",
        "cpf_cnpj": "79999338801",
        "phone": "(11) 99719-2099",
        "email": "fulano@mail.me",
        "birthdate": "1989-03-28",
        "ip": "192.168.0.1",
        "billing_address": {
            "street": "Rua Júlio Gonzalez",
            "number": "1023",
            "district": "Barra Funda",
            "complement": "Sala 02",
            "city": "São Paulo",
            "state": "SP",
            "zipcode": "01156-060"
        },
        "shipping_address": {
            "street": "Rua Júlio Gonzalez",
            "number": "1023",
            "district": "Barra Funda",
            "complement": "Sala 02",
            "city": "São Paulo",
            "state": "SP",
            "zipcode": "01156-060"
        }
    },
    "products": [
        {
            "name": "Luke Skywalker Lightsaber Blue",
            "description": "Luke Skywalker Real Lightsaber in Blue",
            "unit_price": 100.00,
            "quantity": 1,
            "sku": "LIGHTSIDE"
        }
    ]
}
```

#### Example of Payment with Credit Card Tokening

```json
{
  "amount": 97.65,
  "callback_url": "https://9a32ecb90e4cb7ef0f44d6262ec7b5d9.m.pipedream.net",
  "payment": {
    "type": "card",
    "method": "visa",
    "installments": 1,
    "card": {
      "tokenize": true,
      "holder": "FULANO DA SILVA",
      "number": "4532 2876 7749 2847",
      "expiry_month": "03",
      "expiry_year": "2021",
      "cvv": "123"
    }
  },
  "customer": {
    "name": "Jack Jins",
    "cpf_cnpj": "799.993.388-01"
  }
}
```

#### Example of Payment using only Card's Token.

```json
{
  "amount": 97.65,
  "callback_url": "https://9a32ecb90e4cb7ef0f44d6262ec7b5d9.m.pipedream.net",
  "payment": {
    "type": "card",
    "installments": 1,
    "card": {
      "token": "d2ce5a32-c67e-4c50-8726-f6fc43c477fb"
    }
  },
  "customer": {
    "name": "Jack Jins",
    "cpf_cnpj": "799.993.388-01"
  }
}
```

#### Example of Payment via Credit Card with Creating an Subscription|Charge

```json
{
    "amount": 99.00,
    "callback_url": "https://99mystore.com.br/pluscommercebr/callback",
    "payment": {
        "type": "card",
        "method": "visa",
        "installments": 1,
        "capture": true,
        "card": {
            "holder": "FULANO DA SILVA",
            "number": "4111 1111 1111 1111",
            "expiry_month": "03",
            "expiry_year": "2021",
            "cvv": "123"
        }
    },
    "customer": {
        "name": "Fulano da Silva",
        "cpf_cnpj": "79999338801",
        "phone": "(11) 99719-2099",
        "email": "fulano@mail.me",
        "birthdate": "1989-03-28",
        "ip": "192.168.0.1",
        "billing_address": {
            "street": "Rua Júlio Gonzalez",
            "number": "1023",
            "district": "Barra Funda",
            "complement": "Sala 02",
            "city": "São Paulo",
            "state": "SP",
            "zipcode": "01156-060"
        }
    },
    "subscription": {
        "frequency": 1,
        "interval": "month",
        "start_date": "2020-10-18"
    }
}
```

#### Example of Payment via Credit Card with Payment Split

```json
{
    "amount": 99.00,
    "callback_url": "https://99mystore.com.br/pluscommercebr/callback",
    "payment": {
        "type": "card",
        "method": "visa",
        "installments": 1,
        "capture": true,
        "card": {
            "holder": "FULANO DA SILVA",
            "number": "4111 1111 1111 1111",
            "expiry_month": "03",
            "expiry_year": "2021",
            "cvv": "123"
        }
    },
    "customer": {
        "name": "Fulano da Silva",
        "cpf_cnpj": "79999338801",
        "ip": "192.168.0.1",
        "billing_address": {
            "street": "Rua Júlio Gonzalez",
            "number": "1023",
            "district": "Barra Funda",
            "complement": "Sala 02",
            "city": "São Paulo",
            "state": "SP",
            "zipcode": "01156-060"
        }
    },
    "split_rules": [
        {
            "seller_id": "vendedor1@mail.me",
            "amount": 15.87,
            "liable": true
        },
        {
            "seller_id": "vendedor2@mail.me",
            "percentage": 20.0,
            "liable": true,
            "charge_processing_fee": false
        }
    ]
}
```

#### Example of Pix Payment (Complete)

```json
{
    "amount": 97.86,
    "callback_url": "https://99mystore.com.br/pluscommercebr/callback",
    "order_id": "1234567",
    "payment": {
        "type": "pix",
        "method": "pix",
        "pix_expires_in": 60
    },
    "customer": {
        "name": "Fulano da Silva",
        "cpf_cnpj": "79999338801",
        "phone": "(11) 99719-2099",
        "email": "fulano@mail.me",
        "birthdate": "1989-03-28",
        "ip": "192.168.0.1",
        "billing_address": {
            "street": "Rua Júlio Gonzalez",
            "number": "1023",
            "district": "Barra Funda",
            "complement": "Sala 02",
            "city": "São Paulo",
            "state": "SP",
            "zipcode": "01156-060"
        },
        "shipping_address": {
            "street": "Rua Júlio Gonzalez",
            "number": "1023",
            "district": "Barra Funda",
            "complement": "Sala 02",
            "city": "São Paulo",
            "state": "SP",
            "zipcode": "01156-060"
        }
    },
    "products": [
        {
            "name": "Luke Skywalker Lightsaber Blue",
            "description": "Luke Skywalker Real Lightsaber in Blue",
            "unit_price": 100.00,
            "quantity": 1,
            "sku": "LIGHTSIDE"
        }
    ]
}
```

### Return Status
| Código | Descrição                                                    |
| ------ | ------------------------------------------------------------ |
| 401    | Not authenticated. Check if Basic Authentication keys had been correctly informed. |
| 403    | Not authorized. Your account has no permission to complete this action. |
| 406    | Not accepted. Some sent data can be invalid, verify the data sent on request body. |
| 200    | Payment created.                                             |

### Response content structure
Check it out on the example below the response content from this service.

| Atributo | Tipo | Descrição |
|-|-|-|
| id | numeric | Transaction ID |
| uuid | string | Transaction unique ID |
| resource | string | Resource Type |
| [**attributes**]&#9660; | **Object** | Transaction Data |
| seller_id | string | Seller's unique ID |
| order_id | string | External order's number ID |
| amount | numeric | Transaction value |
| installments | numeric | Transaction Installments number |
| tid | string | External unique Transaction ID (defined by card acquirer) |
| authorization_id | string | External Authorization ID (defined by card acquirer) |
| method | string | Transaction payment method |
| captured_amount | numeric | Captured Transaction's value |
| captured_at | datetime | Date when Transaction was captured |
| url_authentication | string | Authentication link for Authenticated Transactions and Debit Transactions |
| callback_url | string | Transaction Callback URL |
| created_at | datetime | Date of Transaction creation |
| updated_at | datetime | Date of last Transaction update. |
| [**status**]&#9660; | **Object** | Transaciton state Status |
| status.code | numeric | Transaction status code [Payment Status](en-us/payment_status?id=status-de-pagamento) |
| status.message | string | Transaction status message |
| [**acquirer**]&#9660; | **Object** | Card acquirer data |
| acquirer.name | string | Card acquirer name |
| acquirer.message | string | Card acquirer message |
| acquirer.code | string | Card acquirer |
| [**boleto**]&#9660; | **Object** | Bank slip data (only for Bank slip Transactions) |
| boleto.due_date | date | Bank slip due date |
| boleto.digitable_line | string | Bank slip digitable line  (some acquirers won't return this information) |
| boleto.link | string | Bank slip Link |
| [**pix**]&#9660; | **Object** | Pix data(only for Pix Transactions) |
| pix.link | string | Pix link |
| pix.qrcode | string | QR-Code code for rendering |
| [**card**]&#9660; | **Object** | Card data |
| card.holder | string | Card Holder (owner) |
| card.number | string | Masked card number (Bin   + Last4) |
| card.expiry_month | string | Card expiration month |
| card.expiry_year | string | Card expiration Year |
| card.brand | string | Card Brand |
| card.token | string | Card Token (Only for tokenization requests) |
| [**customer**]&#9660; | **Object** | Customer data (purchaser) |
| customer.name | string | Customer Name |
| customer.cpf_cnpj | string | Customer CPF or CNPJ |
| customer.email | string | Customer email |
| customer.phone | string | Customer Phone |
| [**customer.billing_address**]&#9660; | **Object** | Customer's billing address data |
| customer.billing_address.street | string | Billing adress Public Place (Street, Avenue, Etc) |
| customer.billing_address.number | string | Billing adress number |
| customer.billing_address.district | string | Billing adress district |
| customer.billing_address.complement | string | Billing adress |
| customer.billing_address.city | string | Billing adress city |
| customer.billing_address.state | string | Billing adress state |
| customer.billing_address.zipcode | string | Billing adress zip code |
| [**customer.shipping_address**]&#9660; | **Object** | Customer shipping adress data |
| customer.shipping_address.street | string | Shipping adress Public Place (Street, Avenue, Etc) |
| customer.shipping_address.number | string | Shipping adress number |
| customer.shipping_address.district | string | Shipping adress district |
| customer.shipping_address.complement | string | Shipping adress complement |
| customer.shipping_address.city | string | Shipping adress city |
| customer.shipping_address.state | string | Shipping adress state |
| customer.shipping_address.zipcode | string | Shipping adress zip code |
| [**subscription**]&#9660; | **Object** | Subscription Data (Only for Transactions that created Subscriptions) |
| [**products**]&#9660; | **List** | Product List |
| products.[].name | string | Product Name |
| products.[].unit_price | numeric | Product unit price |
| products.[].quantity | numeric | Product quantity |
| products.[].sku | string | Product external code |
| [**antifraud**]&#9660; | **Object** | Anti fraud analysis data |
| antifraud.score | numeric | Score Anti fraud analysis value |
| antifraud.status | string | Anti fraud analysis status |
| antifraud.message | string | Anti fraud analysis message |
| [**split_rules**]&#9660; | **List** | Split rules list |
| split_rules.[].id | numeric | Split rule unique ID |
| split_rules.[].resource | string | Resource name |
| split_rules.[].attributes | **Object** | Split rule data |
| split_rules.[].attributes.receiver_id | string | Receiver unique ID |
| split_rules.[].attributes.percentage | numeric | Percentage value (0.00 to 1.00) |
| split_rules.[].attributes.amount | numeric | Fixed Value |
| split_rules.[].attributes.liable | boolean | Liable in case of chargeback |
| split_rules.[].attributes.charge_processing_fee | boolean | Considers (true) or not (false) the acquirer's charge processing fee |
| split_rules.[].attributes.created_at | datetime | Split rule creation date |
| split_rules.[].attributes.updated_at | datetime | Split rule update date |
| [**receivables**]&#9660; | **Object** | Transaction's receivables data (only for Plus Commerce native payments) |
| receivables.[].id | numeric | Receivable unique ID |
| receivables.[].resource | string | Resource name |
| receivables.[].attributes | **Object** | Receivable data |
| receivables.[].attributes.receiver_id | string | Receivable ID |
| receivables.[].attributes.receiver_uuid | string | Receivable unique ID |
| receivables.[].attributes.transaction | string | Transaction unique ID |
| receivables.[].attributes.status | string | Receivable Status (`pending`,   `paid`, `canceled`, `refunded`, `blocked`) |
| receivables.[].attributes.amount | numeric | Receivable Liquid value |
| receivables.[].attributes.gross_amount | numeric | Receivable Gross Value |
| receivables.[].attributes.installment | numeric | Installment reference ( from 1 to 12) |
| receivables.[].attributes.description | string | Receivable description |
| receivables.[].attributes.paid_at | datetime | Date when Receivable was paid |
| receivables.[].attributes.canceled_at | datetime | Date when Receivable was canceled |
| receivables.[].attributes.expected_on | datetime | Expected Receivable payment date |
| receivables.[].attributes.created_at | datetime | Date when Receivable was created |
| receivables.[].attributes.updated_at | datetime | Receivable update date |
| [**history**] | **List** | Transaction's history list |
| history.[].amount | numeric | Transaction value for the operation |
| history.[].type | string | Operation Type |
| history.[].status | string | Operation Status |
| history.[].response_code | string | Response Code |
| history.[].response_menssage | string | Response message |
| history.[].authorization_code | string | Authorization code |
| history.[].authorization_id | string | Authorization ID |
| history.[].authorization_nsu | string | Transaction's unique sequencial number |
| history.[].created_at | datetime | History creation date |

#### In case of Success

```json
{
  "id": 3012161,
  "uuid": "178dea199ac2071615afc44551baf794",
  "resource": "transactions",
  "attributes": {
    "seller_id": "",
    "order_id": "1012161",
    "amount": 100,
    "installments": 1,
    "tid": "20200921143822322",
    "authorization_id": "",
    "status": {
      "code": 8,
      "message": "CAPTURED"
    },
    "method": "visa",
    "captured_amount": 0,
    "captured_at": "2020-09-21 14:38:26",
     "acquirer": {
      "name": "simulated",
      "message": "transaction canceled",
      "code": ""
    },
    "url_authentication": "",
    "callback_url": "https://99mystore.com.br/pluscommercebr/callback",
    "card": {
      "holder": "FULANO DA SILVA",
      "number": "411111******1111",
      "expiry_month": "03",
      "expiry_year": "2021",
      "brand": "visa"
    },
    "customer": {
      "name": "Fulano da Silva",
      "cpf_cnpj": "79999338801",
      "email": "fulano@mail.me",
      "phone": "",
      "billing_address": {
        "street": "AV CYRO BUENO",
        "number": "600",
        "district": "JD MORUMBI",
        "complement": "SALA 02",
        "city": "PRESIDENTE PRUDENTE",
        "state": "SP",
        "zip_code": "19060560"
      },
      "shipping_address": {
        "street": "",
        "number": "",
        "district": "",
        "complement": "",
        "city": "",
        "state": "",
        "zip_code": ""
      }
    },
    "subscription": null,
    "products": [
      {
        "name": "Darth Vader - Toy",
        "unit_price": 50,
        "quantity": 0,
        "sku": "DARKSIDE"
      }
    ],
    "antifraud": {
      "score": 0,
      "status": "approved",
      "message": "Transação aprovada pela Loja."
    },
    "split_rules": [],
    "receivables": [],
    "history": [
      {
        "amount": 100,
        "type": "created",
        "status": "succeeded",
        "response_code": "",
        "response_message": "",
        "authorization_code": "",
        "authorization_id": "",
        "authorization_nsu": "",
        "created_at": "2020-09-21 14:38:22"
      },
      {
        "amount": 100,
        "type": "authorized",
        "status": "succeeded",
        "response_code": "0",
        "response_message": "transaction approved",
        "authorization_code": "",
        "authorization_id": "20200921143822322",
        "authorization_nsu": "a7336a8c-0654-42a1-8bd4-95d486852f2c",
        "created_at": "2020-09-21 14:38:22"
      },
      {
        "amount": 100,
        "type": "captured",
        "status": "succeeded",
        "response_code": "0",
        "response_message": "transaction captured",
        "authorization_code": "",
        "authorization_id": "20200921143822322",
        "authorization_nsu": "a7336a8c-0654-42a1-8bd4-95d486852f2c",
        "created_at": "2020-09-21 14:38:26"
      }
    ],
    "created_at": "2020-09-21 14:38:22",
    "updated_at": "2020-09-21 14:38:26"
  }
}
```

#### In case of Success (pix)

```json
{
  "id": 1020921,
  "uuid": "49e04b8dcd7fc9cb8a5fbd656404c8c0",
  "resource": "transactions",
  "attributes": {
    "seller_id": "",
    "order_id": "1234567",
    "amount": 97.86,
    "installments": 1,
    "tid": "20201119110305889",
    "authorization_id": "",
    "status": {
      "code": 2,
      "message": "WAITING PAYMENT"
    },
    "method": "pix",
    "captured_amount": 0,
    "captured_at": "0000-00-00 00:00:00",
    "acquirer": {
      "name": "simulated",
      "message": "Pix gerado",
      "code": ""
    },
    "boleto": null,
    "pix": {
      "link": "https://api.pluscommercebr.com.br/pix?t=71d08e50ed9a7d0f20f0db8cacb20d03",
      "qrcode": "00020101021226780014br.gov.bcb.pix2556api.test.com/pix/v2/12345678-4321-1234-5678-12345678901252040000530398654045.005802BR5913EMPRESA TESTE6012Porto Alegre62070503***630492CA"
    },
    "url_authentication": "https://api.pluscommercebr.com.br/pix?t=49e04b8dcd7fc9cb8a5fbd656404c8c0",
    "callback_url": "https://99mystore.com.br/pluscommercebr/callback",
    "card": null,
    "customer": {
      "name": "Fulano da Silva",
      "cpf_cnpj": "79999338801",
      "email": "fulano@mail.me",
      "phone": "(11) 99719-2099",
      "billing_address": {
        "street": "RUA JULIO GONZALEZ",
        "number": "1023",
        "district": "BARRA FUNDA",
        "complement": "SALA 02",
        "city": "SAO PAULO",
        "state": "SP",
        "zip_code": "01156060"
      },
      "shipping_address": {
        "street": "RUA JULIO GONZALEZ",
        "number": "1023",
        "district": "BARRA FUNDA",
        "complement": "SALA 02",
        "city": "SAO PAULO",
        "state": "SP",
        "zip_code": "01156060"
      }
    },
    "subscription": null,
    "charge": null,
    "products": [
      {
        "name": "Luke Skywalker Lightsaber Blue",
        "unit_price": 100,
        "quantity": 0,
        "sku": "LIGHTSIDE"
      }
    ],
    "antifraud": null,
    "split_rules": [],
    "receivables": [],
    "history": [
      {
        "amount": 97.86,
        "type": "created",
        "status": "succeeded",
        "response_code": "",
        "response_message": "",
        "authorization_code": "",
        "authorization_id": "",
        "authorization_nsu": "",
        "created_at": "2020-11-19 11:02:55"
      }
    ],
    "created_at": "2020-11-19 11:02:53",
    "updated_at": "2020-11-19 11:03:06"
  }
}
```

#### In case of failure (Example)

```json
{
  "code": "406",
  "message": {
    "amount": [
      "Amount is required",
      "Amount must be numeric"
    ]
  },
  "resource": "service.payment"
}
```

## Payment Query (Sonda)*
---
<span class="verb httpGET">GET</span> ***/service/consult?id={id}***

---

### Headers

| Field | Value |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|
| x-api-version | 2 |

!> Send only one of the Params below

### Query Params

|  Attribute  |   Type   |   Description           |
|-------------|----------|----------------------------|
|   id      |   string  |  Transaction Unique ID  |
|   uuid      |   string  | Transaction Unique ID  |
|   tid      |   string  |  Transaction's TID  |
|   order_id      |   string  |  Order ID informed in Transaction  |

## Payment Capturing
---
<span class="verb httpPOST">POST</span> ***/service/capture?id={id}***

---

### Headers

| Field | Value |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|
| x-api-version | 2 |

!> Send only one of the Params below

### Query Params

|  Attribute  |   Type   |   Description           |
|-------------|----------|----------------------------|
|   id      |   string  | Transaction Unique ID  |
|   uuid      |   string  | Transaction Unique ID  |
|   tid      |   string  | Transaction's TID  |
|   order_id      |   string  | Order ID informed in Transaction  |

## Cancel Payment
---
<span class="verb httpPOST">POST</span> ***/service/cancel?id={id}***

---

### Headers

| Field | Value |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|
| x-api-version | 2 |

!> Send only one of the Params below

### Query Params

|  Attribute  |   Type   |   Description           |
|-------------|----------|----------------------------|
|   id      |   string  | Transaction Unique ID  |
|   uuid      |   string  | Transaction Unique ID  |
|   tid      |   string  | Transaction's TID  |
|   order_id      |   string  |  Order ID informed in Transaction  |
