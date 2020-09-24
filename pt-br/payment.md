# Pagamento <!-- {docsify-ignore-all} -->

## Overview

> Serviço responsável por Criar, Consultar, Cancelar e Capturar Pagamentos (Transações)

## Criar Pagamento
---
<span class="verb httpPOST">POST</span> ***/service/payment***

---

### Headers

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|
| x-api-version | 2 |

### Parâmetros
> Os parâmetros abaixo devem ser enviados em formato JSON

| Atributo | Tipo | Descrição | Obrigatório | Tamanho |
|:-:|:-:|:-:|:-:|:-:|
| amount | numeric | Valor   do Pagamento | sim | - |
| order_id | string | Número do Pedido (definido pela Loja) | não | 16 |
| callback_url | string | URL   de callback | sim | 255 |
| **[payment]** | **Object** | Dados do Pagamento | sim | - |
| payment.type | string | Tipo   do Pagamento | sim | `card`   ou `boleto` |
| payment.method | string | Método de Pagamento | sim | 20 |
| payment.installments | numeric | Parcelamento   definido | sim | 1   ~ 12 |
| payment.capture | boolean | Definição se o Pagamento deve ser   capturado automaticamente | não | - |
| pament.fraud_analysis | boolean | Definição   se o Pagamento deverá ser consultado no Antifraude | não | - |
| payment.softdescriptor | string | Nome que irá aparecer na Fatura do   cartão. Seu funcionamento depende da funcionalidade estar habilitada na   adquirente. | não | 16 |
| **[payment.card]** | **Object** | Dados   do Cartão de Crédito ou Débito | sim   se payment.type = `card` | - |
| payment.card.holder | string | Nome do Titular do Cartão | sim se payment.type = `card` | 50 |
| payment.card.number | string | Número   do Cartão | sim   se payment.type = `card` | 19 |
| payment.card.expiry_month | string | Mês de validade do cartão (Good Thru) | sim se payment.type = `card` | 2 |
| payment.card.expiry_year | string | Ano   de validade do cartao (Good Thru) | sim   se payment.type = `card` | 4 |
| payment.card.cvv | string | Código de segurança do Cartão | não | 3 ou 4 |
| payment.card.token | string | Código   Tokenizado do Cartão | não | 36 |
| payment.card.tokenize | boolean | Definição se o Cartão deve ser   tokenizado para futuros pagamentos | não | - |
| **[payment.boleto]** | **Object** | Dados   do Boleto | não | - |
| payment.boleto.due_date | date | Data de Vencimento do Boleto | sim se payment.type = `boleto` | `Y-m-d` ou `d/m/Y` |
| payment.boleto.instructions | List | Lista   de Instruções que poderá ser definida no Boleto (depende do Boleto) | não | - |
| payment.boleto.instructions.*.instruction | string | Instrução do Boleto | não | 70 |
| **[customer]** | **Object** | Dados   do Cliente (Pagador) | sim | - |
| customer.name | string | Nome do Cliente | sim | 80 |
| customer.cpf_cnpj | string | CPF   ou CNPJ do Cliente (CPF se Pessoa Física ou CNPJ se Pessoa Juridica) | sim | `cpf`   ou `cnpj` |
| customer.email | string | E-mail do Cliente | não | 80 |
| customer.phone | string | Número   do Telefone ou Celular do Cliente | não | 10   ou 11 |
| customer.birthdate | string | Data de Aniversário do Cliente | não | `Y-m-d` ou `d/m/Y` |
| customer.ip | string | IP   do Cliente (Internet Protocol) | não | `ip` |
| **[customer.billing_address]** | **Object** | Dados do Endereço de Cobrança do   Cliente | não | - |
| customer.billing_address.street | string | Logradouro   (Rua, Avenida, etc) do Endereço de Cobrança | não | 100 |
| customer.billing_address.number | string | Número do Endereço de Cobrança | não | 10 |
| customer.billing_address.district | string | Bairro   do Endereço de Cobrança | não | 80 |
| customer.billing_address.complement | string | Complemento do Endereço de Cobrança | não | 50 |
| customer.billing_address.city | string | Cidade   do Endereço de Cobrança | não | 80 |
| customer.billing_address.state | string | Estado do Endereço de Cobrança | não | 2 |
| customer.billing_address.zipcode | string | CEP   do Endereço de Cobrança | não | 8 |
| **[customer.shipping_address]** | **Object** | Dados do Endereço de Entrega do Cliente | não | - |
| **[products]** | **List** | Lista   de Produtos | não | - |
| products.*.name | string | Nome do Produto | não | 255 |
| products.*.description | string | Descrição   do Produto | não | 255 |
| products.*.unit_price | numeric | Valor Unitário do Produto | não | - |
| products.*.quantity | numeric | Quantidade   de Produtos | não | - |
| products.*.sku | string | Código do Produto | não | 50 |
| **[subscription]** | **Object** | Dados   da Assinatura\|Cobrança Recorrente | não | - |
| subscription.frequency | numeric | Frequência de Intervalos que será   cobrado o valor da Assinatura | sim, se deseja criar uma assinatura | 1 ~ 12 |
| subscription.interval | string | Intervalo   de tempo que será cobrado o valor da Assinatura | sim,   se deseja criar uma assinatura | `day`,   `week`, `month` |
| subscription.start_date | date | Data de início da Assinatura à partir   da primeira cobrança. | sim, se deseja criar uma assinatura | `Y-m-d` ou `d/m/Y` |
| subscription.amount | numeric | Valor   da Assinatura | não | - |
| subscription.installments | numeric | Parcelamento (Cartão) que será aplicado   no Valor da Assinatura | não | - |
| subscription.cycles | numeric | Ciclos   de Cobrança da Assinatura | não | 0   ~ 120 |
| **[subscription.trial]** | **Object** | Dados do Período Promocional da   Assinatura | não | - |
| subscription.trial.amount | numeric | Valor   Promocional da Assinatura | não |  |
| subscription.trial.cycles | numeric | Ciclos de Cobrança do Período   Promocional da Assinatura | não | 0 ~ 120 |
| subscription.trial.frequency | numeric | Frequência   de Intervalos que será cobrado o Valor Promocional da Assinatura | não | 1   ~ 12 |
| **[split_rules]** | **List** | Lista de Regras de Split | não | - |
| split_rules.seller_id | string | Identificação   Única do Vendedor (Marketplace) | sim,   se desejar criar a regra | 50 |
| split_rules.percentage | numeric | Porcentagem que será descontado do   Valor da Transação | não | - |
| split_rules.amount | numeric | Valor   Fixo Definido que será descontado do Valor da Transação | não | - |
| split_rules.liable | boolean | Define se valor deverá ser descontado   em caso de Chargeback | não | - |
| split_rules.charge_processing_fee | boolean | Define   se deverá ser calculado a taxa da transação referente ao valor do split | não | - |

