# Link de Pagamento <!-- {docsify-ignore-all} -->

## Overview

> Serviço responsável por registrar e consultar Links de Pagamento.

## Novo Link de Pagamento
---
<span class="verb httpPOST">POST</span> ***/service/resources/payment_links***

---

### Headers

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|

### Parâmetros 
> Os parâmetros abaixo devem ser enviados em formato JSON

|   Atributo  |   Tipo  |   Descrição  |   Obrigatório  |   Tamanho  |
|-|-|-|-|-|
|   external_code  |   string  |   Código exclusivo do Link de Pagamento, definido pelo usuário  |   sim  |   50  |
|   amount  |   numeric  |   Valor Fixo do Link de Pagamento, em caso de Link de Pagamento sem um valor definido, informe 0 (zero) ou não envie este campo.  |   não  |   -  |
|   **[buttons]**  |   **Object**  |   Dados dos botões de valores pré-programados  |   não  |   -  |
|   buttons.enable  |   boolean  |   Flag para ativar ou não os botões personalizados de valores.  |   não  |   -  |
|   buttons.one  |   numeric  |   Valor do primeiro botão personalizado.  |   não  |   -  |
|   buttons.two  |   numeric  |   Valor do segundo botão personalizado.  |   não  |   -  |
|   buttons.three  |   numeric  |   Valor do terceiro botão personalizado.  |   não  |   -  |
|   description  |   string  |   Descrição do Link de Pagamento, é a mensagem principal do Link de Pagamento, aqui você pode detalhar.  |   sim  |   1024  |
|   header  |   string  |   Texto do Header do Card do Link de pagamento.  |   não  |   30  |
|   subHeader  |   string  |   Sub-texto do Header do Card do Link de Pagamento  |   não  |   60  |
|   expireAt  |   datetime  |   Data de expiração do link (Y-m-d H:i:s)  |   sim  |   -  |

### Exemplo de Conteúdo a ser Enviado
Confira no exemplo abaixo o conteúdo que será enviado no body da requisição.

```json
{
	"external_code": "127",
	"amount": 0,
	"buttons": {
		"enable": true,
		"one": 10.00,
		"two": 25.00,
		"three": 100.00
	},
	"description": "LINK DE PAGAMENTO SUPER BACANA",
	"header": "DOAÇÃO AVULSA",
	"subHeader": "Doação destinada a campanha dos meninos desabrigados do Vale do Ribeira",
	"expireAt": "2020-12-25 23:59:59"
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
  "id": 1000,
  "resource": "payment_links",
  "attributes": {
    "uuid": "15e6d09b-704e-4a41-b1b5-ee8620686f5c",
    "external_code": "130",
    "amount": 0,
    "description": "LINK DE PAGAMENTO SUPER BACANA",
    "buttons": {
      "enable": false,
      "one": 0,
      "two": 0,
      "three": 0
    },
    "header": "Doe Agora",
    "subHeader": "Doação destinada a campanha dos meninos desabrigados do Vale do Ribeira",
    "expires_at": "2020-12-30 23:00:00",
    "created_at": "2020-09-03 15:52:24",
    "updated_at": "2020-09-03 15:52:24"
  },
  "links": {
    "payment": "http://api.ipag.com.br/link?t=15e6d09b-704e-4a41-b1b5-ee8620686f5c"
  }
}
```

#### Em caso de falha

```json
{
  "code": "406",
  "message": {
    "description": [
      "Description is required",
      "Description must be at least 10 characters long"
    ]
  },
  "resource": "store.payment_link"
}
```

## Consultar Link de Pagamento

---

> Você pode listar todas as Transações de um determinado Link de Pagamento

<span class="verb httpGET">GET</span> ***/service/resources/payment_links?external_code={external_code}***

---

### Query Params

|   Atributo  |   Tipo   |   Descrição                |
|-------------|----------|----------------------------|
|   external_code    |   string  |   Código exclusivo do Link de Pagamento, definido pelo usuário  |
|   id    |   numeric  |   Identificação única do Link de Pagamento  |

#### Em caso de sucesso

```json
{
  "id": 1000,
  "resource": "payment_links",
  "attributes": {
    "uuid": "15e6d09b-704e-4a41-b1b5-ee8620686f5c",
    "external_code": "130",
    "amount": 0,
    "description": "LINK DE PAGAMENTO SUPER BACANA",
    "buttons": {
      "enable": false,
      "one": 0,
      "two": 0,
      "three": 0
    },
    "header": "Doe Agora",
    "subHeader": "Doação destinada a campanha dos meninos desabrigados do Vale do Ribeira",
    "expires_at": "2020-12-30 23:00:00",
    "created_at": "2020-09-03 15:52:24",
    "updated_at": "2020-09-03 15:52:24"
  },
  "links": {
    "payment": "http://api.ipag.com.br/link?t=15e6d09b-704e-4a41-b1b5-ee8620686f5c"
  },
  "transactions": [
    {
      "id": 1023021,
      "uuid": "bfce317932ecf18f03891b43b6969b13",
      "resource": "transactions",
      "attributes": {
        "seller_id": "",
        "order_id": "20210208105102",
        "amount": 100,
        "installments": 1,
        "tid": "20210217114556769",
        "authorization_id": "",
        "status": {
          "code": 2,
          "message": "WAITING PAYMENT"
        },
        "method": "boletosimulado",
        "captured_amount": 0,
        "captured_at": "0000-00-00 00:00:00",
        "acquirer": {
          "name": "simulated",
          "message": "Boleto impresso",
          "code": ""
        },
        "created_at": "2021-02-17 11:45:56",
        "updated_at": "2021-02-17 11:45:56"
      }
    }
  ]
}
```