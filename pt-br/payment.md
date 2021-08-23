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
| payment.type | string | Tipo   do Pagamento | sim | `card`, `boleto` ou `pix` |
| payment.method | string | Método de Pagamento | sim | 20 |
| payment.installments | numeric | Parcelamento   definido | sim | 1   ~ 12 |
| payment.capture | boolean | Definição se o Pagamento deve ser   capturado automaticamente | não | - |
| pament.fraud_analysis | boolean | Definição   se o Pagamento deverá ser consultado no Antifraude | não | - |
| payment.softdescriptor | string | Nome que irá aparecer na Fatura do   cartão. Seu funcionamento depende da funcionalidade estar habilitada na   adquirente. | não | 16 |
| payment.pix_expires_in | numeric | Quantidade de minutos para o pagamento Pix expirar. [`Padrão 1440 minutos`] | no | - |
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
| customer.cpf_cnpj | string | CPF   ou CNPJ do Cliente (CPF se Pessoa Física ou CNPJ se Pessoa Juridica) | não | `cpf`   ou `cnpj` |
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
| products.*.name | string | Nome do Produto | não | 100 |
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
| split_rules.hold_receivables | boolean | Define se o Recebível ficará em Espera até a solicitação de liberação (ReceivableReleaseRequest) | não | Padrão: `false` |

### Exemplo de Conteúdo a ser Enviado
Confira nos exemplos abaixo o conteúdo que poderá enviado no body da requisição.

#### Exemplo de Pagamento via Cartão de Crédito (Simples)

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

#### Exemplo de Pagamento via Cartão de Crédito (Completo)

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

#### Exemplo de Pagamento via Cartão de Crédito para Clientes Estrangeiros (Simples)

```json
{
    "amount": 100.00,
    "callback_url": "https://99mystore.com.br/ipag/callback",
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
```

#### Exemplo de Pagamento via Boleto (Completo)

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

#### Exemplo de Pagamento com Tokenização do Cartão de Crédito

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

#### Exemplo de Pagamento utilizando apenas o Token de Cartão

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

#### Exemplo de Pagamento via Cartão de Crédito com Criação de Assinatura|Cobrança Recorrente

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

#### Exemplo de Pagamento via Cartão de Crédito com Split de Pagamento

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

#### Exemplo de Pagamento via Pix (Completo)

