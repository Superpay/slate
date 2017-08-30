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
Modelo WebService | Consumidor digitará os dados de cartão na página de Checkout do eCommerce.
Modelo Redirecionado | Consumidor digitará os dados de cartão na página da adquirente.

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

# Pagamentos com Cartão de Crédito

## Criando uma transação simplificada

Estrutura e exemplo de uma transação simples para cartão de crédito. As funcionalidades, como Análise de Fraude, Recorrência, OneClick e demais formas de pagamento precisam de uma estrutura mais completa para um perfeito funcionamento. Consulte as demais estruturas e exemplos desta documentação.



**REQUISIÇÃO**

<aside class="notice">
Para enviar a transação, acione o método <code>pagamentoTransacaoCompleta</code>
</aside>

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
Para autenticação, enviar `usuario` e `senha`:

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

A utilização desta estrutura é indicada para envio de pedidos com a forma de pagamento cartão de crédito, onde o estabelecimento possui contratação de umas das empresas de análise de risco integradas pelo SuperPay, sendo elas ClearSale (modalidades: Total/Total Garantido, Application, ID e Start) e FControl (modalidade Fila).

<aside class="warning">Contratação a parte com as empresas ClearSale e FControl</aside>


**REQUISIÇÃO**

<aside class="notice">
Para enviar a transação, acione o método <code>pagamentoTransacaoCompleta</code>
</aside>

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
Para autenticação, enviar `usuario` e `senha`:

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


**REQUISIÇÃO**

Abaixo seguem os campos mínimos para finalizar com sucesso uma transação com Múltiplos Cartões.

<aside class="notice">
Para enviar a transação, acione o método <code>pagamentoTransacaoCompletaMaisCartoesCredito</code>
</aside>

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
Para autenticação, enviar `usuario` e `senha`:

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

## Capturando uma transação
Através desta funcionalidade é possível confirmar uma pré autorização na adquirente, assim o consumidor receberá a cobrança em seu cartão e gerará crédito ao lojista.
Os estabelecimentos com captura manual deverão acionar o método de captura em até 5 dias da criação da venda, pois após este período a adquirente cancelará automaticamente a pré autorização.

**REQUISIÇÃO**

<aside class="notice">
Para enviar a transação, acione o método <code>operacaoTransacao</code>
</aside>

> Exemplo captura de transação:

```xml
<soapenv:envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
   <soapenv:header></soapenv:header>
   <soapenv:body>
      <pag:operacaoTransacao>
         <operacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <numeroTransacao>1</numeroTransacao>
            <operacao>1</operacao>
         </operacao>
         <usuario>superpay</usuario>
         <senha>superpay</senha>
      </pag:operacaoTransacao>
   </soapenv:body>
</soapenv:envelope>
```

Para autenticação, enviar `usuario` e `senha`:

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento

Campo | Descrição | Tipo | Tamanho
------| ---------- | ------| ----------
numeroTransacao |	Código que identifica a transação dentro do SuperPay|	Numérico|	Até 8 dígitos
codigoEstabelecimento|	Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway)|	Numérico|	13 dígitos
operacao|	Código que identifica o processo que deseja realizar. Para captura, deve-se enviar o valor 1|Numérico|	1 dígito

**RESPOSTA**


> Exemplo retorno da captura de transação:

```xml
<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <autorizacao>123456</autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>120</codigoFormaPagamento>
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

## Cancelando uma transação
Através desta funcionalidade é possível cancelar uma venda pré autorizada ou capturada. Consulte abaixo o prazo de cancelamento para cada adquirente:

Adquirente | Limite de cancelamento 
------| ----------
Cielo|	300 dias após a geração do pedido
Rede|	24 horas após geração do pedido
Elavon	|24 horas após geração do pedido
GETNET|	24 horas após geração do pedido
Stone	|180 dias após a geração do pedido
Bin	|90 dias após captura do pedido

**REQUISIÇÃO**

<aside class="notice">
Para enviar a transação, acione o método <code>operacaoTransacao</code>
</aside>

> Exemplo cancelamento de transação:

```xml
<soapenv:envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
   <soapenv:header></soapenv:header>
   <soapenv:body>
      <pag:operacaoTransacao>
         <operacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <numeroTransacao>1</numeroTransacao>
            <operacao>2</operacao>
         </operacao>
         <usuario>superpay</usuario>
         <senha>superpay</senha>
      </pag:operacaoTransacao>
   </soapenv:body>
</soapenv:envelope>
```
Para autenticação, enviar `usuario` e `senha`:

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento

Campo | Descrição | Tipo | Tamanho
------| ---------- | ------| ----------
numeroTransacao |	Código que identifica a transação dentro do SuperPay|	Numérico|	Até 8 dígitos
codigoEstabelecimento|	Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway)|	Numérico|	13 dígitos
operacao|	Código que identifica o processo que deseja realizar. Para cancelar, deve-se enviar o valor 2|Numérico|	1 dígito

**RESPOSTA**


> Exemplo retorno do cancelamento da transação:

```xml
<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <autorizacao>123456</autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>120</codigoFormaPagamento>
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <dataAprovacaoOperadora>24/05/2017</dataAprovacaOperadora>
            <mensagemVenda>Transacao cancelada com sucesso</mensagemVenda>
            <numeroComprovanteVenda>1006993069181F841001</numeroComprovanteVenda>
            <numeroTransacao>1</numeroTransacao>
            <parcelas>1</parcelas>
            <statusTransacao>13</statusTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <urlPagamento>14132971582229c00506d-e84d-4526-b902-92190d5aa808<urlpagamento></urlpagamento>
            <valor>200</valor>
            <valorDesconto>0</valorDesconto>
         </return>
      </ns2:pagamentoTransacaoCompletaResponse>
   </soap:body>
</soap:envelope>
```

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

## Estornando uma transação

A funcionalidade de estorno possui o mesmo objetivo do cancelamento, retornar o valor do pedido ao consumidor. A única diferença entre elas, é que neste modelo algumas adquirentes permitem a devolução de apenas parte do valor da venda ao consumidor, como é o caso da Cielo.

**Particulariedades**

* Disponível apenas no plano Corporativo;
* Disponível apenas para cartões de crédito.

Operadora | Prazo para estorno | Particulariedades
------| ----------|------
Cielo|	D+300|	Total e parcial. Para a bandeira Amex, disponível apenas o estorno Total
Bin|	D+0|	Apenas total

<aside class="notice">
Esta funcionalidade está disponível em um WebService diferenciado:

SANDBOX: <code>https://homologacao.superpay.com.br/checkout/servicosEstornoWS.Services?wsdl</code>  


PRODUÇÃO: <code>https://superpay2.superpay.com.br/checkout/servicosEstornoWS.Services?wsdl</code>
</aside>


**REQUISIÇÃO**

<aside class="notice">
Para enviar a transação, acione o método <code>estornaTransacao</code>
</aside>

> Exemplo estorno de transação:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:est="http://estorno.webservices.superpay.ernet.com.br/">
   <soapenv:Header/>
   <soapenv:Body>
      <est:estornaTransacao>
         <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
         <numeroTransacao>2</numeroTransacao>
         <valorEstorno>1000</valorEstorno>
      </est:estornaTransacao>
   </soapenv:Body>
</soapenv:Envelope>
```
Para autenticação, enviar `usuario` e `senha`:

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento

Campo | Descrição | Tipo | Tamanho
------| ---------- | ------| ----------
numeroTransacao |	Código que identifica a transação dentro do SuperPay|	Numérico|	Até 8 dígitos
codigoEstabelecimento|	Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway)|	Numérico|	13 dígitos
valorEstorno|	Valor do estorno |Numérico|	Ate 10 dígitos


**RESPOSTA**

> Exemplo retorno estorno de transação:

```xml
<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:estornaTransacaoResponse xmlns:ns2="http://estorno.webservices.superpay.ernet.com.br/">
         <return>
            Solicitacao de Estorno Cadastrada
         </return>
      </ns2:estornaTransacaoResponse>
   </soap:body>
</soap:envelope>
```



# Pagamentos com Cartão de Débito
## Criando uma transação simplificada

Para pagamentos com cartão de débito é obrigatório a etapa de autenticação do consumidor na página do banco emissor de seu cartão. Sendo assim, após o envio dos dados da transação, o eCommerce deverá redirecionar o consumidor para o campo <code>urlPagamento</code> retornado pelo Gateway.


**Particulariedades**

* Não disponível no fluxo de Antifraude;
* Pagamento com redirecionamento;
* Importante a utilização do recurso de Campainha.


**REQUISIÇÃO**

<aside class="notice">
Para enviar a transação, acione o método <code>pagamentoTransacaoCompleta</code>
</aside>

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
Para autenticação, enviar `usuario` e `senha`:

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

# Pagamentos com Boleto Bancário
## Criando uma transação

