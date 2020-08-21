# Regras de Split {docsify-ignore-all}

## Overview

> Serviço responsável por gerenciar Regras de Split de uma Transação **pré-autorizada(5)**.

## Registrar Nova Regra de Split
---
<span class="verb httpPOST">POST</span> https://api.ipag.com.br/service/resources/split_rules?transaction={id}

---

### Headers

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|

### Parâmetros 
> Os parâmetros abaixo devem ser enviados no BODY em formato JSON

|   Atributo     |   Tipo     |   Descrição               |   Obrigatório                           |   Tamanho  |
|----------------|------------|---------------------------|-----------------------------------------|------------|
|   receiver_id  |   string   |   *id* ou *uuid* do Vendedor  |   sim                               |   36       |
|   amount       |   numeric  |   Valor fixo do split     |   sim, se `percentage` não for enviado  |   -        |
|   percentage   |   numeric  |   Porcetagem              |   sim, se `amount` não for enviado      |   -        |

### Conteúdo de envio
Confira no exemplo abaixo o conteúdo que será enviado no `body` da requisição.

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

### Status de Retorno
|   Código  |   Descrição                                                                                           |
|-----------|-------------------------------------------------------------------------------------------------------|
|   401     |   Não autenticado. Verifique se foi informado corretamente as chaves de API no Basic Authentication.  |
|   403     |   Não Autorizado. Sua conta não tem permissão para realizar essa ação.                                |
|   406     |   Não aceito. Algum dado pode ser inválido verifique os dados enviados no body.                       |
|   200     |   Recurso criado.                                                                                     |

### Estrutura do conteúdo de resposta
Confira no exemplo abaixo a estrutura do conteúdo de resposta desse serviço.

|   Atributo               |   Tipo      |   Descrição                                                     |
|--------------------------|-------------|-----------------------------------------------------------------|
|   id                     |   int       |   Identificação da Regra de Split                               |
|   resources              |   string    |   Tipo do Recurso                                               |
|   **[attributes]**       |   **Object**    |   Objeto com os atributos do recurso                        |
|   receiver_id            |   string    |   uuid do Vendedor (Quem receberá)                              |
|   percentage             |   numeric   |   Porcetagem do Split                                           |
|   amount                 |   numeric   |   Valor fixo do Split (ignorado, quando informado porcentagem)  |
|   liable                 |   boolean   |   Responsável em Chargebacks                                    |
|   charge_processing_fee  |   boolean   |   Cobrar valor parcial da taxa                                  |
|   created_at             |   datetime  |   Data que o recurso foi criado                                 |
|   updated_at             |   datetime  |   Data que o recurso foi atualizado                             |


### Conteúdo de resposta
Confira no exemplo abaixo o conteúdo de resposta desse serviço.

#### Em caso de sucesso:
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

#### Em caso de falha:
```json
{
  "code": "400",
  "message": "This transaction has already been captured and it is not possible to modify its split rules.",
  "resource": "service.split_rules.store"
}
```

## Consultar Regra de Split

> Você pode consultar uma Regra de Split especifica de uma determinada Transação

---
<span class="verb httpGET">GET</span> https://api.ipag.com.br/service/resources/split_rules?transaction={transaction_id}&id={split_rule_id}

---

### Query Params

|   Atributo     |   Valor                            |   Tipo    |   Obrigatório  |
|----------------|------------------------------------|-----------|----------------|
|   transaction  |   Identificação da Transação       |   string  |   sim          |
|   id           |   Identificação da Regra de Split  |   string  |   sim          |

## Listar Regras de Split
---

> Você pode listar todas as Regras de Split de uma determinada Transação

<span class="verb httpGET">GET</span> https://api.ipag.com.br/service/resources/split_rules?transaction={transaction_id}

---

### Query Params

|   Atributo     |   Valor                            |   Tipo    |   Obrigatório  |
|----------------|------------------------------------|-----------|----------------|
|   transaction  |   Identificação da Transação       |   string  |   sim          |

## Deletar Regra de Split
---

!> Você pode deletar uma Regra de Split de uma determinada Transação que ainda não foi Capturada.

<span class="verb httpDELETE">DELETE</span> https://api.ipag.com.br/service/resources/split_rules?transaction={transaction_id}&id={split_rule_id}

---

### Query Params

|   Atributo     |   Valor                            |   Tipo    |   Obrigatório  |
|----------------|------------------------------------|-----------|----------------|
|   transaction  |   Identificação da Transação       |   string  |   sim          |
|   id           |   Identificação da Regra de Split  |   string  |   sim          |

### Status de Retorno
|   Código  |   Descrição                                                                                           |
|-----------|-------------------------------------------------------------------------------------------------------|
|   400     |   Recurso previamente removido ou transação já foi capturada                                          |
|   200     |   Recurso removido com sucesso.                                                                       |
