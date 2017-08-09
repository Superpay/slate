---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - XML
  - JSON

toc_footers:
  - <a href='#'>Documentação API SuperPay</a>


includes:
  - errors

search: true
---
# Bem vindo ao SuperPay
Tudo que você precisa para se integrar ao nosso Gateway.

## Introdução

Gateway de pagamento é uma solução tecnológica, que permite, facilmente, que quaisquer sistemas que possam se comunicar via WebService, realizem cobranças via boleto, cartão e transferências bancárias.
Uma vez integrado ao SuperPay, seu sistema estará pronto para disponibilizar diversas Formas de Pagamento.

## Glossário
Principais termos utilizados no meio eCommerce:

Termo | Descrição 
------| -------
Autorização | A etapa inicial do processo, onde a operadora financeira é acionada pelo SuperPay. Essa etapa verifica a condição de crédito do cliente, ou seja, verifica se o mesmo possui crédito suficiente para realizar a compra. Em caso positivo, aquele valor é sensibilizado no limite do cartão.
Autenticação | Processo onde o portador é encaminhado para a página do banco emissor, afim de assegurar que é o portador legítimo do cartão. 
Captura | A confirmação da transação. Nesta etapa o SuperPay aciona a operadora financeira para confirmar uma transação previamente autorizada. Somente nessa etapa em que o cliente será realmente cobrado.
Cancelamento | Cancelamento de uma compra realizada com cartão.
One Click | Através desta funcionalidade é possível o cadastro dos cartões dos consumidores para serem utilizados em compras futuras.
Recorrência | Agendamento de cobranças. O Gateway é responsável por realizar as cobranças de acordo com a data agendada pelo eCommerce.

# Fluxo de Comunicação

A comunicação com o SuperPay ocorrerá após a finalização do pagamento no Checkout do Ecommerce, de forma transparente para o consumidor.
Ao ser finalizado o pagamento dentro da loja, o Ecommerce deverá consumir os serviços WebServices do SuperPay encaminhando as informações da compra de acordo com a estrutura que será apresentada nesta documentação.

# Credencias de acesso
## Sandbox
O SuperPay disponibiliza um ambiente totalmente gratuito para sua equipe de desenvolvimento realizar testes. Basta solicitar seu ambiente para nossa equipe comercial através do email [comercial@superpay.com.br] com as seguintes informações:
  - Nome da Loja;
  - CNPJ;
  - Email para criação do cadastro.
  
## Produção
Para receber suas credenciais do nosso ambiente de produção, basta acessar nosso site e realizar a contratação de um de nossos planos: [Planos SuperPay](http://www.superpay.com.br/planos)
Caso tenha dúvidas sobre, por gentileza entrar em contato com nossa equipe comercial através do email [comercial@superpay.com.br] ou pelo telefone 11 3544-0678

# Autenticação
Para autenticação conosco, é preciso enviar o usuário e senha WS de seu estabelecimento. Caso ainda não o possua, por gentileza enviar solicitação para nossa equipe de Suporte através do email [servicedesk@superpay.com.br].

# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint retrieves a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

