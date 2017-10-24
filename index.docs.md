---
title: API SuperPay

language_tabs: # must be one of https://git.io/vQNgJ
  - xml
  - curl

toc_footers:
  - <a href='#'>Documentação API SuperPay</a>


search: true
---

# Funcionalidades
## Captura posterior de transações
Com o SuperPay é possível realizar em primeiro momento apenas uma pré autorização, onde o valor da transação será apenas sensibilizado no limite do cartão, e após acionar o método de captura para a confirmação da venda e cobrança no cartão.

## Cancelamento
Cancelamento de transações rapidamente acionando o método de cancelamento.

## Estorno
Com esta funcionalidade é possível realizar cancelamentos parciais e totais, dependendo da adquirente utilizada.

**Disponibilizado em nosso plano Corporativo**

## Múltiplos Cartões
Através desta estrutura é possível o envio de mais de um cartão para aprovação em uma mesma transação, dividindo o valor total do pedido entre os mesmos.

**Disponibilizado em nosso plano Corporativo**

## Cobrança recorrente
Funcionalidade perfeita para estabelecimentos que precisam cobrar regularmente seus clientes. Faça os cadastros de suas mensalidades e o SuperPay cobrará para você de acordo com o período cadastrado.

## Boletos Registrados
Com o registro dos boletos, os bancos possuem conhecimento do mesmo desde sua geração. Atualmente o SuperPay possui em produção os boletos com registro do Itaú Offline, Itaú ShopLine, Santander, Bradesco Offline, Bradesco ShopFácil, Banco do Brasil e Caixa Econômica Federal.

O SuperPay continua trabalhando com carteiras sem registros, basta o banco permitir isto em seu cadastro.
**Para utilização do boleto registrado, entre em contato com comercial@superpay.com.br.**

## Renova Fácil
**Funcionalidade da adquirente Cielo**

Essa funcionalidade facilita a identificação de um cartão que tenha sido substituído por outro no banco emissor. Dessa forma, quando uma transação recorrente é submetida ao Web Service e a Cielo identifica que o cartão utilizado está desatualizado, a operadora retorna ao Gateway os novos dados, que por sua vez, os atualiza na base recorrente e envia novamente a cobrança a operadora, com os dados atualizados.

Bancos Emissores Participantes:

* Bradesco
* Banco do Brasil
* Santander
* Panamericano
* Citi

Para utilizar esta funcionalidade, solicite a Cielo a ativação em seu estabelecimento e ,após isto, ao Suporte SuperPay.

## Pagamentos com um único clique
Funcionalidade que permite o cadastramento de cartão para utilização nas futuras compras, assim o consumidor precisará incluir apenas o código de segurança para finalizar a compra.

**Disponibilizado em nosso plano Corporativo**

## Análise de Risco
O SuperPay possui integração com as empresas especializadas em análises de riscos: ClearSale e FControl.

## Multi Lojas
Permite a criação de diversas “lojas” para um mesmo contrato, podendo estas, serem configuradas com as mesma filiações das instituições financeiras ou diferentes.
Além disto, cada "loja" poderá acessar nosso Painel Administrativo para consultar *apenas* suas vendas.

**Para ativação solicitar ao Comercial SuperPay**

## Retentativa de Pagamento
Esta funcionalidade já gerou mais de 10% de recuperação de vendas com clientes.

As vendas não autorizadas ou com falha de comunicação, serão armazenados para retentativa durante 2 dias.
Não há alteração na estrutura de envio para utilizar esta funcionalidade. O Ecommerce deverá se atentar no retorno deste processo, onde o status para estes casos será 8 (aguardando pagamento) enquanto a venda não for aprovado ou até que sejam fechados (alterados para Não Pago) no terceiro dia.

**Além de solicitar a ativação ao Suporte SuperPay, é preciso uma alteração no campo origemTransacao enviando 5 para os pedidos que desejar a retentativa**


## Switch de Adquirência

Em caso de falha na aprovação da transação, o Gateway redirecionará o pagamento para outra adquirente configurada em seu estabelecimento.

O estabelecimento pode criar regras, juntamente com a equipe de Suporte do SuperPay, podendo assim ordenar o envio para as operadoras por bandeiras.
Lembrando que o roteamento é possível apenas em bandeiras que existem em outras operadoras. Abaixo segue lista de bandeiras que podem ser roteadas:

* Visa
* MasterCard
* Diners
* Discover

Não há alteração na estrutura de envio para utilização desta funcionalidade. O Ecommerce deverá se atentar no retorno deste processo, onde o código da forma ded pagamento será diferente do enviado.

**Disponibilizado em nosso plano Corporativo**

# Contratação e informações meios de pagamento
Na aba lateral direita está disponível exemplos para cada meio de pagamento em XML (para integração SOAP) e cURL (para integração REST).

## Adquirente Cielo

**MODALIDADE WEBSERVICE**

**Contratação**

Contratando a solução da CIELO para e-commerce será possível oferecer na sua loja:

* Vendas de cŕedito autenticadas;

* Cartão de crédito Amex;
* Cartão de crédito Aura;
* Cartão de crédito Diners;
* Cartão de crédito Discover;
* Cartão de crédito Elo;
* Cartão de crédito JCB;
* Cartão de crédito MasterCard;
* Cartão de crédito Visa;

* Cartão de débito Maestro;
* Cartão de débito Visa Electron;

Ao final do processo de contratação, deve-se estar de posse das seguintes informações para ativação da CIELO no Gateway:

* Merchant ID;
* Merchant Key;

O Superpay não participa das negociações entre o estabelecimento e bancos/adquirentes. Desta forma, taxas ou eventuais isenções são tratadas de forma direta entre os envolvidos.

