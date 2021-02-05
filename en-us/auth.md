# Autenticação

> Para o que é utilizada a API KEY ? A API KEY é utilizada para realizar a autenticação para as operações na API do iPag.

## Como funciona

A autenticação para a API ocorre através de HTTP [basic access authentication](https://en.wikipedia.org/wiki/Basic_access_authentication).

Forneça seu login e sua chave secreta de API como o nome e senha de usuário de autenticação básico.

Toda a comunicação deve ser criptografada via SSL. O token de autenticação básica é reversível, no entanto, quando toda a comunicação é sobre HTTPS o contexto de segurança está completamente protegido. Um aplicativo deve enviar um HEADER HTTP Authorization contendo o nome de usuário e senha com cada solicitação.

Todas as chamadas de API devem ser feitas em HTTPS para garantir a segurança.

Basic Auth é trivial para usar de bibliotecas cliente HTTP. Ferramentas como cURL fornecem opções de linha de comando correspondentes.

!> Você deve manter sua chave API segura não importa o quê! NÃO COMPARTILHE sua chave de API com ninguém, nem mesmo nos canais de suporte do iPag. Ninguém que represente de forma legítima o iPag nunca lhe pedirá sua chave da API!

## Como obter a sua Chave de API

Acesse o Painel do iPag ou o painel da Sandbox, clique no seu nome, localizado no canto direito superior. Em seguida, clique em Minha Conta.

Veja o seu API_ID e API_KEY logo no início da página.

!> Caso não possua uma API KEY em sua conta, faça a requisição para suporte@ipag.com.br.