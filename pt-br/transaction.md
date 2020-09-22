# Transação <!-- {docsify-ignore-all} -->

## Overview

> Serviço responsável por listar e consultar transações.

## Listar Transações
---
<span class="verb httpGET">GET</span> https://api.ipag.com.br/service/resources/transactions

---

### Headers

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|

### Status de Retorno
|   Código  |   Descrição                                                                                           |
|-----------|-------------------------------------------------------------------------------------------------------|
|   401     |   Não autenticado. Verifique se foi informado corretamente as chaves de API no Basic Authentication.  |
|   403     |   Não Autorizado. Sua conta não tem permissão para realizar essa ação.                                |
|   201     |   Recurso criado.                                                                                     |

### Estrutura do conteúdo de resposta
Confira no exemplo abaixo a estrutura do conteúdo de resposta desse serviço.

|   Atributo            |   Tipo     |   Descrição                                   |
|-----------------------|------------|-----------------------------------------------|
|   **data**            |   **Array**     |   **Lista de Transações**                |
|   **transação**       |   **Object**    |   Objeto Transação                       |

### Query Params (Filters)

|   Atributo  |   Tipo   |   Descrição                |
|-------------|----------|----------------------------|
|   from_date    |   date  |   Filtro de data (inicio) (yyyy-mm-dd)  |
|   to_date      |   date  |   Filtro de data (fim) (yyyy-mm-dd) |
|   status         |   number  |   Filtro de Status (1 ~ 10)  |
|   limit        |   number  |   Limite de transações retornadas [padrão: 15]  |
|   order        |   string  |   Ordenação do resultado ('asc' | 'desc') [padrão: 'desc']  |
|   page         |   number  |   Página selecionada  |

### Conteúdo de resposta
Confira nos exemplos abaixo o conteúdo de resposta desse serviço.

Em caso de sucesso:
```json
{
    "data": [
        {
            "id": 1011823,
            "uuid": "5851de4d1b821953454733521bf09700",
            "resource": "transactions",
            "attributes": {
                "order_id": "1011823",
                "amount": 529.19,
                "installments": 3,
                "tid": "20200629174147590",
                "authorization_id": "",
                "status": {
                    "code": 5,
                    "message": "Autorizado"
                },
                "method": "visa",
                "captured_amount": 0,
                "captured_at": "0000-00-00 00:00:00",
                "acquirer": "simulated",
                "acquirer_message": "transaction approved",
                "acquirer_code": "",
                "url_authentication": "",
                "callback_url": "",
                "card": {
                    "holder": "FULANO",
                    "number": "411111******1111",
                    "expiry_month": "12",
                    "expiry_year": "2022",
                    "brand": "visa"
                },
                "customer": {
                    "name": "Fulano da Silva",
                    "cpf_cnpj": "799.993.388-01",
                    "email": "fulano@mail.me",
                    "phone": "(12) 31232-3121",
                    "address": {
                        "street": "RUA JULIO GONZALEZ",
                        "number": "100",
                        "district": "BARRA FUNDA",
                        "complement": "",
                        "city": "SAO PAULO",
                        "state": "SP",
                        "zip_code": "01156060"
                    }
                },
                "products": [],
                "antifraud": null,
                "history": [
                    {
                        "amount": 529.19,
                        "type": "created",
                        "status": "succeeded",
                        "response_code": "",
                        "response_message": "",
                        "authorization_code": "",
                        "authorization_id": "",
                        "authorization_nsu": "",
                        "created_at": "2020-06-29 17:41:47"
                    },
                    {
                        "amount": 529.19,
                        "type": "authorized",
                        "status": "succeeded",
                        "response_code": "0",
                        "response_message": "transaction approved",
                        "authorization_code": "",
                        "authorization_id": "20200629174147590",
                        "authorization_nsu": "3cfafe98-15fd-4122-a248-d73ef486ccf2",
                        "created_at": "2020-06-29 17:41:47"
                    }
                ],
                "created_at": "2020-06-29 17:41:47",
                "updated_at": "2020-06-29 17:41:47"
            }
        },
        {
            "id": 1011822,
            "uuid": "bb2f33940ce8183e454f61099a882814",
            "resource": "transactions",
            "attributes": {
                "order_id": "1011822",
                "amount": 582.96,
                "installments": 3,
                "tid": "20200629173856552",
                "authorization_id": "",
                "status": {
                    "code": 5,
                    "message": "Autorizado"
                },
                "method": "visa",
                "captured_amount": 0,
                "captured_at": "0000-00-00 00:00:00",
                "acquirer": "simulated",
                "acquirer_message": "transaction approved",
                "acquirer_code": "",
                "url_authentication": "",
                "callback_url": "",
                "card": {
                    "holder": "FULANO",
                    "number": "411111******1111",
                    "expiry_month": "02",
                    "expiry_year": "2022",
                    "brand": "visa"
                },
                "customer": {
                    "name": "Fulano da Silva",
                    "cpf_cnpj": "799.993.388-01",
                    "email": "fulano@mail.me",
                    "phone": "(12) 31232-3121",
                    "address": {
                        "street": "RUA JULIO GONZALEZ",
                        "number": "100",
                        "district": "BARRA FUNDA",
                        "complement": "",
                        "city": "SAO PAULO",
                        "state": "SP",
                        "zip_code": "01156060"
                    }
                },
                "products": [
                    {
                        "name": "TESTE",
                        "unit_price": 529,
                        "quantity": 1,
                        "sku": "20200629173750"
                    }
                ],
                "antifraud": null,
                "history": [
                    {
                        "amount": 582.96,
                        "type": "created",
                        "status": "succeeded",
                        "response_code": "",
                        "response_message": "",
                        "authorization_code": "",
                        "authorization_id": "",
                        "authorization_nsu": "",
                        "created_at": "2020-06-29 17:38:55"
                    },
                    {
                        "amount": 582.96,
                        "type": "authorized",
                        "status": "succeeded",
                        "response_code": "0",
                        "response_message": "transaction approved",
                        "authorization_code": "",
                        "authorization_id": "20200629173856552",
                        "authorization_nsu": "421b05c8-9675-424b-9b56-b8e1a0858d9b",
                        "created_at": "2020-06-29 17:38:56"
                    }
                ],
                "created_at": "2020-06-29 17:38:55",
                "updated_at": "2020-06-29 17:38:56"
            }
        }
    ],
    "links": {
        "first": "https://api.ipag.com.br/service/resources/transactions?page=1",
        "last": "https://api.ipag.com.br/service/resources/transactions?page=30",
        "prev": null,
        "next": "https://api.ipag.com.br/service/resources/transactions?page=2"
    },
    "meta": {
        "current_page": 1,
        "last_page": 30,
        "from": 1,
        "to": 2,
        "per_page": "2",
        "total": 130
    }
}
```
### Caso de nenhuma transferência encontrada:

```json
{
    "data": []
}
```

## Consultar Transação

---
<span class="verb httpGET">GET</span> https://api.ipag.com.br/service/resources/transactions?id={id}

---

### Query Params

|   Atributo  |   Tipo   |   Descrição                |
|-------------|----------|----------------------------|
|   id        |   string  |   Identificação única da Transferência  |
|   uuid      |   string  |   Identificação única da Transferência  |

### Estrutura do conteúdo de resposta
Confira no exemplo abaixo a estrutura do conteúdo de resposta desse serviço.