Estrutura para geração de boletos com carteiras sem ou com registro.

**Particulariedades**

* O campo `<estadoEnderecoComprador>` deve ser preenchido apenas com a sigla do Estado;
* O campo `<numeroTransacao>` deve conter até 8 dígitos;
* Caso o campo `<vencimentoBoleto>` não for enviado, será utilizado os dias de vencimento configurado internamente no Gateway;
* Para boletos com carteira registrada (contratação a parte), o status retornado pelo SuperPay no primeiro momento será 5 (transação em andamento), enquanto para boletos sem registros é retornado 8 (aguardando pagamento);
* Importante a utilização do recurso de Campainha, para atualização dos pedidos no Ecommerce;
* A conciliação de boletos não é realizada automaticamente, funcionalidade deve ser contratada a parte com o comercial@superpay.com.br.

**REQUISIÇÃO**

<aside class="notice">
Para enviar a transação, acione o método <code>pagamentoTransacaoCompleta</code>
</aside>

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
            <codigoFormaPagamento>29</codigoFormaPagamento>
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
               <documentoComprador>12345678919</documentoComprador>
               <emailComprador>superpay@superpay.com.br</emailComprador>
               <enderecoComprador>Rua do Comprador</enderecoComprador>
               <enderecoEntrega>Rua do Comprador</enderecoEntrega>
               <estadoEnderecoComprador>SP</estadoEnderecoComprador>
               <estadoEnderecoEntrega>SP</estadoEnderecoEntrega>
               <nomeComprador>Testes de integracao Boleto</nomeComprador>
               <numeroEnderecoComprador>123</numeroEnderecoComprador>
               <numeroEnderecoEntrega>123</numeroEnderecoEntrega>
               <paisComprador>BR</paisComprador>
               <paisEntrega>BR</paisEntrega>
               <sexoComprador>M</sexoComprador>
               <telefoneComprador>1234123123</telefoneComprador>
               <telefoneEntrega>1234123123</telefoneEntrega>
               <tipoCliente>1</tipoCliente>
            </dadosUsuarioTransacao>
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
            <numeroTransacao>9012346</numeroTransacao>
            <origemTransacao>1</origemTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <urlCampainha>https://campainha.do</urlCampainha>
            <urlRedirecionamentoNaoPago>http://www.google.com.br</urlRedirecionamentoNaoPago>
            <urlRedirecionamentoPago>http://www.google.com.br</urlRedirecionamentoPago>
            <valor>200</valor>
            <valorDesconto>0</valorDesconto>
            <vencimentoBoleto></vencimentoBoleto>
         </transacao>
         <usuario>superpay</usuario>
         <senha>superpay</senha>
      </pag:pagamentoTransacaoCompleta>
   </soapenv:body>
</soapenv:envelope>
```
Para autenticação, enviar `usuario` e `senha`:

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
urlCampainha | URL será sempre acionada quando o status do pedido mudar. Deve estar preparada para receber dados de campainha | Alfa Numérico | Até 250 caracteres | Não
urlRedirecionamentoPago | Para o modelo de pagamento redirect, O SuperPay redirecionará para essa URL em caso de transação aprovada | Alfa Numérico | Até 250 caracteres | Para pagamentos redirecionáveis é obrigatório
urlRedirecionamentoPago | Para o modelo de pagamento redirect, O SuperPay redirecionará para essa URL em caso de transação negada | Alfa Numérico | Até 250 caracteres | Para pagamentos redirecionáveis é obrigatório
ip | Número do IP de origem. Formato xxx.xxx.xxx.xxx | Alfa Numérico | Até 15 caracteres | Não
idioma	| 1 - Português 2 - Inglês 3 - Espanhol | Numérico | 1 dígito | Sim
origemTransacao | 1 - eCommerce 2 - Mobile 3 - URA | Numérico | 1 dígito | Sim
campoLivre1 | Campo Livre 1 |	Alfa Numérico |	Até 16 caracteres |	Não
campoLivre2 |	Campo Livre 2  | Alfa Numérico | Até 16 caracteres | Não
campoLivre3 |	Campo Livre 3 | Alfa Numérico | Até 16 caracteres | Não
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
sexoComprador |	M – Masculino / F – Feminino |	Alfa Numérico |	1 caracter |	Não
dataNascimentoComprador |	Data de nascimento do comprador. Formato dd/mm/yyyy |	Alfa Numérico|	10 caracteres	|Sim
telefoneComprador | Telefone do comprador sem espaços ou traços |	Alfa Numérico |	Até 10 caracteres |	Sim
dddComprador |	DDD do telefone do comprador |	Alfa Numérico	| Até 3 caracteres |	Sim
ddiComprador |	DDI do telefone do comprador |	Alfa Numérico |	Até 3 caracteres |	Sim
codigoTipoTelefoneComprador |	1 - Outros 2 - Residencial 3 - Comercial |	Numérico |	1 dígito | Sim
dddAdicionalComprador |	DDD do telefone adicional do comprador |	Alfa Numérico |	Até 3 caracteres |	Não
ddiAdicionalComprador |	DDI do telefone adicional do comprador | Alfa Numérico | Até 3 caracteres |	Não
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
Para geração do boleto o eCommerce deverá redirecionar o consumidor para a URl retornada no campo <urlPagamento>

> Exemplo retorno criação boleto:
```xml
<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <autorizacao>0</autorizacao>
            <codigoEstabelecimento>1000000000000</codigoestabelecimento>
            <codigoFormaPagamento>29</codigoformapagamento>
            <codigoTransacaoOperadora>0</codigotransacaooperadora>
            <dataAprovacaoOperadora></dataaprovacaooperadora>
            <mensagemvenda></mensagemvenda>
            <numeroComprovanteVenda></numerocomprovantevenda>
            <numeroTransacao>9012346</numerotransacao>
            <parcelas>1</parcelas>
            <statusTransacao>5</statustransacao>
            <taxaEmbarque>0</taxaembarque>
            <urlPagamento>https://homologacao.superpay.com.br/checkout/GeradorBoleto.do?cod=14956296486904d8312c6-d57a-499e-b53b-504047402e45</urlpagamento>
            <valor>200</valor>
            <valorDesconto>0</valordesconto>
         </return>
      </ns2:pagamentoTransacaoCompletaResponse>
   </soap:body>