### Exemplo de Conteúdo a ser Enviado
Confira nos exemplos abaixo o conteúdo que poderá enviado no body da requisição.

> Exemplo de Pagamento via Cartão de Crédito (Simples)

```json
{
    "amount": 97.86,
    "callback_url": "https://99mystore.com.br/ipag/callback",
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

> Exemplo de Pagamento via Cartão de Crédito (Completo)

```json
{
    "amount": 100.00,
    "callback_url": "https://99mystore.com.br/ipag/callback",
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

> Exemplo de Pagamento via Boleto (Completo)

```json
{
    "amount": 100.00,
    "callback_url": "https://99mystore.com.br/ipag/callback",
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

> Exemplo de Pagamento com Tokenização do Cartão de Crédito

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

> Exemplo de Pagamento utilizando apenas o Token de Cartão

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

> Exemplo de Pagamento via Cartão de Crédito com Criação de Assinatura|Cobrança Recorrente

```json
{
    "amount": 99.00,
    "callback_url": "https://99mystore.com.br/ipag/callback",
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

> Exemplo de Pagamento via Cartão de Crédito com Split de Pagamento

```json
{
    "amount": 99.00,
    "callback_url": "https://99mystore.com.br/ipag/callback",
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

### Status de Retorno
| Código | Descrição                                                                                          |
|--------|----------------------------------------------------------------------------------------------------|
| 401    | Não autenticado. Verifique se foi informado corretamente as chaves de API no Basic Authentication. |
| 403    | Não Autorizado. Sua conta não tem permissão para realizar essa ação.                               |
| 406    | Não aceito. Algum dado pode ser inválido verifique os dados enviados no body.                      |
| 200    | Pagamento criado.                                                                                  |

#### Em caso de sucesso

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
    "acquirer": "simulated",
    "acquirer_message": "transaction captured",
    "acquirer_code": "",
    "url_authentication": "",
    "callback_url": "https://99mystore.com.br/ipag/callback",
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
    "split_rules": {
      "data": []
    },
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

#### Em caso de falha (Exemplo)

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

## Consultar Pagamento (Sonda)
---
<span class="verb httpGET">GET</span> ***/service/consult?id={id}***

---

### Headers

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|
| x-api-version | 2 |

!> Informar apenas um dos parâmetros abaixo

### Query Params

|   Atributo  |   Tipo   |   Descrição                |
|-------------|----------|----------------------------|
|   id      |   string  |   Identificação única da Transação  |
|   uuid      |   string  |   Identificação única da Transação  |
|   tid      |   string  |   TID da Transação  |
|   order_id      |   string  |   Número do Pedido informado na Transação  |

## Capturar Pagamento
---
<span class="verb httpPOST">POST</span> ***/service/capture?id={id}***

---

### Headers

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|
| x-api-version | 2 |

!> Informar apenas um dos parâmetros abaixo

### Query Params

|   Atributo  |   Tipo   |   Descrição                |
|-------------|----------|----------------------------|
|   id      |   string  |   Identificação única da Transação  |
|   uuid      |   string  |   Identificação única da Transação  |
|   tid      |   string  |   TID da Transação  |
|   order_id      |   string  |   Número do Pedido informado na Transação  |

## Cancelar Pagamento
---
<span class="verb httpPOST">POST</span> ***/service/cancel?id={id}***

---

### Headers

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|
| x-api-version | 2 |

!> Informar apenas um dos parâmetros abaixo

### Query Params

|   Atributo  |   Tipo   |   Descrição                |
|-------------|----------|----------------------------|
|   id      |   string  |   Identificação única da Transação  |
|   uuid      |   string  |   Identificação única da Transação  |
|   tid      |   string  |   TID da Transação  |
|   order_id      |   string  |   Número do Pedido informado na Transação  |