|   Atributo            |   Tipo     |   Descrição                                         |
|-----------------------|------------|-----------------------------------------------------|
|   id                  |   string   |   Identificação da transferência no sistema.        |
|   uuid                |   string   |   Identificação da transferência no sistema (UUID). |
|   resource            |   string   |   Tipo do Recurso                            |
|   **[attributes]**        |   **Object**   |   **Objeto com os atributos do recurso**   |
|   order_id            |   string   |   Número do pedido                           |
|   amount            |   number   |  Valor da Transação                            |
|   installments            |   number   |   Número de parcelas                     |
|   tid            |   string   |   TID da Transação                                |
|   authorization_id            |   string   |   ID de Autorização                  |
|   **status**            |   **Object**   |     Status da Transação                |
|   method            |   string   |   Método utilizado na transação                |
|   captured_amount            |   number   |   Valor Capturado                     |
|   captured_at            |   date   |   Data da Captura                           |
|   acquirer            |   string   |   Operadora ou Adquirente                    |
|   acquirer_message            |   string   |   Mensagem de Resposta da Operadora  |
|   acquirer_code            |   string   |   Código de Resposta da Operadora    |
|   url_authentication            |   string   |       URL externa para autenticação ou Link de Boleto |
|   callback_url            |   string   |   URL de Retorno para Callback        |
|   card            |   string   |   Tipo do Recurso                             |
|   **customer**            |   **Object**   |   Dados do Cliente                |
|   **products**            |   **Object**   |   Dados dos Produtos              |
|   **antifraud**            |   **Object**   |   Dados do Antifraude            |
|   **history**            |   **Object**   |   Dados do histórico da Transação  |
|   created_at            |   date   |   Data da Criação do Recurso              |
|   updated_at            |   date   |   Date de Atualização do Recurso          |


### Conteúdo de resposta
Confira nos exemplos abaixo o conteúdo de resposta desse serviço.

Em caso de sucesso:

```json
{
    "id": 1011823,
    "uuid": "5851de4d1b821953454733521bf09700",
    "resource": "transactions",
    "attributes": {
        "order_id": "1011823",
        "amount": 529.19,
        "installments": 3,
        "tid": "20200629174147590",
        "authorization_id": "",
        "status": {
            "code": 5,
            "message": "Autorizado"
        },
        "method": "visa",
        "captured_amount": 0,
        "captured_at": "0000-00-00 00:00:00",
        "acquirer": "simulated",
        "acquirer_message": "transaction approved",
        "acquirer_code": "",
        "url_authentication": "",
        "callback_url": "",
        "card": {
            "holder": "FULANO",
            "number": "411111******1111",
            "expiry_month": "12",
            "expiry_year": "2022",
            "brand": "visa"
        },
        "customer": {
            "name": "Fulano da Silva",
            "cpf_cnpj": "799.993.388-01",
            "email": "fulano@mail.me",
            "phone": "(12) 31232-3121",
            "address": {
                "street": "RUA JULIO GONZALEZ",
                "number": "100",
                "district": "BARRA FUNDA",
                "complement": "",
                "city": "SAO PAULO",
                "state": "SP",
                "zip_code": "01156060"
            }
        },
        "products": [
            {
                "name": "TESTE",
                "unit_price": 529.19,
                "quantity": 1,
                "sku": "20200629174110"
            }
        ],
        "antifraud": null,
        "history": [
            {
                "amount": 529.19,
                "type": "created",
                "status": "succeeded",
                "response_code": "",
                "response_message": "",
                "authorization_code": "",
                "authorization_id": "",
                "authorization_nsu": "",
                "created_at": "2020-06-29 17:41:47"
            },
            {
                "amount": 529.19,
                "type": "authorized",
                "status": "succeeded",
                "response_code": "0",
                "response_message": "transaction approved",
                "authorization_code": "",
                "authorization_id": "20200629174147590",
                "authorization_nsu": "3cfafe98-15fd-4122-a248-d73ef486ccf2",
                "created_at": "2020-06-29 17:41:47"
            }
        ],
        "created_at": "2020-06-29 17:41:47",
        "updated_at": "2020-06-29 17:41:47"
    }
}
```

Em caso de transação não encontrada:

```json
{
    "code": "404",
    "message": "Transaction Not Found",
    "resource": "resource.transaction.view"
}
```