</soap:envelope>
```

Campo | Descrição | Tipo | Tamanho 
------| ----------|------| --------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay | Numérico | 13 dígitos
codigoFormaPagamento | Código da forma de pagamento | Numérico | Até 3 dígitos
valor | Valor da transação.| Numérico | Até 10 dígitos
valorDesconto | Valor desconto | Numérico | Até 10 dígitos
taxaEmbarque | Valor taxa embarque | Numérico | Até 10 dígitos
parcelas | Quantidade de parcelas da transação | Numérico | Até 2 dígitos
urlPagamento | Url para geração do boleto |Alfa Numérico | Até 500 caracteres 
statusTransacao | Status atual da transação | Numérico | Até 2 dígitos
autorizacao | Retornado "0" para boletos | Numérico | Até 20 dígitos
codigoTransacaoOperadora | Retornado "0" para boletos | Numérico | Até 20 dígitos
dataAprovacaoOperadora | Retornado em branco para boletos |Alfa Numérico | Até 10 dígitos
numeroComprovanteVenda | Retornado em branco para boletos |Alfa Numérico | Até 20 dígitos
mensagemVenda | Retornado em branco para boletos |Alfa Numérico | Até 50 dígitos

# Pagamentos com Transferência Eletrônica
## Criando uma transação

Estrutura para criação de transferência eletrônica.

**Particulariedades**

* O campo `<estadoEnderecoComprador>` deve ser preenchido apenas com a sigla do Estado;
* O campo `<numeroTransacao>` deve conter até 8 dígitos;
* Importante a utilização do recurso de Campainha, para atualização dos pedidos no Ecommerce;


**REQUISIÇÃO**

<aside class="notice">
Para enviar a transação, acione o método <code>pagamentoTransacaoCompleta</code>
</aside>

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
            <codigoFormaPagamento>16</codigoFormaPagamento>
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
               <documentoComprador>12345678919</documentoComprador>
               <emailComprador>superpay@superpay.com.br</emailComprador>
               <enderecoComprador>Rua do Comprador</enderecoComprador>
               <enderecoEntrega>Rua do Comprador</enderecoEntrega>
               <estadoEnderecoComprador>SP</estadoEnderecoComprador>
               <estadoEnderecoEntrega>SP</estadoEnderecoEntrega>
               <nomeComprador>Testes de integracao Boleto</nomeComprador>
               <numeroEnderecoComprador>123</numeroEnderecoComprador>
               <numeroEnderecoEntrega>123</numeroEnderecoEntrega>
               <paisComprador>BR</paisComprador>
               <paisEntrega>BR</paisEntrega>
               <sexoComprador>M</sexoComprador>
               <telefoneComprador>1234123123</telefoneComprador>
               <telefoneEntrega>1234123123</telefoneEntrega>
               <tipoCliente>1</tipoCliente>
            </dadosUsuarioTransacao>
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
            <numeroTransacao>9012346</numeroTransacao>
            <origemTransacao>1</origemTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <urlCampainha>https://campainha.do</urlCampainha>
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
Para autenticação, enviar `usuario` e `senha`:

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
urlCampainha | URL será sempre acionada quando o status do pedido mudar. Deve estar preparada para receber dados de campainha | Alfa Numérico | Até 250 caracteres | Não
urlRedirecionamentoPago | URl para onde o consumidor será recirecionado em caso de pagamento aprovado | Alfa Numérico | Até 250 caracteres | Sim
urlRedirecionamentoPago | URl para onde o consumidor será recirecionado em caso de pagamento não aprovado | Alfa Numérico | Até 250 caracteres | Sim
ip | Número do IP de origem. Formato xxx.xxx.xxx.xxx | Alfa Numérico | Até 15 caracteres | Não
idioma	| 1 - Português 2 - Inglês 3 - Espanhol | Numérico | 1 dígito | Sim
origemTransacao | 1 - eCommerce 2 - Mobile 3 - URA | Numérico | 1 dígito | Sim
campoLivre1 | Campo Livre 1 |	Alfa Numérico |	Até 16 caracteres |	Não
campoLivre2 |	Campo Livre 2  | Alfa Numérico | Até 16 caracteres | Não
campoLivre3 |	Campo Livre 3 | Alfa Numérico | Até 16 caracteres | Não
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
sexoComprador |	M – Masculino / F – Feminino |	Alfa Numérico |	1 caracter |	Não
dataNascimentoComprador |	Data de nascimento do comprador. Formato dd/mm/yyyy |	Alfa Numérico|	10 caracteres	|Sim
telefoneComprador | Telefone do comprador sem espaços ou traços |	Alfa Numérico |	Até 10 caracteres |	Sim
dddComprador |	DDD do telefone do comprador |	Alfa Numérico	| Até 3 caracteres |	Sim
ddiComprador |	DDI do telefone do comprador |	Alfa Numérico |	Até 3 caracteres |	Sim
codigoTipoTelefoneComprador |	1 - Outros 2 - Residencial 3 - Comercial |	Numérico |	1 dígito | Sim
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

Para geração do boleto o eCommerce deverá redirecionar o consumidor para a URl retornada no campo <urlPagamento>

> Exemplo retorno criação transação:

```xml
<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <autorizacao>0</autorizacao>
            <codigoEstabelecimento>1000000000000</codigoestabelecimento>
            <codigoFormaPagamento>16</codigoformapagamento>
            <codigoTransacaoOperadora>0</codigotransacaooperadora>
            <dataAprovacaoOperadora></dataaprovacaooperadora>
            <mensagemvenda></mensagemvenda>
            <numeroComprovanteVenda></numerocomprovantevenda>
            <numeroTransacao>9012346</numerotransacao>
            <parcelas>1</parcelas>
            <statusTransacao>5</statustransacao>
            <taxaEmbarque>0</taxaembarque>
            <urlPagamento>https://homologacao.superpay.com.br/checkout/PagamentoItauShopLine/PagamentoItauShopLine.do?code=14956296486904d8312c6-d57a-499e-b53b-504047402e45</urlpagamento>
            <valor>200</valor>
            <valorDesconto>0</valordesconto>
         </return>
      </ns2:pagamentoTransacaoCompletaResponse>
   </soap:body>
</soap:envelope>
```

Campo | Descrição | Tipo | Tamanho 
------| ----------|------| --------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay | Numérico | 13 dígitos
codigoFormaPagamento | Código da forma de pagamento | Numérico | Até 3 dígitos
valor | Valor da transação.| Numérico | Até 10 dígitos
valorDesconto | Valor desconto | Numérico | Até 10 dígitos
taxaEmbarque | Valor taxa embarque | Numérico | Até 10 dígitos
parcelas | Quantidade de parcelas da transação | Numérico | Até 2 dígitos
urlPagamento | Url para geração da página do banco |Alfa Numérico | Até 500 caracteres 
statusTransacao | Status atual da transação | Numérico | Até 2 dígitos
autorizacao | Retornado "0" para transferência | Numérico | Até 20 dígitos
codigoTransacaoOperadora | Retornado "0" para transferência | Numérico | Até 20 dígitos
dataAprovacaoOperadora | Retornado em branco para transferência |Alfa Numérico | Até 10 dígitos
numeroComprovanteVenda | Retornado em branco para transferência |Alfa Numérico | Até 20 dígitos
mensagemVenda | Retornado em branco para transferência |Alfa Numérico | Até 50 dígitos

# Pagamentos Recorrentes

<aside class="notice">
Esta funcionalidade está disponível em um WebService diferenciado:

SANDBOX: <code>https://homologacao.superpay.com.br/checkout/servicosRecorrenciaWS.Services?wsdl</code>


PRODUÇÃO: <code>https://superpay2.superpay.com.br/checkout/servicosRecorrenciaWS.Services?wsdl</code>
</aside>

## Criando uma transação recorrente
Neste modelo, o SuperPay controla os pagamentos que foram cadastrados pelo estabelecimento, de acordo com sua periodicidade, valor e dia de cobrança.

O gateway possui um processo que verifica todos os dias se existem recorrências a serem processadas. Se sim, elas são agendadas para processamento na madrugada e assim que finalizadas, o SuperPay notifica o estabelecimento através do acionamento da campainha, enviada no cadastro da recorrência.


**Particulariedades**

* Disponível apenas para cartão de crédito no modelo WebService;
* Renova Fácil com a operadora Cielo;
* Importante a utilização do recurso de Campainha, para atualização dos pedidos no Ecommerce;

**REQUISIÇÃO**

> Exemplo criação transação recorrente:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:rec="http://recorrencia.webservices.superpay.ernet.com.br/">
   <soapenv:Header/>
   <soapenv:Body>
      <rec:cadastrarRecorrenciaWS>
          <recorrenciaWS>
            <dadosCartao>
               <codigoSeguranca>123</codigoSeguranca>
               <dataValidade>05/2018</dataValidade>
               <nomePortador>Teste</nomePortador>
               <numeroCartao>4444333322221111</numeroCartao>
            </dadosCartao>
            <dadosCobranca>
               <nomeComprador>Nome do Comprador</nomeComprador>
                <telefone>
                </telefone>
            </dadosCobranca>
            <diaCobranca>23</diaCobranca>
            <mesCobranca>10</mesCobranca>
            <estabelecimento>1000000000000</estabelecimento>
            <formaPagamento>170</formaPagamento>
            <numeroRecorrencia>1</numeroRecorrencia>
            <periodicidade>3</periodicidade>
            <primeiraCobranca>1</primeiraCobranca>
            <processarImediatamente>1</processarImediatamente>
            <quantidadeCobrancas>0</quantidadeCobrancas>
            <urlNotificacao></urlNotificacao>
            <valor>100</valor>
         </recorrenciaWS>
         <usuario>
            <senha>superpay</senha>
            <usuario>superpay</usuario>
         </usuario>
      </rec:cadastrarRecorrenciaWS>
   </soapenv:Body>
</soapenv:Envelope>
```
Para autenticação, enviar `usuario` e `senha`:

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento

*cadastrarRecorrenciaWS*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
numeroRecorrencia|	Número da Recorrência deve ser único|	Numérico|	Até 15 dígitos|	Sim
estabelecimento|	Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway)|	Numérico|	13 dígitos|	Sim
valor|	Valor da recorrência. Não devem ser utilizados virgulas nem pontos|	Numérico|	Até 10 dígitos|	Sim
formaPagamento|	Código da forma de pagamento |	Numérico|	Até 3 dígitos|	Sim
dadosCartao|	Array descrito abaixo|	-|	-|	-
dadosCobranca|	Array descrito abaixo|	-|	-|	-
dadosEntrega|	Array descrito abaixo|	-|	-|	-
quantidadeCobrancas|	Quantidade de cobranças, caso 0 a recorrência será feita até que ocorra um cancelamento|	Numérico|	Até 10 dígitos|	Sim
diaCobranca|	Dia para cobrança|	Numérico|	Até 2 dígitos|	Sim
mesCobranca|	Mês para cobrança|	Numérico	|Até 2 dígitos	|Sim
periodicidade|	1 – Semanal, 2 – Quinzenal, 3 – Mensal, 4 – Bimestral, 5 – Trimestral, 6 – Semestral e 7 – Anual|	Numérico|	1 dígito|	Sim
urlNotificacao|	URL para notificação de cada cobrança da recorrência	|Alfa Numérico	|Até 200 caracteres|	Sim
primeiraCobranca|	1– Cobrança será efetuada no mês corrente ao cadastro da recorrência / 2–Cobrança será efetuada no mês seguinte.|	Numérico|	1 dígito|	Sim
processarImediatamente|	1 – A recorrência será processada imediatamente ao cadastro	|Numérico|	1 dígito|	Sim

