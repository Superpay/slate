---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - xml
  - json
  - request
  - response

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

> Exemplo para autenticação:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
 <soapenv:Header/>
  <soapenv:Body>
   <pag:pagamentoTransacaoCompleta>
     <usuario>superpay</usuario>
     <senha>superpay</senha>
   </pag:pagamentoTransacaoCompleta>
  </soapenv:Body>
</soapenv:Envelope>
```


```json
{"usuario":
   {"login": "login", "senha": "senha" }
}
```

# Pagamentos com cartão de crédito - SOAP
## Criando uma transação simplificada

> Exemplo criação transação:

```request
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
 <soapenv:Header/>
  <soapenv:Body>
   <pag:pagamentoTransacaoCompleta>
     <transacao>
     <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
     <codigoFormaPagamento>170</codigoFormaPagamento>
     <numeroTransacao>1</numeroTransacao>
     <dadosUsuarioTransacao>
      <documentoComprador>12312312312</documentoComprador>
      <nomeComprador>Teste SuperPay</nomeComprador>
     </dadosUsuarioTransacao>
     <dataValidadeCartao>12/2026</dataValidadeCartao>
     <nomeTitularCartaoCredito>teste superpay</nomeTitularCartaoCredito>
     <numeroCartaoCredito>4444333322221111</numeroCartaoCredito>
     <codigoSeguranca>123</codigoSeguranca>
     <parcelas>1</parcelas>
     <valor>200</valor>
     </transacao>
     <usuario>superpay</usuario>
     <senha>superpay</senha>
    </pag:pagamentoTransacaoCompleta>
  </soapenv:Body>
</soapenv:Envelope>
```

```response
<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <autorizacao>123456</autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>170</codigoFormaPagamento>
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <dataAprovacaoOperadora>24/05/2017</dataAprovacaOperadora>
            <mensagemVenda>Transacao capturada com sucesso</mensagemVenda>
            <numeroComprovanteVenda>1006993069181F841001</numeroComprovanteVenda>
            <numeroTransacao>1</numeroTransacao>
            <parcelas>1</parcelas>
            <statusTransacao>1</statusTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <urlPagamento>14132971582229c00506d-e84d-4526-b902-92190d5aa808<urlpagamento></urlpagamento>
            <valor>200</valor>
            <valorDesconto>0</valorDesconto>
         </return>
      </ns2:pagamentoTransacaoCompletaResponse>
   </soap:body>
</soap:envelope>
```

Estrutura e exemplo de uma transação simples para cartão de crédito. As funcionalidades, como Análise de Fraude, Recorrência, OneClick e demais formas de pagamento precisam de uma estrutura mais completa para um perfeito funcionamento. Consulte as demais estruturas e exemplos desta documentação.

Endpoint Sandbox: `https://homologacao.superpay.com.br/checkout/servicosPagamentoCompletoWS.Services?wsdl`

Endpoint Produção: `https://superpay2.superpay.com.br/checkout/servicosPagamentoCompletoWS.Services?wsdl`

REQUISIÇÃO

<aside class="notice">
Para enviar a transação, acione o método <code>pagamentoTransacaoCompleta</code>
</aside>

TransacaoCompletaWS

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos | Sim
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway) | Numérico | 13 dígitos | Sim
codigoFormaPagamento | Código da forma de pagamento | Numérico | Até 3 dígitos | Sim
valor | Valor da transação. Deve ser enviado sem pontos ou vírgulas | Numérico | Até 10 dígitos | Sim
parcelas | Quantidade de parcelas da transação. Verificar se forma de pagamento suporta parcelamento | Numérico | Até 2 dígitos | Sim
nomeTitularCartaoCredito | Nome do titular do cartão de crédito (Exatamente como escrito no cartão) | Alfa Numérico | Até 16 dígitos | Sim
numeroCartaoCredito | Numero do cartão de crédito, sem espaços ou traços | Numérico | Até 22 caracteres | Sim
codigoSeguranca | Código de segurança do cartão (campo não é armazenado pelo SuperPay) | Numérico | Até 4 caracteres | Sim
dataValidadeCartao | Data de validade do cartão. Formato mm/yyyy | Alfa Numérico | 7 caracteres | Sim
urlCampainha | URL será sempre acionada quando o status do pedido mudar. Deve estar preparada para receber dados de campainha | Alfa Numérico | Até 250 caracteres | Não
urlRedirecionamentoPago | Para o modelo de pagamento redirect, O SuperPay redirecionará para essa URL em caso de transação aprovada | Alfa Numérico | Até 250 caracteres | Para pagamentos redirecionáveis é obrigatório
urlRedirecionamentoPago | Para o modelo de pagamento redirect, O SuperPay redirecionará para essa URL em caso de transação negada | Alfa Numérico | Até 250 caracteres | Para pagamentos redirecionáveis é obrigatório
dadosUsuarioTransacao | Array dados do comprador | - | - | -

DadosUsuarioTransacao

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
nomeComprador | Nome do comprador| Alfa Numérico | Até 100 caracteres | Não
documentoComprador | Documento principal do comprado| Alfa Numérico | 30 caracteres | Não



RESPOSTA

Campo | Descrição | Tipo | Tamanho 
------| ----------|------| --------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay | Numérico | 13 dígitos
codigoFormaPagamento | Código da forma de pagamento | Numérico | Até 3 dígitos
valor | Valor da transação.| Numérico | Até 10 dígitos
valorDesconto | Valor desconto | Numérico | Até 10 dígitos
taxaEmbarque | Valor taxa embarque | Numérico | Até 10 dígitos
parcelas | Quantidade de parcelas da transação | Numérico | Até 2 dígitos
urlPagamento | Para o modelo redirect. Essa será a URL de redirecionamento da operação |Alfa Numérico | Até 500 caracteres 
statusTransacao | Status atual da transação | Numérico | Até 2 dígitos
autorizacao | Código de autorização da operadora/intermediário financeiro | Numérico | Até 20 dígitos
codigoTransacaoOperadora | Código da transação na operadora/intermediário financeiro | Numérico | Até 20 dígitos
dataAprovacaoOperadora | Data de aprovação na operadora |Alfa Numérico | Até 10 dígitos
numeroComprovanteVenda | Número do comprovante de venda |Alfa Numérico | Até 20 dígitos
mensagemVenda | Mensagem de retorno da operadora |Alfa Numérico | Até 50 dígitos


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

