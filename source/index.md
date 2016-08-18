---
title: Perguntas frequentes

toc_footers:
  - <a href='/Webservice-3.0/'>Integração API 3.0</a>

search: true
---

# Perguntas frequentes

## Como funciona a solução API 3.0 da Cielo?

A solução API 3.0 da plataforma Cielo eCommerce foi desenvolvida com a tecnologia REST. O modelo empregado é bastante simples: Existem duas URLs (endpoint), uma específica operações que causam efeitos colaterais - como autorização, captura e cancelamento de transações, e uma URL específica para operações que não causam efeitos colaterais, como pesquisa de transações.

## Que tipo de conteúdo é enviado na integração?

Para simplificar ao máximo a solução API 3.0, a integração é feita através do envio de JSONs numa arquitetura REST. Cada tipo de mensagem deve ser enviada para um recurso identificado através do path e enviada segundo o método HTTP adequado:

* **POST** - O método HTTP POST é utilizado na criação dos recursos ou no envio de informações que serão processadas. Por exemplo, criação de uma transação.
* **PUT** - O método HTTP PUT é utilizado para atualização de um recurso já existente. Por exemplo, captura ou cancelamento de uma transação previamente autorizada.
* **GET** - O método HTTP GET é utilizado para consultas de recursos já existentes. Por exemplo, consulta de transações.

## É preciso algum software proprietário, como DLLs, para fazer a integração?

Não. A ausência de aplicativos proprietários é uma das características da solução: Não é necessário instalar aplicativos no ambiente da loja virtual **em nenhuma hipótese**.

## Preciso ter uma integração com o Webservice 1.5 para usar a 3.0?