*dadosCartao*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
nomePortador|	Nome do titular do cartão de crédito |	Alfa Numérico|	Até 16 caracteres|	Sim
numeroCartao	|Numero do cartão de crédito, sem espaços ou traços|	Numérico	|Até 22 caracteres|	Sim
codigoSeguranca|	Código de segurança do cartão (campo não é armazenado pelo SuperPay)|	Numérico|	Até 4 caracteres|	Sim
dataValidade|	Data de validade do cartão. Formato mm/yyyy|	Alfa Numérico|	7 caracteres|	Sim

*dadosCobranca*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
nomeComprador|	Nome do comprador|	Alfa Numérico|	Até 100 caracteres	|Caso utilize antifraude, sim
emailComprador|	E-mail do comprador	|Alfa Numérico| Até 100 caracteres|	Caso utilize antifraude, sim
enderecoComprador|	Logrodouro do comprador|	Alfa Numérico	|Até 100 caracteres|	Caso utilize antifraude, sim
bairroComprador|	Bairro do comprador|	Alfa Numérico|	Até 50 caracteres|	Caso utilize antifraude, sim
complementoComprador|	Complemento do endereço comprador|	Alfa Numérico|	Até 50 caracteres|	Não
cidadeComprador|	Cidade do comprador	|Alfa Numérico	|Até 50 caracteres|	Caso utilize antifraude, sim
estadoComprador|	Estado do comprador|	Alfa Numérico	|Até 2 caracteres|	Caso utilize antifraude, sim
cepComprador|	CEP do comprador. Enviar sem traços ou espaços|	Alfa Numérico	|Até 10 caracteres|	Caso utilize antifraude, sim
paisComprador|	Pais do comprador|	Alfa Numérico|	Até 50 caracteres|	Não
telefone[]|	Lista de Telefones|	List|	-|	-

*dadosEntrega*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
nomeEntrega	|Nome de entegra	|Alfa Numérico|	Até 100 caracteres|	Caso utilize antifraude, sim
emailEntrega|	Email de entrega|	Alfa Numérico|	Até 100 caracteres|	Caso utilize antifraude, sim
enderecoEntrega|	Logradouro de entrega	|Alfa Numérico	|Até 100 caracteres|	Caso utilize antifraude, sim
bairroEntrega|	Bairro de entrega|	Alfa Numérico|	Até 50 caracteres|	Caso utilize antifraude, sim
complementoEntrega|	Complemento do endereço de entrega|	Alfa Numérico	|Até 50 caracteres|	Não
cidadeEntrega	| Cidade de entrega|	Alfa Numérico	|Até 50 caracteres|	Caso utilize antifraude, sim
estadoEntrega|	Estado de entrega|	Alfa Numérico|	2 caracteres|	Caso utilize antifraude, sim
cepEntrega|	CEP de entrega. Enviar sem traços ou espaços|	Alfa Numérico	|Até 10 caracteres|	Caso utilize antifraude, sim
paisEntrega|	Pais de entrega|	Alfa Numérico|	Até 50 caracteres|	Não
telefone[]|	Lista de Telefones|	List|	-|	-

*telefone*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
ddd	|DDD do telefone do comprador|	Alfa Numérico|	Até 3 caracteres|	Caso utilize antifraude, sim
ddi|	DDI do telefone do comprador|	Alfa Numérico|	Até 3 caracteres|	Caso utilize antifraude, sim
telefone|	 Número do telefone|	Alfa Numérico|	Até 10 caracteres|	Caso utilize antifraude, sim
tipoTelefone|	1 - Outros 2 - Residencial 3 - Comercial |	Numérico|	Até 2 dígitos|	Sim

**RESPOSTA**

> Exemplo retorno criação transação recorrente:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:rec="http://recorrencia.webservices.superpay.ernet.com.br/">
   <soapenv:Body>
     <ns2:cadastrarRecorrenciaWSResponse xmlns:ns2="http://recorrencia.webservices.superpay.ernet.com.br/">
         <return>
            <estabelecimento>1000000000000</estabelecimento>
            <numeroRecorrencia>1</numeroRecorrencia>
            <status>true</status>
            <valor>100</valor>
         </return>
      </ns2:cadastrarRecorrenciaWSResponse>
   </soapenv:Body>
</soapenv:Envelope>
```

Campo | Descrição 
------| ----------
numeroRecorrencia|	Número da Recorrência
estabelecimento|	Código do estabelecimento, fornecido pelo SuperPay
valor|	Valor
status|	Status da Recorrência ( True - Ativo, False - Inativo)


## Cancelando uma recorrência

**REQUISIÇÃO**

> Exemplo cancelamento transação recorrente:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:rec="http://recorrencia.webservices.superpay.ernet.com.br/">
   <soapenv:Header/>
   <soapenv:Body>
      <rec:cancelarRecorrenciaWS>
         <recorrenciaCancelarWS>
            <estabelecimento>1000000000000</estabelecimento>
            <numeroRecorrencia>1</numeroRecorrencia>
         </recorrenciaCancelarWS>
         <usuario>
            <senha>superpay</senha>
            <usuario>superpay</usuario>
         </usuario>
      </rec:cancelarRecorrenciaWS>
   </soapenv:Body>
</soapenv:Envelope>
```
Para autenticação, enviar `usuario` e `senha`:

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento

Campo | Descrição | Obrigatório
------| ----------| ----------
numeroRecorrencia|	Número da Recorrência a ser cancelada| Sim
estabelecimento	|Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway)| Sim

**RESPOSTA**

> Exemplo retorno cancelamento transação recorrente:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:rec="http://recorrencia.webservices.superpay.ernet.com.br/">
   <soapenv:Body>
     <ns2:cancelarRecorrenciaWSResponse xmlns:ns2="http://recorrencia.webservices.superpay.ernet.com.br/">
         <return>
            <estabelecimento>1000000000000</estabelecimento>
            <numeroRecorrencia>1</numeroRecorrencia>
            <status>false</status>
            <valor>100</valor>
         </return>
      </ns2:ccancelarRecorrenciaWSResponse>
   </soapenv:Body>
</soapenv:Envelope>
```

Campo | Descrição 
------| ----------
numeroRecorrencia|	Número da Recorrência
estabelecimento|	Código do estabelecimento, fornecido pelo SuperPay
valor|	Valor
status|	Status da Recorrência ( True - Ativo, False - Inativo)


# Pagamentos com Token (OneClick)

**Particulariedades**

* Disponível apenas no plano Corporativo;
* Disponível para cartão de crédito e débito.


<aside class="notice">
Esta funcionalidade está disponível em um WebService diferenciado:

SANDBOX: <code>https://homologacao.superpay.com.br/checkout/servicosPagamentoOneClickWS.Services?wsdl</code>


PRODUÇÃO: <code>https://superpay2.superpay.com.br/checkout/servicosPagamentoOneClickWS.Services?wsdl</code>
</aside>

## Cadastrando cartão

Funcionalidade que permite o cadastramento de cartão para utilização nas futuras compras, assim o consumidor precisará incluir apenas o código de segurança para finalizar a compra.


**REQUISIÇÃO**

<aside class="notice">Para criar o token, acione o método <code>cadastraPagamentoOneClickV2</code></aside>

> Exemplo cadastro do cartão:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
   <soapenv:Header/>
   <soapenv:Body>
      <pag:cadastraPagamentoOneClickV2>
         <dadosOneClick>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <emailComprador>suporte@superpay.com.br</emailComprador>
            <formaPagamento>350</formaPagamento>
            <nomeTitularCartaoCredito>Teste</nomeTitularCartaoCredito>
            <numeroCartaoCredito>4444333322221111</numeroCartaoCredito>
            <dataValidadeCartao>10/2018</dataValidadeCartao>
         </dadosOneClick>
         <usuario>superpay</usuario>
         <senha>superpay</senha>
      </pag:cadastraPagamentoOneClickV2>
   </soapenv:Body>
</soapenv:Envelope>
```

Para autenticação, enviar `usuario` e `senha`:

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento


Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------

codigoEstabelecimento|	Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway)|	Numérico|13 dígitos|	Sim
nomeTitularCartaoCredito|	Nome do titular do cartão de crédito |	Alfa Numérico|	Até 25 caracteres|	Sim
numeroCartaoCredito|	Numero do cartão de crédito. Enviar sem espaços e virgúlas|	Numérico|	Até 22 caracteres|	Sim
dataValidadeCartao|	Data de validade do cartão. Formato mm/yyyy|	Alfa Numérico|	7 caracteres	|Sim
emailComprador|	Endereço de e-mail do comprador|	Alfa Numérico|	Até 100 caracteres|	Não
formaPagamento|	Código da forma de pagamento. Clique aqui para maiores informações|	Numérico|	-|	Sim