Para contratar, [acesse aqui](https://www.cielo.com.br/sitecielo/afiliacao/credenciamentoafiliacaonaologado.html?idSolucaoCaptura=81)

**Particulariedades**

* Para esta modalidade é necessário certificado SSL de segurança **2048 bits**;

* Integração na plataforma WebService API 3.0;

* Caso o campo `<codigoSeguranca>` não for enviado ou for enviado com "000", a transação será encaminhada a Cielo como modelo "Recorrente", onde este campo não é obrigatório. Lembrando que para esta utilização é preciso habilitar junto a Adquirente. 
Salientamos que a conversão de seu estabelecimento pode diminuir;

* Esta operadora de cartão permite cadastrar uma informação para aparecer na fatura dos clientes quando realizarem compras sua loja, funcionalidade chamada de SoftDescriptor. Esta deverá possuir até 13 caracteres.
Caso queira utilizar, envie ao Suporte SuperPay o nome desejado para configuração em seu estabelecimento.
Também é possível o envio do SoftDescriptor por pedido, para isto solicite ao Suporte a ativação e envie a informação no campoLivre4 de cada transação;

* Para transações com *cartão de débito* ou *autenticada*, o eCommerce deverá redirecionar o consumidor para a `<urlPagamento>`, onde o mesmo deverá incluir sua senha ou token no ambiente do banco emissor. Apenas após esta etapa, a transação será concluída.


**Processo de Homologação com Adquirente**

Após a integração com o SuperPay, o estabelecimento deverá configurar as credenciais da Cielo no ambiente de produção do SuperPay e apontar sua loja para o ambiente real do Gateway. Após isto, a loja deverá enviar ao Suporte Cielo (cieloecommerce@cielo.com.br) a URL da loja com um produto de teste no valor de R$1,00.
O suporte Cielo realizará os testes em ambiente real e caso esteja dentro das conformidades a loja estará apta a realizar vendas em produção.

**Exemplos Cartão de Crédito**

REQUISIÇÃO

Estrutura simplificada de envio para adquirente Cielo. Caso seu estabelecimento utilize *antifraude*, seguir a [estrutura completa](https://superpay.github.io/soap/#criando-uma-transacao-com-analise-de-fraude).

> Estrutra simplificada SOAP de envio Cielo:

```xml

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
> Estrutra simplificada REST de envio Cielo:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary
{
   "codigoEstabelecimento" : 1000000000000,
   "codigoFormaPagamento" : 171,
   "transacao" : {
      "numeroTransacao" : 123,
      "valor" : 100,
      "parcelas" : 1,
      "idioma" : 1
   },
   "dadosCartao" : {
      "nomePortador" : "Teste Teste",
      "numeroCartao" : "0000000000000001",
      "codigoSeguranca" : "123",
      "dataValidade" : "12/2017"
   },
   "itensDoPedido" : [
  {
      "quantidadeProduto" : 1,
      "valorUnitarioProduto" : 100
  }
   ],
   "dadosCobranca" : {
      "nome" : "Teste Integração",
      "documento" : "12312312312"
   }
}

```

RESPOSTA

Estrtura de retorno adquirente Cielo. Os comentários indicam a informação retornada da adquirente em cada campo.

> Retorno:

```xml

<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <!--Código de autorização-->
            <autorizacao>123456</autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>170</codigoFormaPagamento>
            <!--Código erro em caso de negação-->
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <!--Data retorno adquirente-->
            <dataAprovacaoOperadora>24/05/2017</dataAprovacaOperadora>
            <!--Mensagem adquirente-->
            <mensagemVenda>Transacao capturada com sucesso</mensagemVenda>
            <!--TID-->
            <numeroComprovanteVenda>1006993069181F841001</numeroComprovanteVenda>
            <numeroTransacao>1</numeroTransacao>
            <parcelas>1</parcelas>
            <!--Status que deverá ser tratado pelo eCommerce-->
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

> Retorno:

```curl

--header "Content-Type: application/json"
{
   "numeroTransacao": 123,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 171,
   "valor": 2000,
   "valorDesconto": 0,
   "parcelas": 1,
   <!--Status que deverá ser tratado pelo eCommerce-->
   "statusTransacao": 1,
   <!--Código de autorização-->
   "autorizacao": "123456",
   <!--Código erro em caso de negação-->
   "codigoTransacaoOperadora": "0",
   <!--Data retorno adquirente-->
   "dataAprovacaoOperadora": "24/05/2017",
   <!--TID-->
   "numeroComprovanteVenda": "10069930690009F2122A",
   "nsu": "428706",
   <!--Mensagem adquirente-->
   "mensagemVenda": "Operation Success",
   "urlPagamento": "https://homologacao.superpay.com.br/checkout/PagamentoCielo/PagamentoCielo.do?cod=14956291484887110cf2a-9aeb-4b34-a869-1a61f0611b66",
   "cartoesUtilizados": ["000000*******0001"]
}

```


**Exemplos Cartão de Débito**

REQUISIÇÃO

Estrutura simplificada de envio para adquirente Cielo.


> Estrutra simplificada SOAP de envio Cielo:

```xml

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
 <soapenv:Header/>
  <soapenv:Body>
   <pag:pagamentoTransacaoCompleta>
     <transacao>
     <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
     <codigoFormaPagamento>178</codigoFormaPagamento>
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
     <urlCampainha>http://seusite.com.br/campainha</urlCampainha>
     <urlRedirecionamentoNaoPago>http://seusite.com.br/pago</urlRedirecionamentoNaoPago>
     <urlRedirecionamentoPago>http://seusite.com.br/naopago</urlRedirecionamentoPago>
     </transacao>
     <usuario>superpay</usuario>
     <senha>superpay</senha>
    </pag:pagamentoTransacaoCompleta>
  </soapenv:Body>
</soapenv:Envelope>

```

> Estrutra simplificada REST de envio Cielo:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary
{
   "codigoEstabelecimento" : 1000000000000,
   "codigoFormaPagamento" : 179,
   "transacao" : {
      "numeroTransacao" : 123,
      "valor" : 100,
      "parcelas" : 1,
      "idioma" : 1,
      "urlCampainha" : http://seusite.com.br/campainha,
      "urlResultado" : http://seusite.com.br/retorno
   },
   "dadosCartao" : {
      "nomePortador" : "Teste Teste",
      "numeroCartao" : "0000000000000001",
      "codigoSeguranca" : "123",
      "dataValidade" : "12/2017"
   },
   "itensDoPedido" : [
  {
      "quantidadeProduto" : 1,
      "valorUnitarioProduto" : 100
  }
   ],
   "dadosCobranca" : {
      "nome" : "Teste Integração",
      "documento" : "12312312312"
   }
}

```

RESPOSTA

Estrtura de retorno adquirente Cielo. Os comentários indicam a informação retornada da adquirente em cada campo.

> Retorno:

```xml

<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <!--Código de autorização-->
            <autorizacao>0</autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>178</codigoFormaPagamento>
            <!--Código erro em caso de negação-->
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <!--Data retorno adquirente-->
            <dataAprovacaoOperadora>2017-09-27 13:41:04<</dataAprovacaOperadora>
            <!--Mensagem adquirente-->
            <mensagemVenda></mensagemVenda>
            <!--TID-->
            <numeroComprovanteVenda>1006993069181F841001</numeroComprovanteVenda>
            <numeroTransacao>1</numeroTransacao>
            <parcelas>1</parcelas>
            <!--Status que deverá ser tratado pelo eCommerce-->
            <statusTransacao>5</statusTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <!--URL para redirecionar o consumidor-->
            <urlPagamento>https://homologacao.superpay.com.br/checkout/PagamentoCielo/PagamentoVisaElectron.do?cod=150653046191764ffaa27-0572-43fc-874d-e0bb4c6bf</urlpagamento>
            <valor>200</valor>
            <valorDesconto>0</valorDesconto>
         </return>
      </ns2:pagamentoTransacaoCompletaResponse>
   </soap:body>
</soap:envelope>

```

```curl

--header "Content-Type: application/json"
{
   "numeroTransacao": 123,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 179,
   "valor": 2000,
   "valorDesconto": 0,
   "parcelas": 1,
   <!--Status que deverá ser tratado pelo eCommerce-->
   "statusTransacao": 5,
   <!--Código de autorização-->
   "autorizacao": "0",
   <!--Código erro em caso de negação-->
   "codigoTransacaoOperadora": "0",
   <!--Data retorno adquirente-->
   "dataAprovacaoOperadora": "24/05/2017",
   <!--TID-->
   "numeroComprovanteVenda": "10069930690009F2122A",
   "nsu": "0",
   <!--Mensagem adquirente-->
   "mensagemVenda": "Operation Success",
   <!--URL para redirecionar o consumidor-->
   "urlPagamento": "https://homologacao.superpay.com.br/checkout/PagamentoCielo/PagamentoVisaElectron.do?cod=14956291484887110cf2a-9aeb-4b34-a869-1a61f0611b66",
   "cartoesUtilizados": ["000000*******0001"]
}

```

**CHECKOUT CIELO (REDIRECIONADO)**

**Contratação**

Contratando a solução da CIELO para e-commerce será possível oferecer os seguintes meios de pagamento na sua loja:

* Cartão de crédito Amex;
* Cartão de crédito Aura;
* Cartão de crédito Diners;
* Cartão de crédito Discover;
* Cartão de crédito Elo;
* Cartão de crédito JCB;
* Cartão de crédito MasterCard;
* Cartão de crédito Visa;
* Cartão de débito Maestro;
* Cartão de débito Visa Electron;

Ao final do processo de contratação, deve-se estar de posse das seguintes informações para ativação da CIELO no Gateway:

* Código de filiação;
* Merchant ID;

O Superpay não participa das negociações entre o estabelecimento e bancos/adquirentes. Desta forma, taxas ou eventuais isenções são tratadas de forma direta entre os envolvidos.

Para contratar, [acesse aqui](https://www.cielo.com.br/aceite-cartao/checkout/)

**Particulariedades**

* Para utilização correta desta forma de pagamento, deve se enviado ao Suporte SuperPay uma URL para redirecionamento do usuário após finalização do pagamento. Esta URL deve ser informada de forma completa, iniciando em HTTP ou HTTPS;

* O eCommerce deverá redirecionar o consumidor para a URL retornada no campo `<urlPagamento>`;

* Importante a utilização da [campainha](https://superpay.github.io/soap/#notificacao-vendas-comuns) para atualização de status no eCommerce após finalização do pagamento;

* O eCommerce enviará um único código ao Gateway relacionado ao meio de pagamento Checkout (52) e após a abertura da URL o consumidor escolherá a bandeira de cartão;

* Necessário algumas configurações no painel Cielo. Passo a passo no próximo tópico deste documento.


**Configuração painel Cielo**

Etapas para configuração:

1. Acesse o gerenciador Cielo
2. Clique na aba "Configurações" --> "Configurações da loja"
3. Primeiramente habilite em "Modo de Teste"
4. Inclua as URLs abaixo:

EM TESTES
*Subtituir o valor 10000000000 pelo código de estabelecimento SuperPay*

Nome Campo|URL 
------| ----------
URL Retorno   |	https://homologacao.superpay.com.br/checkout/PagamentoCielo/RetornoCheckout?codE=10000000000&acao=retorno
URL Notificação	|https://homologacao.superpay.com.br/checkout/PagamentoCielo/NotificacaoCheckout?codE=10000000000&acao=notificacao
URL de Mudança de Status| https://homologacao.superpay.com.br/checkout/PagamentoCielo/NotificacaoCheckout?codE=10000000000&acao=mudancaStatus


EM PRODUÇÃO
*Subtituir o valor 10000000000 pelo código de estabelecimento SuperPay*


Nome Campo|URL 
------| ----------
URL Retorno   |	https://superpay2.superpay.com.br/checkout/PagamentoCielo/RetornoCheckout?codE=10000000000&acao=retorno
URL Notificação|	https://superpay2.superpay.com.br/checkout/PagamentoCielo/NotificacaoCheckout?codE=10000000000&acao=notificacao
URL de Mudança de Status| https://superpay2.superpay.com.br/checkout/PagamentoCielo/NotificacaoCheckout?codE=10000000000&acao=mudancaStatus



**Processo de Homologação com Adquirente**

Após a integração com o SuperPay, o estabelecimento deverá configurar as credenciais da Cielo no ambiente de produção do SuperPay e apontar sua loja para o ambiente real do Gateway. Após isto, a loja deverá enviar ao Suporte Cielo (cieloecommerce@cielo.com.br) a URL da loja com um produto de teste no valor de R$1,00.
O suporte Cielo realizará os testes em ambiente real e caso esteja dentro das conformidades a loja estará apta a realizar vendas em produção.


**Exemplos**

REQUISIÇÃO

Estrutura simplificada de envio para adquirente Cielo. Caso seu estabelecimento utilize *antifraude*, seguir a [estrutura completa](https://superpay.github.io/soap/#criando-uma-transacao-com-analise-de-fraude).

> Estrutra simplificada SOAP de envio Cielo:

```xml

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
 <soapenv:Header/>
  <soapenv:Body>
   <pag:pagamentoTransacaoCompleta>
     <transacao>
     <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
     <codigoFormaPagamento>52</codigoFormaPagamento>
     <numeroTransacao>1</numeroTransacao>
     <dadosUsuarioTransacao>
      <documentoComprador>12312312312</documentoComprador>
      <nomeComprador>Teste SuperPay</nomeComprador>
     </dadosUsuarioTransacao>
     <parcelas>1</parcelas>
     <valor>200</valor>
     </transacao>
     <usuario>superpay</usuario>
     <senha>superpay</senha>
    </pag:pagamentoTransacaoCompleta>
  </soapenv:Body>
</soapenv:Envelope>

```

> Estrutra simplificada REST de envio Cielo:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary
{
   "codigoEstabelecimento" : 1000000000000,
   "codigoFormaPagamento" : 52,
   "transacao" : {
      "numeroTransacao" : 123,
      "valor" : 100,
      "parcelas" : 1,
      "idioma" : 1
   },
   "itensDoPedido" : [
  {
      "quantidadeProduto" : 1,
      "valorUnitarioProduto" : 100
  }
   ],
   "dadosCobranca" : {
      "nome" : "Teste Integração",
      "documento" : "12312312312"
   }
}

```

RESPOSTA

Estrtura de retorno adquirente Cielo. Os comentários indicam a informação retornada da adquirente em cada campo.

> Retorno:

```xml

<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <!--Código de autorização-->
            <autorizacao>0</autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>52</codigoFormaPagamento>
            <!--Código erro em caso de negação-->
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <!--Data retorno adquirente-->
            <dataAprovacaoOperadora/>
            <!--Mensagem adquirente-->
            <mensagemVenda/>
            <!--TID-->
            <numeroComprovanteVenda/>
            <numeroTransacao>1</numeroTransacao>
            <parcelas>1</parcelas>
            <statusTransacao>5</statusTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <!--Redirecionar o consumidor-->
            <urlPagamento>https://homologacao.superpay.com.br/checkout/PagamentoCielo/checkout?cod=14132971582229c00506d-e84d-4526-b902-92190d5aa808<urlpagamento></urlpagamento>
            <valor>200</valor>
            <valorDesconto>0</valorDesconto>
         </return>
      </ns2:pagamentoTransacaoCompletaResponse>
   </soap:body>
</soap:envelope>

```
```curl

--header "Content-Type: application/json"
{
   "numeroTransacao": 123,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 52,
   "valor": 2000,
   "valorDesconto": 0,
   "parcelas": 1,
   <!--Status que deverá ser tratado pelo eCommerce-->
   "statusTransacao": 5,
   <!--Código de autorização-->
   "autorizacao": "",
   <!--Código erro em caso de negação-->
   "codigoTransacaoOperadora": "",
   <!--Data retorno adquirente-->
   "dataAprovacaoOperadora": "",
   <!--TID-->
   "numeroComprovanteVenda": "",
   "nsu": "",
   <!--Mensagem adquirente-->
   "mensagemVenda": "",
   <!--Redirecionar o consumidor-->
   "urlPagamento": "https://homologacao.superpay.com.br/checkout/PagamentoCielo/PagamentoCielo.do?cod=14956291484887110cf2a-9aeb-4b34-a869-1a61f0611b66",
   "cartoesUtilizados": ["000000*******0001"]
}

```

## Adquirente Rede

**MODALIDADE WEBSERVICE**

**Contratação**

Contratando a solução da e-Rede será possível oferecer na sua loja:

* Cartão de crédito aunteticado;

* Cartão de crédito Visa;
* Cartão de crédito MasterCard;
* Cartão de crédito Diners;
* Cartão de crédito Hiper;
* Cartão de crédito HierpCard;
* Cartão de crédito JCB;
* Cartão de crédito Credz;


* Cartão de débito Visa Electron;
* Cartão de débito Maestro;


Ao final do processo de contratação, deve-se estar de posse das seguintes informações para ativação do e-Rede no Gateway:

* Filiação (PV);
* Token;

O Superpay não participa das negociações entre o estabelecimento e bancos/adquirentes. Desta forma, taxas ou eventuais isenções são tratadas de forma direta entre os envolvidos.

Informações sobre a contratação, [acesse aqui](https://www.userede.com.br/nossos-produtos/e-rede)

**Particulariedades**

* Para esta modalidade é necessário certificado SSL de segurança **2048 bits**;

* Esta operadora de cartão permite cadastrar uma informação para aparecer na fatura dos clientes quando realizarem compras sua loja, funcionalidade chamada de SoftDescriptor. Esta deverá possuir até 13 caracteres.
Caso queira utilizar, envie ao Suporte SuperPay o nome desejado para configuração em seu estabelecimento.

Também é possível o envio do SoftDescriptor por pedido, para isto solicite ao Suporte a ativação e envie a informação no campoLivre4 de cada transação;

* Para transações com *cartão de débito* ou *autenticada*, o eCommerce deverá enviar o "userAgente" (Identificador do browser utilizado pelo comprador no momento da compra) no campo `<campoLivre1>` e redirecionar o consumidor para o campo `<urlPagamento>` recebida no retorno, onde o mesmo deverá incluir sua senha ou token no ambiente do banco emissor. Apenas após esta etapa, a transação será concluída.


**Exemplos Cartão de Crédito**

REQUISIÇÃO

Estrutura simplificada de envio para adquirente Rede. Caso seu estabelecimento utilize *antifraude*, seguir a [estrutura completa](https://superpay.github.io/soap/#criando-uma-transacao-com-analise-de-fraude).

> Estrutra simplificada SOAP de envio Rede:

```xml

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
 <soapenv:Header/>
  <soapenv:Body>
   <pag:pagamentoTransacaoCompleta>
     <transacao>
     <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
     <codigoFormaPagamento>190</codigoFormaPagamento>
     <numeroTransacao>1</numeroTransacao>
     <dadosUsuarioTransacao>
      <documentoComprador>12312312312</documentoComprador>
      <nomeComprador>Teste SuperPay</nomeComprador>
     </dadosUsuarioTransacao>
     <dataValidadeCartao>02/2019</dataValidadeCartao>
     <nomeTitularCartaoCredito>teste superpay</nomeTitularCartaoCredito>
     <numeroCartaoCredito>4002479199570736</numeroCartaoCredito>
     <codigoSeguranca>123</codigoSeguranca>
     <parcelas>1</parcelas>
     <valor>511100</valor>
     </transacao>
     <usuario>superpay</usuario>
     <senha>superpay</senha>
    </pag:pagamentoTransacaoCompleta>
  </soapenv:Body>
</soapenv:Envelope>

```
> Estrutra simplificada REST de envio Rede:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary
{
   "codigoEstabelecimento" : 1000000000000,
   "codigoFormaPagamento" : 191,
   "transacao" : {
      "numeroTransacao" : 123,
      "valor" : 500000,
      "parcelas" : 1,
      "idioma" : 1
   },
   "dadosCartao" : {
      "nomePortador" : "Teste Teste",
      "numeroCartao" : "5899160000000005",
      "codigoSeguranca" : "123",
      "dataValidade" : "01/2019"
   },
   "itensDoPedido" : [
  {
      "quantidadeProduto" : 1,
      "valorUnitarioProduto" : 500000
  }
   ],
   "dadosCobranca" : {
      "nome" : "Teste Integração",
      "documento" : "12312312312"
   }
}

```

RESPOSTA

Estrtura de retorno adquirente Rede. Os comentários indicam a informação retornada da adquirente em cada campo.

> Retorno:

```xml

<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <!--Código de autorização-->
            <autorizacao>123456</autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>190</codigoFormaPagamento>
            <!--Código retorno Cielo-->
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <!--Data retorno adquirente-->
            <dataAprovacaoOperadora>24/05/2017</dataAprovacaOperadora>
            <!--Mensagem adquirente-->
            <mensagemVenda>00 - Success.</mensagemVenda>
            <!--TID-->
            <numeroComprovanteVenda>10117092708342800232</numeroComprovanteVenda>
            <numeroTransacao>1</numeroTransacao>
            <parcelas>1</parcelas>
            <!--Status que deverá ser tratado pelo eCommerce-->
            <statusTransacao>1</statusTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <urlPagamento>14132971582229c00506d-e84d-4526-b902-92190d5aa808<urlpagamento></urlpagamento>
            <valor>511100</valor>
            <valorDesconto>0</valorDesconto>
         </return>
      </ns2:pagamentoTransacaoCompletaResponse>
   </soap:body>
</soap:envelope>

```

> Retorno:

```curl

--header "Content-Type: application/json"
{
   "numeroTransacao": 123,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 191,
   "valor": 500000,
   "valorDesconto": 0,
   "parcelas": 1,
   <!--Status que deverá ser tratado pelo eCommerce-->
   "statusTransacao": 1,
   <!--Código de autorização-->
   "autorizacao": "123456",
   <!--Código retorno Cielo-->
   "codigoTransacaoOperadora": "0",
   <!--Data retorno adquirente-->
   "dataAprovacaoOperadora": "24/05/2017",
   <!--TID-->
   "numeroComprovanteVenda": "10117092708342800232",
   "nsu": "428706",
   <!--Mensagem adquirente-->
   "mensagemVenda": "00 - Success",
   "urlPagamento": "14956291484887110cf2a-9aeb-4b34-a869-1a61f0611b66",
   "cartoesUtilizados": ["589916******0005"]
}

```

**Exemplos Cartão de Débito**

REQUISIÇÃO

Estrutura simplificada de envio para adquirente Rede.


> Estrutra simplificada SOAP de envio Rede:

```xml

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
 <soapenv:Header/>
  <soapenv:Body>
   <pag:pagamentoTransacaoCompleta>
     <transacao>
     <campoLivre1>Mozilla/5.0 (iPad; U; CPU OS 3_2_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405</campoLivre1>
     <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
     <codigoFormaPagamento>197</codigoFormaPagamento>
     <numeroTransacao>1</numeroTransacao>
     <dadosUsuarioTransacao>
      <documentoComprador>12312312312</documentoComprador>
      <nomeComprador>Teste SuperPay</nomeComprador>
     </dadosUsuarioTransacao>
     <dataValidadeCartao>01/2019</dataValidadeCartao>
     <nomeTitularCartaoCredito>teste superpay</nomeTitularCartaoCredito>
     <numeroCartaoCredito>5899160000000005</numeroCartaoCredito>
     <codigoSeguranca>132</codigoSeguranca>
     <parcelas>1</parcelas>
     <valor>500000</valor>
     <urlCampainha>http://seusite.com.br/campainha</urlCampainha>
     <urlRedirecionamentoNaoPago>http://seusite.com.br/pago</urlRedirecionamentoNaoPago>
     <urlRedirecionamentoPago>http://seusite.com.br/naopago</urlRedirecionamentoPago>
     </transacao>
     <usuario>superpay</usuario>
     <senha>superpay</senha>
    </pag:pagamentoTransacaoCompleta>
  </soapenv:Body>
</soapenv:Envelope>

```

> Estrutra simplificada REST de envio Rede:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary
{
   "codigoEstabelecimento" : 1000000000000,
   "codigoFormaPagamento" : 198,
   "transacao" : {
      "numeroTransacao" : 123,
      "valor" : 511100,
      "parcelas" : 1,
      "idioma" : 1,
      "urlCampainha" : http://seusite.com.br/campainha,
      "urlResultado" : http://seusite.com.br/retorno,
      "campoLivre1" : Mozilla/5.0 (iPad; U; CPU OS 3_2_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405
   },
   "dadosCartao" : {
      "nomePortador" : "Teste Teste",
      "numeroCartao" : "4002479199570736",
      "codigoSeguranca" : "132",
      "dataValidade" : "01/2019"
   },
   "itensDoPedido" : [
  {
      "quantidadeProduto" : 1,
      "valorUnitarioProduto" : 100
  }
   ],
   "dadosCobranca" : {
      "nome" : "Teste Integração",
      "documento" : "12312312312"
   }
}

```

RESPOSTA

Estrtura de retorno adquirente Rede. Os comentários indicam a informação retornada da adquirente em cada campo.

> Retorno:

```xml

<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <!--Código de autorização-->
            <autorizacao>0</autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>197</codigoFormaPagamento>
            <!--Código erro em caso de negação-->
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <!--Data retorno adquirente-->
            <dataAprovacaoOperadora>2017-09-27 13:41:04<</dataAprovacaOperadora>
            <!--Mensagem adquirente-->
            <mensagemVenda>220 - Transaction request with authentication received. Redirect URL sent.</mensagemVenda>
            <!--TID-->
            <numeroComprovanteVenda/>
            <numeroTransacao>1</numeroTransacao>
            <parcelas>1</parcelas>
            <!--Status que deverá ser tratado pelo eCommerce-->
            <statusTransacao>8</statusTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <!--URL para redirecionar o consumidor-->
            <urlPagamento>https://homologacao.superpay.com.br/checkout/erede/pg.do?cod=1506533536609b7edee8b-7549-488a-9ae1-65f9f92a1b4c</urlpagamento>
            <valor>500000</valor>
            <valorDesconto>0</valorDesconto>
         </return>
      </ns2:pagamentoTransacaoCompletaResponse>
   </soap:body>
</soap:envelope>

```

```curl

--header "Content-Type: application/json"
{
   "numeroTransacao": 123,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 198,
   "valor": 511100,
   "valorDesconto": 0,
   "parcelas": 1,
   <!--Status que deverá ser tratado pelo eCommerce-->
   "statusTransacao": 8,
   <!--Código de autorização-->
   "autorizacao": "0",
   <!--Código de erro-->
   "codigoTransacaoOperadora": "0",
   <!--Data retorno adquirente-->
   "dataAprovacaoOperadora": "24/05/2017",
   <!--TID-->
   "numeroComprovanteVenda": "10069930690009F2122A",
   "nsu": "0",
   <!--Mensagem adquirente-->
   "mensagemVenda": "00 - Success",
   <!--URL para redirecionar o consumidor-->
   "urlPagamento": "https://homologacao.superpay.com.br/checkout/erede/pg.do?cod=1506533536609b7edee8b-7549-488a-9ae1-65f9f92a1b4c",
   "cartoesUtilizados": ["400247******0736"]
}

```

##Adquirente Bin - First Data


**Contratação**

Contratando a solução da Bin eCommerce será possível oferecer na sua loja:

* Cartão de crédito Visa;
* Cartão de crédito MasterCard;
* Cartão de crédito Cabal;


Ao final do processo de contratação, deve-se estar de posse das seguintes informações para ativação do e-Rede no Gateway:

* Store Id;
* User Id;
* Password;
* Arquivo do certificado BIN;
* Senha do certificado;
* Terminal ID.

O Superpay não participa das negociações entre o estabelecimento e bancos/adquirentes. Desta forma, taxas ou eventuais isenções são tratadas de forma direta entre os envolvidos.

Informações sobre a contratação, [acesse aqui](https://www.bin.com.br/content/bin/pt_br/home/peca-ja/solicite-seu-credenciamento.html)

**Particulariedades**

* Para esta modalidade é necessário certificado SSL de segurança **2048 bits**;

* Integração apenas na modalidade WebService.


**Exemplos**

REQUISIÇÃO

Estrutura simplificada de envio para adquirente Bin. Caso seu estabelecimento utilize *antifraude*, seguir a [estrutura completa](https://superpay.github.io/soap/#criando-uma-transacao-com-analise-de-fraude).

> Estrutra simplificada SOAP de envio Bin:

```xml

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
 <soapenv:Header/>
  <soapenv:Body>
   <pag:pagamentoTransacaoCompleta>
     <transacao>
     <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
     <codigoFormaPagamento>380</codigoFormaPagamento>
     <numeroTransacao>1</numeroTransacao>
     <dadosUsuarioTransacao>
      <documentoComprador>12312312312</documentoComprador>
      <nomeComprador>Teste SuperPay</nomeComprador>
     </dadosUsuarioTransacao>
     <dataValidadeCartao>12/2026</dataValidadeCartao>
     <nomeTitularCartaoCredito>teste superpay</nomeTitularCartaoCredito>
     <numeroCartaoCredito>4488540010253330004</numeroCartaoCredito>
     <codigoSeguranca>123</codigoSeguranca>
     <parcelas>1</parcelas>
     <valor>100</valor>
     </transacao>
     <usuario>superpay</usuario>
     <senha>superpay</senha>
    </pag:pagamentoTransacaoCompleta>
  </soapenv:Body>
</soapenv:Envelope>

```
> Estrutra simplificada REST de envio Bin:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary
{
   "codigoEstabelecimento" : 1000000000000,
   "codigoFormaPagamento" : 381,
   "transacao" : {
      "numeroTransacao" : 123,
      "valor" : 500000,
      "parcelas" : 1,
      "idioma" : 1
   },
   "dadosCartao" : {
      "nomePortador" : "Teste Teste",
      "numeroCartao" : "5547220000000102",
      "codigoSeguranca" : "123",
      "dataValidade" : "12/2017"
   },
   "itensDoPedido" : [
  {
      "quantidadeProduto" : 1,
      "valorUnitarioProduto" : 100
  }
   ],
   "dadosCobranca" : {
      "nome" : "Teste Integração",
      "documento" : "12312312312"
   }
}

```

RESPOSTA

Estrtura de retorno adquirente Bin. Os comentários indicam a informação retornada da adquirente em cada campo.

> Retorno:

```xml

<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <!--Código de autorização-->
            <autorizacao>657409</autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>380</codigoFormaPagamento>
            <!--Código de retorno BIN-->
            <codigoTransacaoOperadora>9113</codigoTransacaoOperadora>
            <!--Data retorno adquirente-->
            <dataAprovacaoOperadora>2017-10-20 16:36:43</dataAprovacaOperadora>
            <!--Mensagem adquirente-->
            <mensagemVenda>APPROVAL 000009113</mensagemVenda>
            <!--TID-->
            <numeroComprovanteVenda>Y:657409:4514266711:PPX :632615</numeroComprovanteVenda>
            <numeroTransacao>1</numeroTransacao>
            <parcelas>1</parcelas>
            <!--Status que deverá ser tratado pelo eCommerce-->
            <statusTransacao>1</statusTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <urlPagamento>14132971582229c00506d-e84d-4526-b902-92190d5aa808<urlpagamento></urlpagamento>
            <valor>100</valor>
            <valorDesconto>0</valorDesconto>
         </return>
      </ns2:pagamentoTransacaoCompletaResponse>
   </soap:body>
</soap:envelope>

```

> Retorno:

```curl

--header "Content-Type: application/json"
{
   "numeroTransacao": 123,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 381,
   "valor": 100,
   "valorDesconto": 0,
   "parcelas": 1,
   <!--Status que deverá ser tratado pelo eCommerce-->
   "statusTransacao": 1,
   <!--Código de autorização-->
   "autorizacao": "657409",
   <!--Código retorno Bin-->
   "codigoTransacaoOperadora": "0",
   <!--Data retorno adquirente-->
   "dataAprovacaoOperadora": "20/10/2017 16:36:43",
   <!--TID-->
   "numeroComprovanteVenda": "Y:657409:4514266711:PPX :632615",
   "nsu": "",
   <!--Mensagem adquirente-->
   "mensagemVenda": "APPROVAL 000009113",
   "urlPagamento": "14956291484887110cf2a-9aeb-4b34-a869-1a61f0611b66",
   "cartoesUtilizados": ["554722******0102"]
}

```

##Adquirente GetNet


**Contratação**

Contratando a solução da GetNet eCommerce será possível oferecer na sua loja:

* Cartão de crédito Visa;
* Cartão de crédito MasterCard;


Ao final do processo de contratação, deve-se estar de posse das seguintes informações para ativação do e-Rede no Gateway:

* Merchant Id;
* Terminal Id;
* Usuário;
* Senha.

O Superpay não participa das negociações entre o estabelecimento e bancos/adquirentes. Desta forma, taxas ou eventuais isenções são tratadas de forma direta entre os envolvidos.

Informações sobre a contratação, [acesse aqui](https://www.getnet.com.br/#/solucoes/e-commerce)


**Particulariedades**

* Para esta modalidade é necessário certificado SSL de segurança **2048 bits**;

* Integração apenas na modalidade WebService.


**Exemplos**

REQUISIÇÃO

Estrutura simplificada de envio para adquirente GetNet. Caso seu estabelecimento utilize *antifraude*, seguir a [estrutura completa](https://superpay.github.io/soap/#criando-uma-transacao-com-analise-de-fraude).

> Estrutra simplificada SOAP de envio GetNet:

```xml

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
 <soapenv:Header/>
  <soapenv:Body>
   <pag:pagamentoTransacaoCompleta>
     <transacao>
     <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
     <codigoFormaPagamento>270</codigoFormaPagamento>
     <numeroTransacao>1</numeroTransacao>
     <dadosUsuarioTransacao>
      <documentoComprador>12312312312</documentoComprador>
      <nomeComprador>Teste SuperPay</nomeComprador>
     </dadosUsuarioTransacao>
     <dataValidadeCartao>12/2026</dataValidadeCartao>
     <nomeTitularCartaoCredito>teste superpay</nomeTitularCartaoCredito>
     <numeroCartaoCredito>4012001038166662</numeroCartaoCredito>
     <codigoSeguranca>123</codigoSeguranca>
     <parcelas>1</parcelas>
     <valor>10000</valor>
     </transacao>
     <usuario>superpay</usuario>
     <senha>superpay</senha>
    </pag:pagamentoTransacaoCompleta>
  </soapenv:Body>
</soapenv:Envelope>

```
> Estrutra simplificada REST de envio GetNet:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary
{
   "codigoEstabelecimento" : 1000000000000,
   "codigoFormaPagamento" : 271,
   "transacao" : {
      "numeroTransacao" : 123,
      "valor" : 500000,
      "parcelas" : 1,
      "idioma" : 1
   },
   "dadosCartao" : {
      "nomePortador" : "Teste Teste",
      "numeroCartao" : "5453010000083303",
      "codigoSeguranca" : "123",
      "dataValidade" : "12/2017"
   },
   "itensDoPedido" : [
  {
      "quantidadeProduto" : 1,
      "valorUnitarioProduto" : 100
  }
   ],
   "dadosCobranca" : {
      "nome" : "Teste Integração",
      "documento" : "12312312312"
   }
}

```

RESPOSTA

Estrtura de retorno adquirente GetNet. Os comentários indicam a informação retornada da adquirente em cada campo.

> Retorno:

```xml

<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <!--Código de autorização-->
            <autorizacao>1234</autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>270</codigoFormaPagamento>
            <!--Código de retorno Getnet-->
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <!--Data retorno adquirente-->
            <dataAprovacaoOperadora>1016</dataAprovacaOperadora>
            <!--Mensagem adquirente-->
            <mensagemVenda>CAPTURED</mensagemVenda>
            <!--TID-->
            <numeroComprovanteVenda>1662429594</numeroComprovanteVenda>
            <numeroTransacao>1</numeroTransacao>
            <parcelas>1</parcelas>
            <!--Status que deverá ser tratado pelo eCommerce-->
            <statusTransacao>1</statusTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <urlPagamento>14132971582229c00506d-e84d-4526-b902-92190d5aa808<urlpagamento></urlpagamento>
            <valor>10000</valor>
            <valorDesconto>0</valorDesconto>
         </return>
      </ns2:pagamentoTransacaoCompletaResponse>
   </soap:body>
</soap:envelope>

```

> Retorno:

```curl

--header "Content-Type: application/json"
{
   "numeroTransacao": 123,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 271,
   "valor": 100,
   "valorDesconto": 0,
   "parcelas": 1,
   <!--Status que deverá ser tratado pelo eCommerce-->
   "statusTransacao": 1,
   <!--Código de autorização-->
   "autorizacao": "1234",
   <!--Código retorno GetNet-->
   "codigoTransacaoOperadora": "0",
   <!--Data retorno adquirente-->
   "dataAprovacaoOperadora": "1017",
   <!--TID-->
   "numeroComprovanteVenda": "1662429594",
   "nsu": "",
   <!--Mensagem adquirente-->
   "mensagemVenda": "CAPTURED",
   "urlPagamento": "14956291484887110cf2a-9aeb-4b34-a869-1a61f0611b66",
   "cartoesUtilizados": ["545301******3303"]
}

```

##Adquirente Stone


**Contratação**

Contratando a solução da Stone eCommerce será possível oferecer na sua loja:

* Cartão de crédito Visa;
* Cartão de crédito MasterCard;


Ao final do processo de contratação, deve-se estar de posse das seguintes informações para ativação do e-Rede no Gateway:

* Sale affiliation key;
* Stone Code;

O Superpay não participa das negociações entre o estabelecimento e bancos/adquirentes. Desta forma, taxas ou eventuais isenções são tratadas de forma direta entre os envolvidos.

Informações sobre a contratação, [acesse aqui](https://stone-pagamentos.typeform.com/to/A4caIq)


**Particulariedades**

* Para esta modalidade é necessário certificado SSL de segurança **2048 bits**;

* Integração apenas na modalidade WebService.


**Exemplos**

REQUISIÇÃO

Estrutura simplificada de envio para adquirente Stone. Caso seu estabelecimento utilize *antifraude*, seguir a [estrutura completa](https://superpay.github.io/soap/#criando-uma-transacao-com-analise-de-fraude).

> Estrutra simplificada SOAP de envio Stone:

```xml

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
 <soapenv:Header/>
  <soapenv:Body>
   <pag:pagamentoTransacaoCompleta>
     <transacao>
     <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
     <codigoFormaPagamento>350</codigoFormaPagamento>
     <numeroTransacao>1</numeroTransacao>
     <dadosUsuarioTransacao>
      <documentoComprador>12312312312</documentoComprador>
      <nomeComprador>Teste SuperPay</nomeComprador>
     </dadosUsuarioTransacao>
     <dataValidadeCartao>12/2026</dataValidadeCartao>
     <nomeTitularCartaoCredito>teste superpay</nomeTitularCartaoCredito>
     <numeroCartaoCredito>4444111122223333</numeroCartaoCredito>
     <codigoSeguranca>123</codigoSeguranca>
     <parcelas>1</parcelas>
     <valor>100</valor>
     </transacao>
     <usuario>superpay</usuario>
     <senha>superpay</senha>
    </pag:pagamentoTransacaoCompleta>
  </soapenv:Body>
</soapenv:Envelope>

```
> Estrutra simplificada REST de envio Stone:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary
{
   "codigoEstabelecimento" : 1000000000000,
   "codigoFormaPagamento" : 351,
   "transacao" : {
      "numeroTransacao" : 123,
      "valor" : 500000,
      "parcelas" : 1,
      "idioma" : 1
   },
   "dadosCartao" : {
      "nomePortador" : "Teste Teste",
      "numeroCartao" : "5431111111111111",
      "codigoSeguranca" : "123",
      "dataValidade" : "12/2017"
   },
   "itensDoPedido" : [
  {
      "quantidadeProduto" : 1,
      "valorUnitarioProduto" : 100
  }
   ],
   "dadosCobranca" : {
      "nome" : "Teste Integração",
      "documento" : "12312312312"
   }
}

```

RESPOSTA

Estrtura de retorno adquirente Stone. Os comentários indicam a informação retornada da adquirente em cada campo.

> Retorno:

```xml

<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <!--Código de autorização-->
            <autorizacao>1234</autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>350</codigoFormaPagamento>
            <!--Código de retorno Stone-->
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <!--Data retorno adquirente-->
            <dataAprovacaoOperadora>2014-10-16 17:20:33-03:00</dataAprovacaOperadora>
            <!--Mensagem adquirente-->
            <mensagemVenda>Transação Aprovada</mensagemVenda>
            <!--TID-->
            <numeroComprovanteVenda>1662429594</numeroComprovanteVenda>
            <numeroTransacao>1</numeroTransacao>
            <parcelas>1</parcelas>
            <!--Status que deverá ser tratado pelo eCommerce-->
            <statusTransacao>1</statusTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <urlPagamento>14132971582229c00506d-e84d-4526-b902-92190d5aa808<urlpagamento></urlpagamento>
            <valor>100</valor>
            <valorDesconto>0</valorDesconto>
         </return>
      </ns2:pagamentoTransacaoCompletaResponse>
   </soap:body>
</soap:envelope>

```

> Retorno:

```curl

--header "Content-Type: application/json"
{
   "numeroTransacao": 123,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 351,
   "valor": 100,
   "valorDesconto": 0,
   "parcelas": 1,
   <!--Status que deverá ser tratado pelo eCommerce-->
   "statusTransacao": 1,
   <!--Código de autorização-->
   "autorizacao": "1234",
   <!--Código retorno Stone-->
   "codigoTransacaoOperadora": "0",
   <!--Data retorno adquirente-->
   "dataAprovacaoOperadora": "20/10/2017 16:36:43",
   <!--TID-->
   "numeroComprovanteVenda": "1662429594",
   "nsu": "",
   <!--Mensagem adquirente-->
   "mensagemVenda": "Transação Aprovada",
   "urlPagamento": "14956291484887110cf2a-9aeb-4b34-a869-1a61f0611b66",
   "cartoesUtilizados": ["543111******1111"]
}

```

##Banco Banrisul

**Contratação**

Contratando a solução BanriCompras será possível oferecer na sua loja:

* Boleto sem registro;
* Pagamento pré datado;
* Pagamentos parcelados;
* Pagamentos á vista.


Ao final do processo de contratação, deve-se estar de posse das seguintes informações para ativação do e-Rede no Gateway:

* Código do estabelecimento Banrisul;
* Código da rede;
* Senha de consulta.

O Superpay não participa das negociações entre o estabelecimento e bancos/adquirentes. Desta forma, taxas ou eventuais isenções são tratadas de forma direta entre os envolvidos.

Informações sobre a contratação, entre em contato com *banrisul_cartoes_atendimento_adquirencia@banrisul.com.br*


**Particulariedades**

* Modalidades com redirecionamento;
* Para utilização das modalidades Banricompras, todos os campos referente aos dados do cliente devem ser preenchidos;
* Se não for informado uma data de vencimento do boleto, a data de vencimento que aparecerá no boleto será os dias de vencimento configurado internamente no Gateway;
* Processo de homologação junto ao Banrisul obrigatório para liberação em produção.


**Configurações ambiente Banrisul**

Para o Gateway de Pagamento funcionar corretamente, é necessário configurar algumas urls no ambiente do Banricompras.

CAMPAINHA

Ambiente | Link Painel Banrisul | Url Campainha | Método para envio
-------- | -------------------- | ------------- | -----------------
HOMOLOGAÇÃO | https://ww4.banrisul.com.br/banricompras/ | https://homologacao.superpay.com.br/checkout/Banrisul/NotificacaoBanrisul.do | POST
PRODUÇÃO    | https://ww7.banrisul.com.br/banricompras/ | https://superpay2.superpay.com.br/checkout/Banrisul/NotificacaoBanrisul.do | POST


PÁGINAS DE AVISO DE OPERAÇÃO

Ambiente | Link Painel Banrisul | Url Sucesso | Url Não Pago
-------- | -------------------- | ------------ | ----------------
HOMOLOGAÇÃO | https://ww4.banrisul.com.br/banricompras/ | https://homologacao.superpay.com.br/checkout/Banrisul/RedirecionamentoBanrisulOk.do | https://homologacao.superpay.com.br/superpay/Banrisul/RedirecionamentoBanrisulNoOk.do
PRODUÇÃO    | https://ww7.banrisul.com.br/banricompras/ | https://superpay2.superpay.com.br/checkout/Banrisul/RedirecionamentoBanrisulOk.do |  https://superpay2.superpay.com.br/checkout/Banrisul/RedirecionamentoBanrisulNoOk.do


**Processo de Homologação**

Após realizar a integração com o SuperPay em ambiente de testes e configurações no painel Banrisul, enviar para *tecnologia_homologacoes@banrisul.com.br* o link de acesso a página de testes, bem como o código de usuário e senha para login. Assim que o processo for finalizado pela equipe Banrisul, os mesmos enviarão os dados de produção para o estabelecimento.


**Exemplos**

REQUISIÇÃO

Estrutura de envio para banco Banrisul. 

> Estrutra SOAP de envio Banrisul:

```xml

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
   <soapenv:Header/>
   <soapenv:Body>
      <pag:pagamentoTransacaoCompleta>
         <transacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>23</codigoFormaPagamento>
            <dadosUsuarioTransacao>
               <bairroEnderecoComprador>Centro</bairroEnderecoComprador>
               <bairroEnderecoEntrega>Centro</bairroEnderecoEntrega>
               <cepEnderecoComprador>09710240</cepEnderecoComprador>
               <cepEnderecoEntrega>09710240</cepEnderecoEntrega>
               <cidadeEnderecoComprador>São Paulo</cidadeEnderecoComprador>
               <cidadeEnderecoEntrega>São Paulo</cidadeEnderecoEntrega>
               <codigoCliente></codigoCliente>
               <codigoTipoTelefoneComprador>1</codigoTipoTelefoneComprador>
               <codigoTipoTelefoneEntrega>1</codigoTipoTelefoneEntrega>
               <complementoEnderecoComprador></complementoEnderecoComprador>
               <complementoEnderecoEntrega></complementoEnderecoEntrega>
               <dataNascimentoComprador>01/01/1990</dataNascimentoComprador>
               <dddComprador>12</dddComprador>
               <dddEntrega>12</dddEntrega>
               <ddiComprador>55</ddiComprador>
               <ddiEntrega>55</ddiEntrega>
               <documentoComprador>12345678900</documentoComprador>
               <emailComprador>teste@teste.com.br</emailComprador>
               <enderecoComprador>Rua da Casa</enderecoComprador>
               <enderecoEntrega>Rua da Casa</enderecoEntrega>
               <estadoEnderecoComprador>sp</estadoEnderecoComprador>
               <estadoEnderecoEntrega>sp</estadoEnderecoEntrega>
               <nomeComprador>Nome do comprador</nomeComprador>
               <numeroEnderecoComprador>123</numeroEnderecoComprador>
               <numeroEnderecoEntrega>123</numeroEnderecoEntrega>
               <paisComprador>brasil</paisComprador>
               <paisEntrega>brasil</paisEntrega>
               <sexoComprador>m</sexoComprador>
               <telefoneComprador>36535915</telefoneComprador>
               <telefoneEntrega>36535915</telefoneEntrega>
               <tipoCliente>1</tipoCliente>
            </dadosUsuarioTransacao>
            <IP>10.10.0.1</IP>
            <idioma>1</idioma>
            <itensDoPedido>
               <codigoCategoria>1</codigoCategoria>
               <codigoProduto>1</codigoProduto>
               <nomeCategoria>Produto Teste</nomeCategoria>
               <nomeProduto>Produto Teste</nomeProduto>
               <quantidadeProduto>1</quantidadeProduto>
               <valorUnitarioProduto>100</valorUnitarioProduto>
            </itensDoPedido>
            <numeroTransacao>123456</numeroTransacao>
            <origemTransacao></origemTransacao>
            <parcelas>1</parcelas>
            <taxaEmbarque>0</taxaEmbarque>
            <urlCampainha>http://minhaloja.com.br/notificação.asp</urlCampainha>
            <urlRedirecionamentoNaoPago>http://www.pagamentonaopago.com.br</urlRedirecionamentoNaoPago>
            <urlRedirecionamentoPago>http://www.pagamentopago.com.br</urlRedirecionamentoPago>
            <valor>100</valor>
            <valorDesconto>0</valorDesconto>
         </transacao>
         <usuario>superpay</usuario>
         <senha>superpay</senha>
      </pag:pagamentoTransacaoCompleta>
   </soapenv:Body>
</soapenv:Envelope>

```
> Estrutra  REST de envio Banrisul:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary
{
   codigoEstabelecimento: 1000000000000,
   codigoFormaPagamento: 26,
   transacao: {
    numeroTransacao: 1,
    valor: 2000,
    valorDesconto: 0,
    parcelas : 1,
    urlCampainha : http://seusite.com.br/campainha,
    urlResultado : http://seusite.com.br/retorno,
    ip : "192.168.12.110",
    idioma : 1,
    dataVencimentoBoleto : "10/10/2018"
   },
   itensDoPedido: [
  {
    codigoProduto: 1,
    nomeProduto: Blusa,
    codigoCategoria: 1,
    nomeCategoria : Roupa,
    quantidadeProduto : 1,
    valorUnitarioProduto : 2000
  }
   ],
   dadosCobranca : {
    codigoCliente : 1,
    tipoCliente : 1,
    nome : Teste SuperPay,
    email : teste@teste.com,
    dataNascimento : "",
    sexo : "M",
    documento : "12312321312",
    endereco : {
    logradouro : Rua Teste,
    numero : 123,
    complemento : "",
    cep : 12345-678,
    bairro : Bairro Teste,
    cidade : Cidade Teste,
    estado : SP,
    pais : BR
  },
  telefone : [
  {
    tipoTelefone : 1,
    ddi : 55,
    ddd : 12,
    telefone : 1234-5678
  }
  ]
   },
   dadosEntrega : {
    nome : Teste SuperPay,
    email : teste@teste.com,
    endereco : {
    logradouro : Rua teste,
    numero : 123,
    complemento : "",
    cep : 12345-678,
    bairro : Bairro Teste,
    cidade : Cidade Teste,
    estado : SP,
    pais : BR
  },
  telefone : [
  {
    tipoTelefone : 1,
    ddi : 55,
    ddd : 12,
    telefone : 1234-5678
  }
  ]
   }
}
```

RESPOSTA

Estrtura de retorno adquirente Banrisul.

> Retorno:

```xml

<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <autorizacao></autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>23</codigoFormaPagamento>
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <dataAprovacaoOperadora></dataAprovacaOperadora>
            <mensagemVenda></mensagemVenda>
            <numeroComprovanteVenda></numeroComprovanteVenda>
            <numeroTransacao>1234</numeroTransacao>
            <parcelas>1</parcelas>
            <statusTransacao>8</statusTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <urlPagamento>https://homologacao.superpay.com.br/checkout/Avista/PagamentoBanrisul.do?cod=141348960683a720e602-5631-4725-8f79-268c06795a3c</urlpagamento>
            <valor>100</valor>
            <valorDesconto>0</valorDesconto>
         </return>
      </ns2:pagamentoTransacaoCompletaResponse>
   </soap:body>
</soap:envelope>

```

> Retorno:

```curl

--header "Content-Type: application/json"
{
   "numeroTransacao": 1,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 26,
   "valor": 2000,
   "valorDesconto": 0,
   "parcelas": 1,
   "statusTransacao": 8,
   "autorizacao": ,
   "codigoTransacaoOperadora": "0",
   "dataAprovacaoOperadora": ,
   "numeroComprovanteVenda": ,
   "nsu": ,
   "mensagemVenda": ,
   "urlPagamento": "https://homologacao.superpay.com.br/checkout/Boleto/PagamentoBanrisul.do?cod=141348960683a720e602-5631-4725-8f79-268c06795a3c"
}

```

##Itaú ShopLine

**Contratação**

Contratando a solução Itaú ShopLine será possível oferecer na sua loja:

* Boleto *sem* ou *com* registro, dependendo de seu contrato com o banco;
* Transferência eletrônica;

Ao final do processo de contratação, deve-se estar de posse das seguintes informações para ativação do e-Rede no Gateway:

* Código da empresa;
* Chave de acesso.

O Superpay não participa das negociações entre o estabelecimento e bancos/adquirentes. Desta forma, taxas ou eventuais isenções são tratadas de forma direta entre os envolvidos.

Informações sobre a contratação, [acesse aqui](https://www.itau.com.br/empresas/recebimentos/shopline/)


**Particulariedades**

* Modalidades com redirecionamento;
* O tamanho do número do pedido deverá ser de no máximo **8 dígitos**;
* Para utilização das modalidades ShopLine, todos os campos referente aos dados do cliente devem ser preenchidos;
* Tempo padrão de consulta no banco para atualização do status no SuperPay: 120 dias;
* Se não for informado uma data de vencimento do boleto, a data de vencimento que aparecerá no boleto será os dias de vencimento configurado internamente no Gateway;



**Configurações ambiente Itaú ShopLine**

Para o Gateway de Pagamento funcionar corretamente, é necessário configurar URL de retorno no BankLine.


Acesse o BankLine, aba Cobrança > Itaú Shopline > Informações Cadastrais e inclua a URL abaixo no campo "URL Retorno"


<aside class="notice">
URL RETORNO: https://superpay2.superpay.com.br/checkout/
</aside>



**Exemplos**

REQUISIÇÃO

Estrutura de envio para banco Itaú. 

> Estrutra SOAP de envio Itaú:

```xml

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
   <soapenv:Header/>
   <soapenv:Body>
      <pag:pagamentoTransacaoCompleta>
         <transacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>17</codigoFormaPagamento>
            <dadosUsuarioTransacao>
               <bairroEnderecoComprador>Centro</bairroEnderecoComprador>
               <bairroEnderecoEntrega>Centro</bairroEnderecoEntrega>
               <cepEnderecoComprador>09710240</cepEnderecoComprador>
               <cepEnderecoEntrega>09710240</cepEnderecoEntrega>
               <cidadeEnderecoComprador>São Paulo</cidadeEnderecoComprador>
               <cidadeEnderecoEntrega>São Paulo</cidadeEnderecoEntrega>
               <codigoCliente></codigoCliente>
               <codigoTipoTelefoneComprador>1</codigoTipoTelefoneComprador>
               <codigoTipoTelefoneEntrega>1</codigoTipoTelefoneEntrega>
               <complementoEnderecoComprador></complementoEnderecoComprador>
               <complementoEnderecoEntrega></complementoEnderecoEntrega>
               <dataNascimentoComprador>01/01/1990</dataNascimentoComprador>
               <dddComprador>12</dddComprador>
               <dddEntrega>12</dddEntrega>
               <ddiComprador>55</ddiComprador>
               <ddiEntrega>55</ddiEntrega>
               <documentoComprador>12345678900</documentoComprador>
               <emailComprador>teste@teste.com.br</emailComprador>
               <enderecoComprador>Rua da Casa</enderecoComprador>
               <enderecoEntrega>Rua da Casa</enderecoEntrega>
               <estadoEnderecoComprador>sp</estadoEnderecoComprador>
               <estadoEnderecoEntrega>sp</estadoEnderecoEntrega>
               <nomeComprador>Nome do comprador</nomeComprador>
               <numeroEnderecoComprador>123</numeroEnderecoComprador>
               <numeroEnderecoEntrega>123</numeroEnderecoEntrega>
               <paisComprador>brasil</paisComprador>
               <paisEntrega>brasil</paisEntrega>
               <sexoComprador>m</sexoComprador>
               <telefoneComprador>36535915</telefoneComprador>
               <telefoneEntrega>36535915</telefoneEntrega>
               <tipoCliente>1</tipoCliente>
            </dadosUsuarioTransacao>
            <IP>10.10.0.1</IP>
            <idioma>1</idioma>
            <itensDoPedido>
               <codigoCategoria>1</codigoCategoria>
               <codigoProduto>1</codigoProduto>
               <nomeCategoria>Produto Teste</nomeCategoria>
               <nomeProduto>Produto Teste</nomeProduto>
               <quantidadeProduto>1</quantidadeProduto>
               <valorUnitarioProduto>100</valorUnitarioProduto>
            </itensDoPedido>
            <numeroTransacao>1</numeroTransacao>
            <origemTransacao></origemTransacao>
            <parcelas>1</parcelas>
            <taxaEmbarque>0</taxaEmbarque>
            <urlCampainha>http://minhaloja.com.br/notificação.asp</urlCampainha>
            <urlRedirecionamentoNaoPago>http://www.pagamentonaopago.com.br</urlRedirecionamentoNaoPago>
            <urlRedirecionamentoPago>http://www.pagamentopago.com.br</urlRedirecionamentoPago>
            <valor>100</valor>
            <valorDesconto>0</valorDesconto>
            <vencimentoBoleto>10/10/2018</vencimentoBoleto>
         </transacao>
         <usuario>superpay</usuario>
         <senha>superpay</senha>
      </pag:pagamentoTransacaoCompleta>
   </soapenv:Body>
</soapenv:Envelope>

```
> Estrutra  REST de envio Itaú:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary
{
   codigoEstabelecimento: 1000000000000,
   codigoFormaPagamento: 16,
   transacao: {
    numeroTransacao: 1,
    valor: 2000,
    valorDesconto: 0,
    parcelas : 1,
    urlCampainha : http://seusite.com.br/campainha,
    urlResultado : http://seusite.com.br/retorno,
    ip : "192.168.12.110",
    idioma : 1
   },
   itensDoPedido: [
  {
    codigoProduto: 1,
    nomeProduto: Blusa,
    codigoCategoria: 1,
    nomeCategoria : Roupa,
    quantidadeProduto : 1,
    valorUnitarioProduto : 2000
  }
   ],
   dadosCobranca : {
    codigoCliente : 1,
    tipoCliente : 1,
    nome : Teste SuperPay,
    email : teste@teste.com,
    dataNascimento : "",
    sexo : "M",
    documento : "12312321312",
    endereco : {
    logradouro : Rua Teste,
    numero : 123,
    complemento : "",
    cep : 12345-678,
    bairro : Bairro Teste,
    cidade : Cidade Teste,
    estado : SP,
    pais : BR
  },
  telefone : [
  {
    tipoTelefone : 1,
    ddi : 55,
    ddd : 12,
    telefone : 1234-5678
  }
  ]
   },
   dadosEntrega : {
    nome : Teste SuperPay,
    email : teste@teste.com,
    endereco : {
    logradouro : Rua teste,
    numero : 123,
    complemento : "",
    cep : 12345-678,
    bairro : Bairro Teste,
    cidade : Cidade Teste,
    estado : SP,
    pais : BR
  },
  telefone : [
  {
    tipoTelefone : 1,
    ddi : 55,
    ddd : 12,
    telefone : 1234-5678
  }
  ]
   }
}
```

RESPOSTA

Estrtura de retorno Itaú.

> Retorno:

```xml

<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <autorizacao></autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>17</codigoFormaPagamento>
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <dataAprovacaoOperadora></dataAprovacaOperadora>
            <mensagemVenda></mensagemVenda>
            <numeroComprovanteVenda></numeroComprovanteVenda>
            <numeroTransacao>1</numeroTransacao>
            <parcelas>1</parcelas>
            <statusTransacao>8</statusTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <urlPagamento>https://homologacao.superpay.com.br/checkout/PagamentoItauShopline/PagamentoItauShopline.do?cod=141348960683a720e602-5631-4725-8f79-268c06795a3c</urlpagamento>
            <valor>100</valor>
            <valorDesconto>0</valorDesconto>
         </return>
      </ns2:pagamentoTransacaoCompletaResponse>
   </soap:body>
</soap:envelope>

```

> Retorno:

```curl

--header "Content-Type: application/json"
{
   "numeroTransacao": 1,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 16,
   "valor": 2000,
   "valorDesconto": 0,
   "parcelas": 1,
   "statusTransacao": 8,
   "autorizacao": ,
   "codigoTransacaoOperadora": "0",
   "dataAprovacaoOperadora": ,
   "numeroComprovanteVenda": ,
   "nsu": ,
   "mensagemVenda": ,
   "urlPagamento": "https://homologacao.superpay.com.br/checkout/PagamentoItauShopline/PagamentoItauShopline.do?cod=141348960683a720e602-5631-4725-8f79-268c06795a3c"
}

```

##Bradesco ShopFácil

**BOLETO**

**Contratação**

Contratando a solução Bradesco ShopFácil será possível oferecer na sua loja:

* Boleto *sem* ou *com* registro, dependendo de seu contrato com o banco;


Ao final do processo de contratação, deve-se estar de posse das seguintes informações para ativação do e-Rede no Gateway:

* Merchantid;
* Email de acesso ao gerenciador;
* Chave de Acesso; (Gerada dentro do gerenciador do Bradesco, passo a passo abaixo);
* Número da Carteira.

O Superpay não participa das negociações entre o estabelecimento e bancos/adquirentes. Desta forma, taxas ou eventuais isenções são tratadas de forma direta entre os envolvidos.

Informações sobre a contratação, [acesse aqui]( http://www.bradesco.com.br/html/corporate/produtos-servicos/parcerias-e-oportunidades/shopfacil-empresa.shtm)


**Particulariedades**

* Modalidades com redirecionamento;
* Para utilização das modalidades ShopFácil, todos os campos referente aos dados do cliente devem ser preenchidos;
* Se não for informado uma data de vencimento do boleto, a data de vencimento que aparecerá no boleto será os dias de vencimento configurado internamente no Gateway;



**Configurações ambiente ShopFácil**

*Gerando chave de acesso*

Procedimento idêntico em ambos ambientes:

Ambiente | URL
-------- | --------
Homologação |  [https://homolog.meiosdepagamentobradesco.com.br/gerenciadorapi](https://homolog.meiosdepagamentobradesco.com.br/gerenciadorapi)
Produção    |  [https://meiosdepagamentobradesco.com.br/gerenciadorapi](https://meiosdepagamentobradesco.com.br/gerenciadorapi)

Após logar, acesse Configurações > Meios de Pagamento > Boleto Bancário, inclua uma palavra secreta para geração da chave.



*Configurando URL de notificação*

Após logar, acesse Configuração > Meios de Pagamento > Boleto Bancário, incluir no campo "URL de notificação":

Ambiente | URL
-------- | --------
Homologação | https://homologacao.superpay.com.br/checkout/bradesco/confirmaBoletoRegistro
Produção | https://superpay2.superpay.com.br/checkout/bradesco/confirmaBoletoRegistro


**Processo de Homologação**

Procedimento deve ser realizado em duas etapas, ambiente de homologação e ambiente de Produção:

*Homologação*

Realizar as configurações no painel Bradesco conforme informado acima e solicitar ao Suporte SuperPay realizar a configuração do meio de pagamento em nosso ambiente de homologação, informando a ele os dados abaixo:

* Merchantid;
* Email de acesso ao gerenciador de homologação;
* Chave de acesso de homologação;
* Número da carteira;


A loja virtual deverá estar apontando para o ambiente de homologação e depois disto basta enviar email kit@scopus.com.br com a URL da loja com um produto de R$1,00 disponível para testes e o CNPJ do estabelecimento.
Depois que a equipe do Bradesco validar este ambiente, o estabelecimento receberá novos dados, desta vez os dados reais da loja. E o procedimento deverá ser repedito porém desta vez em ambiente de produção.

*Produção*

Realizar as configurações no painel Bradesco conforme informado acima e solicitar ao Suporte SuperPay realizar a configuração do meio de pagamento em nosso ambiente de produção, informando a ele os dados abaixo:

* Merchantid;
* Email de acesso ao gerenciador de homologação;
* Chave de acesso de homologação;
* Número da carteira;


A loja virtual deverá estar apontando para o ambiente de produção e depois disto basta enviar email kit@scopus.com.br com a URL da loja com um produto de R$1,00 disponível para testes e o CNPJ do estabelecimento.
Depois que a equipe do Bradesco validar este ambiente, o estabelecimento estará apto a realizar vendas em produção.


**Exemplos**

REQUISIÇÃO

Estrutura de envio para banco Bradesco. 

> Estrutra SOAP de envio Bradesco Boleto:

```xml

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
   <soapenv:Header/>
   <soapenv:Body>
      <pag:pagamentoTransacaoCompleta>
         <transacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>19</codigoFormaPagamento>
            <dadosUsuarioTransacao>
               <bairroEnderecoComprador>Centro</bairroEnderecoComprador>
               <bairroEnderecoEntrega>Centro</bairroEnderecoEntrega>
               <cepEnderecoComprador>09710240</cepEnderecoComprador>
               <cepEnderecoEntrega>09710240</cepEnderecoEntrega>
               <cidadeEnderecoComprador>São Paulo</cidadeEnderecoComprador>
               <cidadeEnderecoEntrega>São Paulo</cidadeEnderecoEntrega>
               <codigoCliente></codigoCliente>
               <codigoTipoTelefoneComprador>1</codigoTipoTelefoneComprador>
               <codigoTipoTelefoneEntrega>1</codigoTipoTelefoneEntrega>
               <complementoEnderecoComprador></complementoEnderecoComprador>
               <complementoEnderecoEntrega></complementoEnderecoEntrega>
               <dataNascimentoComprador>01/01/1990</dataNascimentoComprador>
               <dddComprador>12</dddComprador>
               <dddEntrega>12</dddEntrega>
               <ddiComprador>55</ddiComprador>
               <ddiEntrega>55</ddiEntrega>
               <documentoComprador>12345678900</documentoComprador>
               <emailComprador>teste@teste.com.br</emailComprador>
               <enderecoComprador>Rua da Casa</enderecoComprador>
               <enderecoEntrega>Rua da Casa</enderecoEntrega>
               <estadoEnderecoComprador>sp</estadoEnderecoComprador>
               <estadoEnderecoEntrega>sp</estadoEnderecoEntrega>
               <nomeComprador>Nome do comprador</nomeComprador>
               <numeroEnderecoComprador>123</numeroEnderecoComprador>
               <numeroEnderecoEntrega>123</numeroEnderecoEntrega>
               <paisComprador>brasil</paisComprador>
               <paisEntrega>brasil</paisEntrega>
               <sexoComprador>m</sexoComprador>
               <telefoneComprador>36535915</telefoneComprador>
               <telefoneEntrega>36535915</telefoneEntrega>
               <tipoCliente>1</tipoCliente>
            </dadosUsuarioTransacao>
            <IP>10.10.0.1</IP>
            <idioma>1</idioma>
            <itensDoPedido>
               <codigoCategoria>1</codigoCategoria>
               <codigoProduto>1</codigoProduto>
               <nomeCategoria>Produto Teste</nomeCategoria>
               <nomeProduto>Produto Teste</nomeProduto>
               <quantidadeProduto>1</quantidadeProduto>
               <valorUnitarioProduto>100</valorUnitarioProduto>
            </itensDoPedido>
            <numeroTransacao>1</numeroTransacao>
            <origemTransacao></origemTransacao>
            <parcelas>1</parcelas>
            <taxaEmbarque>0</taxaEmbarque>
            <urlCampainha>http://minhaloja.com.br/notificação.asp</urlCampainha>
            <urlRedirecionamentoNaoPago>http://www.pagamentonaopago.com.br</urlRedirecionamentoNaoPago>
            <urlRedirecionamentoPago>http://www.pagamentopago.com.br</urlRedirecionamentoPago>
            <valor>100</valor>
            <valorDesconto>0</valorDesconto>
            <vencimentoBoleto>10/10/2018</vencimentoBoleto>
         </transacao>
         <usuario>superpay</usuario>
         <senha>superpay</senha>
      </pag:pagamentoTransacaoCompleta>
   </soapenv:Body>
</soapenv:Envelope>

```
> Estrutra  REST de envio Bradesco Transferência:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary
{
   codigoEstabelecimento: 1000000000000,
   codigoFormaPagamento: 18,
   transacao: {
    numeroTransacao: 1,
    valor: 2000,
    valorDesconto: 0,
    parcelas : 1,
    urlCampainha : http://seusite.com.br/campainha,
    urlResultado : http://seusite.com.br/retorno,
    ip : "192.168.12.110",
    idioma : 1
   },
   itensDoPedido: [
  {
    codigoProduto: 1,
    nomeProduto: Blusa,
    codigoCategoria: 1,
    nomeCategoria : Roupa,
    quantidadeProduto : 1,
    valorUnitarioProduto : 2000
  }
   ],
   dadosCobranca : {
    codigoCliente : 1,
    tipoCliente : 1,
    nome : Teste SuperPay,
    email : teste@teste.com,
    dataNascimento : "",
    sexo : "M",
    documento : "12312321312",
    endereco : {
    logradouro : Rua Teste,
    numero : 123,
    complemento : "",
    cep : 12345-678,
    bairro : Bairro Teste,
    cidade : Cidade Teste,
    estado : SP,
    pais : BR
  },
  telefone : [
  {
    tipoTelefone : 1,
    ddi : 55,
    ddd : 12,
    telefone : 1234-5678
  }
  ]
   },
   dadosEntrega : {
    nome : Teste SuperPay,
    email : teste@teste.com,
    endereco : {
    logradouro : Rua teste,
    numero : 123,
    complemento : "",
    cep : 12345-678,
    bairro : Bairro Teste,
    cidade : Cidade Teste,
    estado : SP,
    pais : BR
  },
  telefone : [
  {
    tipoTelefone : 1,
    ddi : 55,
    ddd : 12,
    telefone : 1234-5678
  }
  ]
   }
}
```

RESPOSTA

Estrtura de retorno Bradesco:

> Retorno:

```xml

<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <autorizacao></autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>19</codigoFormaPagamento>
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <dataAprovacaoOperadora></dataAprovacaOperadora>
            <mensagemVenda></mensagemVenda>
            <numeroComprovanteVenda></numeroComprovanteVenda>
            <numeroTransacao>1</numeroTransacao>
            <parcelas>1</parcelas>
            <statusTransacao>8</statusTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <urlPagamento>https://homolog.meiosdepagamentobradesco.com.br/api/Bradesco?token=WGl1NURoSUJvc3NPZ1RPZ0Iwb3B3MWtvaU1FZXZ5MDFHZ0M1cy8zdUpTQkR5eFhtRHQ2VmR0Rmg5em9sbUtNTg..</urlpagamento>
            <valor>100</valor>
            <valorDesconto>0</valorDesconto>
         </return>
      </ns2:pagamentoTransacaoCompletaResponse>
   </soap:body>
</soap:envelope>

```

> Retorno:

```curl

--header "Content-Type: application/json"
{
   "numeroTransacao": 1,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 18,
   "valor": 2000,
   "valorDesconto": 0,
   "parcelas": 1,
   "statusTransacao": 8,
   "autorizacao": ,
   "codigoTransacaoOperadora": "0",
   "dataAprovacaoOperadora": ,
   "numeroComprovanteVenda": ,
   "nsu": ,
   "mensagemVenda": ,
   "urlPagamento": "https://homologacao.superpay.com.br/checkout/Transferencia/PagamentoBradescoShopFacil.do?cod=141348960683a720e602-5631-4725-8f79-268c06795a3c"
}

```

## Banco do Brasil Online

**Contratação**

Contratando a solução BBOnline será possível oferecer na sua loja:

* Boleto *sem* ou *com* registro, dependendo de seu contrato com o banco;
* Transferência Eletrônica.


Ao final do processo de contratação, deve-se estar de posse das seguintes informações para ativação do e-Rede no Gateway:

* Códido do convênio;
* Código cobrança.


O Superpay não participa das negociações entre o estabelecimento e bancos/adquirentes. Desta forma, taxas ou eventuais isenções são tratadas de forma direta entre os envolvidos.

Informações sobre a contratação, [acesse aqui](http://www.bb.com.br/portalbb/page100,116,2532,1,1,1,1.bb?codigoNoticia=5575&codigoMenu=164&codigoRet=5435&bread=6_2)


**Particulariedades**

* Modalidades com redirecionamento;
* Para utilização das modalidades BBOnline, todos os campos referente aos dados do cliente devem ser preenchidos;
* Se não for informado uma data de vencimento do boleto, a data de vencimento que aparecerá no boleto será os dias de vencimento configurado internamente no Gateway;



**Exemplos**

REQUISIÇÃO

Estrutura de envio para banco do Brasil. 

> Estrutra SOAP de envio BBOnline Boleto:

```xml

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
   <soapenv:Header/>
   <soapenv:Body>
      <pag:pagamentoTransacaoCompleta>
         <transacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>20</codigoFormaPagamento>
            <dadosUsuarioTransacao>
               <bairroEnderecoComprador>Centro</bairroEnderecoComprador>
               <bairroEnderecoEntrega>Centro</bairroEnderecoEntrega>
               <cepEnderecoComprador>09710240</cepEnderecoComprador>
               <cepEnderecoEntrega>09710240</cepEnderecoEntrega>
               <cidadeEnderecoComprador>São Paulo</cidadeEnderecoComprador>
               <cidadeEnderecoEntrega>São Paulo</cidadeEnderecoEntrega>
               <codigoCliente></codigoCliente>
               <codigoTipoTelefoneComprador>1</codigoTipoTelefoneComprador>
               <codigoTipoTelefoneEntrega>1</codigoTipoTelefoneEntrega>
               <complementoEnderecoComprador></complementoEnderecoComprador>
               <complementoEnderecoEntrega></complementoEnderecoEntrega>
               <dataNascimentoComprador>01/01/1990</dataNascimentoComprador>
               <dddComprador>12</dddComprador>
               <dddEntrega>12</dddEntrega>
               <ddiComprador>55</ddiComprador>
               <ddiEntrega>55</ddiEntrega>
               <documentoComprador>12345678900</documentoComprador>
               <emailComprador>teste@teste.com.br</emailComprador>
               <enderecoComprador>Rua da Casa</enderecoComprador>
               <enderecoEntrega>Rua da Casa</enderecoEntrega>
               <estadoEnderecoComprador>sp</estadoEnderecoComprador>
               <estadoEnderecoEntrega>sp</estadoEnderecoEntrega>
               <nomeComprador>Nome do comprador</nomeComprador>
               <numeroEnderecoComprador>123</numeroEnderecoComprador>
               <numeroEnderecoEntrega>123</numeroEnderecoEntrega>
               <paisComprador>brasil</paisComprador>
               <paisEntrega>brasil</paisEntrega>
               <sexoComprador>m</sexoComprador>
               <telefoneComprador>36535915</telefoneComprador>
               <telefoneEntrega>36535915</telefoneEntrega>
               <tipoCliente>1</tipoCliente>
            </dadosUsuarioTransacao>
            <IP>10.10.0.1</IP>
            <idioma>1</idioma>
            <itensDoPedido>
               <codigoCategoria>1</codigoCategoria>
               <codigoProduto>1</codigoProduto>
               <nomeCategoria>Produto Teste</nomeCategoria>
               <nomeProduto>Produto Teste</nomeProduto>
               <quantidadeProduto>1</quantidadeProduto>
               <valorUnitarioProduto>100</valorUnitarioProduto>
            </itensDoPedido>
            <numeroTransacao>1</numeroTransacao>
            <origemTransacao></origemTransacao>
            <parcelas>1</parcelas>
            <taxaEmbarque>0</taxaEmbarque>
            <urlCampainha>http://minhaloja.com.br/notificação.asp</urlCampainha>
            <urlRedirecionamentoNaoPago>http://www.pagamentonaopago.com.br</urlRedirecionamentoNaoPago>
            <urlRedirecionamentoPago>http://www.pagamentopago.com.br</urlRedirecionamentoPago>
            <valor>100</valor>
            <valorDesconto>0</valorDesconto>
            <vencimentoBoleto>10/10/2018</vencimentoBoleto>
         </transacao>
         <usuario>superpay</usuario>
         <senha>superpay</senha>
      </pag:pagamentoTransacaoCompleta>
   </soapenv:Body>
</soapenv:Envelope>

```
> Estrutra  REST de envio BBOnline Transferênia:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary
{
   codigoEstabelecimento: 1000000000000,
   codigoFormaPagamento: 21,
   transacao: {
    numeroTransacao: 1,
    valor: 2000,
    valorDesconto: 0,
    parcelas : 1,
    urlCampainha : http://seusite.com.br/campainha,
    urlResultado : http://seusite.com.br/retorno,
    ip : "192.168.12.110",
    idioma : 1
   },
   itensDoPedido: [
  {
    codigoProduto: 1,
    nomeProduto: Blusa,
    codigoCategoria: 1,
    nomeCategoria : Roupa,
    quantidadeProduto : 1,
    valorUnitarioProduto : 2000
  }
   ],
   dadosCobranca : {
    codigoCliente : 1,
    tipoCliente : 1,
    nome : Teste SuperPay,
    email : teste@teste.com,
    dataNascimento : "",
    sexo : "M",
    documento : "12312321312",
    endereco : {
    logradouro : Rua Teste,
    numero : 123,
    complemento : "",
    cep : 12345-678,
    bairro : Bairro Teste,
    cidade : Cidade Teste,
    estado : SP,
    pais : BR
  },
  telefone : [
  {
    tipoTelefone : 1,
    ddi : 55,
    ddd : 12,
    telefone : 1234-5678
  }
  ]
   },
   dadosEntrega : {
    nome : Teste SuperPay,
    email : teste@teste.com,
    endereco : {
    logradouro : Rua teste,
    numero : 123,
    complemento : "",
    cep : 12345-678,
    bairro : Bairro Teste,
    cidade : Cidade Teste,
    estado : SP,
    pais : BR
  },
  telefone : [
  {
    tipoTelefone : 1,
    ddi : 55,
    ddd : 12,
    telefone : 1234-5678
  }
  ]
   }
}
```

RESPOSTA

Estrtura de retorno BBOnline:

> Retorno:

```xml

<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <autorizacao></autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>20</codigoFormaPagamento>
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <dataAprovacaoOperadora></dataAprovacaOperadora>
            <mensagemVenda></mensagemVenda>
            <numeroComprovanteVenda></numeroComprovanteVenda>
            <numeroTransacao>1</numeroTransacao>
            <parcelas>1</parcelas>
            <statusTransacao>8</statusTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <urlPagamento>https://homologacao.superpay.com.br/checkout/PagamentoBBOnline/PagamentoBBOnline.do?cod=147499950329455715d65-621f-4b80-896c-5d1a645fb9e0</urlpagamento>
            <valor>100</valor>
            <valorDesconto>0</valorDesconto>
         </return>
      </ns2:pagamentoTransacaoCompletaResponse>
   </soap:body>
</soap:envelope>

```

> Retorno:

```curl

--header "Content-Type: application/json"
{
   "numeroTransacao": 1,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 21,
   "valor": 2000,
   "valorDesconto": 0,
   "parcelas": 1,
   "statusTransacao": 8,
   "autorizacao": ,
   "codigoTransacaoOperadora": "0",
   "dataAprovacaoOperadora": ,
   "numeroComprovanteVenda": ,
   "nsu": ,
   "mensagemVenda": ,
   "urlPagamento": "https://homologacao.superpay.com.br/checkout/PagamentoBBOnline/PagamentoBBOnline.do?cod=147499950329455715d65-621f-4b80-896c-5d1a645fb9e0"
}

```

## Boletos Offlines

**BOLETOS REGISTRADOS**

Com esta modalidade, os bancos possuem conehcimento do boleto desde sua geração, permitindo o lojista realizar protestos ao cliente caso o pagamento não for realizado.

*Para utilização de qualquer boleto com carteira registrada é preciso realizar a abertura de relacionamento entre o banco e VAN homologada com o SuperPay.*

**Santander**

* agência;
* conta;
* código do cedente;
* número da carteira;
* espécie de documento;  (Exemplos: DM, RM, ME, NP)
* código de transmissão.


**Caixa Econômica Federal**

* agência;
* conta;
* código do convênio;
* número da carteira;
* código de operação;
* espécie de documento.  (Exemplos: DM, RM, ME, NP)


**Itaú**

* agência;
* conta;
* número da carteira;
* espécie de documento.  (Exemplos: DM, RM, ME, NP)


**Bradesco**

* agência;
* conta;
* número da carteira;
* espécie de documento.  (Exemplos: DM, RM, ME, NP)


**Banco do Brasil**

* agência;
* conta;
* código do convênio;
* número da carteira;
* variação da carteira;
* espécie de documento.  (Exemplos: DM, RM, ME, NP);
* client ID;
* secret;


**Particulariedades**

* Para a geração de Boleto offlines, todos os campos referente aos dados do cliente devem ser preenchidos;
* Arquivo de registro e conciliação com layout de 400 posições para todos os bancos;
* Campo `<estadoComprador>` deve ser preenchido pela sigla;
* O tamanho do número do pedido, deverá possuir no máximo 8 dígitos;
* Se não for informado uma data de vencimento do boleto na requisição, a data de vencimento que aparecerá no boleto será os dias de vencimento configurados internamente no Gateway;
* A URL retornada no campo `<urlPagamento>` em SOAP e `<url_acesso>` em REST deverá ser repassada ao comprador para visualização/pagamento do boleto;
* A requisição e retorno do SuperPay para boletos registrados possuem a mesma estrutura dos sem registro, porém o status a ser retornado será 5 (transação em andamento) ao invés de 8 (aguardando pagamento);
* Para ativação do boleto registrado e Módulo de Conciliação entrar em contato com *comercial@superpay.com.br*;
* O Superpay não participa das negociações entre o estabelecimento e bancos/administradoras. Desta forma, taxas ou eventuais isenções são tratadas de forma direta entre os envolvidos.


**BOLETOS SEM REGISTROS**

O Gateway de Pagamento aceita boletos sem registro emitidos pelos seguintes bancos:

* Itaú;
* Bradesco;
* Banco do Brasil;
* Caixa;
* Santander.

Ao final do processo de contratação, deve-se estar de posse das seguintes informações para ativação dos boletos no Gateway:

* Convênio;
* Agência;
* Conta;
* Número da Carteira;
* Espécie de Documento. (Exemplos: DM, RM, ME, NP)


**Particulariedades**

* Para a geração de Boleto offlines, todos os campos referente aos dados do cliente devem ser preenchidos.
* Campo `<estadoComprador>` deve ser preenchido pela sigla;
* O tamanho do número do pedido, deverá possuir no máximo 8 dígitos;
* Se não for informado uma data de vencimento do boleto na requisição, a data de vencimento que aparecerá no boleto será os dias de vencimento configurados internamente no Gateway;
* A URL retornada no campo `<urlPagamento>` em SOAP e `<url_acesso>` em REST deverá ser repassada ao comprador para visualização/pagamento do boleto;
* Conciliação de boletos não é realizada automaticamente, para tal deve ser contratado o Módulo de Conciliação do Gateway. Para informações entrar em contato com *comercial@superpay.com.br*;
* O Superpay não participa das negociações entre o estabelecimento e bancos/administradoras. Desta forma, taxas ou eventuais isenções são tratadas de forma direta entre os envolvidos.


**Exemplos**

REQUISIÇÃO

Estrutura de envio para geração de boletos sem/com registro. 

> Estrutra SOAP de envio Boleto Registrado:

```xml

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
   <soapenv:Header/>
   <soapenv:Body>
      <pag:pagamentoTransacaoCompleta>
         <transacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>29</codigoFormaPagamento>
            <dadosUsuarioTransacao>
               <bairroEnderecoComprador>Centro</bairroEnderecoComprador>
               <bairroEnderecoEntrega>Centro</bairroEnderecoEntrega>
               <cepEnderecoComprador>09710240</cepEnderecoComprador>
               <cepEnderecoEntrega>09710240</cepEnderecoEntrega>
               <cidadeEnderecoComprador>São Paulo</cidadeEnderecoComprador>
               <cidadeEnderecoEntrega>São Paulo</cidadeEnderecoEntrega>
               <codigoCliente></codigoCliente>
               <codigoTipoTelefoneComprador>1</codigoTipoTelefoneComprador>
               <codigoTipoTelefoneEntrega>1</codigoTipoTelefoneEntrega>
               <complementoEnderecoComprador></complementoEnderecoComprador>
               <complementoEnderecoEntrega></complementoEnderecoEntrega>
               <dataNascimentoComprador>01/01/1990</dataNascimentoComprador>
               <dddComprador>12</dddComprador>
               <dddEntrega>12</dddEntrega>
               <ddiComprador>55</ddiComprador>
               <ddiEntrega>55</ddiEntrega>
               <documentoComprador>12345678900</documentoComprador>
               <emailComprador>teste@teste.com.br</emailComprador>
               <enderecoComprador>Rua da Casa</enderecoComprador>
               <enderecoEntrega>Rua da Casa</enderecoEntrega>
               <estadoEnderecoComprador>sp</estadoEnderecoComprador>
               <estadoEnderecoEntrega>sp</estadoEnderecoEntrega>
               <nomeComprador>Nome do comprador</nomeComprador>
               <numeroEnderecoComprador>123</numeroEnderecoComprador>
               <numeroEnderecoEntrega>123</numeroEnderecoEntrega>
               <paisComprador>brasil</paisComprador>
               <paisEntrega>brasil</paisEntrega>
               <sexoComprador>m</sexoComprador>
               <telefoneComprador>36535915</telefoneComprador>
               <telefoneEntrega>36535915</telefoneEntrega>
               <tipoCliente>1</tipoCliente>
            </dadosUsuarioTransacao>
            <IP>10.10.0.1</IP>
            <idioma>1</idioma>
            <itensDoPedido>
               <codigoCategoria>1</codigoCategoria>
               <codigoProduto>1</codigoProduto>
               <nomeCategoria>Produto Teste</nomeCategoria>
               <nomeProduto>Produto Teste</nomeProduto>
               <quantidadeProduto>1</quantidadeProduto>
               <valorUnitarioProduto>100</valorUnitarioProduto>
            </itensDoPedido>
            <numeroTransacao>1</numeroTransacao>
            <origemTransacao></origemTransacao>
            <parcelas>1</parcelas>
            <taxaEmbarque>0</taxaEmbarque>
            <urlCampainha>http://minhaloja.com.br/notificação.asp</urlCampainha>
            <urlRedirecionamentoNaoPago>http://www.pagamentonaopago.com.br</urlRedirecionamentoNaoPago>
            <urlRedirecionamentoPago>http://www.pagamentopago.com.br</urlRedirecionamentoPago>
            <valor>100</valor>
            <valorDesconto>0</valorDesconto>
            <vencimentoBoleto>10/10/2018</vencimentoBoleto>
         </transacao>
         <usuario>superpay</usuario>
         <senha>superpay</senha>
      </pag:pagamentoTransacaoCompleta>
   </soapenv:Body>
</soapenv:Envelope>

```
> Estrutra  REST de envio Boleto sem registro:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary
{
   codigoEstabelecimento: 1000000000000,
   codigoFormaPagamento: 30,
   transacao: {
    numeroTransacao: 1,
    valor: 2000,
    valorDesconto: 0,
    parcelas : 1,
    urlCampainha : http://seusite.com.br/campainha,
    urlResultado : http://seusite.com.br/retorno,
    ip : "192.168.12.110",
    idioma : 1
   },
   itensDoPedido: [
  {
    codigoProduto: 1,
    nomeProduto: Blusa,
    codigoCategoria: 1,
    nomeCategoria : Roupa,
    quantidadeProduto : 1,
    valorUnitarioProduto : 2000
  }
   ],
   dadosCobranca : {
    codigoCliente : 1,
    tipoCliente : 1,
    nome : Teste SuperPay,
    email : teste@teste.com,
    dataNascimento : "",
    sexo : "M",
    documento : "12312321312",
    endereco : {
    logradouro : Rua Teste,
    numero : 123,
    complemento : "",
    cep : 12345-678,
    bairro : Bairro Teste,
    cidade : Cidade Teste,
    estado : SP,
    pais : BR
  },
  telefone : [
  {
    tipoTelefone : 1,
    ddi : 55,
    ddd : 12,
    telefone : 1234-5678
  }
  ]
   },
   dadosEntrega : {
    nome : Teste SuperPay,
    email : teste@teste.com,
    endereco : {
    logradouro : Rua teste,
    numero : 123,
    complemento : "",
    cep : 12345-678,
    bairro : Bairro Teste,
    cidade : Cidade Teste,
    estado : SP,
    pais : BR
  },
  telefone : [
  {
    tipoTelefone : 1,
    ddi : 55,
    ddd : 12,
    telefone : 1234-5678
  }
  ]
   }
}
```

RESPOSTA

Estrtura de retorno Boleto registrado:

> Retorno:

```xml

<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <autorizacao></autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>29</codigoFormaPagamento>
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <dataAprovacaoOperadora></dataAprovacaOperadora>
            <mensagemVenda></mensagemVenda>
            <numeroComprovanteVenda></numeroComprovanteVenda>
            <numeroTransacao>1</numeroTransacao>
            <parcelas>1</parcelas>
            <statusTransacao>5</statusTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <urlPagamento>https://homologacao.superpay.com.br/superpay/GeradorBoleto.do?cod=1413487983447baddcb56-0126-4353-9253-538f64d8c1d2</urlpagamento>
            <valor>100</valor>
            <valorDesconto>0</valorDesconto>
         </return>
      </ns2:pagamentoTransacaoCompletaResponse>
   </soap:body>
</soap:envelope>

```

> Retorno Boleto sem registro:

```curl

--header "Content-Type: application/json"
{
   "numeroTransacao": 1,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 30,
   "valor": 2000,
   "valorDesconto": 0,
   "parcelas": 1,
   "statusTransacao": 8,
   "autorizacao": ,
   "codigoTransacaoOperadora": "0",
   "dataAprovacaoOperadora": ,
   "numeroComprovanteVenda": ,
   "nsu": ,
   "mensagemVenda": ,
   "urlPagamento": "https://homologacao.superpay.com.br/superpay/GeradorBoleto.do?cod=1413487983447baddcb56-0126-4353-9253-538f64d"
}

```

##SafetyPay

Com este meio de pagamento é possível trabalhar com transferências entre vários bancos sem possuir contrato com cada um deles.


**Contratação**

Contratando a solução da SafetyPay para e-commerce será possível oferecer na sua loja:

* Transferência entre bancos;


Ao final do processo de contratação, deve-se estar de posse das seguintes informações para ativação dos boletos no Gateway:

* API Key;
* Signature Key;
* Usuário;
* Senha.

O Superpay não participa das negociações entre o estabelecimento e bancos/adquirentes. Desta forma, taxas ou eventuais isenções são tratadas de forma direta entre os envolvidos.

Para contratar, [acesse aqui.](http://www.safetypay.com/pt/produtos-e-solucoes/pagamento-online/)


**Particulariedades**

* Modalidade com redirecionamento;
* A URL retornada no campo `<urlPagamento>` em SOAP e `<url_acesso>` em REST deverá ser repassada ao comprador para visualização/pagamento do boleto;
* Enviar todos campos referente aos dados do comprador.


**Configuração ambiente SafetyPay**

Configurar em seu painel safetyPay a URL de notificação do Gateway + seu código de estabelecimento SuperPay:

Ambiente | URL notificação
-------- | ---------------
Homologação |  https://homologacao.superpay.com.br/checkout/SafetyPay/Notificacao?codigoEstabelecimento={CODIGOSUPERPAY}
Produção |  https://superpay2.superpay.com.br/checkout/SafetyPay/Notificacao?codigoEstabelecimento={CODIGOSUPERPAY}

Exemplo URL notificação:  https://superpay2.superpay.com.br/checkout/SafetyPay/Notificacao?codigoEstabelecimento=1000000000000


**Exemplos**

REQUISIÇÃO

Estrutura de envio SafetyPay. 

> Estrutra SOAP de envio SafetyPay:

```xml

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
   <soapenv:Header/>
   <soapenv:Body>
      <pag:pagamentoTransacaoCompleta>
         <transacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>155</codigoFormaPagamento>
            <dadosUsuarioTransacao>
               <bairroEnderecoComprador>Centro</bairroEnderecoComprador>
               <bairroEnderecoEntrega>Centro</bairroEnderecoEntrega>
               <cepEnderecoComprador>09710240</cepEnderecoComprador>
               <cepEnderecoEntrega>09710240</cepEnderecoEntrega>
               <cidadeEnderecoComprador>São Paulo</cidadeEnderecoComprador>
               <cidadeEnderecoEntrega>São Paulo</cidadeEnderecoEntrega>
               <codigoCliente></codigoCliente>
               <codigoTipoTelefoneComprador>1</codigoTipoTelefoneComprador>
               <codigoTipoTelefoneEntrega>1</codigoTipoTelefoneEntrega>
               <complementoEnderecoComprador></complementoEnderecoComprador>
               <complementoEnderecoEntrega></complementoEnderecoEntrega>
               <dataNascimentoComprador>01/01/1990</dataNascimentoComprador>
               <dddComprador>12</dddComprador>
               <dddEntrega>12</dddEntrega>
               <ddiComprador>55</ddiComprador>
               <ddiEntrega>55</ddiEntrega>
               <documentoComprador>12345678900</documentoComprador>
               <emailComprador>teste@teste.com.br</emailComprador>
               <enderecoComprador>Rua da Casa</enderecoComprador>
               <enderecoEntrega>Rua da Casa</enderecoEntrega>
               <estadoEnderecoComprador>sp</estadoEnderecoComprador>
               <estadoEnderecoEntrega>sp</estadoEnderecoEntrega>
               <nomeComprador>Nome do comprador</nomeComprador>
               <numeroEnderecoComprador>123</numeroEnderecoComprador>
               <numeroEnderecoEntrega>123</numeroEnderecoEntrega>
               <paisComprador>brasil</paisComprador>
               <paisEntrega>brasil</paisEntrega>
               <sexoComprador>m</sexoComprador>
               <telefoneComprador>36535915</telefoneComprador>
               <telefoneEntrega>36535915</telefoneEntrega>
               <tipoCliente>1</tipoCliente>
            </dadosUsuarioTransacao>
            <IP>10.10.0.1</IP>
            <idioma>1</idioma>
            <itensDoPedido>
               <codigoCategoria>1</codigoCategoria>
               <codigoProduto>1</codigoProduto>
               <nomeCategoria>Produto Teste</nomeCategoria>
               <nomeProduto>Produto Teste</nomeProduto>
               <quantidadeProduto>1</quantidadeProduto>
               <valorUnitarioProduto>100</valorUnitarioProduto>
            </itensDoPedido>
            <numeroTransacao>1</numeroTransacao>
            <origemTransacao></origemTransacao>
            <parcelas>1</parcelas>
            <taxaEmbarque>0</taxaEmbarque>
            <urlCampainha>http://minhaloja.com.br/notificação.asp</urlCampainha>
            <urlRedirecionamentoNaoPago>http://www.pagamentonaopago.com.br</urlRedirecionamentoNaoPago>
            <urlRedirecionamentoPago>http://www.pagamentopago.com.br</urlRedirecionamentoPago>
            <valor>100</valor>
            <valorDesconto>0</valorDesconto>
         </transacao>
         <usuario>superpay</usuario>
         <senha>superpay</senha>
      </pag:pagamentoTransacaoCompleta>
   </soapenv:Body>
</soapenv:Envelope>

```
> Estrutra  REST de envio SafetyPay:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary
{
   codigoEstabelecimento: 1000000000000,
   codigoFormaPagamento: 155,
   transacao: {
    numeroTransacao: 1,
    valor: 2000,
    valorDesconto: 0,
    parcelas : 1,
    urlCampainha : http://seusite.com.br/campainha,
    urlResultado : http://seusite.com.br/retorno,
    ip : "192.168.12.110",
    idioma : 1
   },
   itensDoPedido: [
  {
    codigoProduto: 1,
    nomeProduto: Blusa,
    codigoCategoria: 1,
    nomeCategoria : Roupa,
    quantidadeProduto : 1,
    valorUnitarioProduto : 2000
  }
   ],
   dadosCobranca : {
    codigoCliente : 1,
    tipoCliente : 1,
    nome : Teste SuperPay,
    email : teste@teste.com,
    dataNascimento : "",
    sexo : "M",
    documento : "12312321312",
    endereco : {
    logradouro : Rua Teste,
    numero : 123,
    complemento : "",
    cep : 12345-678,
    bairro : Bairro Teste,
    cidade : Cidade Teste,
    estado : SP,
    pais : BR
  },
  telefone : [
  {
    tipoTelefone : 1,
    ddi : 55,
    ddd : 12,
    telefone : 1234-5678
  }
  ]
   },
   dadosEntrega : {
    nome : Teste SuperPay,
    email : teste@teste.com,
    endereco : {
    logradouro : Rua teste,
    numero : 123,
    complemento : "",
    cep : 12345-678,
    bairro : Bairro Teste,
    cidade : Cidade Teste,
    estado : SP,
    pais : BR
  },
  telefone : [
  {
    tipoTelefone : 1,
    ddi : 55,
    ddd : 12,
    telefone : 1234-5678
  }
  ]
   }
}
```

RESPOSTA

Estrtura de retorno SafetyPay:

> Retorno:

```xml

<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <autorizacao></autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>155</codigoFormaPagamento>
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <dataAprovacaoOperadora></dataAprovacaOperadora>
            <mensagemVenda></mensagemVenda>
            <numeroComprovanteVenda></numeroComprovanteVenda>
            <numeroTransacao>1</numeroTransacao>
            <parcelas>1</parcelas>
            <statusTransacao>8</statusTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <urlPagamento>https://homologacao.superpay.com.br/checkout/PagamentoSafetyPay/PagamentoSafetyPay.do?cod=141348960683a720e602-5631-4725-8f79-268c06795a3c</urlpagamento>
            <valor>100</valor>
            <valorDesconto>0</valorDesconto>
         </return>
      </ns2:pagamentoTransacaoCompletaResponse>
   </soap:body>
</soap:envelope>

```

> Retorno SafetyPay:

```curl

--header "Content-Type: application/json"
{
   "numeroTransacao": 1,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 155,
   "valor": 2000,
   "valorDesconto": 0,
   "parcelas": 1,
   "statusTransacao": 8,
   "autorizacao": ,
   "codigoTransacaoOperadora": "0",
   "dataAprovacaoOperadora": ,
   "numeroComprovanteVenda": ,
   "nsu": ,
   "mensagemVenda": ,
   "urlPagamento": "https://homologacao.superpay.com.br/checkout/PagamentoSafetyPay/PagamentoSafetyPay.do?cod=141348960683a720e602-5631-4725-8f79-268c06795a3c"
}

```

##Intermediário Financeiro

**Contratação**

Abaixo segue lista de intermediário financeiro disponíveis através do Superpay:

* PagSeguro;
* PayPal.


Ao final do processo de contratação, deve-se estar de posse das seguintes informações para ativação no Gateway:


**PagSeguro**

* email
* token

Contração [acesse aqui](https://pagseguro.uol.com.br/para_seu_negocio/venda_pela_internet.jhtml)


**PayPal**

* assinatura
* email
* usuário
* senha

Contração [acesse aqui](https://www.paypal.com/br/home)


O Superpay não participa das negociações entre o estabelecimento e bancos/administradoras. Desta forma, taxas ou eventuais isenções são tratadas de forma direta entre os envolvidos.


**Particulariedades**

* Modalidade com redirecionamento;
* A URL retornada no campo `<urlPagamento>` em SOAP e `<url_acesso>` em REST deverá ser repassada ao comprador para abertura do intermediador;
* Enviar todos campos referente aos dados do comprador.


**Configuração ambiente PagSeguro**

Configurar em seu painel PagSeguro a URL de notificação do Gateway:


Ambiente | URL notificação 
-------- | ---------------
Homologação |  https://homologacao.superpay.com.br/checkout/PagamentoPagSeguro/RetornoPagSeguro.do
Produção |  https://superpay2.superpay.com.br/checkout/PagamentoPagSeguro/RetornoPagSeguro.do


**Exemplos**

REQUISIÇÃO

Estrutura de envio PayPal. 

> Estrutra SOAP de envio PayPal:

```xml

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
   <soapenv:Header/>
   <soapenv:Body>
      <pag:pagamentoTransacaoCompleta>
         <transacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>111</codigoFormaPagamento>
            <dadosUsuarioTransacao>
               <bairroEnderecoComprador>Centro</bairroEnderecoComprador>
               <bairroEnderecoEntrega>Centro</bairroEnderecoEntrega>
               <cepEnderecoComprador>09710240</cepEnderecoComprador>
               <cepEnderecoEntrega>09710240</cepEnderecoEntrega>
               <cidadeEnderecoComprador>São Paulo</cidadeEnderecoComprador>
               <cidadeEnderecoEntrega>São Paulo</cidadeEnderecoEntrega>
               <codigoCliente></codigoCliente>
               <codigoTipoTelefoneComprador>1</codigoTipoTelefoneComprador>
               <codigoTipoTelefoneEntrega>1</codigoTipoTelefoneEntrega>
               <complementoEnderecoComprador></complementoEnderecoComprador>
               <complementoEnderecoEntrega></complementoEnderecoEntrega>
               <dataNascimentoComprador>01/01/1990</dataNascimentoComprador>
               <dddComprador>12</dddComprador>
               <dddEntrega>12</dddEntrega>
               <ddiComprador>55</ddiComprador>
               <ddiEntrega>55</ddiEntrega>
               <documentoComprador>12345678900</documentoComprador>
               <emailComprador>teste@teste.com.br</emailComprador>
               <enderecoComprador>Rua da Casa</enderecoComprador>
               <enderecoEntrega>Rua da Casa</enderecoEntrega>
               <estadoEnderecoComprador>sp</estadoEnderecoComprador>
               <estadoEnderecoEntrega>sp</estadoEnderecoEntrega>
               <nomeComprador>Nome do comprador</nomeComprador>
               <numeroEnderecoComprador>123</numeroEnderecoComprador>
               <numeroEnderecoEntrega>123</numeroEnderecoEntrega>
               <paisComprador>brasil</paisComprador>
               <paisEntrega>brasil</paisEntrega>
               <sexoComprador>m</sexoComprador>
               <telefoneComprador>36535915</telefoneComprador>
               <telefoneEntrega>36535915</telefoneEntrega>
               <tipoCliente>1</tipoCliente>
            </dadosUsuarioTransacao>
            <IP>10.10.0.1</IP>
            <idioma>1</idioma>
            <itensDoPedido>
               <codigoCategoria>1</codigoCategoria>
               <codigoProduto>1</codigoProduto>
               <nomeCategoria>Produto Teste</nomeCategoria>
               <nomeProduto>Produto Teste</nomeProduto>
               <quantidadeProduto>1</quantidadeProduto>
               <valorUnitarioProduto>100</valorUnitarioProduto>
            </itensDoPedido>
            <numeroTransacao>1</numeroTransacao>
            <origemTransacao></origemTransacao>
            <parcelas>1</parcelas>
            <taxaEmbarque>0</taxaEmbarque>
            <urlCampainha>http://minhaloja.com.br/notificação.asp</urlCampainha>
            <urlRedirecionamentoNaoPago>http://www.pagamentonaopago.com.br</urlRedirecionamentoNaoPago>
            <urlRedirecionamentoPago>http://www.pagamentopago.com.br</urlRedirecionamentoPago>
            <valor>100</valor>
            <valorDesconto>0</valorDesconto>
         </transacao>
         <usuario>superpay</usuario>
         <senha>superpay</senha>
      </pag:pagamentoTransacaoCompleta>
   </soapenv:Body>
</soapenv:Envelope>

```
> Estrutra  REST de envio PagSeguro:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary
{
   codigoEstabelecimento: 1000000000000,
   codigoFormaPagamento: 39,
   transacao: {
    numeroTransacao: 1,
    valor: 2000,
    valorDesconto: 0,
    parcelas : 1,
    urlCampainha : http://seusite.com.br/campainha,
    urlResultado : http://seusite.com.br/retorno,
    ip : "192.168.12.110",
    idioma : 1
   },
   itensDoPedido: [
  {
    codigoProduto: 1,
    nomeProduto: Blusa,
    codigoCategoria: 1,
    nomeCategoria : Roupa,
    quantidadeProduto : 1,
    valorUnitarioProduto : 2000
  }
   ],
   dadosCobranca : {
    codigoCliente : 1,
    tipoCliente : 1,
    nome : Teste SuperPay,
    email : teste@teste.com,
    dataNascimento : "",
    sexo : "M",
    documento : "12312321312",
    endereco : {
    logradouro : Rua Teste,
    numero : 123,
    complemento : "",
    cep : 12345-678,
    bairro : Bairro Teste,
    cidade : Cidade Teste,
    estado : SP,
    pais : BR
  },
  telefone : [
  {
    tipoTelefone : 1,
    ddi : 55,
    ddd : 12,
    telefone : 1234-5678
  }
  ]
   },
   dadosEntrega : {
    nome : Teste SuperPay,
    email : teste@teste.com,
    endereco : {
    logradouro : Rua teste,
    numero : 123,
    complemento : "",
    cep : 12345-678,
    bairro : Bairro Teste,
    cidade : Cidade Teste,
    estado : SP,
    pais : BR
  },
  telefone : [
  {
    tipoTelefone : 1,
    ddi : 55,
    ddd : 12,
    telefone : 1234-5678
  }
  ]
   }
}
```

RESPOSTA

Estrtura de retorno PayPal:

> Retorno:

```xml

<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <autorizacao></autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>111</codigoFormaPagamento>
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <dataAprovacaoOperadora></dataAprovacaOperadora>
            <mensagemVenda></mensagemVenda>
            <numeroComprovanteVenda></numeroComprovanteVenda>
            <numeroTransacao>1</numeroTransacao>
            <parcelas>1</parcelas>
            <statusTransacao>8</statusTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <urlPagamento>https://homologacao.superpay.com.br/checkout/PagamentoPayPal/PagamentoPayPal.do?cod=141348960683a720e602-5631-4725-8f79-268c06795a3c</urlpagamento>
            <valor>100</valor>
            <valorDesconto>0</valorDesconto>
         </return>
      </ns2:pagamentoTransacaoCompletaResponse>
   </soap:body>
</soap:envelope>

```

> Retorno PagSeguro:

```curl

--header "Content-Type: application/json"
{
   "numeroTransacao": 1,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 39,
   "valor": 2000,
   "valorDesconto": 0,
   "parcelas": 1,
   "statusTransacao": 8,
   "autorizacao": ,
   "codigoTransacaoOperadora": "0",
   "dataAprovacaoOperadora": ,
   "numeroComprovanteVenda": ,
   "nsu": ,
   "mensagemVenda": ,
   "urlPagamento": "https://homologacao.superpay.com.br/checkout/PagamentoPagSeguro/PagamentoPagSeguro.do?cod=1413489786995834a2f60-aa50-4615-92bd-45c46a7397a5"
}

```


# Módulo Magento

[Baixe aqui seu Módulo](https://superpay.acelerato.com/base-de-conhecimento/#/artigos/5)

**Particulariedades**

* Não prestamos Suporte;
* As ações de captura, cancelamento e estorno não estão disponíveis no Módulo Magento. Estas ações deverão ser realizadas diretamente no painel administrativo SuperPay;
* As funcionalidades de Recorrência, Múltiplos Cartões, Retentativa de Pagamento e Switch de Adquirência atualmente não estão disponíveis no Módulo Magento;
* O Módulo Magento SuperPay é compatível com as versões 1.5.X, 1.6.X, 1.7.X, 1.8.X, 1.9.X do Magento;
* As adquirentes disponíveis no Módulo são: <code>Cielo, Rede, Elavon, GetNet e Stone</code>;
* Os bancos disponíveis no Módulo são: <code>Itau, Bradesco, Caixa, HSBC, Banco do Brasil</code>.

**Instalação**

* Para utilização do Módulo é necessário a habilitação do SoapClient no php.ini.

1. Descompacte o conteúdo do arquivo SuperPay_Magento_1.2.x.zip no diretório raiz de instalação do Magento.
2. Atualize o cache do Magento, através do item System/Cache Management do administrador do sistema.
3. O módulo está instalado! 

**Configuração**

1. Após a instalação do Módulo Magento Superpay, acesse a aba System/Configuration e clique em Payment Methods, encontrada na aba Sales.
2. Feito o passo anterior, conseguirá visualizar as abas relacionadas as configurações do SuperPay.
3. Clicar na aba "SuperPay Cartões" ou "SuperPay banco/boletos/redirect" para configuração dos campos abaixo:
   * Título;
   * Ambiente (Homologação ou Produção);
   * Código do estabelecimento SuperPay;
   * Usuário;
   * Senha;
   * Escolher as bandeiras/bancos em "Cartão de credito" / "Banco".



