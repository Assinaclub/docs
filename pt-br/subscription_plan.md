# Plano de Assinatura <!-- {docsify-ignore-all} -->

## Overview

> Serviço responsável por gerenciar Planos de Assinatura.

## Novo Plano de Assinatura
---
<span class="verb httpPOST">POST</span> ***/service/resources/plans***

---

### Headers

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |
| Accept | application/json |

### Parâmetros
> Os parâmetros abaixo devem ser enviados em formato JSON

|   Atributo   |   Tipo  |                 Descrição                | Obrigatório |             Tamanho            |
|:------------:|:-------:|:----------------------------------------:|:-----------:|:------------------------------:|
|     name     |  string |               Nome do Plano              |     Sim     |               255              |
|  description |  string |            Descrição do Plano            |     Sim     |               255              |
|    amount    | numeric |              Preço do Plano              |     Sim     |                -               |
|   frequency  |  string |     Frequência de pagamento do Plano     |     Sim     | `monthly`, `weekly` ou `daily` |
|   interval   | numeric |     Intervalo de pagamento do Plano      |     Sim     |             1 ~ 12             |
|    cycles    | numeric |        Ciclos de cobrança do Plano       |     Não     |                -               |
|     [**trial**]&#9660;  |  **Object** |   Dados do período promocional do Plano  |     Não     |       -         |
| trial.amount | numeric |        Preço promocional do Plano        |     Não     |                -               |
| trial.cycles | numeric |  Ciclos de cobrança promocional do Plano |     Não     |                -               |


### Exemplo de Conteúdo a ser Enviado
Confira no exemplo abaixo o conteúdo que será enviado no body da requisição.

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
### Status de Retorno
|   Código  |   Descrição                                                                                           |
|-----------|-------------------------------------------------------------------------------------------------------|
|   401     |   Não autenticado. Verifique se foi informado corretamente as chaves de API no Basic Authentication.  |
|   403     |   Não Autorizado. Sua conta não tem permissão para realizar essa ação.                                |
|   406     |   Não aceito. Algum dado pode ser inválido verifique os dados enviados no body.                       |
|   201     |   Recurso criado.    |

### Estrutura do conteúdo de resposta
Confira no exemplo abaixo a estrutura do conteúdo de resposta desse serviço.

|         Atributo        |    Tipo    |                Descrição                |
|:-----------------------:|:----------:|:---------------------------------------:|
|           type          |   string   |             Tipo do Recurso             |
|            id           |   numeric  |               ID do Plano               |
|    [**attributes**]&#9660;    | **Object** |              Dados do Plano             |
|           name          |   string   |              Nome do Plano              |
|       description       |   string   |            Descrição do Plano           |
|          amount         |   numeric  |              Preço do Plano             |
|        frequency        |   string   |     Frequência de pagamento do Plano    |
|         interval        |   numeric  |     Intervalo de pagamento do plano     |
|          cycles         |   numeric  |       Ciclos de cobrança do plano       |
|        created_at       |    date    |        Data de cadastro do Plano        |
|        updated_at       |    date    |       Data de atualização do Plano      |
|        [**links**]&#9660;     | **Object** |  Dados do período promocional do plano  |
|       links.payment      |   string  |        Link de pagamento para criação de Assinatura       |
|        [**trial**]&#9660;     | **Object** |  Dados do período promocional do plano  |
|       trial.amount      |   numeric  |        Preço promocional do plano       |
|       trial.cycles      |   numeric  | Ciclos de cobrança promocional do plano |


### Em caso de sucesso

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

## Alterar Plano de Assinatura
---
<span class="verb httpPUT">PUT</span> ***/service/resources/plans?id={id}***

---

### Headers

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |

!> Para alterar os dados de um **Plano** utilize os mesmos campos descritos no [Registrar Novo Plano](pt-br/subscription_plan?id=novo-plano) no `body`.<br>
Neste caso todos os campos são opcionais.

### Exemplo de Conteúdo a ser Enviado
Confira no exemplo abaixo o conteúdo que será enviado no body da requisição.

```json
{
    "amount" : 100.00
}
```


## Consultar Plano de Assinatura
---
<span class="verb httpGET">GET</span> ***/service/resources/plans?id={id}***

---

### Query Params

|   Atributo  |   Tipo   |   Descrição                |
|-------------|----------|----------------------------|
|   id        |   string |   ID do Plano de Assinatura |



> [Retorno da Consulta](pt-br/subscription_plan?id=em-caso-de-sucesso)

## Listar Planos de Assinatura
---
<span class="verb httpGET">GET</span> ***/service/resources/plans***

---

## Deletar Plano de Assinatura
---
<span class="verb httpDELETE">DELETE</span> ***/service/resources/plans?id={id}***

---

> Em caso de sucesso o **Response** será `200 OK`

!> Um Plano de Assinatura só poderá ser deletado se, e somente se não houver nenhuma assinatura utilizando-o.