Não, não precisa. Você pode fazer sua primeira integração diretamente com a solução API 3.0. Contudo, se a loja já tiver uma integração com o Webservice 1.5, o [Guia de Migração Cielo](https://developercielo.github.io/Guia-de-migracao-1.5x3.0/) ajudará a compreender as diferenças para facilitar a migração para a solução API 3.0.

## Como o Webservice da Cielo sabe que a requisição é da minha loja?

Todas as requisições a Web Service da Cielo **devem conter o header de autenticação do lojista**, composto pelo número de credenciamento, `MerchantId`, e chave de acesso, `MerchantKey`, fornecidos pela Cielo no momento da afiliação.

## Preciso fazer uma afiliação antes de testar a integração?

Não é necessário uma afiliação para utilizar o Sanbox Cielo. Basta acessar o [Cadastro do Sandbox](https://cadastrosandbox.cieloecommerce.cielo.com.br/) e criar uma conta de testes. Ao fim do cadastro você receberá um `MerchantId` e um `MerchantKey`, que deverão ser utilizados para autenticar todas as requisições feitas para os endpoints da API.

## O Sandbox possui um ambiente onde posso ver as transações de testes?

Não; o Sandbox, porém, permite que o desenvolvedor armazene o `TID` da transação e, então, envie uma requisição de consulta. Com os `TID` armazenados, o desenvolvedor consegue, inclusive, desenvolver seu próprio portal de consultas e gestão de transações.

## Preciso utilizar cartões de crédito de verdade para testar a integração?

Não; você pode utilizar o meio de pagamento simulado, que é um meio de pagamento que emula a utilizaçao de pagamentos com Cartão de Crétido. Com esse meio de pagamento é possivel simular todos os fluxos de Autorização, Captura e Cancelamento. Para melhor utilização do Meio de Pagamento Simulado, estamos disponibilizando cartões de testes na tabela abaixo. Os status das transações serão conforme a utilização de cada cartão.

|Status da Transação|Cartões para realização dos testes|Código de Retorno|Mensagem de Retorno|
|-------------------|----------------------------------|-----------------|-------------------|
|Autorizado|0000.0000.0000.0001 / 0000.0000.0000.0004|4|Operação realizada com sucesso|
|Não Autorizado|0000.0000.0000.0002|2|Não Autorizada|
|Autorização Aleatória|0000.0000.0000.0009|4 / 99|Operation Successful / Time Out|
|Não Autorizado|0000.0000.0000.0007|77|Cartão Cancelado|
|Não Autorizado|0000.0000.0000.0008|70|Problemas com o Cartão de Crédito|
|Não Autorizado|0000.0000.0000.0005|78|Cartão Bloqueado|
|Não Autorizado|0000.0000.0000.0003|57|Cartão Expirado|
|Não Autorizado|0000.0000.0000.0006|99|Time Out|

<aside class="notice">As informações de Cód.Segurança (CVV) e validade podem ser aleatórias, mantendo o formato - CVV (3 dígitos) Validade (MM/YYYY).</aside>

## Existe um meio de pagamento simulado que não seja cartão?

Não. Atualmente o Sandbox da Cielo oferece o meio de pagamento simulado apenas para cartões.

## O que é uma transação?

O elemento central do Cielo E-commerce é a transação, criada a partir de uma requisição HTTP ao Webservice da Cielo. A identificação única de uma transação na Cielo é feita através do campo **TID**, que está presente no retorno das mensagens de autorização. Esse campo é essencial para realizar consultas, capturas e cancelamentos.

## A API 3.0 só aceita transações com cartão de crédito?

Não; com a solução API 3.0 o lojista pode transacionar com cartões de crédito, de débito, boleto e transferência eletrônica. Ainda, com cartões de crédito o lojista ainda pode fazer autorizações recorrentes em diversos períodos diferentes.

## Posso transacionar em qualquer moeda?

Apesar de constar na documentação que o campo `Payments.Currency` aceita diversos valores, a API 3.0 suporta apenas o Real Brasileiro - **BRL** - atualmente.

## Como faço uma autorização direta?

A autorização direta caracteriza-se por ser uma transação onde não há a autenticação do portador, seja por opção (e risco) do lojista, seja porque a bandeira ou emissor não tem suporte. A autorização direta pode ser feita de duas formas: tradicional (com os dados do cartão) ou através de um Card Token.

## Como faço uma autorização recorrente?

A autorização recorrente pode ser feita de duas formas: através do envio de um Card Token previamente cadastrado, ou através de um cartão. A transação recorrente é praticamente igual à transação tradicional, as mudanças consistem nas regras que o emissor e a bandeira utilizam para autorizar ou negar uma transação.

## É possível fazer uma captura em um momento após a autorização?

Uma transação autorizada somente gera o crédito para o estabelecimento comercial caso ela seja capturada. Por isso, toda venda que o lojista queira efetivar será preciso realizar a captura (ou confirmação) da transação.

Para vendas na modalidade de Crédito, essa confirmação pode ocorrer em dois momentos:

* Imediatamente após a autorização (captura total);
* Posterior à autorização (captura total ou parcial).

No primeiro caso, não é necessário enviar uma requisição de captura, pois ela é feita automaticamente pela Cielo após a autorização da transação. Para tanto, é preciso configurar a requisição de transação definindo-se o valor “true” para a TAG <capturar>, conforme visto na seção “Criando uma transação”.

Já no segundo caso, é preciso fazer uma “captura posterior”, através de uma nova requisição ao Webservice da Cielo para confirmar a transação e receber o valor da venda.

<aside class="warning"><strong>Atenção!</strong>: Na modalidade de débito não existe essa opção: toda transação de débito autorizada é capturada automaticamente.</aside>

## É possível cancelar uma transação?

O cancelamento é utilizado quando o lojista decide não efetivar um pedido de compra, seja por insuficiência de estoque, por desistência da compra pelo consumidor, ou qualquer outro motivo. Seu uso faz-se necessário principalmente se a transação estiver capturada, pois haverá débito na fatura do portador, caso ela não seja cancelada.

<aside class="notice">Se a transação estiver apenas autorizada e a loja queira cancelá-la, o pedido de cancelamento não é necessário, pois após o prazo de captura expirar, ela será cancelada automaticamente pelo sistema.</aside>

## Existe alguma regra para leitura de cartões?

A leitura dos dados do cartão no ambiente próprio é controlada por regras definidas pelo programa de segurança imposto pelas bandeiras de cartões.

Para a Visa, esse programa é o conhecido como AIS (Account Information Security) PCI. Para maiores informações acesse: [www.cielo.com.br](www.cielo.com.br) > Serviços > Serviços de Segurança > AIS – Programa de Segurança da Informação , ou entre em contato conosco.

Para a Mastercard o programa de segurança é o SDP (Site Data Protection) PCI. Para maiores informações acesse: [http://www.mastercard.com/us/sdp/index.html](http://www.mastercard.com/us/sdp/index.html), ou entre em contato conosco.

Ademais, atendidos os requisitos, no momento do credenciamento E-commerce deve ser mencionada a escolha por leitura do cartão na própria loja.
