# Credit Card Tokenization <!-- {docsify-ignore-all} -->

## New Token

<span class="verb httpPOST">POST</span> ***/service/resources/card_tokens***

### Headers

| Content-Type | application/json |
| ------------ | ---------------- |
| Authorization | Basic `token` |

### Parameters
> The parameters below must be sent in JSON fotmat

#### Credit Card
| Attribute | Type | Description | Size |
| -------- | ---- | --------- | ------- |
| **[card]** | **Object** | **Card Data** | - |
| holderName | string | Card's Holder Name (owner) | 50 |
| number | string | Card Number | 19 |
| expiryMonth | string | Card Expiry Month | 2 |
| expiryYear | string | Card Expiry Year | 4 |
| cvv | string | Card's Security Code | 4 |

#### Card Holder (Owner)
| Attribute | Type | Description | Size |
| -------- | ---- | --------- | ------- |
| **[holder]**| **Object** | **Card Holder (Owner) data** | - |
| name | string | Holder's complete name | 50 |
| cpfCnpj | string | Holder's CPF or CNPJ | 14 |
| mobilePhone | string | Holder's Cell phone | 16 |
| birthdate | string | Holder's birth date | YYYY-MM-DD |
| **[address]** | **Object** | **Holder's address data** | - |
| street | string | Public Place(Street, Avenue, Etc) | 70 |
| number | string | Address Number | 10 |
| district | string | Address district name | 100 |
| complement | string | Address complement | 100 |
| city | string | Address city | 50 |
| state | string | Address state | 2 |
| zipcode |string | Address zip code | 8 |

### Content Example
Check out in the example below the content that can be sent on request body.

```json
{
    "card": {
        "holderName": "Frederic Sales",
        "number": "4024 0071 1251 2933",
        "expiryMonth": "02",
        "expiryYear": "2023",
        "cvv": "431"
    },
    "holder": {
        "name": "Frederic Sales",
        "cpfCnpj": "79999338801",
        "mobilePhone": "1899767866",
        "birthdate": "1989-03-28",
        "address": {
            "street": "Rua dos Testes",
            "number": "100",
            "district": "Tamboré",
            "zipcode": "06460080",
            "city": "Barueri",
            "state": "SP"
        }
    }
}
```

### Return
| Código | Descrição |
| -------- | ---- |
| 401 | Not authenticated. Check if Basic Authentication keys had been correctly informed |
| 403 | Not authorized. Your account has no permission to complete this action. |
| 406 | Not accepted. Some sent data can be invalid, verify the data sent on request body |
| 200, 201 | Resource created |

### Response Content Structure
Check in the example below this service's response content structure

| Attribute | Type | Description |
| -------- | ---- | --------- |
| token | string | Credit Card generated token |
| resource | string | Resource type: card_token |
| **[attributes]** | **Object** | **Object with the attributes of the resource** |
| validated_at | string | Date when token was validated |
| expires_at | string | Date when token will expire. |
| created_at | date | Date when the resource was created |
| updated_at | date | Date when was the last time resource was updated. |
| **[card]** | **Object** | **Card data** |
| is_active | boolean | If token is active (true) or not (false) for using. |
| holder | string | Card's Holder name |
| bin | string | First 6 digits of the card number |
| last4 | string | Last 4 digits of the card number |
| brand | string | Card's brand |
| **holder** | **Object** | **Card Holder's data** |
| name | string | Holder's complete name |
| cpf | string | Holder's CPF or CNPJ |
| **contacts** | **Object** | **Card Holder's contact data ** |
| type | string | Contact type |
| number | string | Contact number |
| **addresses** | **Object** | **Card Holder's Address Collection ** |
| street | string | Public Place (Street, Avenue, Etc) |
| number | string | Address number |
| district | string | Address district |
| complement | string | Address Complement |
| city | string | Address City |
| state | string | Address State |
| zipcode | string | Address Zip Code |

### Response Example
Check out in the examples below this service's response content

#### In case of success:
```json
{
    "token": "7be25f8a-ba77-4370-ae19-1e9f57b09797",
    "resource": "card_token",
    "attributes": {
        "card": {
            "is_active": true,
            "holder": "FULANO",
            "bin": "411111",
            "last4": "1111",
            "brand": "visa"
        },
        "holder": {
            "name": "FULANO",
            "cpf": "27151617003",
            "email": "",
            "contacts": [
                {
                    "type": "mobile",
                    "number": "11997191000"
                }
            ],
            "addresses": [
                {
                    "type": "billing",
                    "street": "Rua João",
                    "number": "1000",
                    "district": "Barra Funda",
                    "complement": "Apto 567",
                    "city": "São Paulo",
                    "state": "SP",
                    "zipcode": "01156060"
                }
            ]
        },
        "validated_at": "2020-06-09 09:42:48",
        "expires_at": "2021-02-28 23:59:59",
        "created_at": "2020-06-09 09:42:48",
        "updated_at": "2020-06-09 09:42:48"
    }
}
```

#### In case of failure:
```json
{
    "code": "406",
    "message": {
        "holder": [
            "Holder is required",
            "Holder Invalid"
        ],
        "holder.name": [
            "Holder.name is required",
            "Holder.name must be at least 3 characters long",
            "Holder.name must not exceed 80 characters"
        ],
        "holder.cpfCnpj": [
            "Holder.cpfCnpj is required",
            "Holder.cpfCnpj is not a valid CPF/CNPJ."
        ],
        "holder.birthdate": [
            "Holder.birthdate is required",
            "Holder.birthdate must be date with format 'Y-m-d'"
        ],
        "holder.mobilePhone": [
            "Holder.mobilePhone is required",
            "Holder.mobilePhone is not a valid phone."
        ]
    },
    "resource": "resource.card_tokens.store"
}
```

## Query Token

<span class="verb httpGET">GET</span> ***/service/resources/card_tokens?token={token}***