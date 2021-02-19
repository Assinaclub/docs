# Authentication

> What API KEY is used for? API KEY it is used to authenticate the iPag's API operations.

## How it works

The authentication to the API happens through HTTP [basic access authentication](https://en.wikipedia.org/wiki/Basic_access_authentication).

You must provide your login and your secret API key as basic authentication user name and password.

All the comunication must be encrypted via SSL. The basic auth token it is reversible, however, when all the commutication is using HTTPS the security context it is completely protected. An application must send an HEADER HTTP Authorization having the user name and password with each request.

All the API calls must be done in HTTPS to ensure the security.

Basic Auth it is essential to use HTTP client library. Some tools like cURL provides corresponding command line as an option.

!> You must keep your API key safe, under any circumstances. DO NOT SHARE your API key with anybody, not even in iPag's support channel. Nobody legitimately from iPag will request your API key!

## How to get your API Key

Access the iPag's Painel or the Sandbox painel, click on yout name, at the right top corner. Then, click on My Account (Minha Conta).

See your API_ID and API_KEY right on the beginning of the page.

!> In case you do not have an API KEY in your account, do your request at suporte@ipag.com.br.