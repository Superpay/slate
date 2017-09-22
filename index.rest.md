---
title: API SuperPay

language_tabs: # must be one of https://git.io/vQNgJ
  - curl

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


**CARTÕES E REGRAS PARA TESTES**

*CIELO API 3.0*


**Aprovação**

Bandeira | Número do cartão | Código de segurança | Data de validade 
------| -------|------| -------
Qualquer | 0000000000000001 | 123 | qualquer posteiror ao dia corrente

**Negação**

Bandeira | Número do cartão | Código de segurança | Data de validade 
------| -------|------| -------
Qualquer | 0000000000000002 | 123| qualquer posteiror ao dia corrente


*ELAVON*

Para testar esta modalidade as seguintes regras abaixo devem ser utilizadas:

* Para aprovação da venda, o valor total da venda deve  ser em valor inteiro, ou seja, sem centavos;
* Para negação, basta enviar o valor total da venda quebrado, ou seja, com centavos;
* Como o ambiente de testes da Elavon é compartilhado, sugerimos gerar o número do pedido baseado em YYMMDDHHMMSS (ano, mês, dia, hora, minuto, segundo).


Bandeira | Número do cartão | Código de segurança | Data de validade 
------| -------|------| -------
Visa|	4444111122223333|	123|	Qualquer posterior ao dia corrente
MasterCard|	5431111111111111|	123|	Qualquer posterior ao dia corrente
Diners	|38000000000006|	123	|Qualquer posterior ao dia corrente
Discover|	6011111111111117|	123|	Qualquer posterior ao dia corrente


*REDE KOMERCI*

Para testes com esta operadora não há necessidade de contratação, basta solicitar ao Suporte SuperPay a configuração desta forma de pagamento.
Esta operadora não possui regras para aprovação ou negação dos pedidos, os retornos são aleatórios independente dos dados enviados.


*BIN - FIRST DATA*


* Utilizar os dados abaixo para aprovação;
* Para aprovação com a bandeira Cabal, o valor do pedido deve ser superior a R$100,00.


Bandeira | Número do cartão | Código de segurança | Data de validade 
------| -------|------| -------
Visa|	4488540010253330004|	123|	12/2026
MasterCard|	5547220000000102|	123|	12/2017
Cabal*|	6042030000204200 | 123|	09/2029


*GETNET*

Para aprovação das vendas, seguir tabelas abaixo:


Bandeira | Número do cartão | Código de segurança | Data de validade 
------| -------|------| -------
Visa|	4012001038166662| 456|	10/2017
MasterCard|	5453010000083303 |321|	10/2017


Categoria | Número de parcelas | Valor
------| -------|------
A vista	| 1 	nnn,nn
Parcelado|	2 |	nn6nn,02
Parcelado| 3	|nn6nn,03
Parcelado|	4 |	nn6nn,04


*BOLETOS OFFLINE*

Neste ambiente não configuramos dados reais para geração dos boletos por segurança. Caso queira utilizar esta forma de pagamento solicite ao Suporte SuperPay e o boleto será configurado com dados fictícios.

Obs: Neste ambiente os testes ocorrem apenas com boletos sem registros.


*INTERMEDIÁRIOS FINANCEIROS*

