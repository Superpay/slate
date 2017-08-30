---
title: API SuperPay

language_tabs: # must be one of https://git.io/vQNgJ
  - xml
  - curl

toc_footers:
  - <a href='#'>Documentação API SuperPay</a>


search: true
---
# Bem vindo ao SuperPay
Tudo que você precisa para se integrar ao nosso Gateway.

## Introdução
Gateway de pagamento é uma solução tecnológica, que permite, facilmente, que quaisquer sistemas que possam se comunicar via WebService, realizem cobranças via boleto, cartão e transferências bancárias. Uma vez integrado ao SuperPay, seu sistema estará pronto para disponibilizar diversas formas de pagamento.

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

# Integrações 
## Integração REST

Para maiores informações [acesse aqui](https://superpay.github.io/rest).

## Integração SOAP
Para maiores informações [acesse aqui](https://superpay.github.io/soap).


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

**Exemplos**

REQUISIÇÃO

Estrutura simplificada de envio para adquirente Cielo. Caso seu estabelecimento utilize *antifraude*, seguir a [estrutura completa](https://superpay.github.io/soap/#criando-uma-transacao-com-analise-de-fraude).

> Estrutra simplificada de envio Cielo:

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

RESPOSTA

Estrtura de retorno adquirente Cielo. Os comentários no XML indicam a informação retornada da adquirente em cada campo.

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

> Estrutra simplificada de envio Cielo:

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

RESPOSTA

Estrtura de retorno adquirente Cielo. Os comentários no XML indicam a informação retornada da adquirente em cada campo.

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
