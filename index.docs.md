---
title: API SuperPay

toc_footers:
  - <a href='#'>Documentação API SuperPay</a>


search: false
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

## Múltiplos Cartões
Através desta estrutura é possível o envio de mais de um cartão para aprovação em uma mesma transação, dividindo o valor total do pedido entre os mesmos.

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

# Integração REST
Acesse o link: [https://superpay.github.io/rest](https://superpay.github.io/rest)

# Integração SOAP
Acesse o link: [https://superpay.github.io/soap](https://superpay.github.io/soap)