Para testes com este meio de pagamento é preciso possuir cadastro com a instituição financeira.

  
## Produção
Para receber suas credenciais do nosso ambiente de produção, basta acessar nosso site e realizar a contratação de um de nossos planos: [Planos SuperPay](http://www.superpay.com.br/planos)
Caso tenha dúvidas sobre, por gentileza entrar em contato com nossa equipe comercial através do email [comercial@superpay.com.br] ou pelo telefone 11 3544-0678

# Autenticação
Para autenticação conosco, é preciso enviar o usuário e senha no HEADER da requisição. Caso ainda não o possua, por gentileza enviar solicitação para nossa equipe de Suporte através do email [servicedesk@superpay.com.br].


```json

{"usuario":
{"login": "login", "senha": "senha" }
}

```

# EndPoint
## Sandbox
`https://homologacao.superpay.com.br/checkout/api/v2/transacao`

## Produção
`https://superpay2.superpay.com.br/checkout/api/v2/transacao`


# Pagamentos com Cartão de Crédito

## Criando uma transação simplificada

Estrutura e exemplo de uma transação simples para cartão de crédito. As funcionalidades, como Análise de Fraude, Recorrência e demais formas de pagamento precisam de uma estrutura mais completa para um perfeito funcionamento. Consulte as demais estruturas e exemplos desta documentação.



**REQUISIÇÃO**

<aside class="notice">
Para enviar a transação, utilize o método <code>POST</code>
</aside>

> Exemplo criação transação:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary
{
   "codigoEstabelecimento" : 1000000000000,
   "codigoFormaPagamento" : 170,
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
      "codigoProduto" : 1,
      "nomeProduto" : "Produto 1",
      "codigoCategoria" : 1,
      "nomeCategoria" : "categoria",
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

Para autenticação, enviar `login` e `senha` no HEADER:

Campo | Descrição 
------| ----------
login | Login do estabelecimento
senha | Senha do estabelecimento


Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway) | Numérico | 13 dígitos | Sim
codigoFormaPagamento | [Código da forma de pagamento](https://superpay.github.io/rest/#forma-de-pagamento) | Numérico | Até 3 dígitos | Sim
transacao | Nó reservado para informações da transação | - | - | -
dadosCartao | Nó reservado para dados de cartão | - | - | -
dadosCobranca | Nó reservado para informações dos dados de cobrança | - | - | -


*transacao*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos | Sim
valor | Valor da transação. Deve ser enviado sem pontos ou vírgulas | Numérico | Até 10 dígitos | Sim
parcelas | Quantidade de parcelas da transação. Verificar se forma de pagamento suporta parcelamento | Numérico | Até 2 dígitos | Sim
urlCampainha | URL será sempre acionada quando o status do pedido mudar. Deve estar preparada para receber dados de campainha | Alfa Numérico | Até 250 caracteres | Não
urlResultado | Para o modelo de pagamento redirect, O SuperPay redirecionará para essa URL | Alfa Numérico | Até 250 caracteres | Para pagamentos redirecionáveis é obrigatório


*dadosCartao*
Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
nomePortador | Nome do titular do cartão de crédito (Exatamente como escrito no cartão) | Alfa Numérico | Até 16 dígitos | Sim
numeroCartao | Numero do cartão de crédito, sem espaços ou traços | Numérico | Até 22 caracteres | Sim
codigoSeguranca | Código de segurança do cartão (campo não é armazenado pelo SuperPay) | Numérico | Até 4 caracteres | Sim
dataValidade | Data de validade do cartão. Formato mm/yyyy | Alfa Numérico | 7 caracteres | Sim


*dadosCobranca*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
nome | Nome do comprador| Alfa Numérico | Até 100 caracteres | Não
documento | Documento principal do comprado| Alfa Numérico | 30 caracteres | Não


**RESPOSTA**

> Exemplo retorno transação:

```curl

--header "Content-Type: application/json"
{
   "numeroTransacao": 123,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 170,
   "valor": 2000,
   "valorDesconto": 0,
   "parcelas": 1,
   "statusTransacao": 1,
   "autorizacao": "123456",
   "codigoTransacaoOperadora": "0",
   "dataAprovacaoOperadora": "24/05/2017",
   "numeroComprovanteVenda": "10069930690009F2122A",
   "nsu": "428706",
   "mensagemVenda": "Operation Success",
   "urlPagamento": "https://homologacao.superpay.com.br/checkout/PagamentoCielo/PagamentoCielo.do?cod=14956291484887110cf2a-9aeb-4b34-a869-1a61f0611b66",
   "cartoesUtilizados": ["000000*******0001"]
}

```


Campo | Descrição | Tipo | Tamanho 
------| ----------|------| --------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay | Numérico | 13 dígitos
codigoFormaPagamento | [Código da forma de pagamento](https://superpay.github.io/rest/#forma-de-pagamento) | Numérico | Até 3 dígitos
valor | Valor da transação.| Numérico | Até 10 dígitos
valorDesconto | Valor desconto | Numérico | Até 10 dígitos
taxaEmbarque | Valor taxa embarque | Numérico | Até 10 dígitos
parcelas | Quantidade de parcelas da transação | Numérico | Até 2 dígitos
urlPagamento | Para o modelo redirect. Essa será a URL de redirecionamento da operação |Alfa Numérico | Até 500 caracteres 
statusTransacao | [Status atual da transação](https://superpay.github.io/rest/#status-de-transacao) | Numérico | Até 2 dígitos
autorizacao | Código de autorização da adquirente | Numérico | Até 20 dígitos
codigoTransacaoOperadora | Código da transação na adquirente | Numérico | Até 20 dígitos
dataTprovacaoOperadora | Data que a transação foi enviada a adquirente |Alfa Numérico | Até 10 dícaracteresgitos
dataAprovacaoOperadora | Data de aprovação na adquirente |Alfa Numérico | Até 10 caracteres
numeroComprovanteVenda | Número do comprovante de venda |Alfa Numérico | Até 20 caracteres
nsu | Número do NSU da adquirente | Alfa Numérico | Até 20 caracteres
mensagemVenda | Mensagem de retorno da adquirente |Alfa Numérico | Até 50 caracteres
cartoesUtilizados | Cartões mascarados utilizados na transação | Alfa Numérico | Até 20 caracteres


## Criando uma transação com Análise de Fraude

A utilização desta estrutura é indicada para envio de pedidos com a forma de pagamento cartão de crédito, onde o estabelecimento possui contratação de umas das empresas de análise de risco integradas pelo SuperPay, sendo elas ClearSale (modalidades: Total/Total Garantido, Application, ID e Start) e FControl (modalidade Fila).

<aside class="warning">Contratação a parte com as empresas ClearSale e FControl</aside>


**REQUISIÇÃO**

<aside class="notice">
Para enviar a transação, acione o método <code>POST</code>
</aside>

> Exemplo criação transação:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary
{
   "codigoEstabelecimento" : 1000000000000,
   "codigoFormaPagamento" : 190,
   "transacao" : {
      "numeroTransacao" : 1234,
      "valor" : 2000,
      "valorDesconto" : 0,
      "parcelas" : 1,
      "urlCampainha" : "http://seusite.com.br/campainha",
      "ip" : "192.168.12.110",
      "idioma" : 1,
      "campoLivre1" : "",
      "campoLivre2" : "",
      "campoLivre3" : "",
      "campoLivre4" : "",
      "campoLivre5" : ""
   },
   "dadosCartao" : {
      "nomePortador" : "Teste Teste",
      "numeroCartao" : "4002479199570736",
      "codigoSeguranca" : "132",
      "dataValidade" : "12/2020"
   },
   "itensDoPedido" : [
  {
      "codigoProduto" : 1,
      "nomeProduto" : "Produto 1",
      "codigoCategoria" : 1,
      "nomeCategoria" : "categoria",
      "quantidadeProduto" : 1,
      "valorUnitarioProduto" : 2000
  }
   ],
   "dadosCobranca" : {
      "codigoCliente" : 1,
      "tipoCliente" : 1,
      "nome" : "Teste 123",
      "email" : "teste@teste.com",
      "dataNascimento" : "10/01/1975",
      "sexo" : "M",
      "documento" : "123.123.123-12",
      "endereco" : {
         "logradouro" : "Rua",
         "numero" : "123",
         "complemento" : "",
         "cep" : "12345-678",
         "bairro" : "Bairro",
         "cidade" : "Cidade",
         "estado" : "SP",
         "pais" : "BR"
        },
      "telefone" : [
        {
         "tipoTelefone" : "1",
         "ddi" : "55",
         "ddd" : "12",
         "telefone" : "1234-5678"
        }
      ]
   },
   "dadosEntrega" : { 
      "nome" : "Teste 123",
      "endereco" : {
         "logradouro" : "Rua",
         "numero" : "123",
         "complemento" : "",
         "cep" : "12345-678",
         "bairro" : "Bairro",
         "cidade" : "Cidade",
         "estado" : "SP",
         "pais" : "BR"
        },
      "telefone" : [
        {
         "tipoTelefone" : "1",
         "ddi" : "55",
         "ddd" : "12",
         "telefone" : "1234-5678"
        }
      ]
   }
}

```


Para autenticação, enviar `usuario` e `senha`:

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento


Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway) | Numérico | 13 dígitos | Sim
codigoFormaPagamento | [Código da forma de pagamento](https://superpay.github.io/rest/#forma-de-pagamento) | Numérico | Até 3 dígitos | Sim
transacao | Nó reservado para informações da transação | - | - | -
dadosCartao | Nó reservado para dados de cartão | - | - | -
itensDoPedido | Nó reservado para informações dos produtos | - | - | - 
dadosCobranca | Nó reservado para informações dos dados de cobrança | - | - | -
telefone |Nó reservado para informações de telefone | - | - | - 
dadosEntrega | Nó reservado para informações de dados de entrega | - | - | -


*transacao*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos | Sim
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway) | Numérico | 13 dígitos | Sim
codigoFormaPagamento | Código da forma de pagamento | Numérico | Até 3 dígitos | Sim
valor | Valor da transação. Deve ser enviado sem pontos ou vírgulas | Numérico | Até 10 dígitos | Sim
moeda |	Tipo da moeda. OBS: Disponível 'USD" apenas para PayPal Internacional |	Alfa Numérico |	Até 10 caracteres |	Não
tipoParcelamento |	Use "E" para estabelecimento, use "A" para administradora. Caso não for enviado será utilizado as configurações do seu estabelecimento |	Alfa Numérico |	1 caracter|	Não
valorDesconto |	Valor do desconto da transação. Campo apenas informativo |	Numérico	|Até 10 dígitos	|Sim
parcelas | Quantidade de parcelas da transação. Verificar se forma de pagamento suporta parcelamento | Numérico | Até 2 dígitos | Sim
urlCampainha | URL será sempre acionada quando o status do pedido mudar. Deve estar preparada para receber dados de campainha | Alfa Numérico | Até 250 caracteres | Não
urlResultado | Para o modelo de pagamento redirect, O SuperPay redirecionará para essa URL | Alfa Numérico | Até 250 caracteres | Para pagamentos redirecionáveis é obrigatório
ip	| Número do IP do usuário final/cliente. Formato xxx.xxx.xxx.xxx |	Alfa Numérico	|Até 15 caracteres	|Sim
idioma|	1 - Português 2 - Inglês 3 - Espanhol|	Numérico|	 - |	Sim
campoLivre1|	Campo Livre 1|	Alfa Numérico|	Até 16 caracteres|	Não
campoLivre2|	Campo Livre 2  (Envio do FingerPrint ClearSale)|	Alfa Numérico|	Até 16 caracteres|	Não
campoLivre3|	Campo Livre 3 (Envio do canal de venda ClearSale Total/Total Garantido e Application - solicitar ativação para Suporte SuperPay)|	Alfa Numérico|	Até 16 caracteres|	Não
campoLivre4|	Campo Livre 4|	Alfa Numérico|	Até 16 caracteres|	Não
campoLivre5|	Campo Livre 5|	Alfa Numérico|	Até 16 caracteres|	Não


*dadosCartao*
Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
nomePortador | Nome do titular do cartão de crédito (Exatamente como escrito no cartão) | Alfa Numérico | Até 16 dígitos | Sim
numeroCartao | Numero do cartão de crédito, sem espaços ou traços | Numérico | Até 22 caracteres | Sim
codigoSeguranca | Código de segurança do cartão (campo não é armazenado pelo SuperPay) | Numérico | Até 4 caracteres | Sim
dataValidade | Data de validade do cartão. Formato mm/yyyy | Alfa Numérico | 7 caracteres | Sim


*dadosCobranca*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
nome | Nome do comprador| Alfa Numérico | Até 100 caracteres | Sim
documento | Documento principal do comprado| Alfa Numérico | 30 caracteres | Sim
documento2 |Documento principal do comprado| Alfa Numérico | 30 caracteres | Não
email |	E-mail do comprador|	Alfa Numérico|	20 caracteres|	Sim
codigoCliente |	Código do Comprador|	Alfa Numérico|	20 caracteres|	Não
dataNascimento |	Data Nascimento Comprador|	Alfa Numérico|	10 caracteres	| Sim
sexo |	Sexo Comprador|	Alfa Numérico|	2 caracteres	|Não
tipoCliente|	Tipo do Cliente - 1 - Pessoa Física      2 - Pessoa Jurídica|	Numérico|	Até 8 dígitos|	Sim
endereco	|Nó reservado para dados de endereço do comprador|	 - 	| - |	 - 
telefone	|Nó reservado para dados de telefone do comprador	| -	| -|	 -

*itensDoPedido*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
codigoProduto|	Código único que identifica cada produto|	Alfa Numérico|20 caracteres|	Sim
codigoCategoria|	Código que identifica categoria do produto|	Alfa Numérico|	20 caracteres|	Sim
nomeProduto|	Nome do Produto|	Alfa Numérico|	100 caracteres	|Sim
quantidadeProduto	|Quantidade comprada do produto	|Numérico	|Até 8 dígitos|	Sim
valorUnitarioProduto	|Valor unitário do produto. Deve ser enviado sem pontos ou vírgulas|	Numérico|	Até 10 dígitos|	Sim
nomeCategoria	|Nome da categoria do produto|	Alfa Numérico	|100 caracteres| Sim


*endereco*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
logradouro |	Endereço do comprador|	Alfa Numérico|	Sim
numero|	Número do comprador|	Alfa Numérico|	Sim
bairro	|Bairro do comprador|	Alfa Numérico|	Sim
complemento|	Complemento do endereço	| Alfa Numérico |Não
cidade|	Cidade do comprador	|Alfa Numérico	|Sim
estado|	Estado do comprador|	Alfa Numérico |	Sim
cep	|CEP do comprador|	Alfa Numérico	|Sim
pais|	País do comprador|	Alfa Numérico	|Sim


*telefone*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
tipoTelefone|	1 = Outros / 2 = Residencial / 3 = Comercial / 4 = Recados / 5 = Cobrança / 6 = Temporário|	Numérico| Sim
ddi|	Código DDI do telefone|	Alfa Numérico	|Sim
ddd|	Código DDD do telefone|	Alfa Numérico|	Sim
telefone|	Número do telefone|	Alfa Numérico|	Sim


*dadosEntrega* 

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
nome	|Nome do comprador|	Alfa Numérico|	20 caracteres	|Não
email|	E-mail do comprador	|Alfa Numérico	|20 caracteres	|Não
endereco|	Nó reservado para dados de endereço do comprador|	 - |	 - |	 - 
telefone|	Nó reservado para dados de telefone do comprador|	-|	-|	-
|

**RESPOSTA**

> Exemplo retorno transação:

```curl

--header "Content-Type: application/json"
{  "numeroTransacao": 1234,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 190,
   "valor": 2000, 
   "valorDesconto": 0, 
   "parcelas": 1, 
   "statusTransacao": 1,
   "autorizacao": "12260",
   "codigoTransacaoOperadora": "0",
   "dataAprovacaoOperadora": "24/05/2017", 
   "numeroComprovanteVenda": "10117092009151900057", 
   "nsu": "428706",
   "mensagemVenda": "00 - Success",
   "urlPagamento": "https://homologacao.superpay.com.br/checkout/PagamentoCielo/PagamentoCielo.do?cod=14956291484887110cf2a-9aeb-4b34-a869-1a61f0611b66", 
   "cartoesUtilizados": ["400247******0736"]
}

```

Campo | Descrição | Tipo | Tamanho 
------| ----------|------| --------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay | Numérico | 13 dígitos
codigoFormaPagamento | [Código da forma de pagamento](https://superpay.github.io/rest/#forma-de-pagamento) | Numérico | Até 3 dígitos
valor | Valor da transação.| Numérico | Até 10 dígitos
valorDesconto | Valor desconto | Numérico | Até 10 dígitos
taxaEmbarque | Valor taxa embarque | Numérico | Até 10 dígitos
parcelas | Quantidade de parcelas da transação | Numérico | Até 2 dígitos
urlPagamento | Para o modelo redirect. Essa será a URL de redirecionamento da operação |Alfa Numérico | Até 500 caracteres 
statusTransacao | [Status atual da transação](https://superpay.github.io/rest/#status-de-transacao) | Numérico | Até 2 dígitos
autorizacao | Código de autorização da adquirente | Numérico | Até 20 dígitos
codigoTransacaoOperadora | Código da transação na adquirente | Numérico | Até 20 dígitos
dataAprovacaoOperadora | Data de aprovação na adquirente |Alfa Numérico | Até 10 dígitos
numeroComprovanteVenda | Número do comprovante de venda |Alfa Numérico | Até 20 dígitos
nsu | Número de NSU | Alfa Numérico | Até 10 dígitos
mensagemVenda | Mensagem de retorno da adquirente |Alfa Numérico | Até 50 dígitos
cartoesUtilizados | Número de cartão truncado utilizado na transação | Alfa Numérico | Até 20 dígitos


## Capturando uma transação
Através desta funcionalidade é possível confirmar uma pré autorização na adquirente, assim o consumidor receberá a cobrança em seu cartão e gerará crédito ao lojista.
Os estabelecimentos com captura manual deverão acionar o método de captura em até 5 dias da criação da venda, pois após este período a adquirente cancelará automaticamente a pré autorização.

**REQUISIÇÃO**

<aside class="notice">
Para enviar a transação, acione o método <code>POST</code>
</aside>

> Exemplo captura de transação:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao/10000000000000/1234/capturar
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary

```

Para autenticação, enviar `usuario` e `senha`:

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento

Campo | Descrição 
------| ---------- 
numeroTransacao |	Código que identifica a transação dentro do SuperPay
codigoEstabelecimento|	Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway)
acao|	Ação a ser realizada. Enviar "capturar" 

**RESPOSTA**


> Exemplo retorno da captura de transação:

```curl

--header "Content-Type: application/json"
{
   "numeroTransacao": 1234,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 170,
   "valor": 2000,
   "valorDesconto": 0,
   "parcelas": 1,
   "statusTransacao": 1,
   "autorizacao": "123456",
   "codigoTransacaoOperadora": "0",
   "dataAprovacaoOperadora": "24/05/2017",
   "numeroComprovanteVenda": "10069930690009F2122A",
   "nsu": "428706",
   "mensagemVenda": "Operation Success",
   "urlPagamento": "https://homologacao.superpay.com.br/checkout/PagamentoCielo/PagamentoCielo.do?cod=14956291484887110cf2a-9aeb-4b34-a869-1a61f0611b66",
   "cartoesUtilizados": ["000000*******0001"]
}

```

Campo | Descrição | Tipo | Tamanho 
------| ----------|------| --------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay | Numérico | 13 dígitos
codigoFormaPagamento | [Código da forma de pagamento](https://superpay.github.io/rest/#forma-de-pagamento) | Numérico | Até 3 dígitos
valor | Valor da transação.| Numérico | Até 10 dígitos
valorDesconto | Valor desconto | Numérico | Até 10 dígitos
taxaEmbarque | Valor taxa embarque | Numérico | Até 10 dígitos
parcelas | Quantidade de parcelas da transação | Numérico | Até 2 dígitos
urlPagamento | Para o modelo redirect. Essa será a URL de redirecionamento da operação |Alfa Numérico | Até 500 caracteres 
statusTransacao | [Status atual da transação](https://superpay.github.io/rest/#status-de-transacao) | Numérico | Até 2 dígitos
autorizacao | Código de autorização da adquirente | Numérico | Até 20 dígitos
codigoTransacaoOperadora | Código da transação na adquirente | Numérico | Até 20 dígitos
dataAprovacaoOperadora | Data de aprovação na adquirente |Alfa Numérico | Até 10 dígitos
numeroComprovanteVenda | Número do comprovante de venda |Alfa Numérico | Até 20 dígitos
mensagemVenda | Mensagem de retorno da adquirente |Alfa Numérico | Até 50 dígitos
cartoesUtilizados | Número de cartão truncado utilizado na transação | Alfa Numérico | Até 20 dígitos


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
Para enviar a transação, acione o método <code>POST</code>
</aside>

> Exemplo cancelamento de transação:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao/10000000000000/1234/cancelar
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary

```

Para autenticação, enviar `usuario` e `senha`:

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento


Campo | Descrição 
------| ----------

numeroTransacao |	Código que identifica a transação dentro do SuperPay
codigoEstabelecimento|	Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway)
acao|	Ação a ser realizada. Enviar "cancelar" 


**RESPOSTA**

> Exemplo retorno de cancelamento de transação:

```curl

--header "Content-Type: application/json"
{
   "numeroTransacao": 1234,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 170,
   "valor": 2000,
   "valorDesconto": 0,
   "parcelas": 1,
   "statusTransacao": 13,
   "autorizacao": "123456",
   "codigoTransacaoOperadora": "0",
   "dataAprovacaoOperadora": "24/05/2017",
   "numeroComprovanteVenda": "10069930690009F2122A",
   "nsu": "428706",
   "mensagemVenda": "Operation Success",
   "urlPagamento": "https://homologacao.superpay.com.br/checkout/PagamentoCielo/PagamentoCielo.do?cod=14956291484887110cf2a-9aeb-4b34-a869-1a61f0611b66",
   "cartoesUtilizados": ["000000*******0001"]
}

```

Campo | Descrição | Tipo | Tamanho 
------| ----------|------| --------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay | Numérico | 13 dígitos
codigoFormaPagamento | [Código da forma de pagamento](https://superpay.github.io/rest/#forma-de-pagamento) | Numérico | Até 3 dígitos
valor | Valor da transação.| Numérico | Até 10 dígitos
valorDesconto | Valor desconto | Numérico | Até 10 dígitos
taxaEmbarque | Valor taxa embarque | Numérico | Até 10 dígitos
parcelas | Quantidade de parcelas da transação | Numérico | Até 2 dígitos
urlPagamento | Para o modelo redirect. Essa será a URL de redirecionamento da operação |Alfa Numérico | Até 500 caracteres 
statusTransacao | [Status atual da transação](https://superpay.github.io/rest/#status-de-transacao) | Numérico | Até 2 dígitos
autorizacao | Código de autorização da adquirente | Numérico | Até 20 dígitos
codigoTransacaoOperadora | Código da transação na adquirente | Numérico | Até 20 dígitos
dataAprovacaoOperadora | Data de aprovação na adquirente |Alfa Numérico | Até 10 dígitos
numeroComprovanteVenda | Número do comprovante de venda |Alfa Numérico | Até 20 dígitos
mensagemVenda | Mensagem de retorno da adquirente |Alfa Numérico | Até 50 dígitos
cartoesUtilizados | Número de cartão truncado utilizado na transação | Alfa Numérico | Até 20 dígitos


# Pagamentos com Cartão de Débito
## Criando uma transação simplificada

Para pagamentos com cartão de débito é obrigatório a etapa de autenticação do consumidor na página do banco emissor de seu cartão. Sendo assim, após o envio dos dados da transação, o eCommerce deverá redirecionar o consumidor para o campo <code>urlPagamento</code> retornado pelo Gateway.


**Particulariedades**

* Não disponível no fluxo de Antifraude;
* Pagamento com redirecionamento;
* Importante a utilização do recurso de Campainha.


**REQUISIÇÃO**

<aside class="notice">
Para enviar a transação, acione o método <code>POST</code>
</aside>

> Exemplo criação transação:

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
      "urlCampainha" : "http://seusite.com.br/campainha",
      "urlResultado" : "http://seusite.com.br/retorno"
   },
   "dadosCartao" : {
      "nomePortador" : "Teste Teste",
      "numeroCartao" : "0000000000000001",
      "codigoSeguranca" : "123",
      "dataValidade" : "12/2017"
   },
   "itensDoPedido" : [
  {
      "codigoProduto" : 1,
      "nomeProduto" : "Produto 1",
      "codigoCategoria" : 1,
      "nomeCategoria" : "categoria",
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

Para autenticação, enviar `login` e `senha` no HEADER:

Campo | Descrição 
------| ----------
login | Login do estabelecimento
senha | Senha do estabelecimento


Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway) | Numérico | 13 dígitos | Sim
codigoFormaPagamento | [Código da forma de pagamento](https://superpay.github.io/rest/#forma-de-pagamento) | Numérico | Até 3 dígitos | Sim
transacao | Nó reservado para informações da transação | - | - | -
dadosCartao | Nó reservado para dados de cartão | - | - | -
dadosCobranca | Nó reservado para informações dos dados de cobrança | - | - | -


*transacao*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos | Sim
valor | Valor da transação. Deve ser enviado sem pontos ou vírgulas | Numérico | Até 10 dígitos | Sim
parcelas | Quantidade de parcelas da transação. Verificar se forma de pagamento suporta parcelamento | Numérico | Até 2 dígitos | Sim
urlCampainha | URL será sempre acionada quando o status do pedido mudar. Deve estar preparada para receber dados de campainha | Alfa Numérico | Até 250 caracteres | Sim
urlResultado | O SuperPay redirecionará para essa URL na finalização do pagamento| Alfa Numérico | Até 250 caracteres | Sim


*dadosCartao*
Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
nomePortador | Nome do titular do cartão de crédito (Exatamente como escrito no cartão) | Alfa Numérico | Até 16 dígitos | Sim
numeroCartao | Numero do cartão de crédito, sem espaços ou traços | Numérico | Até 22 caracteres | Sim
codigoSeguranca | Código de segurança do cartão (campo não é armazenado pelo SuperPay) | Numérico | Até 4 caracteres | Sim
dataValidade | Data de validade do cartão. Formato mm/yyyy | Alfa Numérico | 7 caracteres | Sim


*dadosCobranca*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
nome | Nome do comprador| Alfa Numérico | Até 100 caracteres | Não
documento | Documento principal do comprado| Alfa Numérico | 30 caracteres | Não


**RESPOSTA**

> Exemplo retorno transação:

```curl

--header "Content-Type: application/json"
{
   "numeroTransacao": 123,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 170,
   "valor": 2000,
   "valorDesconto": 0,
   "parcelas": 1,
   "statusTransacao": 1,
   "autorizacao": "123456",
   "codigoTransacaoOperadora": "0",
   "dataAprovacaoOperadora": "24/05/2017",
   "numeroComprovanteVenda": "10069930690009F2122A",
   "nsu": "428706",
   "mensagemVenda": "Operation Success",
   "urlPagamento": "https://homologacao.superpay.com.br/checkout/PagamentoCielo/PagamentoVisaElectron.do?cod=14132971582229c00506d-e84d-4526-b902-92190d5aa808",
   "cartoesUtilizados": ["000000*******0001"]
}

```


Campo | Descrição | Tipo | Tamanho 
------| ----------|------| --------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay | Numérico | 13 dígitos
codigoFormaPagamento | [Código da forma de pagamento](https://superpay.github.io/rest/#forma-de-pagamento) | Numérico | Até 3 dígitos
valor | Valor da transação.| Numérico | Até 10 dígitos
valorDesconto | Valor desconto | Numérico | Até 10 dígitos
taxaEmbarque | Valor taxa embarque | Numérico | Até 10 dígitos
parcelas | Quantidade de parcelas da transação | Numérico | Até 2 dígitos
urlPagamento | URL para redirecionar o consumidor para autenticação |Alfa Numérico | Até 500 caracteres 
statusTransacao | [Status atual da transação](https://superpay.github.io/rest/#status-de-transacao) | Numérico | Até 2 dígitos
autorizacao | Código de autorização da adquirente | Numérico | Até 20 dígitos
codigoTransacaoOperadora | Código da transação na adquirente | Numérico | Até 20 dígitos
dataTprovacaoOperadora | Data que a transação foi enviada a adquirente |Alfa Numérico | Até 10 dícaracteresgitos
dataAprovacaoOperadora | Data de aprovação na adquirente |Alfa Numérico | Até 10 caracteres
numeroComprovanteVenda | Número do comprovante de venda |Alfa Numérico | Até 20 caracteres
nsu | Número do NSU da adquirente | Alfa Numérico | Até 20 caracteres
mensagemVenda | Mensagem de retorno da adquirente |Alfa Numérico | Até 50 caracteres
cartoesUtilizados | Cartões mascarados utilizados na transação | Alfa Numérico | Até 20 caracteres

# Pagamentos com Boleto Bancário
## Criando uma transação

Estrutura para geração de boletos com carteiras sem ou com registro.

**Particulariedades**

* O campo `<estado>` deve ser preenchido apenas com a sigla do Estado;
* O campo `<numeroTransacao>` deve conter até 8 dígitos;
* Caso o campo `<dataVencimentoBoleto>` não for enviado, será utilizado os dias de vencimento configurado internamente no Gateway;
* Para boletos com carteira registrada (contratação a parte), o status retornado pelo SuperPay no primeiro momento será 5 (transação em andamento), enquanto para boletos sem registros é retornado 8 (aguardando pagamento);
* Importante a utilização do recurso de Campainha, para atualização dos pedidos no Ecommerce;
* A conciliação de boletos não é realizada automaticamente, funcionalidade deve ser contratada a parte com o comercial@superpay.com.br.

**REQUISIÇÃO**

<aside class="notice">
Para enviar a transação, acione o método <code>POST</code>
</aside>

> Exemplo criação transação:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary
{
   "codigoEstabelecimento" : 1000000000000,
   "codigoFormaPagamento" : 29,
   "transacao" : {
      "numeroTransacao" : 1234,
      "valor" : 2000,
      "valorDesconto" : 0,
      "parcelas" : 1,
      "urlCampainha" : "http://seusite.com.br/campainha",
      "urlResultado" : "http://seusite.com.br/retorno",
      "ip" : "192.168.12.110",
      "idioma" : 1,
      "dataVencimentoBoleto":"13/12/2017"
   },
   "itensDoPedido" : [
  {
      "codigoProduto" : 1,
      "nomeProduto" : "Produto 1",
      "codigoCategoria" : 1,
      "nomeCategoria" : "categoria",
      "quantidadeProduto" : 1,
      "valorUnitarioProduto" : 2000
  }
   ],
   "dadosCobranca" : {
      "codigoCliente" : 1,
      "tipoCliente" : 1,
      "nome" : "Teste 123",
      "email" : "teste@teste.com",
      "dataNascimento" : "10/01/1975",
      "sexo" : "M",
      "documento" : "123.123.123-12",
      "endereco" : {
         "logradouro" : "Rua",
         "numero" : "123",
         "complemento" : "",
         "cep" : "12345-678",
         "bairro" : "Bairro",
         "cidade" : "Cidade",
         "estado" : "SP",
         "pais" : "BR"
        },
      "telefone" : [
        {
         "tipoTelefone" : "1",
         "ddi" : "55",
         "ddd" : "12",
         "telefone" : "1234-5678"
        }
      ]
   },
   "dadosEntrega" : { 
      "nome" : "Teste 123",
      "endereco" : {
         "logradouro" : "Rua",
         "numero" : "123",
         "complemento" : "",
         "cep" : "12345-678",
         "bairro" : "Bairro",
         "cidade" : "Cidade",
         "estado" : "SP",
         "pais" : "BR"
        },
      "telefone" : [
        {
         "tipoTelefone" : "1",
         "ddi" : "55",
         "ddd" : "12",
         "telefone" : "1234-5678"
        }
      ]
   }
}

```

Para autenticação, enviar `usuario` e `senha`:

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento


Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway) | Numérico | 13 dígitos | Sim
codigoFormaPagamento | [Código da forma de pagamento](https://superpay.github.io/rest/#forma-de-pagamento) | Numérico | Até 3 dígitos | Sim
transacao | Nó reservado para informações da transação | - | - | -
dadosCobranca | Nó reservado para informações dos dados de cobrança | - | - | -
telefone |Nó reservado para informações de telefone | - | - | - 
dadosEntrega | Nó reservado para informações de dados de entrega | - | - | -


*transacao*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos | Sim
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway) | Numérico | 13 dígitos | Sim
codigoFormaPagamento | [Código da forma de pagamento](https://superpay.github.io/rest/#forma-de-pagamento) | Numérico | Até 3 dígitos | Sim
valor | Valor da transação. Deve ser enviado sem pontos ou vírgulas | Numérico | Até 10 dígitos | Sim
valorDesconto |	Valor do desconto da transação. Campo apenas informativo |	Numérico	|Até 10 dígitos	|Sim
urlCampainha | URL será sempre acionada quando o status do pedido mudar. Deve estar preparada para receber dados de campainha | Alfa Numérico | Até 250 caracteres | Não
dataVencimentoBoleto | Data de vencimento do boleto, se não enviado será utilizado os dias da configuração |Alfa Numérico| Até 10 caracteres | Não
urlResultado | Para o modelo de pagamento redirect, O SuperPay redirecionará para essa URL | Alfa Numérico | Até 250 caracteres | Para pagamentos redirecionáveis é obrigatório
ip	| Número do IP do usuário final/cliente. Formato xxx.xxx.xxx.xxx |	Alfa Numérico	|Até 15 caracteres	|Não
idioma|	1 - Português 2 - Inglês 3 - Espanhol|	Numérico|	 - |	Sim
campoLivre1|	Campo Livre 1|	Alfa Numérico|	Até 16 caracteres|	Não
campoLivre2|	Campo Livre 2  |Alfa Numérico|	Até 16 caracteres|	Não
campoLivre3|	Campo Livre 3 |	Alfa Numérico|	Até 16 caracteres|	Não
campoLivre4|	Campo Livre 4|	Alfa Numérico|	Até 16 caracteres|	Não
campoLivre5|	Campo Livre 5|	Alfa Numérico|	Até 16 caracteres|	Não


*dadosCobranca*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
nome | Nome do comprador| Alfa Numérico | Até 100 caracteres | Sim
documento | Documento principal do comprado| Alfa Numérico | 30 caracteres | Sim
documento2 |Documento principal do comprado| Alfa Numérico | 30 caracteres | Não
email |	E-mail do comprador|	Alfa Numérico|	20 caracteres|	Sim
codigoCliente |	Código do Comprador|	Alfa Numérico|	20 caracteres|	Não
dataNascimento |	Data Nascimento Comprador|	Alfa Numérico|	10 caracteres	| Não
sexo |	Sexo Comprador|	Alfa Numérico|	2 caracteres	|Não
tipoCliente|	Tipo do Cliente - 1 - Pessoa Física      2 - Pessoa Jurídica|	Numérico|	Até 8 dígitos|	Sim
endereco	|Nó reservado para dados de endereço do comprador|	 - 	| - |	 - 
telefone	|Nó reservado para dados de telefone do comprador	| -	| -|	 -


*endereco*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
logradouro |	Endereço do comprador|	Alfa Numérico|	Sim
numero|	Número do comprador|	Alfa Numérico|	Sim
bairro	|Bairro do comprador|	Alfa Numérico|	Sim
complemento|	Complemento do endereço	| Alfa Numérico |Não
cidade|	Cidade do comprador	|Alfa Numérico	|Sim
estado|	Estado do comprador|	Alfa Numérico |	Sim
cep	|CEP do comprador|	Alfa Numérico	|Sim
pais|	País do comprador|	Alfa Numérico	|Não


*telefone*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
tipoTelefone|	1 = Outros / 2 = Residencial / 3 = Comercial / 4 = Recados / 5 = Cobrança / 6 = Temporário|	Numérico| Sim
ddi|	Código DDI do telefone|	Alfa Numérico	|Não
ddd|	Código DDD do telefone|	Alfa Numérico|	Não
telefone|	Número do telefone|	Alfa Numérico|	Não


*dadosEntrega* 

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
nome	|Nome do comprador|	Alfa Numérico|	20 caracteres	|Não
email|	E-mail do comprador	|Alfa Numérico	|20 caracteres	|Não
endereco|	Nó reservado para dados de endereço do comprador|	 - |	 - |	 - 
telefone|	Nó reservado para dados de telefone do comprador|	-|	-|	-



**RESPOSTA**
Para geração do boleto o eCommerce deverá redirecionar o consumidor para a URl retornada no campo <urlPagamento>

> Exemplo retorno criação boleto:

```curl

--header "Content-Type: application/json"
{  "numeroTransacao": 1234,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 29,
   "valor": 2000, 
   "valorDesconto": 0, 
   "parcelas": 1, 
   "statusTransacao": 8,
   "autorizacao": "0",
   "codigoTransacaoOperadora": "0",
   "dataAprovacaoOperadora": "", 
   "numeroComprovanteVenda": "", 
   "nsu": "",
   "mensagemVenda": "",
   "urlPagamento": "https://homologacao.superpay.com.br/checkout/Gerarboleto.do?cod=14956296486904d8312c6-d57a-499e-b53b-504047402e45"
}

```

Campo | Descrição | Tipo | Tamanho 
------| ----------|------| --------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay | Numérico | 13 dígitos
codigoFormaPagamento | [Código da forma de pagamento](https://superpay.github.io/rest/#forma-de-pagamento) | Numérico | Até 3 dígitos
valor | Valor da transação.| Numérico | Até 10 dígitos
valorDesconto | Valor desconto | Numérico | Até 10 dígitos
taxaEmbarque | Valor taxa embarque | Numérico | Até 10 dígitos
parcelas | Quantidade de parcelas da transação | Numérico | Até 2 dígitos
urlPagamento | Url para geração do boleto |Alfa Numérico | Até 500 caracteres 
statusTransacao | [Status atual da transação](https://superpay.github.io/rest/#status-de-transacao) | Numérico | Até 2 dígitos
autorizacao | Retornado "0" para boletos | Numérico | Até 20 dígitos
codigoTransacaoOperadora | Retornado "0" para boletos | Numérico | Até 20 dígitos
dataAprovacaoOperadora | Retornado em branco para boletos |Alfa Numérico | Até 10 dígitos
numeroComprovanteVenda | Retornado em branco para boletos |Alfa Numérico | Até 20 dígitos
mensagemVenda | Retornado em branco para boletos |Alfa Numérico | Até 50 dígitos

# Pagamentos com Transferência Eletrônica
## Criando uma transação

Estrutura para criação de transferência eletrônica.

**Particulariedades**

* O campo `<estado>` deve ser preenchido apenas com a sigla do Estado;
* O campo `<numeroTransacao>` deve conter até 8 dígitos;
* Importante a utilização do recurso de Campainha, para atualização dos pedidos no Ecommerce;


**REQUISIÇÃO**

<aside class="notice">
Para enviar a transação, acione o método <code>POST</code>
</aside>

> Exemplo criação transação:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary
{
   "codigoEstabelecimento" : 1000000000000,
   "codigoFormaPagamento" : 16,
   "transacao" : {
      "numeroTransacao" : 1234,
      "valor" : 2000,
      "valorDesconto" : 0,
      "parcelas" : 1,
      "urlCampainha" : "http://seusite.com.br/campainha",
      "urlResultado" : "http://seusite.com.br/retorno",
      "ip" : "192.168.12.110",
      "idioma" : 1
   },
   "itensDoPedido" : [
  {
      "codigoProduto" : 1,
      "nomeProduto" : "Produto 1",
      "codigoCategoria" : 1,
      "nomeCategoria" : "categoria",
      "quantidadeProduto" : 1,
      "valorUnitarioProduto" : 2000
  }
   ],
   "dadosCobranca" : {
      "codigoCliente" : 1,
      "tipoCliente" : 1,
      "nome" : "Teste 123",
      "email" : "teste@teste.com",
      "dataNascimento" : "10/01/1975",
      "sexo" : "M",
      "documento" : "123.123.123-12",
      "endereco" : {
         "logradouro" : "Rua",
         "numero" : "123",
         "complemento" : "",
         "cep" : "12345-678",
         "bairro" : "Bairro",
         "cidade" : "Cidade",
         "estado" : "SP",
         "pais" : "BR"
        },
      "telefone" : [
        {
         "tipoTelefone" : "1",
         "ddi" : "55",
         "ddd" : "12",
         "telefone" : "1234-5678"
        }
      ]
   },
   "dadosEntrega" : { 
      "nome" : "Teste 123",
      "endereco" : {
         "logradouro" : "Rua",
         "numero" : "123",
         "complemento" : "",
         "cep" : "12345-678",
         "bairro" : "Bairro",
         "cidade" : "Cidade",
         "estado" : "SP",
         "pais" : "BR"
        },
      "telefone" : [
        {
         "tipoTelefone" : "1",
         "ddi" : "55",
         "ddd" : "12",
         "telefone" : "1234-5678"
        }
      ]
   }
}

```

Para autenticação, enviar `usuario` e `senha`:

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento


Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway) | Numérico | 13 dígitos | Sim
codigoFormaPagamento | [Código da forma de pagamento](https://superpay.github.io/rest/#forma-de-pagamento) | Numérico | Até 3 dígitos | Sim
transacao | Nó reservado para informações da transação | - | - | -
itensDoPedido | Nó reservado para informações dos produtos | - | - | - 
dadosCobranca | Nó reservado para informações dos dados de cobrança | - | - | -
telefone |Nó reservado para informações de telefone | - | - | - 
dadosEntrega | Nó reservado para informações de dados de entrega | - | - | -


*transacao*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos | Sim
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway) | Numérico | 13 dígitos | Sim
codigoFormaPagamento | Código da forma de pagamento | Numérico | Até 3 dígitos | Sim
valor | Valor da transação. Deve ser enviado sem pontos ou vírgulas | Numérico | Até 10 dígitos | Sim
valorDesconto |	Valor do desconto da transação. Campo apenas informativo |	Numérico	|Até 10 dígitos	|Sim
urlCampainha | URL será sempre acionada quando o status do pedido mudar. Deve estar preparada para receber dados de campainha | Alfa Numérico | Até 250 caracteres | Não
urlResultado | Para o modelo de pagamento redirect, O SuperPay redirecionará para essa URL | Alfa Numérico | Até 250 caracteres | Para pagamentos redirecionáveis é obrigatório
ip	| Número do IP do usuário final/cliente. Formato xxx.xxx.xxx.xxx |	Alfa Numérico	|Até 15 caracteres	|Não
idioma|	1 - Português 2 - Inglês 3 - Espanhol|	Numérico|	 - |	Sim


*dadosCobranca*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
nome | Nome do comprador| Alfa Numérico | Até 100 caracteres | Sim
documento | Documento principal do comprado| Alfa Numérico | 30 caracteres | Sim
documento2 |Documento principal do comprado| Alfa Numérico | 30 caracteres | Não
email |	E-mail do comprador|	Alfa Numérico|	20 caracteres|	Sim
codigoCliente |	Código do Comprador|	Alfa Numérico|	20 caracteres|	Não
dataNascimento |	Data Nascimento Comprador|	Alfa Numérico|	10 caracteres	| Não
sexo |	Sexo Comprador|	Alfa Numérico|	2 caracteres	|Não
tipoCliente|	Tipo do Cliente - 1 - Pessoa Física      2 - Pessoa Jurídica|	Numérico|	Até 8 dígitos|	Sim
endereco	|Nó reservado para dados de endereço do comprador|	 - 	| - |	 - 
telefone	|Nó reservado para dados de telefone do comprador	| -	| -|	 -


*endereco*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
logradouro |	Endereço do comprador|	Alfa Numérico|	Sim
numero|	Número do comprador|	Alfa Numérico|	Sim
bairro	|Bairro do comprador|	Alfa Numérico|	Sim
complemento|	Complemento do endereço	| Alfa Numérico |Não
cidade|	Cidade do comprador	|Alfa Numérico	|Sim
estado|	Estado do comprador|	Alfa Numérico |	Sim
cep	|CEP do comprador|	Alfa Numérico	|Sim
pais|	País do comprador|	Alfa Numérico	|Não


*telefone*

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
tipoTelefone|	1 = Outros / 2 = Residencial / 3 = Comercial / 4 = Recados / 5 = Cobrança / 6 = Temporário|	Numérico| Sim
ddi|	Código DDI do telefone|	Alfa Numérico	|Não
ddd|	Código DDD do telefone|	Alfa Numérico|	Não
telefone|	Número do telefone|	Alfa Numérico|	Não


*dadosEntrega* 

Campo | Descrição | Tipo | Tamanho | Obrigatório
------| ----------|------| --------|------------
nome	|Nome do comprador|	Alfa Numérico|	20 caracteres	|Não
email|	E-mail do comprador	|Alfa Numérico	|20 caracteres	|Não
endereco|	Nó reservado para dados de endereço do comprador|	 - |	 - |	 - 
telefone|	Nó reservado para dados de telefone do comprador|	-|	-|	-


**RESPOSTA**

Para geração da transferência, o eCommerce deverá redirecionar o consumidor para a URl retornada no campo <urlPagamento>


> Exemplo retorno criação transação:

```curl

--header "Content-Type: application/json"
{  "numeroTransacao": 1234,
   "codigoEstabelecimento": "1000000000000",
   "codigoFormaPagamento": 16,
   "valor": 2000, 
   "valorDesconto": 0, 
   "parcelas": 1, 
   "statusTransacao": 8,
   "autorizacao": "0",
   "codigoTransacaoOperadora": "0",
   "dataAprovacaoOperadora": "", 
   "numeroComprovanteVenda": "", 
   "nsu": "",
   "mensagemVenda": "",
   "urlPagamento": "https://homologacao.superpay.com.br/checkout/PagamentoIatuShopLine/PagamentoItauShopLine.do?cod=132971582229c00506d-e84d-4526-b902-92190d5aa808"
}

```

Campo | Descrição | Tipo | Tamanho 
------| ----------|------| --------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay | Numérico | 13 dígitos
codigoFormaPagamento | [Código da forma de pagamento](https://superpay.github.io/rest/#forma-de-pagamento) | Numérico | Até 3 dígitos
valor | Valor da transação.| Numérico | Até 10 dígitos
valorDesconto | Valor desconto | Numérico | Até 10 dígitos
taxaEmbarque | Valor taxa embarque | Numérico | Até 10 dígitos
parcelas | Quantidade de parcelas da transação | Numérico | Até 2 dígitos
urlPagamento | Url para geração da página do banco |Alfa Numérico | Até 500 caracteres 
statusTransacao | [Status atual da transação](https://superpay.github.io/rest/#status-de-transacao) | Numérico | Até 2 dígitos
autorizacao | Retornado "0" para transferência | Numérico | Até 20 dígitos
codigoTransacaoOperadora | Retornado "0" para transferência | Numérico | Até 20 dígitos
dataAprovacaoOperadora | Retornado em branco para transferência |Alfa Numérico | Até 10 dígitos
numeroComprovanteVenda | Retornado em branco para transferência |Alfa Numérico | Até 20 dígitos
mensagemVenda | Retornado em branco para transferência |Alfa Numérico | Até 50 dígitos


# Pagamentos Recorrentes

<aside class="notice">
Esta funcionalidade está disponível em um EndPoint diferenciado:

SANDBOX: <code>	https://homologacao.superpay.com.br/checkout/api/v2/recorrencia</code>


PRODUÇÃO: <code>https://superpay2.superpay.com.br/checkout/api/v2/recorrencia</code>
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


```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary
{
    "estabelecimento": "1000000000000",
    "recorrencia": {
        "formaPagamento": 170,
        "numeroRecorrencia": 2,
        "valor": 13000,
        "modalidade": "1",
        "periodicidade": "3",
        "urlNotificacao": "http://teste.com.br/campainha",
        "processarImediatamente": "true",
        "quantidadeCobrancas": "0",
        "dataPrimeiraCobranca": "30/06/2017",
        
        "dadosCobranca": {
            "nomeComprador": "Teste Recorrencia",
            "documento": "12312312312",
            "telefone": {
                "tipoTelefone": "1"
            }
        },
        "dadosCartao": {
            "nomePortador": "Teste",
            "numeroCartao": "0000000000000001",
            "codigoSeguranca": "287",
            "dataValidade": "01/2018"
        }
    }
}

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
formaPagamento|	[Código da forma de pagamento](https://superpay.github.io/rest/#forma-de-pagamento) |	Numérico|	Até 3 dígitos|	Sim
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

```curl

--header "Content-Type: application/json"
"recorrencia": {
   "estabelecimento": "1000000000000",
   "numeroRecorrencia": 2,
   "codigoFormaPagamento": 170,
   "valor": 13000,
   "numeroCobrancaTotal": 0,
   "numeroCobrancaRestantes": -1,
   "status": 0,
   "mensagem": "Processamento realizado com sucesso.",
   "numeroPedido": 20001,
   "statusTransacao": 1,
   "autorizacao": "123456",
   "codigoTransacaoOperadora": "00",
   "dataAprovacaoOperadora": "30/05/2017",
   "numeroComprovanteVenda": "1006993069000891071A",
   "mensagemVenda": "Operation Success"
   }

```

Campo | Descrição 
------| ----------
numeroRecorrencia|	Número da Recorrência
estabelecimento|	Código do estabelecimento, fornecido pelo SuperPay
valor|	Valor
codigoFormaPagamento	|[Código da forma de pagamento cadastrada](https://superpay.github.io/rest/#forma-de-pagamento)
numeroCobrancaTotal|	Quantidade máxima de cobranças
numeroCobrancaRestantes	|Quantidade de cobranças restantes
status|	Status atual da recorrência
mensagem|	Mensagem da recorrência
numeroPedido|	Número da Cobrança Recorrente
statusTransacao|	[Status atual da transação recorrente](https://superpay.github.io/rest/#status-de-transacao)
autorizacao|	Código de autorização da Adquirente
codigoTransacaoOperadora|	Código de erro da Adquirente
dataAprovacaoOperadora|	Data aprovação Adquirente
numeroComprovanteVenda|	Número Comprovante Adquirente
mensagemVenda	|Mensagem Venda Adquirente


## Cancelando uma recorrência

**REQUISIÇÃO**

> Exemplo cancelamento transação recorrente:

```curl

curl
--request GET https://homologacao.superpay.com.br/checkout/api/v2/recorrencia/10000000000000/2/cancelar
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary

```
Para autenticação, enviar `usuario` e `senha`:

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento

Campo | Descrição | Tipo
------| ----------| ----------
numeroRecorrencia|	Número da Recorrência a ser cancelada| Numérico
estabelecimento	|Código que identifica o estabelecimento dentro do SuperPay (fornecido pelo gateway)| Numérico
acao | Ação para realizar o processamento | "cancelar"


**RESPOSTA**

> Exemplo retorno cancelamento transação recorrente:

```curl

--header "Content-Type: application/json"
"recorrencia": {
   "estabelecimento": "1000000000000",
   "numeroRecorrencia": 2,
   "codigoFormaPagamento": 170,
   "valor": 13000,
   "numeroCobrancaTotal": 3,
   "numeroCobrancaRestantes": 3,
   "status": 0,
   "mensagem": "Recorrência cancelada com sucesso.",
   "numeroPedido": null,
   "statusTransacao": null,
   "autorizacao": null,
   "codigoTransacaoOperadora": null,
   "dataAprovacaoOperadora": null,
   "numeroComprovanteVenda": null,
   "mensagemVenda": null
  }

```

Campo | Descrição 
------| ----------
numeroRecorrencia|	Número da Recorrência
estabelecimento|	Código do estabelecimento, fornecido pelo SuperPay
valor|	Valor
codigoFormaPagamento	|[Código da forma de pagamento](https://superpay.github.io/rest/#forma-de-pagamento)
numeroCobrancaTotal|	Quantidade máxima de cobranças
numeroCobrancaRestantes	|Quantidade de cobranças restantes
status|	Status atual da recorrência
mensagem|	Mensagem da recorrência
numeroPedido|	Número da Cobrança Recorrente
statusTransacao|	[Status atual da transação recorrente](https://superpay.github.io/rest/#status-de-transacao)
autorizacao|	Código de autorização da Adquirente
codigoTransacaoOperadora|	Código de erro da Adquirente
dataAprovacaoOperadora|	Data aprovação Adquirente
numeroComprovanteVenda|	Número Comprovante Adquirente
mensagemVenda	|Mensagem Venda Adquirente

# Checkout SuperPay

Através desta funcionalidade, é possível finalizar pagamentos dentro do ambiente seguro do SuperPay. O consumidor será redirecionado para o Checkout do Gateway e assim finalizará sua transação.
Os meios de pagamentos disponíveis serão todos que estiverem configurados no estabelecimento.

## Criando uma transação

**Particulariedades**

* Funcionalidade com redirecionamento;
* Importante a utilização do recurso de Campainha, para atualização dos pedidos no Ecommerce;
* Estrutura completa e definição da forma de pagamento pelo consumidor.

**REQUISIÇÃO**

<aside class="notice">
Para enviar a transação, acione o método <code>POST</code>
</aside>


> Exemplo de estrutura de requisição simplificada:

```curl

curl
--request POST https://homologacao.superpay.com.br/checkout/api/v2/transacao
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary
{
     "codigoEstabelecimento": 1000000000000,
     "codigoFormaPagamento" : 997,
     "transacao": {
        "numeroTransacao": 9879690,
        "valor": 2000,
        "valorDesconto": 0,
        "dataVencimentoBoleto":,
        "parcelas": 1,
        "urlCampainha": "http://sualoja.com.br_campainha/",
        "urlResultado": "http://sualoja.com.br",
        "ip": "192.168.12.110",
        "idioma": 1
     },
     "checkout": {
        "processar": 0,
        "tipoPagamento" : 0
     },
     "itensDoPedido": [
        {
          "codigoProduto": 1,
          "nomeProduto": "Produto 1",
          "codigoCategoria": 1,
          "nomeCategoria": "categoria",
          "quantidadeProduto": 1,
          "valorUnitarioProduto": 1199
        }
     ]
     "dadosCobranca": {
        "nome": "Teste 123",
        "email": "teste@teste.com",
        "documento": "12345671234",
        "endereco": {
           "logradouro": "Rua",
           "numero": "123",
           "complemento": "",
           "cep": "12345-678",
           "bairro": "Bairro",
           "cidade": "Cidade",
           "estado": "SP",
           "pais": "BR"
         },
         "telefone": [
            {
            "tipoTelefone": "1",
            "ddi": "55",
            "ddd": "12",                                                 
            "telefone": "1234-5678"
            }
         ]
      },
      "dadosEntrega": {
         "nome": "Teste 123",
         "email": "teste@teste.com",
         "endereco": {
            "logradouro": "Rua",
            "numero": "123",
            "complemento": "",
            "cep": "12345-678",
            "bairro": "Bairro",
            "cidade": "Cidade",
            "estado": "SP",
            "pais": "BR"
          },
          "telefone": [
             {
             "tipoTelefone": "1",
             "ddi": "55",
             "ddd": "12",
             "telefone": "1234-5678"
             }
          ]
     }
}


```

Para autenticação, enviar `usuario` e `senha`:

Campo | Descrição 
------| ----------
usuario | Login do estabelecimento
senha | Senha do estabelecimento


Campo | Descrição | Tipo | Obrigatório
------| ---------- | ------- | ---------
codigoEstabelecimento |	Código único que identifica o estabelecimento dentro do SuperPay|	Numérico|	Sim
codigoFormaPagamento|	Código da forma de pagamento - Enviar 997|	Numérico|	Sim


*transacao*

Campo | Descrição | Tipo | Obrigatório
------| ---------- | ------- | ---------
numeroTransacao	|Código único que identifica o pedido|	Numérico|	Sim
valor|	Valor da transação. Não utilizar vírgulas ou pontos|	Numérico|	Sim
valorDesconto|	Valor do desconto da transação. Campo apenas informativo|	Numérico|	Sim
parcelas|	Quantidade de parcelas da transação|	Numérico|	Sim
urlCampainha|	URL será sempre acionada quando o status do pedido mudar. Deve estar preparada para receber dados de campainha|	Alfa Numérico	Não
urlResultado|	Consumidor será redirecionado para esta URL, quando finalizar o pagamento|	Alfa Numérico|	Não
idioma	|1 - Português 2 - Inglês 3 - Espanhol |	Numérico|	Sim
ip|	Número do IP do usuário final/cliente. Formato xxx.xxx.xxx.xxx|	Alfa Numérico|	Caso utilizar antifraude, sim
dataVencimentoBoleto|	Data do Vencimento do boleto. Formato dd/mm/aaaa|	Alfa Numérico|	Não
campoLivre1|	Campo Livre 1	|Alfa Numérico|	Não
campoLivre2|	Campo Livre 2|	Alfa Numérico	|Não
campoLivre3|	Campo Livre 3	|Alfa Numérico|	Não
campoLivre4|	Campo Livre 4	|Alfa Numérico	|Não
campoLivre5|	Campo Livre 5	|Alfa Numérico	|Não


*checkout*

Campo | Descrição | Tipo | Obrigatório
------| ---------- | ------- | ---------
processar|	Enviar 0 - pagamento comum|	Numérico|	Sim
tipoPagamento	|0 - Todas formas de pagamento;
1 - Cartões de Crédito;
2 - Cartões de Débito;
3 - Boleto;
4 - Transferência;
| Numérico |	Sim

*itensPedido*

Campo | Descrição | Tipo | Obrigatório
------| ---------- | ------- | ---------
codigoProduto|	Código único que identifica o produto|	Alfa Numérico|	Caso utilize antifraude, sim
nomeProduto|	Nome do Produto	Alfa Numérico|	Caso utilize antifraude, sim
quantidadeProduto|	Quantidade do produto|	Numérico|	Sim
valorUnitarioProduto|	Valor unitário do produto. Deve ser enviado sem pontos ou vírgulas|	Numérico| Sim
codigoCategoria	|Código que identifica categoria do produto|	Alfa Numérico|Caso utilize antifraude, sim
nomeCategoria|	Nome da categoria do produto	|Alfa Numérico|	Caso utilize antifraude, sim


*dadosCobranca*

Campo | Descrição | Tipo | Obrigatório
------| ---------- | ------- | ---------
nome|	Nome do comprador|	Alfa Numérico|	20 caracteres	|Sim
email	|E-mail do comprador|	Alfa Numérico|	20 caracteres|	Sim
documento|	Documento do Comprador|	Alfa Numérico|	100 caracteres|	Sim
tipoCliente|	1 - Pessoa Física      2 - Pessoa Jurídica|	Numérico|	Caso utilize antifraude, sim
endereco|	Nó reservado para dados de endereço do comprador|	 - |	 - 
telefone|	Nó reservado para dados de telefone do comprador|	 -|	 -

*endereco*

Campo | Descrição | Tipo | Obrigatório
------| ---------- | ------- | ---------
logradouro|	Endereço do comprador|	Alfa Numérico|	Caso utilize antifraude, sim
numero|	Número do comprador|	Alfa Numérico|	Caso utilize antifraude, sim
bairro|	Bairro do comprador|	Alfa Numérico|	Caso utilize antifraude, sim
complemento|	Complemento do endereço	|Alfa Numérico|	Não
cidade|	Cidade do comprador|	Alfa Numérico|	Caso utilize antifraude, sim
estado|	Estado do comprador|	Alfa Numérico|	Caso utilize antifraude, sim
cep	|CEP do comprador|	Alfa Numérico|	Caso utilize antifraude, sim
pais|	País do comprador|	Alfa Numérico|	Caso utilize antifraude, sim

*telefone*

Campo | Descrição | Tipo | Obrigatório
------| ---------- | ------- | ---------
tipoTelefone|	1 = Outros / 2 = Residencial / 3 = Comercial / 4 = Recados / 5 = Cobrança / 6 = Temporário|	Numérico|	Sim
ddi|	Código DDI do telefone|	Alfa Numérico|	Caso utilize antifraude, sim
ddd|	Código DDD do telefone|	Alfa Numérico|	Caso utilize antifraude, sim
telefone|	Número do telefone|	Alfa Numérico|	Caso utilize antifraude, sim

*dadosEntrega*

Campo | Descrição | Tipo | Obrigatório
------| ---------- | ------- | ---------
nome|	Nome do comprador|	Alfa Numérico|	Não
email|	E-mail do comprador|	Alfa Numérico|	Não
endereco|	Nó reservado para dados de endereço do comprador|- |-
telefone|	Nó reservado para dados de telefone do comprador|-|-


**RESPOSTA**

> Exemplo estrutura de resposta:

```curl

--header "Content-Type: application/json"
{"urlPagamento": "https://homologacao.superpay.com.br/checkout/checkout/redirecionaPagamento?codigoPagamento=150609150796704e7-5b25-4a41-a753-a09bfb66db8d&tipoPagamento=0&oneClick=false"}

```

Campo | Descrição 
------| ---------- 
urlPagamento | URL para redirecionar o consumidor para o Checkout


# Consultas
## Consultando uma transação
Para receber novamente os dados de retorno de uma venda, realize uma consulta.

**REQUISIÇÃO**

<aside class="notice">Para consultas, acione o método <code>GET</code></aside>

> Exemplo consulta:

```curl

curl
--request GET https://homologacao.superpay.com.br/checkout/api/v2/transacao/10000000000000/1234
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary

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

```curl

--header "Content-Type: application/json"
{   "numeroTransacao": 1234,
    "codigoEstabelecimento": "1000000000000",
    "codigoFormaPagamento": 170, 
    "valor": 2000, 
    "valorDesconto": 0, 
    "parcelas": 1,
    "statusTransacao": 13,
    "autorizacao": "123456",
    "codigoTransacaoOperadora": "0", 
    "dataAprovacaoOperadora": "24/05/2017",
    "numeroComprovanteVenda": "10069930690009F2122A",
    "nsu": "428706",
    "mensagemVenda": "Operation Success",
    "urlPagamento": "https://homologacao.superpay.com.br/checkout/PagamentoCielo/PagamentoCielo.do?cod=14956291484887110cf2a-9aeb-4b34-a869-1a61f0611b66",
    "cartoesUtilizados": ["000000*******0001"]
}

```

Campo | Descrição | Tipo | Tamanho 
------| ----------|------| --------
numeroTransacao | Código que identifica a transação dentro do SuperPay | Numérico | Até 19 dígitos
codigoEstabelecimento | Código que identifica o estabelecimento dentro do SuperPay | Numérico | 13 dígitos
codigoFormaPagamento | [Código da forma de pagamento](https://superpay.github.io/rest/#forma-de-pagamento) | Numérico | Até 3 dígitos
valor | Valor da transação.| Numérico | Até 10 dígitos
valorDesconto | Valor desconto | Numérico | Até 10 dígitos
taxaEmbarque | Valor taxa embarque | Numérico | Até 10 dígitos
parcelas | Quantidade de parcelas da transação | Numérico | Até 2 dígitos
urlPagamento | Url para autenticação em caso de cartão de débito |Alfa Numérico | Até 500 caracteres 
statusTransacao | [Status atual da transação](https://superpay.github.io/rest/#status-de-transacao) | Numérico | Até 2 dígitos
autorizacao | Número de autorização da adquirente | Numérico | Até 20 dígitos
codigoTransacaoOperadora | Código de retorno da adquirente | Numérico | Até 20 dígitos
dataAprovacaoOperadora | Data aprovação |Alfa Numérico | Até 10 dígitos
numeroComprovanteVenda | Número Comprovante de venda |Alfa Numérico | Até 20 dígitos
mensagemVenda | Mensagem de venda |Alfa Numérico | Até 50 dígitos
cartoesUtilizados | Cartões mascarados utilizados na transação | Alfa Numérico | Até 20 caracteres

## Consulta recorrente

Consulta para receber informações da recorrência e da última cobrança realizada.

**REQUISIÇÃO**

<aside class="notice">Para consulta, acione o método <code>GET</code></aside>

> Exemplo consulta:

```curl

curl
--request GET https://homologacao.superpay.com.br/checkout/api/v2/recorrencia/10000000000000/2
--header "Content-Type: application/json"
--header "usuario:{"login":"superpay","senha":"superpay"}"
--data-binary

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

```curl

--header "Content-Type: application/json"
"recorrencia": {
   "estabelecimento": "1000000000000",
   "numeroRecorrencia": 2,
   "codigoFormaPagamento": 170,
   "valor": 13000,
   "numeroCobrancaTotal": 0,
   "numeroCobrancaRestantes": -1,
   "status": 0,
   "mensagem": "Processamento realizado com sucesso.",
   "numeroPedido": 20001,
   "statusTransacao": 1,
   "autorizacao": "123456",
   "codigoTransacaoOperadora": "00",
   "dataAprovacaoOperadora": "30/05/2017",
   "numeroComprovanteVenda": "1006993069000891071A",
   "mensagemVenda": "Operation Success"
   }

```

Campo | Descrição 
------| ----------
numeroRecorrencia|	Número da Recorrência
estabelecimento|	Código do estabelecimento, fornecido pelo SuperPay
valor|	Valor
statusTransacao	|[Status atual da transação recorrente](https://superpay.github.io/rest/#status-de-transacao)
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

Importante lembrar que a chamada de campainha não informa qual o status atual da transação e apenas que houve uma alteração, sendo assim, o estabelecimento deve realizar uma consulta (através da função de [consultaTransacao](https://superpay.github.io/rest/#consultando-uma-transacao)) para verificar com mais detalhes a situação atual da transação.

*Caso a URL de campainha estiver em HTTPS, informar ao Suporte SuperPay, servicedesk@superpay.com.br*

> Exemplo de chamada:

```curl

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

Neste fluxo de recorrência, o estabelecimento receberá a campainha informando qual recorrência houve a cobrança e, depois disto, deverá acionar a [consulta da recorrência](https://superpay.github.io/rest/#consulta-recorrente) para receber o número do pedido, e assim, acionar a [consulta da transação](https://superpay.github.io/rest/#consultando-uma-transacao) para recebimento do status.

*Caso a URL de campainha estiver em HTTPS, informar ao Suporte SuperPay, servicedesk@superpay.com.br*

> Exemplo de chamada:

```curl

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


# Downloads
## Exemplos de integração

Temos alguns exemplos para te auxiliar no processo de integração com o Gateway, basta [acessar aqui](https://superpay.acelerato.com/base-de-conhecimento/#/artigos/110) e realizar o download do exemplo da linguagem de programação de sua loja virtual.