**RESPOSTA**

> Exemplo retorno cadastro do cartão. O retorno da requisição de cadastro será apenas o Token, este deverá ser armazenado no Ecommerce para as próximas compras

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:Body>
      <ns2:cadastraPagamentoOneClickV2Response xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>149624974900187da0175-509d-4b01-9158-a96e3ed9ccae</return>
      </ns2:cadastraPagamentoOneClickV2Response>
   </soap:body>
</soap:envelope>
```

## Pagamento com token

Com o Token recebido no momento do cadastro, é possível realizar o pagamento.


**REQUISIÇÃO**

<aside class="notice">Para pagamento com token, acione o método <code>pagamentoOneClickV2</code></aside>

> Exemplo estrutura simplificada para estabelecimentos que não utilizam antifraude:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
   <soapenv:Header/>
   <soapenv:Body>
      <pag:pagamentoOneClickV2>
         <transacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <dadosUsuarioTransacao>
               <nomeComprador>Nome do comprador</nomeComprador>
               <documentoComprador>12312312312</documentoComprador>
            </dadosUsuarioTransacao>
            <numeroTransacao>10</numeroTransacao>
            <parcelas>1</parcelas>
            <cvv>123</cvv>
            <token>149624974900187da0175-509d-4b01-9158-a96e3ed9ccae</token>
            <urlCampainha>http://sualoja.com.br/campainha</urlcampainha>
            <valor>100</valor>
         </transacao>
         <usuario>superpay</usuario>
         <senha>superpay</senha>
      </pag:pagamentoOneClickV2>
   </soapenv:Body>
</soapenv:Envelope>
```

Para autenticação, enviar `usuario` e `senha`:

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento

*transacaoOneClickWS*

numeroTransacao|	Código que identifica a transação dentro do SuperPay|	Numérico|	Até 19 dígitos|	Sim
codigoEstabelecimento| Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway)|	Numérico| 13 dígitos|	Sim
token|	Valor único obtido através do serviço “cadastroPagamentoOneClick2”|	Alfa Numérico|	Até 100 caracteres|	Sim
cvv	| Código de segurança do cartão |Numérico|	Até 4 dígitos|	Sim
valor|	Valor da transação. Deve ser enviado sem pontos ou vírgulas|	Numérico|	Até 10 dígitos	|Sim
valorDesconto|	Valor do desconto da transação. Campo apenas informativo|	Numérico|	Até 10 dígitos|	Sim
taxaEmbarque|	Valor da taxa de embarque. Campo apenas informativo|	Numérico|	Até 10 dígitos|	Sim
parcelas|	Quantidade de parcelas da transação. Verificar se forma de pagamento suporta parcelamento|	Numérico|Até 2 dígitos | Sim
urlCampainha|	URL será sempre acionada quando o status do pedido mudar. Deve estar preparada para receber dados de campainha|	Alfa Numérico	|Até 250 caracteres|	Não
urlRedirecionamentoPago	|Para o modelo de pagamento redirect, O SuperPay redirecionará para essa URL em caso de transação aprovada|	Alfa Numérico|	Até 250 caracteres|	Para pagamentos com redirect é obrigatório
urlRedirecionamentoNaoPago|	Para o modelo de pagamento redirect, O SuperPay redirecionará para essa URL em caso de transação reprovada|	Alfa Numérico|	Até 250 caracteres|	Para pagamentos com redirect é obrigatório
ip|	Número do IP do usuário final/cliente. Formato xxx.xxx.xxx.xxx|	Alfa Numérico|	Até 15 caracteres|	Não
idioma|	1 - Português 2 - Inglês 3 - Espanhol|	Numérico|	 - |	Sim
origemTransacao|	1 - eCommerce 2 - Mobile 3 - URA |	Numérico|	 - |	Sim
campoLivre1|	Campo Livre 1|	Alfa Numérico|	Até 16 caracteres|	Não
campoLivre2|	Campo Livre 2|	Alfa Numérico|	Até 16 caracteres|	Não
campoLivre3|	Campo Livre 3|	Alfa Numérico|	Até 16 caracteres|	Não
campoLivre4|	Campo Livre 4|	Alfa Numérico|	Até 16 caracteres|	Não
campoLivre5|	Campo Livre 5|	Alfa Numérico|	Até 16 caracteres|	Não
dadosUsuarioTransacao|	Informações para cobrança e entrega. Informações importantes para análise de fraude|	 -|	 -|	 -
itensDoPedido|	Lista com Itens que estão sendo comprados. Informações importantes para análise de fraude e intermediários financeiros	| -	 - |	 - 

*dadosUsuarioTransacao*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
codigoCliente |	Código que identifica o cliente no estabelecimento |	Alfa Numérico |	20 caracteres	|Não
tipoCliente |	1 - Pessoa Física 2 - Pessoa Jurídica |	Numérico	| 1 dígito	|Sim
nomeComprador |	Nome do comprador |	Alfa Numérico	| Até 100 caracteres	| Caso utilize antifraude sim
documentoComprador |	Documento principal do comprador |	Alfa Numérico	| 30 caracteres	| Caso utilize antifraude sim
sexoComprador |	M – Masculino / F – Feminino |	Alfa Numérico |	1 caracter |	Não
dataNascimentoComprador |	Data de nascimento do comprador. Formato dd/mm/yyyy |	Alfa Numérico|	10 caracteres	|Caso utilize antifraude sim
telefoneComprador | Telefone do comprador sem espaços ou traços |	Alfa Numérico |	Até 10 caracteres |	Caso utilize antifraude sim
dddComprador |	DDD do telefone do comprador |	Alfa Numérico	| Até 3 caracteres |	Caso utilize antifraude sim
ddiComprador |	DDI do telefone do comprador |	Alfa Numérico |	Até 3 caracteres |	Caso utilize antifraude sim
codigoTipoTelefoneComprador |	1 - Outros 2 - Residencial 3 - Comercial |	Numérico |	1 dígito | Sim
emailComprador|	E-mail do comprador|	Alfa Numérico|	Até 100 caracteres|	Caso utilize antifraude sim
enderecoComprador|	Logradouro do comprador|	Alfa Numérico|	Até 100 caracteres|	Caso utilize antifraude sim
numeroEnderecoComprador|	Número do logradouro do comprador|	Alfa Numérico|	Até 10 caracteres|	Caso utilize antifraude sim
bairroEnderecoComprador|	Bairro comprador|	Alfa Numérico|	Até 50 caracteres|	Caso utilize antifraude sim
complementoEnderecoComprador|	Complemento do endereço comprador|	Alfa Numérico|	Até 50 caracteres|	Não
cidadeEnderecoComprador|	Cidade do comprador|	Alfa Numérico	|Até 50 caracteres|	Caso utilize antifraude sim
estadoEnderecoComprador|	Estado do comprador|	Alfa Numérico|	Até 2 caracteres|	Caso utilize antifraude sim
cepEnderecoComprador|	CEP do comprador. Enviar sem traços ou espaços|	Alfa Numérico|	Até 10 caracteres|	Caso utilize antifraude sim
enderecoEntrega| Logradouro de entrega|	Alfa Numérico	|Até 100 caracteres|	Caso utilize antifraude sim
numeroEnderecoEntrega|	Número do logradouro de entrega|	Alfa Numérico|	Até 10 caracteres|	Caso utilize antifraude sim
bairroEnderecoEntrega|	Bairro do logradouro de entrega|	Alfa Numérico|	Até 50 caracteres|	Caso utilize antifraude sim
complementoEnderecoEntrega|	Complemento do endereço de entrega|	Alfa Numérico	|Até 50 caracteres|	Não
cidadeEnderecoEntrega|	Cidade de entrega|	Alfa Numérico|	Até 50 caracteres|	Caso utilize antifraude sim
estadoEnderecoEntrega|	Estado de entrega|	Alfa Numérico|	2 caracteres|	Caso utilize antifraude sim
cepEnderecoEntrega|	CEP de entrega. Enviar sem traços ou espaços|	Alfa Numérico|	Até 10 caracteres|	Caso utilize antifraude sim
telefoneEntrega|	Telefone de entrega. Sem espaços ou traços|	Alfa Numérico|	Até 10 caracteres|	Caso utilize antifraude sim
dddEntrega|	DDD do telefone de entrega|	Alfa Numérico|	Até 3 caracteres|	Caso utilize antifraude sim
ddiEntrega|	DDI do telefone de entrega|	Alfa Numérico|	Até 3 caracteres|	Caso utilize antifraude sim
codigoTipoTelefoneEntrega|	1 - Outros 2 - Residencial 3 - Comercial |	Numérico|	1 dígito|	Sim