```json
{
    "amount": 97.86,
    "callback_url": "https://99mystore.com.br/ipag/callback",
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

### Status de Retorno
| Código | Descrição                                                                                          |
|--------|----------------------------------------------------------------------------------------------------|
| 401    | Não autenticado. Verifique se foi informado corretamente as chaves de API no Basic Authentication. |
| 403    | Não Autorizado. Sua conta não tem permissão para realizar essa ação.                               |
| 406    | Não aceito. Algum dado pode ser inválido verifique os dados enviados no body.                      |
| 200    | Pagamento criado.                                                                                  |

### Estrutura do conteúdo de resposta
Confira no exemplo abaixo a estrutura do conteúdo de resposta desse serviço.

| Atributo | Tipo | Descrição |
|-|-|-|
| id | numeric | ID da Transação |
| uuid | string | Identificação   única da Transação |
| resource | string | Tipo do Recurso |
| [**attributes**]&#9660; | **Object** | Dados   da Transação |
| seller_id | string | Identificação única do vendedor |
| order_id | string | Identificação   externa do número do pedido |
| amount | numeric | Valor da Transação |
| installments | numeric | Número   de Parcelas da Transação |
| tid | string | Identificação Externa Única da   Transação (Definido pela Adquirente) |
| authorization_id | string | Identificação   Externa da Autorização (Definido pela Adquirente) |
| method | string | Método   de Pagamento da Transação |
| captured_amount | numeric | Valor Capturado da Transação |
| captured_at | datetime | Data   em que a Transação foi capturada |
| url_authentication | string | Link de Autenticação para   Transações Autenticadas e de Débito |
| callback_url | string | Url   de Callback da Transação |
| created_at | datetime | Data   de Criação da Transação |
| updated_at | datetime | Data da última atualização   da Transação |
| [**status**]&#9660; | **Object** | Dados do Estado da Transação |
| status.code | numeric | Código   do Status da Transação [Status de Pagamento](pt-br/payment_status?id=status-de-pagamento) |
| status.message | string | Mensagem do Status da Transação |
| [**acquirer**]&#9660; | **Object** | Dados da Adquirente |
| acquirer.name | string | Nome   da Adquirente |
| acquirer.message | string | Mensagem da Adquirente |
| acquirer.code | string | Código   de Retorno da Adquirente |
| [**boleto**]&#9660; | **Object** | Dados do Boleto (Somente para   Transações do Tipo Boleto) |
| boleto.due_date | date | Data   de Vencimento do Boleto |
| boleto.digitable_line | string | Linha Digitável do Boleto   (Algumas adquirentes podem não retornar esta informação) |
| boleto.link | string | Link   do Boleto |
| [**pix**]&#9660; | **Object** | Dados do Pix (Somente para   Transações do Tipo Pix) |
| pix.link | string | Link   do Pix |
| pix.qrcode | string | Código do QR-Code para renderização |
| [**card**]&#9660; | **Object** | Dados do Cartão |
| card.holder | string | Titular   do Cartão |
| card.number | string | Número Mascarado do Cartão (Bin   + Last4) |
| card.expiry_month | string | Mês   de vencimento do Cartão |
| card.expiry_year | string | Ano de vencimento do Cartão |
| card.brand | string | Bandeira   do Cartão |
| card.token | string | Token do Cartão (Somente para   solicitações de tokenização) |
| [**customer**]&#9660; | **Object** | Dados   do Cliente (Pagador) |
| customer.name | string | Nome do Cliente |
| customer.cpf_cnpj | string | CPF   ou CNPJ do Cliente |
| customer.email | string | E-mail do Cliente |
| customer.phone | string | Telefone   do Cliente |
| [**customer.billing_address**]&#9660; | **Object** | Dados do Endereço de Cobrança do   Cliente |
| customer.billing_address.street | string | Logradouro   (Rua, Avenida, Etc) do Endereço de Cobrança |
| customer.billing_address.number | string | Número do Endereço de Cobrança |
| customer.billing_address.district | string | Bairro   do Endereço de Cobrança |
| customer.billing_address.complement | string | Complemento do Endereço de   Cobrança |
| customer.billing_address.city | string | Cidade   do Endereço de Cobrança |
| customer.billing_address.state | string | Estado do Endereço de Cobrança |
| customer.billing_address.zipcode | string | CEP   do Endereço de Cobrança |
| [**customer.shipping_address**]&#9660; | **Object** | Dados do Endereço de Entrega do   Cliente |
| customer.shipping_address.street | string | Logradouro   (Rua, Avenida, Etc) do Endereço de Entrega |
| customer.shipping_address.number | string | Número do Endereço de Entrega |
| customer.shipping_address.district | string | Bairro   do Endereço de Entrega |
| customer.shipping_address.complement | string | Complemento do Endereço de   Entrega |
| customer.shipping_address.city | string | Cidade   do Endereço de Entrega |
| customer.shipping_address.state | string | Estado do Endereço de Entrega |
| customer.shipping_address.zipcode | string | CEP   do Endereço de Entrega |
| [**subscription**]&#9660; | **Object** | Dados da Assinatura (Somente   para Transações que criam Assinaturas) |
| [**products**]&#9660; | **List** | Lista dos Produtos |
| products.[].name | string | Nome do Produto |
| products.[].unit_price | numeric | Valor   Unitário do Produto |
| products.[].quantity | numeric | Quantidade do Produto |
| products.[].sku | string | Código   Externo do Produto |
| [**antifraud**]&#9660; | **Object** | Dados da Análise de Fraude |
| antifraud.score | numeric | Valor   de Score da Análise de Fraude |
| antifraud.status | string | Status da Análise de Fraude |
| antifraud.message | string | Mensagem   da Análise de Fraude |
| [**split_rules**]&#9660; | **List** | Lista das Regras de Split |
| split_rules.[].id | numeric | Identificação Única da Regra de Split |
| split_rules.[].resource | string | Nome do Recurso |
| split_rules.[].attributes | **Object** | Dados   da Regra de Split |
| split_rules.[].attributes.receiver_id | string | Identificação Única do Recebedor |
| split_rules.[].attributes.percentage | numeric | Valor   da Porcentagem (0.00 à 1.00) |
| split_rules.[].attributes.amount | numeric | Valor Fixo |
| split_rules.[].attributes.liable | boolean | Responsável   em caso de Chargeback |
| split_rules.[].attributes.charge_processing_fee | boolean | Considera a Taxa da Adquirente   na Divisão |
| split_rules.[].attributes.created_at | datetime | Data   de criação da Regra de Split |
| split_rules.[].attributes.updated_at | datetime | Data de alteração da Regra de   Split |
| [**receivables**]&#9660; | **Object** | Dados   dos Recebíveis da Transação (Somente para Sub-adquirência nativa) |
| receivables.[].id | numeric | Identificação   Única do Recebível |
| receivables.[].resource | string | Nome   do Recurso |
| receivables.[].attributes | **Object** | Dados do Recebível |
| receivables.[].attributes.receiver_id | string | ID   do Recebedor |
| receivables.[].attributes.receiver_uuid | string | Identificação Única do Recebedor |
| receivables.[].attributes.transaction | string | Identificação   Única da Transação |
| receivables.[].attributes.status | string | Status do Recebível (`pending`,   `paid`, `canceled`, `refunded`, `blocked`) |
| receivables.[].attributes.amount | numeric | Valor   Líquido do Recebível |
| receivables.[].attributes.gross_amount | numeric | Valor Bruto do Recebível |
| receivables.[].attributes.installment | numeric | Referência   da Parcela ( De 1 à 12) |
| receivables.[].attributes.description | string | Descrição do Recebível |
| receivables.[].attributes.paid_at | datetime | Data   em que o Recebível foi pago |
| receivables.[].attributes.canceled_at | datetime | Data em que o Recebível foi   cancelado |
| receivables.[].attributes.expected_on | datetime | Data   Esperada de pagamento do Recebível |
| receivables.[].attributes.created_at | datetime | Data de criação do Recebível |
| receivables.[].attributes.updated_at | datetime | Data   de alteração do Recebível |
| [**history**] | **List** | Lista   do histórico da Transação |
| history.[].amount | numeric | Valor da Transação para a   Operação |
| history.[].type | string | Tipo   de Operação |
| history.[].status | string | Status da Operação |
| history.[].response_code | string | Código   de Resposta |
| history.[].response_menssage | string | Mensagem da Resposta |
| history.[].authorization_code | string | Código   de Autorização |
| history.[].authorization_id | string | Identificação de autorização |
| history.[].authorization_nsu | string | Número   Sequêncial Único da Transação |
| history.[].created_at | datetime | Data de criação do histórico |

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
     "acquirer": {
      "name": "simulated",
      "message": "transaction canceled",
      "code": ""
    },
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

#### Em caso de sucesso (pix)

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
      "link": "https://api.ipag.com.br/pix?t=71d08e50ed9a7d0f20f0db8cacb20d03",
      "qrcode": "00020101021226780014br.gov.bcb.pix2556api.test.com/pix/v2/12345678-4321-1234-5678-12345678901252040000530398654045.005802BR5913EMPRESA TESTE6012Porto Alegre62070503***630492CA"
    },
    "url_authentication": "https://api.ipag.com.br/pix?t=49e04b8dcd7fc9cb8a5fbd656404c8c0",
    "callback_url": "https://99mystore.com.br/ipag/callback",
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
