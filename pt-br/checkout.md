# Checkout <!-- {docsify-ignore-all} -->

## Overview

> Serviço responsável por Criar Checkouts

## Criar Checkout
---
<span class="verb httpPOST">POST</span> ***/service/resources/checkout***

---

### Headers

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|

### Parâmetros
> Os parâmetros abaixo devem ser enviados em formato JSON

| Atributo | Tipo | Descrição | Obrigatório | Tamanho |
|-|-|-|-|-|
| [**customer**]&#9660; | **Object** | Dados do Cliente (Pagador) | sim | - |
| customer.name | string | Nome do Cliente | sim | 80 |
| customer.tax_receipt | string | CPF ou CNPJ do   Cliente (CPF se Pessoa Física ou CNPJ se Pessoa Juridica) | sim | `cpf` ou `cnpj` |
| customer.email | string | E-mail do   Cliente | não | 80 |
| customer.phone | string | Número do   Telefone ou Celular do Cliente | não | 10 ou 11 |
| customer.birthdate | string | Data de   Aniversário do Cliente | não | `Y-m-d` ou `d/m/Y` |
| [**address**]&#9660; | **Object** | Dados do   Endereço do Cliente | não | - |
| address.street | string | Logradouro (Rua,   Avenida, etc) do Endereço | não | 100 |
| address.number | string | Número do   Endereço | não | 10 |
| address.district | string | Bairro do   Endereço | não | 80 |
| address.complement | string | Complemento do   Endereço | não | 50 |
| address.city | string | Cidade do   Endereço | não | 80 |
| address.state | string | Estado do   Endereço | não | 2 |
| address.zip_code | string | CEP do   Endereço | não | 8 |
| [**installment_setting**]&#9660; | **Object** | Configurações de Parcelamento | não | - |
| installment_setting.max_installments | numeric | Quantidade Máxima de Parcelas | não | 1 ~ 12 |
| installment_setting.min_installment_value | numeric | Valor Mínimo da Parcela | não | - |
| installment_setting.interest | numeric | Juros (em %) | não | - |
| installment_setting.interest_free_installments | numeric | Quantidade de Parcelas Sem Juros | não | 1 ~ 12 |
| [**order**]&#9660; | **Object** | Dados do Pedido | sim | - |
| order.order_id | string | Número do Pedido (definido pela Loja) | não | 16 |
| order.amount | numeric | Valor do Pedido | sim | - |
| order.return_url | string | URL de callback | não | 255 |
| order.return_type | string | Tipo de callback | sim | `json` ou `xml` |
| [**products**]&#9660; | **List** | **List**a de Produtos | não | - |
| products.*.name | string | Nome do Produto | não | 100 |
| products.*.unit_price | numeric | Valor Unitário do Produto | não | - |
| products.*.quantity | numeric | Quantidade de Produtos | não | - |
| products.*.sku | string | Código do Produto | não | 50 |
| products.*.description | string | Descrição do Produto | não | 255 |
| [**split_rules**]&#9660; | **List** | **List**a de Regras de Split | não | - |
| split_rules.*.receiver | string | Identificação Única do Vendedor   (Marketplace) | sim, se desejar criar a regra | 50 |
| split_rules.*.percentage | numeric | Porcentagem que   será descontado do Valor da Transação | não | - |
| split_rules.*.amount | numeric | Valor Fixo   Definido que será descontado do Valor da Transação | não | - |
| seller_id | string | ID do Vendedor (Utilizado para realizar transações em nome do Vendedor) | não | 50 |
| expires_in | numeric | Quantidade de minutos para o Link de pagamento expirar. [`Padrão 1440 minutos`] | não | - |

### Exemplo de Conteúdo a ser Enviado
Confira nos exemplos abaixo o conteúdo que poderá enviado no body da requisição.

```json
{
    "customer": {
        "name": "Fulano da Silva",
        "tax_receipt": "212.348.796-11",
        "email": "teste@email.com",
        "phone": "(11) 2222-3333",
        "birthdate": "1987-11-21",
        "address": {
            "zip_code": "01156-060",
            "street": "Rua Júlio Gonzalez",
            "number": "1023",
            "district": "Barra Funda",
            "complement": "Sala 02",
            "city": "São Paulo",
            "state": "SP"
        }
    },
    "installment_setting": {
        "max_installments": 12,
        "min_installment_value": 5,
        "interest": 0,
        "interest_free_installments": 12
    },
    "order": {
        "order_id": "100001",
        "amount": "15.00",
        "return_url": "https:\/\/www.loja.com.br\/callback",
        "return_type": "json"
    },
    "products": [
        {
            "name": "Caneca",
            "unit_price": "15.00",
            "quantity": 1,
            "sku": "CAN123",
            "description": "Caneca verde"
        },
        {
            "name": "Camiseta",
            "unit_price": "30.00",
            "quantity": 1,
            "sku": "CAM145G"
        }
    ],
    "split_rules": [
        {
            "receiver": "qwertyuiopasdfghjklzxcvbnm123456",
            "percentage": "50"
        },
        {
            "receiver": "654321mnbvcxzlkjhgfdsapoiuytrewq",
            "percentage": "20"
        }
    ],
    "expires_in": 60
}
```


### Estrutura do conteúdo de resposta
Confira no exemplo abaixo a estrutura do conteúdo de resposta desse serviço.

| Atributo | Tipo | Descrição |
|:-:|:-:|:-:|
| id | numeric | ID do Checkout |
| resource | string | Tipo do Recurso |
| [**attributes**]&#9660; | **Object** | Dados do Checkout |
| token | string | Token do Checkout |
| link | string | URL do Checkout |
| expires_at | date | Data de Validade do Checkout |
| created_at | date | Data de Criação do Checkout |
| updated_at | date | Data da Última Atualização do Checkout |

#### Em caso de sucesso

```json
{
  "id": 154,
  "resource": "checkout",
  "attributes": {
    "token": "v4b7043bv01938n1498n0809bh0a9834",
    "link": "https://api.pluscommercebr.com.br/vpos?checkout=v4b7043bv01938n1498n0809bh0a9834",
    "created_at": "2020-11-05 15:59:13",
    "created_at": "2020-11-05 14:59:13",
    "updated_at": "2020-11-05 14:59:13"
  }
}
```