*itemPedidoTransacaoWS*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
codigoProduto|	Código único que identifica cada produto|	Alfa Numérico|	20 caracteres|	Caso utilize antifraude sim
codigoCategoria|	Código que identifica categoria do produto|	Alfa Numérico|	20 caracteres|	Caso utilize antifraude sim
nomeProduto|	Nome do Produto	Alfa Numérico	|100 caracteres	|Caso utilize antifraude sim
quantidadeProduto|	Quantidade comprada do produto|	Numérico|	Até 8 dígitos|	Caso utilize antifraude sim
valorUnitarioProduto|	Valor unitário do produto. Deve ser enviado sem pontos ou vírgulas|	Numérico|	Até 10 dígitos	|Caso utilize antifraude sim
nomeCategoria|	Nome da categoria do produto	|Alfa Numérico|	100 caracteres|	Caso utilize antifraude sim

**RESPOSTA**

> Exemplo retorno pagamento com token:

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:Body>
      <ns2:pagamentoTransacaoCompletaResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <autorizacao>123456</autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>350</codigoFormaPagamento>
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <dataAprovacaoOperadora>24/05/2017</dataAprovacaOperadora>
            <mensagemVenda>Transacao capturada com sucesso</mensagemVenda>
            <numeroComprovanteVenda>1006993069181F841001</numeroComprovanteVenda>
            <numeroTransacao>10</numeroTransacao>
            <parcelas>1</parcelas>
            <statusTransacao>1</statusTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <urlPagamento>14132971582229c00506d-e84d-4526-b902-92190d5aa808<urlpagamento></urlpagamento>
            <valor>100</valor>
            <valorDesconto>0</valorDesconto>
         </return>
      </ns2:pagamentoTransacaoCompletaResponse>
   </soap:Body>
</soap:Envelope>
```

Campo | Descrição | Tipo | Tamanho 
------| ----------|------| --------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay | Numérico | 13 dígitos
codigoFormaPagamento | Código da forma de pagamento | Numérico | Até 3 dígitos
valor | Valor da transação.| Numérico | Até 10 dígitos
valorDesconto | Valor desconto | Numérico | Até 10 dígitos
taxaEmbarque | Valor taxa embarque | Numérico | Até 10 dígitos
parcelas | Quantidade de parcelas da transação | Numérico | Até 2 dígitos
urlPagamento | Url para autenticação em caso de cartão de débito |Alfa Numérico | Até 500 caracteres 
statusTransacao | Status atual da transação | Numérico | Até 2 dígitos
autorizacao | Número de autorização da adquirente | Numérico | Até 20 dígitos
codigoTransacaoOperadora | Código de retorno da adquirente | Numérico | Até 20 dígitos
dataAprovacaoOperadora | Data aprovação |Alfa Numérico | Até 10 dígitos
numeroComprovanteVenda | Número Comprovante de venda |Alfa Numérico | Até 20 dígitos
mensagemVenda | Mensagem de venda |Alfa Numérico | Até 50 dígitos

## Alterando cadastro

Este método permite a alteração dos dados já cadastrados na base do Gateway.

**REQUISIÇÃO**

<aside class="notice">Para pagamento com token, acione o método <code>alteraCadastraPagamentoOneClick</code></aside>

> Exemplo de alteração de cadastro
 
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
   <soapenv:Header/>
   <soapenv:Body>
      <pag:alteraCadastraPagamentoOneClick>
         <dadosOneClick>
            <codigoEstabelecimento>1318336765212</codigoEstabelecimento>
            <dataValidadeCartao>11/2018</dataValidadeCartao>
            <emailComprador>teste@suporte.com.br</emailComprador>
            <formaPagamento>380</formaPagamento>
            <nomeTitularCartaoCredito>teste SuperPay</nomeTitularCartaoCredito>
            <numeroCartaoCredito>1111222233334444</numeroCartaoCredito>
         </dadosOneClick>
         <token>1476210884949a25ed2d6-17cf-4ac4-af22-5c67d7907ef5</token>
         <usuario>superpay</usuario>
         <senha>superpay</senha>
      </pag:alteraCadastraPagamentoOneClick>
   </soapenv:Body>
</soapenv:Envelope>
```

Para autenticação, enviar `usuario` e `senha`:

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
codigoEstabelecimento|	Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway)|	Numérico|	13 dígitos|	Sim
numeroCartaoCredito|	Numero do cartão de crédito. Enviar sem espaços e virgúlas|	Numérico|	Até 22 caracteres|	Sim
dataValidadeCartao|	Data de validade do cartão. Formato mm/yyyy|	Alfa Numérico|	7 caracteres|	Sim
emailComprador|	Endereço de e-mail do comprador|	Alfa Numérico|	Até 100 caracteres|	Não
formaPagamento|	Código da forma de pagamento|	Numérico|	-|	Sim
token	|Token do cadastro que será alterado	|Alfa Numérico|	Até 100 caracteres|	Sim

**RESPOSTA**

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:Body>
      <ns2:alteraCadastraPagamentoOneClickResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
       <return>
            <codigoEstabelecimento>1010101010101010</codigoEstabelecimento>
            <codigoSeguranca/>
            <dataValidadeCartao>11/2018</dataValidadeCartao>
            <emailComprador>teste@suporte.com.br</emailComprador>
            <formaPagamento>380</formaPagamento>
            <nomeTitularCartaoCredito>Teste SuperPay</nomeTitularCartaoCredito>
            <numeroCartaoCredito>111122******4444</numeroCartaoCredito>
       </return>
      </ns2:alteraCadastraPagamentoOneClickResponse>
   </soap:body>
</soap:envelope>
```

Campo | Descrição | Tipo | Tamanho 
------| ----------|------| --------
codigoEstabelecimento|	Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway)|	Numérico|	13 dígitos
nomeTitularCartaoCredito| Nome do titular do cartão de crédito| Alfa Numérico|Até 25 caracteres
numeroCartaoCredito|	Numero do cartão de crédito. Enviar sem espaços e virgúlas|	Numérico|	Até 22 caracteres
dataValidadeCartao|	Data de validade do cartão. Formato mm/yyyy|	Alfa Numérico|	7 caracteres
emailComprador|	Endereço de e-mail do comprador|	Alfa Numérico|	Até 100 caracteres
formaPagamento|	Código da forma de pagamento|	Numérico|	-


# Consultas
## Consultando uma transação
Para receber novamente os dados de retorno de uma venda, realize uma consulta.

**REQUISIÇÃO**

<aside class="notice">Para consultas, acione o método <code>consultaTransacao</code></aside>

> Exemplo consulta:

```xml
<soapenv:envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
   <soapenv:header></soapenv:header>
   <soapenv:body>
      <pag:consultaTransacao>
         <consultaTransacaoWS>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <numeroTransacao>1</numeroTransacao>
         </consultaTransacaoWS>
         <usuario>superpay</usuario>
         <senha>superpay</senha>
      </pag:consultaTransacao>
   </soapenv:body>
</soapenv:envelope>
```
Para autenticação, enviar `usuario` e `senha`:

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------| ----------
numeroTransacao|	Código que identifica a transação dentro do SuperPay|	Numérico|	Até 8 dígitos| Sim
codigoEstabelecimento|	Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway)|	Numérico|	13 dígitos| Sim

**RESPOSTA**

> Exemplo retorno:

```xml
<soap:envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:body>
      <ns2:consultaTransacaoResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <autorizacao>123456</autorizacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>120</codigoFormaPagamento>
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <dataAprovacaoOperadora>24/05/2017</dataAprovacaOperadora>
            <mensagemVenda>Transacao cancelada com sucesso</mensagemVenda>
            <numeroComprovanteVenda>1006993069181F841001</numeroComprovanteVenda>
            <numeroTransacao>1</numeroTransacao>
            <parcelas>1</parcelas>
            <statusTransacao>13</statusTransacao>
            <taxaEmbarque>0</taxaEmbarque>
            <urlPagamento>14132971582229c00506d-e84d-4526-b902-92190d5aa808<urlpagamento></urlpagamento>
            <valor>200</valor>
            <valorDesconto>0</valorDesconto>
         </return>
      </ns2:consultaTransacaoResponse>
   </soap:body>
