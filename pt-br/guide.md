# Veja como funciona o Fluxo de Transações 

> Toda transação é representada por um código de retorno das APIs, este código representa o ponto que a transação se encontra em seu fluxo de vida, veja abaixo a lista completa de código e ocorrência para cada opção de pagamento de uma transação:

Código	Nome	Descrição	Ocorrência
1	Pendente	Iniciado uma transação pelo comprador, porém até o momento o iPag não recebeu nenhuma informação sobre o pagamento.	Boleto e Bitcoin
2	Processamento	Esta transação está em processamento e em breve deve retornar o status final da situação do pagamento.	Todos
3	Autorizado	A transação foi paga pelo comprador e o iPag já recebeu uma confirmação da instituição financeira responsável pelo processamento.	Todos
5	Em disputa	Dentro do prazo de liberação da transação o comprador abriu uma disputa para fins de contestação da transação.	Cartão de Crédito
6	Devolvido	A transação foi devolvida, assim o valor desta retornou para o comprador.	Cartão de Crédito
7	Baixado	O boleto bancário foi baixado automáticamente pela instituição financeira após 29 dias de seu vencimento.	Boleto
8	Recusado	A transação foi recusada pela operadora do cartão de crédito.	Cartões de Crédito e Débito
11	Liberado	A transação foi liberada por um usuário com perfil financeiro, entretanto até o momento não foi paga.	Boleto
12	Em cancelamento	A transação está em cancelamento até o período de baixa	Boleto
13	Chargeback	A transação não foi reconhecida pelo titular do cartão	Cartões de Crédito e Débito
14	Pré-Autorizado	A transação foi pré-autorizada no cartão de crédito do cliente. É necessário realizar a captura para finalizar a cobrança.	Cartões de Crédito
Acompanhe o fluxo de dados abaixo para verificar o comportamento de uma transação conforme sua opção de pagamento:

