# Voucher <!-- {docsify-ignore-all} -->

## Overview

> Serviço responsável por registrar e consultar vouchers.
> Vouchers são Transações que utilizam o Meio de Pagamento Voucher.


## Novo Voucher
---
<span class="verb httpPOST">POST</span> https://api.ipag.com.br/service/resources/vouchers

---

### Headers

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|

### Parâmetros 
> Os parâmetros abaixo devem ser enviados em formato JSON
    
| Atributo      | Tipo       | Descrição                                              | Obrigatório | Tamanho |
|---------------|------------|--------------------------------------------------------|-------------|---------|
| [**order**]   | **Object** | Dados do Pedido                                        | sim         |  -  |
| order_id      | string     | Número do Pedido                                       | sim         | 16 |
| amount        | numeric    | Valor do Pedido (Voucher)                              | sim         | -    |
| created_at    | datetime   | Data de emissão do voucher                             | sim         | `Y-m-d H:i:s` |
| callback_url  | string     | Url de Retorno (Callback)                              | sim         | 255 |
| [**seller**]  | **Object** | Dados do Vendedor                                      | sim         |  -   |
| cpf_cnpj      | string     | CPF ou CNPJ do *Vendedor*                              | sim         | CPF ou CNPJ Válidos |
| [**customer**]| **Object** | Dados do Comprador ou Cliente                          | sim         |  -   |
| name          | string     | Nome da Empresa ou Pessoa Física                       | sim         | 80      |
| cpf_cnpj      | string     | CPF ou CNPJ do *Cliente*                               | não         | CPF ou CNPJ Válidos |
| email         | string     | Endereço de e-mail do cliente                          | não         | 50      |
| phone         | string     | Número de Telefone ou Celular                          | não         | -       |
| birthdate     | date       | Data de Nascimento                                     | não         | -       |
| [**customer**][**address**] | **Object** | Dados do Endereço do *Cliente*           | não         | -       |
| street        | string     | Logradouro (Rua, Av, etc...)                           | não         | 70      |
| number        | string     | Número do Endereço                                     | não         | 10      |
| district      | string     | Nome do Bairro do Endereço                             | não         | 100     |
| complement    | string     | Complemento do Endereço                                | não         | 100     |
| city          | string     | Nome da Cidade                                         | não         | 50      |
| state         | string     | Sigla do Estado                                        | não         | 2       |
| zipcode       | string     | CEP do Endereço                                        | não         | 8       |


### Exemplo de Conteúdo a ser Enviado
Confira no exemplo abaixo o conteúdo que será enviado no body da requisição.

```json
{
    "order": {
        "order_id": "1000077",
        "amount": 499.99,
        "created_at": "2020-08-03 21:45:10",
        "callback_url": "https://meusite.com.br/retorno"
    },
    "seller": {
        "cpf_cnpj": "799.998.833-01"
    },
    "customer": {
        "name": "FULANO DA SILVA",
        "cpf_cnpj": "949.373.210-05",
        "email": "fulano@mail.me",
        "phone": "(11) 99780-1000",
        "birthdate": "1990-10-10",
        "address": {
            "street": "Av. Brasil",
            "number": "1000",
            "district": "Centro",
            "complement": "Ap 451",
            "city": "São Paulo",
            "state": "SP"
        }
    }
}
```

### Status de Retorno
|   Código  |   Descrição                                                                                           |
|-----------|-------------------------------------------------------------------------------------------------------|
|   401     |   Não autenticado. Verifique se foi informado corretamente as chaves de API no Basic Authentication.  |
|   403     |   Não Autorizado. Sua conta não tem permissão para realizar essa ação.                                |
|   406     |   Não aceito. Algum dado pode ser inválido verifique os dados enviados no body.                       |
|   201     |   Recurso criado.    | 


#### Em caso de sucesso

```json
{
  "id": 1011957,
  "uuid": "166a3751923064f757e01091a9659fe9",
  "resource": "vouchers",
  "attributes": {
    "seller": {
      "id": 23642,
      "uuid": "25044c8f6892d2f084f72025c377023d",
      "resource": "sellers",
      "attributes": {
        "is_active": true,
        "name": "Joseferson Mattias",
        "cpf_cnpj": "799.993.388-01"
      }
    },
    "order_id": "1000077",
    "amount": 499.99,
    "installments": 1,
    "tid": "a40f90f9-029f-40b1-9d07-9bf36001a447",
    "authorization_id": "",
    "status": {
      "code": 8,
      "message": "Aprovado e Capturado"
    },
    "method": "voucher",
    "captured_amount": 499.99,
    "captured_at": "2020-08-07 13:57:45",
    "callback_url": "https://meusite.com.br/retorno",
    "customer": {
      "name": "FULANO DA SILVA",
      "cpf_cnpj": "949.373.210-05",
      "email": "fulano@mail.me",
      "phone": "(11) 99780-1000",
      "address": {
        "street": "",
        "number": "",
        "district": "",
        "complement": "",
        "city": "",
        "state": "",
        "zip_code": ""
      }
    },
    "products": [],
    "history": [
      {
        "amount": 499.99,
        "type": "created",
        "status": "succeeded",
        "response_code": "",
        "response_message": "",
        "authorization_code": "",
        "authorization_id": "a40f90f9-029f-40b1-9d07-9bf36001a447",
        "authorization_nsu": "20200807135745256",
        "created_at": "2020-08-07 13:57:45"
      },
      {
        "amount": 499.99,
        "type": "captured",
        "status": "succeeded",
        "response_code": "",
        "response_message": "",
        "authorization_code": "",
        "authorization_id": "a40f90f9-029f-40b1-9d07-9bf36001a447",
        "authorization_nsu": "20200807135745256",
        "created_at": "2020-08-07 13:57:45"
      }
    ],
    "created_at": "2020-08-07 13:57:45",
    "updated_at": "2020-08-07 13:57:45"
  }
}
```

#### Em caso de falha

```json
{
  "code": "406",
  "message": {
    "seller.cpf_cnpj": [
      "Seller.cpf Cnpj is not a valid CPF/CNPJ."
    ]
  },
  "resource": "store.voucher"
}
```

## Cancelar Voucher
Em breve...

## Consultar Voucher
Em breve...

## Listar Vouchers
Em breve...

## Listar Vouchers Por Vendedor (CPF/CNPJ)
Em breve...