</soap:envelope>
```

Campo | Descrição | Tipo | Tamanho 
------| ----------|------| --------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay | Numérico | 13 dígitos
codigoFormaPagamento | Código da forma de pagamento | Numérico | Até 3 dígitos
valor | Valor da transação.| Numérico | Até 10 dígitos
valorDesconto | Valor desconto | Numérico | Até 10 dígitos
taxaEmbarque | Valor taxa embarque | Numérico | Até 10 dígitos
parcelas | Quantidade de parcelas da transação | Numérico | Até 2 dígitos
urlPagamento | Url para autenticação em caso de cartão de débito |Alfa Numérico | Até 500 caracteres 
statusTransacao | Status atual da transação | Numérico | Até 2 dígitos
autorizacao | Número de autorização da adquirente | Numérico | Até 20 dígitos
codigoTransacaoOperadora | Código de retorno da adquirente | Numérico | Até 20 dígitos
dataAprovacaoOperadora | Data aprovação |Alfa Numérico | Até 10 dígitos
numeroComprovanteVenda | Número Comprovante de venda |Alfa Numérico | Até 20 dígitos
mensagemVenda | Mensagem de venda |Alfa Numérico | Até 50 dígitos

## Consultando uma transação com retorno data de Antifraude
Método de consulta com estrutura parecida ao método comum, porém com um novo campo informando a data e horário do retorno da antifraude ao Gateway.

**Particularidades**

* Disponível apenas para estabelecimentos que possuem contrato com a Clear Sale nas modalidades Total/Total Garantido e Application;
* Para utilização solicitar ativação ao Suporte SuperPay.

**REQUISIÇÃO**

<aside class="notice">Para consultas, acione o método <code>consultaDataAntifraude</code></aside>

> Exemplo consulta:

```xml
<soapenv:envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
   <soapenv:header></soapenv:header>
   <soapenv:body>
      <pag:consultaTransacao>
         <consultaTransacaoWS>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <numeroTransacao>1</numeroTransacao>
         </consultaTransacaoWS>
         <usuario>superpay</usuario>
         <senha>superpay</senha>
      </pag:consultaTransacao>
   </soapenv:body>
</soapenv:envelope>
```
Para autenticação, enviar `usuario` e `senha`:

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------| ----------
numeroTransacao|	Código que identifica a transação dentro do SuperPay|	Numérico|	Até 8 dígitos| Sim
codigoEstabelecimento|	Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway)|	Numérico|	13 dígitos| Sim

**RESPOSTA**

> Exemplo de retorno:

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:Body>
      <ns2:consultaDataAntifraudeResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <numeroTransacao>6903</numeroTransacao>
            <codigoEstabelecimento>1000000000000</codigoEstabelecimento>
            <codigoFormaPagamento>170</codigoFormaPagamento>
            <valor>200</valor>
            <valorDesconto>0</valorDesconto>
            <taxaEmbarque>0</taxaEmbarque>
            <parcelas>1</parcelas>
            <urlPagamento>1495797252959508629a6-16f2-4311-a79b-e290b4d64237</urlPagamento>
            <statusTransacao>1</statusTransacao>
            <autorizacao>345051</autorizacao>
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <dataAprovacaoOperadora>2017-05-26 08:14:17</dataAprovacaoOperadora>
            <dataRetornoAntifraude>26/05/2017 17:36:41</dataRetornoAntifraude>
            <numeroComprovanteVenda>0526081417585</numeroComprovanteVenda>
            <mensagemVenda>Transação capturada com sucesso</mensagemVenda>
         </return>
      </ns2:consultaDataAntifraudeResponse>
   </soap:Body>
</soap:Envelope>
```
Campo | Descrição | Tipo | Tamanho 
------| ----------|------| --------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay | Numérico | 13 dígitos
codigoFormaPagamento | Código da forma de pagamento | Numérico | Até 3 dígitos
valor | Valor da transação.| Numérico | Até 10 dígitos
valorDesconto | Valor desconto | Numérico | Até 10 dígitos
taxaEmbarque | Valor taxa embarque | Numérico | Até 10 dígitos
parcelas | Quantidade de parcelas da transação | Numérico | Até 2 dígitos
urlPagamento | Url para autenticação em caso de cartão de débito |Alfa Numérico | Até 500 caracteres 
statusTransacao | Status atual da transação | Numérico | Até 2 dígitos
autorizacao | Número de autorização da adquirente | Numérico | Até 20 dígitos
codigoTransacaoOperadora | Código de retorno da adquirente | Numérico | Até 20 dígitos
dataAprovacaoOperadora | Data aprovação |Alfa Numérico | Até 10 dígitos
dataRetornoAntifraude | Data e hora de retorno da antifraude. **Caso o Gateway não tenha recebido o retorno ainda, este campo retornará em branco. | Alfa Numérico |Até 30 caracteres
numeroComprovanteVenda | Número Comprovante de venda |Alfa Numérico | Até 20 dígitos
mensagemVenda | Mensagem de venda |Alfa Numérico | Até 50 dígitos

## Consultando token
Para visualizar os dados de cartão cadastrados em um Token, basta acionar o método de consulta dentro do WS de estorno.

**REQUISIÇÃO**

<aside class="notice">Para consulta, acione o método <code>consultaDadosOneClick</code></aside>

> Exemplo de consulta:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:pag="http://pagamentos.webservices.superpay.ernet.com.br/">
   <soapenv:Header/>
   <soapenv:Body>
      <pag:consultaDadosOneClick>
         <token>1476210884949a25ed2d6-17cf-4ac4-af22-5c67d7907ef5</token>
         <usuario>superpay</usuario>
         <senha>superpay</senha>
      </pag:consultaDadosOneClick>
   </soapenv:Body>
</soapenv:Envelope>
```

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------| ----------
token|	Token a ser consultado|	Alfa Numérico|	100 caracteres| Sim

**RESPOSTA**

> Exemplo retorno:

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:Body>
      <ns2:consultaDadosOneClickResponse xmlns:ns2="http://pagamentos.webservices.superpay.ernet.com.br/">
         <return>
            <codigoEstabelecimento>1318336765212</codigoEstabelecimento>
            <codigoSeguranca/>
            <dataValidadeCartao>11/2016</dataValidadeCartao>
            <emailComprador>teste@suporte.com.br</emailComprador>
            <formaPagamento>121</formaPagamento>
            <numeroCartaoCredito>555555*****5555</numeroCartaoCredito>
         </return>
      </ns2:consultaDadosOneClickResponse>
   </soap:Body>
</soap:Envelope>
```

## Consulta recorrente

Consulta para receber informações da recorrência e da última cobrança realizada.

**REQUISIÇÃO**

<aside class="notice">Para consulta, acione o método <code>rec:consultaTransacaoRecorrenciaWS</code></aside>

> Exemplo consulta:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:rec="http://recorrencia.webservices.superpay.ernet.com.br/">
   <soapenv:Header/>
   <soapenv:Body>
     <rec:consultaTransacaoRecorrenciaWS>
         <recorrenciaConsultaWS>
            <estabelecimento>1000000000000</estabelecimento>
            <numeroRecorrencia>1</numeroRecorrencia>
         </recorrenciaConsultaWS>
         <usuario>
            <senha>superpay</senha>
            <usuario>superpay</usuario>
         </usuario>
      </rec:consultaTransacaoRecorrenciaWS>
   </soapenv:Body>
</soapenv:Envelope>
```
Para autenticação, enviar `usuario` e `senha`:

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------| ----------
numeroRecorrencia|	Número da Recorrência a ser consultado|	Numérico|	Até 8 dígitos| Sim
estabelecimento|	Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway)|	Numérico|	13 dígitos| Sim

**RESPOSTA**

> Exemplo retorno:

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:Body>
      <ns2:consultaTransacaoRecorrenciaWSResponse xmlns:ns2="http://recorrencia.webservices.superpay.ernet.com.br/">
         <return>
            <autorizacao>145681</autorizacao>
            <codigoFormapagamento>170</codigoFormapagamento>
            <codigoTransacaoOperadora>0</codigoTransacaoOperadora>
            <dataAprovacaoOperadora>2017-06-02 10:20:30</dataAprovacaoOperadora>
            <estabelecimento>1000000000000</estabelecimento>
            <mensagemVenda>Transação capturada com sucesso</mensagemVenda>
            <numeroCobrancaRestantes>0</numeroCobrancaRestantes>
            <numeroCobrancaTotal>Sem Limite</numeroCobrancaTotal>
            <numeroComprovanteVenda>0602102031041</numeroComprovanteVenda>
            <numeroPedido>1</numeroPedido>
            <numeroRecorrencia>10000001</numeroRecorrencia>
            <statusTransacao>1</statusTransacao>
            <valor>100</valor>
         </return>
      </ns2:consultaTransacaoRecorrenciaWSResponse>
   </soap:Body>
</soap:Envelope>
```

Campo | Descrição 
------| ----------
numeroRecorrencia|	Número da Recorrência
estabelecimento|	Código do estabelecimento, fornecido pelo SuperPay
valor|	Valor
statusTransacao	|Status da Parcela Recorrente
numeroPedido|	Número do Pedido
numeroCobrancaTotal|	Quantidade total de cobranças pedidas no cadastro. Caso não tenha sido pedido um valor limite, este campo retornará a mensagem: “Sem Limite"
numeroCobrancaRestantes|	Quantidade restante de cobranças a serem efetuadas.
autorizacao|	Código de autorização retornado pela operadora. Retornado apenas se alguma cobrança já ocorreu
codigoTransacaoOperadora|	Código de transação retornado pela operadora. Retornado apenas se alguma cobrança já ocorreu
dataAprovacaoOperadora|	Data de aprovação retornado pela operadora. Retornado apenas se alguma cobrança já ocorreu
numeroComprovanteVenda|	Número do comprovante de venda retornado pela operadora. Retornado apenas se alguma cobrança já ocorreu
mensagemVenda	|Mensagem de venda retornado pela operadora. Retornado apenas se alguma cobrança já ocorreu

