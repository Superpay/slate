---
title: API SuperPay

language_tabs: # must be one of https://git.io/vQNgJ
  - xml

toc_footers:
  - <a href='#'>Documentação API SuperPay</a>


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
* Nome da Loja;
* CNPJ;
* Email para criação do cadastro.

  
## Produção
Para receber suas credenciais do nosso ambiente de produção, basta acessar nosso site e realizar a contratação de um de nossos planos: [Planos SuperPay](http://www.superpay.com.br/planos)
Caso tenha dúvidas sobre, por gentileza entrar em contato com nossa equipe comercial através do email [comercial@superpay.com.br] ou pelo telefone 11 3544-0678

# Autenticação
Para autenticação conosco, é preciso enviar o usuário e senha WS de seu estabelecimento. Caso ainda não o possua, por gentileza enviar solicitação para nossa equipe de Suporte através do email [servicedesk@superpay.com.br].

# EndPoint
## Sandbox
`https://homologacao.superpay.com.br/checkout/servicosPagamentoCompletoWS.Services?wsdl`

## Produção
`https://superpay2.superpay.com.br/checkout/servicosPagamentoCompletoWS.Services?wsdl`

# Pagamentos com cartão de crédito

Abaixo URL dos ambientes:


Endpoint Produção: `https://superpay2.superpay.com.br/checkout/servicosPagamentoCompletoWS.Services?wsdl`

## Criando uma transação simplificada

> Exemplo criação transação:

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

> Exemplo retorno transação:

```xml
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



**REQUISIÇÃO**

<aside class="notice">
Para enviar a transação, acione o método <code>pagamentoTransacaoCompleta</code>
</aside>

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento

*transacaoCompletaWS*

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

*dadosUsuarioTransacao*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
nomeComprador | Nome do comprador| Alfa Numérico | Até 100 caracteres | Não
documentoComprador | Documento principal do comprado| Alfa Numérico | 30 caracteres | Não




**RESPOSTA**


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
autorizacao | Código de autorização da adquirente | Numérico | Até 20 dígitos
codigoTransacaoOperadora | Código da transação na adquirente | Numérico | Até 20 dígitos
dataAprovacaoOperadora | Data de aprovação na adquirente |Alfa Numérico | Até 10 dígitos
numeroComprovanteVenda | Número do comprovante de venda |Alfa Numérico | Até 20 dígitos
mensagemVenda | Mensagem de retorno da adquirente |Alfa Numérico | Até 50 dígitos


## Criando uma transação com Análise de Fraude

> Exemplo criação transação:

```xml
<soapenv:envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
   <soapenv:header></soapenv:header>
   <soapenv:body>
      <pag:pagamentoTransacaoCompleta>
         <transacao>
            <campoLivre1></campoLivre1>
            <campoLivre2></campoLivre2>
            <campoLivre3></campoLivre3>
            <campoLivre4></campoLivre4>
            <campoLivre5></campoLivre5>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>170</codigoFormaPagamento>
            <codigoSeguranca>123</codigoSeguranca>
            <dadosUsuarioTransacao>
               <bairroEnderecoComprador>Vila</bairroEnderecoComprador>
               <bairroEnderecoEntrega>centro</bairroEnderecoEntrega>
               <cepEnderecoComprador>05707001</cepEnderecoComprador>
               <cepEnderecoEntrega>05707001</cepEnderecoEntrega>
               <cidadeEnderecoComprador>Sao Paulo</cidadeEnderecoComprador>
               <cidadeEnderecoEntrega>Sao Paulo</cidadeEnderecoEntrega>
               <codigoCliente>1</codigoCliente>
               <codigoTipoTelefoneComprador>1</codigoTipoTelefoneComprador>
               <codigoTipoTelefoneEntrega>1</codigoTipoTelefoneEntrega>
               <complementoEndereCocomprador></complementoEnderecoComprador>
               <complementoEnderecoEntrega></complementoEnderecoEntrega>
               <dataNascimentoComprador>10/01/1980</dataNascimentoComprador>
               <dddComprador>11</dddComprador>
               <dddEntrega>11</dddEntrega>
               <ddiComprador>55</ddiComprador>
               <ddiEntrega>55</ddiEntrega>
               <documento2Comprador></documento2Comprador>
               <documentoComprador>12345678919</documentoComprador>
               <emailComprador>superpay@superpay.com.br</emailComprador>
               <enderecoComprador>Rua do Comprador</enderecoComprador>
               <enderecoEntrega>Rua do Comprador</enderecoEntrega>
               <estadoEnderecoComprador>SP</estadoEnderecoComprador>
               <estadoEnderecoEntrega>SP</estadoEnderecoEntrega>
               <nomeComprador>Testes de integracao Cartão</nomeComprador>
               <numeroEnderecoComprador>123</numeroEnderecoComprador>
               <numeroEnderecoEntrega>123</numeroEnderecoEntrega>
               <paisComprador>BR</paisComprador>
               <paisEntrega>BR</paisEntrega>
               <sexoComprador>M</sexoComprador>
               <telefoneComprador>1234123123</telefoneComprador>
               <telefoneEntrega>1234123123</telefoneEntrega>
               <tipoCliente>1</tipoCliente>
            </dadosUsuarioTransacao>
           <dataDalidadeCartao>10/2017</dataValidadeCartao>
            <IP>10.100.1.12</IP>
            <idioma>1</idioma>
            <itensDoPedido>
               <codigoCategoria>1</codigoCategoria>
               <codigoProduto>1</codigoProduto>
               <nomeCategoria>Roupa</nomeCategoria>
               <nomeProduto>Camiseta</nomeProduto>
               <quantidaDeProduto>1</quantidaDeProduto>
               <valorUnitarioProduto>200</valorUnitarioProduto>
            </itensDoPedido>
            <nomeTitularCartaoCredito>Teste Integracao</nomeTitularCartaoCredito>
            <numeroCartaoCredito>4444333322221111</numeroCartaoCredito>
            <numeroTransacao>1</numeroTransacao>
            <origemTransacao>1</origemTransacao>
            <parcelas>1</parcelas>
            <taxaEmbarque>0</taxaEmbarque>
            <urlCampainha>http://www.sualoja.campainha.com.br</urlCampainha>
            <urlRedirecionamentoNaoPago>http://www.google.com.br</urlRedirecionamentoNaoPago>
            <urlRedirecionamentoPago>http://www.google.com.br</urlRedirecionamentoPago>
            <valor>200</valor>
            <valorDesconto>0</valorDesconto>
         </transacao>
         <usuario>superpay</usuario>
         <senha>superpay</senha>
      </pag:pagamentoTransacaoCompleta>
   </soapenv:body>
</soapenv:envelope>
```

> Exemplo retorno transação:

```xml
<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <autorizacao>123456</autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>170</codigoFormaPagamento>
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <dataAprovacaoOperadora>24/05/2017</dataAprovacaOperadora>
            <mensagemVenda>Transacao autorizada</mensagemVenda>
            <numeroComprovanteVenda>1006993069181F841001</numeroComprovanteVenda>
            <numeroTransacao>1</numeroTransacao>
            <parcelas>1</parcelas>
            <statusTransacao>15</statusTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <urlPagamento>14132971582229c00506d-e84d-4526-b902-92190d5aa808<urlpagamento></urlpagamento>
            <valor>200</valor>
            <valorDesconto>0</valorDesconto>
         </return>
      </ns2:pagamentoTransacaoCompletaResponse>
   </soap:body>
</soap:envelope>
```

A utilização desta estrutura é indicada para envio de pedidos com a forma de pagamento cartão de crédito, onde o estabelecimento possui contratação de umas das empresas de análise de risco integradas pelo SuperPay, sendo elas ClearSale (modalidades: Total/Total Garantido, Application, ID e Start) e FControl (modalidade Fila).

<aside class="warning">Contratação a parte com as empresas ClearSale e FControl</aside>


**REQUISIÇÃO**

<aside class="notice">
Para enviar a transação, acione o método <code>pagamentoTransacaoCompleta</code>
</aside>

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento


*transacaoCompletaWS*

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
ip | Número do IP de origem. Formato xxx.xxx.xxx.xxx | Alfa Numérico | Até 15 caracteres | Não
idioma	| 1 - Português 2 - Inglês 3 - Espanhol | Numérico | 1 dígito | Sim
origemTransacao | 1 - eCommerce 2 - Mobile 3 - URA | Numérico | 1 dígito | Sim
campoLivre1 | Campo Livre 1 |	Alfa Numérico |	Até 16 caracteres |	Não
campoLivre2 |	Campo Livre 2 (Envio do FingerPrint ClearSale) | Alfa Numérico | Até 16 caracteres | Não
campoLivre3 |	Campo Livre 3 (Envio do canal de venda ClearSale Total/Total Garantido e Application - solicitar ativação para Suporte SuperPay)  | Alfa Numérico | Até 16 caracteres | Não
campoLivre4 |	Campo Livre 4 | Alfa Numérico |Até 16 caracteres | Não
campoLivre5 |	Campo Livre 5 |	Alfa Numérico |	Até 16 caracteres| Não
dadosUsuarioTransacao | Array dados do comprador | - | - | -
itensDoPedido | Lista itens do pedido | - | - | -

*dadosUsuarioTransacao*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
codigoCliente |	Código que identifica o cliente no estabelecimento |	Alfa Numérico |	20 caracteres	|Não
tipoCliente |	1 - Pessoa Física 2 - Pessoa Jurídica |	Numérico	| 1 dígito	|Sim
nomeComprador |	Nome do comprador |	Alfa Numérico	| Até 100 caracteres	| Sim
documentoComprador |	Documento principal do comprador |	Alfa Numérico	| 30 caracteres	| Sim
documento2Comprador |	Documento complementar do comprador |	Alfa Numérico |	30 caracteres |	Não
sexoComprador |	M – Masculino / F – Feminino |	Alfa Numérico |	1 caracter |	Não
dataNascimentoComprador |	Data de nascimento do comprador. Formato dd/mm/yyyy |	Alfa Numérico|	10 caracteres	|Sim
telefoneComprador | Telefone do comprador sem espaços ou traços |	Alfa Numérico |	Até 10 caracteres |	Sim
dddComprador |	DDD do telefone do comprador |	Alfa Numérico	| Até 3 caracteres |	Sim
ddiComprador |	DDI do telefone do comprador |	Alfa Numérico |	Até 3 caracteres |	Sim
codigoTipoTelefoneComprador |	1 - Outros 2 - Residencial 3 - Comercial |	Numérico |	1 dígito | Sim
telefoneAdicionalComprador |	Telefone adicional do comprador. Sem espaços ou traços |	Alfa Numérico |	Até 10 caracteres | Não
dddAdicionalComprador |	DDD do telefone adicional do comprador |	Alfa Numérico |	Até 3 caracteres |	Não
ddiAdicionalComprador |	DDI do telefone adicional do comprador | Alfa Numérico | Até 3 caracteres |	Não
codigoTipoTelefoneAdicionalComprador |	1 - Outros 2 - Residencial 3 - Comercial |	Numérico	|1 dígito|	Sim
emailComprador|	E-mail do comprador|	Alfa Numérico|	Até 100 caracteres|	Sim
enderecoComprador|	Logradouro do comprador|	Alfa Numérico|	Até 100 caracteres|	Sim
numeroEnderecoComprador|	Número do logradouro do comprador|	Alfa Numérico|	Até 10 caracteres|	Sim
bairroEnderecoComprador|	Bairro comprador|	Alfa Numérico|	Até 50 caracteres|	Sim
complementoEnderecoComprador|	Complemento do endereço comprador|	Alfa Numérico|	Até 50 caracteres|	Não
cidadeEnderecoComprador|	Cidade do comprador|	Alfa Numérico	|Até 50 caracteres|	Sim
estadoEnderecoComprador|	Estado do comprador|	Alfa Numérico|	Até 2 caracteres|	Sim
cepEnderecoComprador|	CEP do comprador. Enviar sem traços ou espaços|	Alfa Numérico|	Até 10 caracteres|	Sim
enderecoEntrega| Logradouro de entrega|	Alfa Numérico	|Até 100 caracteres|	Não
numeroEnderecoEntrega|	Número do logradouro de entrega|	Alfa Numérico|	Até 10 caracteres|	Não
bairroEnderecoEntrega|	Bairro do logradouro de entrega|	Alfa Numérico|	Até 50 caracteres|	Não
complementoEnderecoEntrega|	Complemento do endereço de entrega|	Alfa Numérico	|Até 50 caracteres|	Não
cidadeEnderecoEntrega|	Cidade de entrega|	Alfa Numérico|	Até 50 caracteres|	Não
estadoEnderecoEntrega|	Estado de entrega|	Alfa Numérico|	2 caracteres|	Não
cepEnderecoEntrega|	CEP de entrega. Enviar sem traços ou espaços|	Alfa Numérico|	Até 10 caracteres|	Não
telefoneEntrega|	Telefone de entrega. Sem espaços ou traços|	Alfa Numérico|	Até 10 caracteres|	Não
dddEntrega|	DDD do telefone de entrega|	Alfa Numérico|	Até 3 caracteres|	Não
ddiEntrega|	DDI do telefone de entrega|	Alfa Numérico|	Até 3 caracteres|	Não
codigoTipoTelefoneEntrega|	1 - Outros 2 - Residencial 3 - Comercial |	Numérico|	1 dígito|	Sim
telefoneAdicionalEntrega|	Telefone adicional de entrega. Sem espaços ou traços|	Alfa Numérico|	Até 3 caracteres|	Não
dddAdicionalEntrega|	DDD do telefone adicional de entrega	|Alfa Numérico	|Até 3 caracteres|	Não
ddiAdicionalEntrega|	DDI do telefone adicional de entrega|	Alfa Numérico	|Até 3 caracteres|	Não
codigoTipoTelefoneAdicionalEntrega|	1 - Outros 2 - Residencial 3 - Comercial |	Numérico|	1 dígito|	Sim

*itemPedidoTransacaoWS*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
codigoProduto|	Código único que identifica cada produto|	Alfa Numérico|	20 caracteres|	Sim
codigoCategoria|	Código que identifica categoria do produto|	Alfa Numérico|	20 caracteres|	Sim
nomeProduto|	Nome do Produto	Alfa Numérico	|100 caracteres	|Sim
quantidadeProduto|	Quantidade comprada do produto|	Numérico|	Até 8 dígitos|	Sim
valorUnitarioProduto|	Valor unitário do produto. Deve ser enviado sem pontos ou vírgulas|	Numérico|	Até 10 dígitos	|Sim
nomeCategoria|	Nome da categoria do produto	|Alfa Numérico|	100 caracteres|	Sim


**RESPOSTA**

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
autorizacao | Código de autorização da adquirente | Numérico | Até 20 dígitos
codigoTransacaoOperadora | Código da transação na adquirente | Numérico | Até 20 dígitos
dataAprovacaoOperadora | Data de aprovação na adquirente |Alfa Numérico | Até 10 dígitos
numeroComprovanteVenda | Número do comprovante de venda |Alfa Numérico | Até 20 dígitos
mensagemVenda | Mensagem de retorno da adquirente |Alfa Numérico | Até 50 dígitos

## Criando uma transação com múltiplos cartões
Esta estrutura permite o envio de dois ou mais cartões em uma mesma requisição, permitindo ao consumidor dividir o valor total da venda entre os seus cartões de crédito.
A venda só será finalizada com sucesso, se todos os cartões forem aprovados.

**Particulariedades**

* Disponível apenas no plano Corporativo;
* Disponível apenas para cartões de crédito modalidade WebService;
* Não disponível no fluxo de Antifraude.

> Exemplo criação transação:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
 <soapenv:Header/>
  <soapenv:Body>
   <pag:pagamentoTransacaoCompletaMaisCartoesCredito>
     <transacao>
       <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
       <!--enviar o array dadosCartoesCredito de acordo com a quantidade de cartões-->
       <dadosCartoesCredito>
         <codigoFormaPagamento>170</codigoFormaPagamento>
         <codigoSeguranca>123</codigoSeguranca>
         <dataValidadeCartao>12/2016</dataValidadeCartao>
         <nomeTitularCartaoCredito>Teste</nomeTitularCartaoCredito>
         <numeroCartaoCredito>4444333322221111</numeroCartaoCredito>
         <parcelas>1</parcelas>
         <valor>100</valor>
       </dadosCartoesCredito>
       <dadosCartoesCredito>
         <codigoFormaPagamento>171</codigoFormaPagamento>
         <codigoSeguranca>123</codigoSeguranca>
         <dataValidadeCartao>12/2016</dataValidadeCartao>
         <nomeTitularCartaoCredito>Teste</nomeTitularCartaoCredito>
         <numeroCartaoCredito>4444333322221111</numeroCartaoCredito>
         <parcelas>1</parcelas>
         <valor>100</valor>
       </dadosCartoesCredito>
       <dadosUsuarioTransacao>
         <documentoComprador>12312312312</documentoComprador>
         <nomeComprador>Teste SuperPay</nomeComprador>
       </dadosUsuarioTransacao>
       <idioma>1</idioma>
       <numeroTransacao>10</numeroTransacao>
       <urlCampainha></urlCampainha>
     </transacao>
     <usuario>superpay</usuario>
     <senha>superpay</senha>
    </pag:pagamentoTransacaoCompletaMaisCartoesCredito>
  </soapenv:Body>
</soapenv:Envelope>
```

> Exemplo retorno transação:

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:Body>
      <ns2:pagamentoTransacaoCompletaMaisCartoesCreditoResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <codigoEstabelecimento>1318336765212</codigoEstabelecimento>
            <detalhesFormaPagamentoMultiplosCartoes>
               <autorizacao>123456</autorizacao>
               <codigoFormaPagamento>170</codigoFormaPagamento>
               <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
               <dataAprovacaoOperadora>24/05/2017</dataAprovacaoOperadora>
               <mensagemVenda>Transação autorizada</mensagemVenda>
               <numeroComprovanteVenda>10069930690009F2530A</numeroComprovanteVenda>
               <parcelas>1</parcelas>
               <taxaEmbarque>0</taxaEmbarque>
               <valor>100</valor>
               <valorDesconto>0</valorDesconto>
            </detalhesFormaPagamentoMultiplosCartoes>
            <detalhesFormaPagamentoMultiplosCartoes>
               <autorizacao>0</autorizacao>
               <codigoFormaPagamento>171</codigoFormaPagamento>
               <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
               <dataAprovacaoOperadora>24/05/2017</dataAprovacaoOperadora>
               <mensagemVenda>Transação autorizada</mensagemVenda>
               <numeroComprovanteVenda>10069930690009F2531A</numeroComprovanteVenda>
               <parcelas>1</parcelas>
               <taxaEmbarque>0</taxaEmbarque>
               <valor>200</valor>
               <valorDesconto>0</valorDesconto>
            </detalhesFormaPagamentoMultiplosCartoes>
            <numeroTransacao>10</numeroTransacao>
            <statusTransacao>1</statusTransacao>
         </return>
      </ns2:pagamentoTransacaoCompletaMaisCartoesCreditoResponse>
   </soap:Body>
</soap:Envelope>
```

**REQUISIÇÃO**

Abaixo seguem os campos mínimos para finalizar com sucesso uma transação com Múltiplos Cartões.

<aside class="notice">
Para enviar a transação, acione o método <code>pagamentoTransacaoCompletaMaisCartoesCredito</code>
</aside>

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento

*transacaoCompletaWS*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos | Sim
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway) | Numérico | 13 dígitos | Sim
urlCampainha | URL será sempre acionada quando o status do pedido mudar. Deve estar preparada para receber dados de campainha | Alfa Numérico | Até 250 caracteres | Não
idioma | 1 - Português 2 - Inglês 3 - Espanhol | Numérico | 1 dígito | Sim
dadosUsuarioTransacao | Array dados do comprador | - | - | -
dadosCartoesCredito | Lista com informações dos cartões de créditos | - | - | -

*dadosUsuarioTransacao*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
nomeComprador | Nome do comprador| Alfa Numérico | Até 100 caracteres | Não
documentoComprador | Documento principal do comprado| Alfa Numérico | 30 caracteres | Não

*dadosCartaoCredito*

O array com os dados abaixo devem ser repetidos de acordo com a qauntidade de cartão a ser enviada.

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
codigoFormaPagamento |	Código da forma de pagamento |	Numérico |  2 dígitos	| Sim
valor | Valor da transação. Deve ser enviado sem pontos ou vírgulas |	Numérico |	Até 10 dígitos | Sim
parcelas |	Quantidade de parcelas da transação. Verificar se forma de pagamento suporta parcelamento |Numérico |	Até 2 dígitos|	Sim
nomeTitularCartaoCredito|	Nome do titular do cartão de crédito (Exatamente como escrito no cartão)|	Alfa Numérico|	Até 16 caracteres|	Sim
numeroCartaoCredito|	Numero do cartão de crédito, sem espaços ou traços|	Numérico|	Até 22 caracteres|	Sim
codigoSeguranca|	Código de segurança do cartão (campo não é armazenado pelo SuperPay)|	Numérico|	Até 4 caracteres|	Sim
dataValidadeCartao|	Data de validade do cartão. Formato mm/yyyy|	Alfa Numérico|	7 caracteres|	Sim

**RESPOSTA**

Campo | Descrição | Tipo | Tamanho 
------| ----------|------| --------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay | Numérico | 13 dígitos
statusTransacao | Status atual da transação | Numérico | Até 2 dígitos
detalhesFormaPagamentoMultiplosCartoesWS|	Lista com informações dos cartões de créditos|-|-|-

*detalhesFormaPagamentoMultiplosCartoesWS*

Campo | Descrição | Tipo | Tamanho 
------| ----------|------| --------
valor|	Valor da transação|	Numérico	|Até 10 dígitos
valorDesconto|	Valor do desconto da transação|	Numérico|	Até 10 dígitos
taxaEmbarque|	Valor da taxa de embarque|	Numérico|	Até 10 dígitos
parcelas|	Quantidade de parcelas da transação|	Numérico|	Até 2 dígitos
autorizacao	|Código de autorização da adquirente|	Numérico|	Até 20 dígitos
codigoTransacaoOperadora|	Código da transação na adquirente |	Numérico|	Até 20 dígitos
dataAprovacaoOperadora|	Data de aprovação na adquirente|	Alfa Numérico	|Até 10 caracteres
numeroComprovanteVenda	|Número do comprovante de venda	|Alfa Numérico|	Até 20 caracteres
mensagemVenda|	Mensagem de retorno da adquirente|	Alfa Numérico|	Até 50 caracteres

# Pagamentos com cartão de débito
Para pagamentos com cartão de débito é obrigatório a etapa de autenticação do consumidor na página do banco emissor de seu cartão. Sendo assim, após o envio dos dados da transação, o eCommerce deverá redirecionar o consumidor para o campo <code>urlPagamento<code> retornado pelo Gateway.


**Particulariedades**

* Não disponível no fluxo de Antifraude;
* Pagamento com redirecionamento;
* Importante a utilização do recurso de Campainha.

> Exemplo criação transação:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
 <soapenv:Header/>
  <soapenv:Body>
   <pag:pagamentoTransacaoCompleta>
     <transacao>
     <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
     <codigoFormaPagamento>179</codigoFormaPagamento>
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
     <urlCampainha>http://www.sualoja.campainha.com.br</urlCampainha>
     <urlRedirecionamentoNaoPago>https://www.sualoja.com.br/NaoPago</urlRedirecionamentoNaoPago>
     <urlRedirecionamentoPago>https://www.sualoja.com.br/Pago</urlRedirecionamentoPago>
     </transacao>
     <usuario>superpay</usuario>
     <senha>superpay</senha>
    </pag:pagamentoTransacaoCompleta>
  </soapenv:Body>
</soapenv:Envelope>
```

> Exemplo retorno transação:

```xml
<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <autorizacao>0</autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>179</codigoFormaPagamento>
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <dataAprovacaoOperadora>24/05/2017</dataAprovacaOperadora>
            <mensagemVenda></mensagemVenda>
            <numeroComprovanteVenda>1006993069181F841001</numeroComprovanteVenda>
            <numeroTransacao>1</numeroTransacao>
            <parcelas>1</parcelas>
            <statusTransacao>5</statusTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <urlPagamento>https://homologacao.superpay.com.br/checkout/PagamentoCielo/PagamentoVisaElectron.do?          cod=1503069157836b501aa77-6988-427c-9c8e-bead7409c669</urlpagamento>
            <valor>200</valor>
            <valorDesconto>0</valorDesconto>
         </return>
      </ns2:pagamentoTransacaoCompletaResponse>
   </soap:body>
</soap:envelope>
```

**REQUISIÇÃO**

<aside class="notice">
Para enviar a transação, acione o método <code>pagamentoTransacaoCompleta</code>
</aside>

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento


*transacaoCompletaWS*

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

*dadosUsuarioTransacao*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
nomeComprador | Nome do comprador| Alfa Numérico | Até 100 caracteres | Não
documentoComprador | Documento principal do comprado| Alfa Numérico | 30 caracteres | Não




**RESPOSTA**


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
autorizacao | Código de autorização da adquirente | Numérico | Até 20 dígitos
codigoTransacaoOperadora | Código da transação na adquirente | Numérico | Até 20 dígitos
dataAprovacaoOperadora | Data de aprovação na adquirente |Alfa Numérico | Até 10 dígitos
numeroComprovanteVenda | Número do comprovante de venda |Alfa Numérico | Até 20 dígitos
mensagemVenda | Mensagem de retorno da adquirente |Alfa Numérico | Até 50 dígitos