# Post de Notificação
## Notificação vendas comuns

O sistema de campainha existe para notificar o estabelecimento sobre uma atualização de status na transação. Toda vez que ocorre qualquer alteração de status em uma transação, é feita uma chamada via <code>POST</code> ao campo “urlCampainha” (enviada como parâmetro junto da transação), essa chamada enviará alguns dados que identificarão a transação, e assim o estabelecimento saberá em qual transação houve uma mudança de status.

Importante lembrar que a chamada de campainha não informa qual o status atual da transação e apenas que houve uma alteração, sendo assim, o estabelecimento deve realizar uma consulta (através da função de [consultaTransacao](https://superpay.github.io/soap/#consultando-uma-transacao)) para verificar com mais detalhes a situação atual da transação.

*Caso a URL de campainha estiver em HTTPS, informar ao Suporte SuperPay, servicedesk@superpay.com.br*

> Exemplo de chamada:

```xml

POST HTTP
Content-Type: application/x-www-form-urlencoded
Content-Length: 125
numeroTransacao=123&codigoEstabelecimento=1000000000000&campoLivre=A&campoLivre2=B&campoLivre3=C&campoLivre4=D&campoLivre5=E

```

Detalhamento dos campos retornados:

Campo | Descrição | Tipo
------| ----------| -------
numeroTransacao|	Código que identifica a transação dentro do SuperPay|	Numérico
codigoEstabelecimento|	Código que identifica o estabelecimento dentro do SuperPay|	Numérico
campoLivre1|	Campo Livre 1|	Alfa Numérico
campoLivre2|	Campo Livre 2|	Alfa Numérico
campoLivre3|	Campo Livre 3|	Alfa Numérico
campoLivre4|	Campo Livre 4|	Alfa Numérico
campoLivre5|	Campo Livre 5|	Alfa Numérico

## Notificação cobrança recorrente

Neste fluxo de recorrência, o estabelecimento receberá a campainha informando qual recorrência houve a cobrança e, depois disto, deverá acionar a [consulta da recorrência](https://superpay.github.io/soap/#consulta-recorrente) para receber o número do pedido, e assim, acionar a [consulta da transação](https://superpay.github.io/soap/#consultando-uma-transacao) para recebimento do status.

*Caso a URL de campainha estiver em HTTPS, informar ao Suporte SuperPay, servicedesk@superpay.com.br*

> Exemplo de chamada:

```xml

POST HTTP
Content-Type: application/x-www-form-urlencoded
Content-Length: 125
numeroRecorrencia=1&codigoEstabelecimento=1000000000000&campoLivre=A&campoLivre2=B&campoLivre3=C&campoLivre4=D&campoLivre5=E

```

Detalhamento dos campos retornados:

Campo | Descrição | Tipo
------| ----------| -------
numeroTransacao|	Código que identifica a transação dentro do SuperPay|	Numérico
codigoEstabelecimento|	Código que identifica o estabelecimento dentro do SuperPay|	Numérico
campoLivre1|	Campo Livre 1|	Alfa Numérico
campoLivre2|	Campo Livre 2|	Alfa Numérico
campoLivre3|	Campo Livre 3|	Alfa Numérico
campoLivre4|	Campo Livre 4|	Alfa Numérico
campoLivre5|	Campo Livre 5|	Alfa Numérico

# Códigos da API
## Status de transação

Código | Nome | Descrição
------| ----------| -------
1 |Pago e Capturado| Transação está autorizada e confirmada na instituição financeira
2 |Pago e Não Capturado |Transação está apenas autorizada, aguardando confirmação (captura)
3| Não Pago |Transação negada pela instituição financeira
5|Transação em Andamento|Comum para pagamentos cartão redirect ou pagamentos com autenticação
8|Aguardando Pagamento|Comum para pagamentos com boletos e pedidos em reprocessamento
9|Falha na Operadora|Houve um problema no processamento com a adquirente
13|Cancelada|Transação cancelada na adquirente
14|Estornada|A venda foi estornada na adquirente
15|Em Análise de Fraude|A transação foi enviada para o sistema de análise de riscos. Status transitório
17|Recusado pelo AntiFraude|A transação foi negada pelo sistema análise de risco
18|Falha na Antifraude|Falha. Não foi possível enviar pedido para a análise de Risco, porém será reenviado
21|Boleto Pago a menor|O boleto foi pago com valor menor do emitido
22|Boleto Pago a maior|O boleto foi pago com valor maior do emitido
23|Estorno Parcial|A venda estonada na adquirente parcialmente
24|Estorno Não Autorizado|O Estorno não foi autorizado pela adquirente
25|Falha no estorno|Falha ao enviar estorno para a operadora
31|Transação já Paga|Transação já existente e finalizada na adquirente
40|Aguardando Cancelamento |Processo de cancelamento em andamento

## Forma de Pagamento
### Cartões

Código|Bandeira | Tecnologia | Adquirente| Modelo
------| ----------| -------| -------| -------
52|Checkout| Cielo e-Commerce| Cielo| Redirect
170|Visa|Cielo API 3.0|	Cielo	| WebService
171|MasterCard|Cielo API 3.0|	Cielo|	WebService
172|American Express|Cielo API 3.0|Cielo|	WebService
173|Elo|Cielo API 3.0|Cielo	|WebService
174|Diners|	Cielo API 3.0|	Cielo|WebService
175|Discover|Cielo API 3.0|	Cielo|	WebService
176|Aura|	Cielo API 3.0|	Cielo|	WebService
177|JCB|Cielo API 3.0|	Cielo|	WebService
178|Maestro|Cielo API 3.0|	Cielo|	WebService
179|Visa Electron|	Cielo API 3.0	|Cielo|	WebService
80|Visa| Komerci| Rede| Redirect
81|MasterCard| Komerci| Rede |Redirect
82|Diners| Komerci| Rede| Redirect
90|Visa |Komerci| Rede| WebService
91|MasterCard| Komerci| Rede| WebService
92|Diners| Komerci | Rede| WebService 
93|Hipercard |Komerci| Rede| WebService
94|Hiper| Komerci | Rede| WebService
204|Visa| ElavonWS| Elavon |WebService
205|MasterCard| ElavonWS| Elavon WebService
206|Diners |ElavonWS |Elavon| WebService
207|Discover| ElavonWS |Elavon| WebService
270|Visa| GetNetWS |GetNet| WebService 
271|MasterCard |GetNetWS| GetNet |WebService 
350|Visa| StoneWs |Stone| WebService
351|MasterCard| StoneWs| Stone| WebService
380|Visa |	BinWS|	Bin-FirstData|	WebService
381|MasterCard|	BinWS|	Bin-FirstData|	WebService
382|Cabal|	BinWS|	Bin-FirstData|	WebService

### Boletos e Transferências

**Modalidade Online**

Código|Banco | Modalidade | Tecnologia
------| ----------| -------| -------
16|Itaú |Transferência| Itaú Shopline 
17 |Itaú| Boleto Online| Itaú Shopline
18|Bradesco| Transferência| Bradesco Shopfacil
20|Banco do Brasil| Boleto| BBOnline 
21|Banco do Brasil|	Transferência	|BBOnline
23|Banrisul |Transferência |Banricompras
24|Banrisul|Parcelamento| Banricompras.com
25|Banrisul| Pré Datado |Banricompras.com
26|Banrisul| Boleto| Banricompras.com
105| Bradesco|	Boleto Online|	Bradesco ShopFácil


**Modalidade Offline**

Código|Banco | Modalidade | Tecnologia
------| ----------| -------| -------
28|Banco do Brasil| Boleto Offline |Banco do Brasil 
29|Itaú Boleto| Offline Itaú
30|Bradesco |Boleto Offline| Bradesco 
34|Caixa Econômica Federal| Boleto Offline| Caixa Econômica Federal
41|Santander| Boleto Offline |Santander
48|Banco do Brasil|	Boleto Offline com registro|	Banco do Brasil

### Intermediários Financeiros

Código|Nome
------| ----------

39|PagSeguro 
111|PayPal Nacional 
112|PayPal Internacional 
155|SafetyPay 

# Anexos
## Informações sobre os meios de pagamento
### Adquirente Cielo
#### Modalidade WebService

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

#### Checkout Cielo (Redirecionado)

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



download
modulo

