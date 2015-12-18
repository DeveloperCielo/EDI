---
title: Manual de desenvolvimento Extrato EDI

search: true

toc_footers:
  - <a href='/index.html'>Manual de Integração EDI</a>
---

# Introdução

O Extrato Eletrônico (EDI) é um  produto disponibilizado pela Cielo aos clientes que necessitam de automatização no processo de conciliação. Nele, as informações são transmitidas de forma padronizada sem intervenção manual por meio do canal EDI, proporcionando agilidade e segurança no tráfego das informações.

![Fluxo](images/fluxo-edi.jpg)

# Benefícios

* Permite a realização da conciliação contábil e financeira de forma automatizada;
* Maior agilidade e eficiência operacional;
* Integração com a automação comercial de vendas;
* Segurança no recebimento das informações;
* Atendimento especializado.

# Tipos de extrato EDI

É possível solicitar o recebimento dos seguintes tipos de arquivos de Extrato Eletrônico:

|Tipo de arquivo|Informação|Tipo de registro|Conciliação|
|---------------|----------|----------------|-----------|
|03 - Vendas com Plano Parcelado|<ul><li>Vendas concluídas (apresentadas) no dia anterior, ajustes e transações rejeitadas, com a previsão de pagamento.</li><li>Todo o plano parcelado da venda realizada no dia anterior.</ul><br /><strong>Obs.:</strong> a previsão de pagamento é enviada somente para a primeira parcela. Caso seja alterada por ajustes, os registros de aceleração e reagendamento das parcelas envolvidas serão apresentados.|<ul><li>0 - Header</li><li>1 - Detalhe RO (com a primeira parcela)</li><li>2 - Detalhe CV</li><li>1 - Detalhe RO (previsão da segunda parcela em diante)</li><li>9 – Trailer</li></ul>|Conferir se todas as vendas realizadas foram recebidas pela Cielo.|
|04 - Pagamentos|<ul><li>Valores pagos na conta-corrente no dia do envio do Extrato: detalha os ROs e ajustes  compensados no dia.</li><li>Transações compensadas, já antecipadas ou cedidas anteriormente e pagamentos de parcelas em aberto.</li></ul>|<ul><li>0 - Header</li><li>1 - Detalhe RO</li><li>2 - Detalhe CV</li><li>9 – Trailer</li></ul>|Conferir origem do pagamento recebido (tipo de venda, bandeira e cliente/estabelecimento que efetuou a venda). Conciliação com a conta-corrente.|
|06 - Antecipação de Recebíveis|Operação de ARV realizada pela Cielo no dia anterior ao envio do arquivo e respectivo detalhe dos ROs e CVs que foram antecipados.|<ul><li>0 - Header</li><li>5 - Detalhe de ARV</li><li>6 - Detalhe ROs antecipados</li><li>2 - Detalhe CV </li><li>7 - Detalhe de débitos de ROs Antecipados</li><li>2 - Detalhe CV </li><li>9 – Trailer</li></ul><br/><strong>Obs.:</strong> Será demonstrado o registro tipo 5 para cada operação realizada por domicílio bancário.|Atualizar o fluxo de caixa, considerando os valores já recebidos através da antecipação.|
| 07 - Cessão de Recebíveis|<ul><li>Operação de cessão realizada no banco domicílio (Bradesco ou HSBC) no dia anterior ao envio do arquivo e respectivo detalhe dos ROs cedidos.</li><li>Não é demonstrada a condição comercial negociada ou parcela cedida.</li></ul>|<ul><li>0 - Header</li><li>5 - Detalhe de cessão de recebíveis</li><li>6 - Detalhe ROs cedidos</li><li>9 – Trailer</li></ul><br/><strong>Obs.:</strong> Será demonstrado o registro tipo 5 para cada operação realizada por domicílio bancário.|Atualizar o fluxo de caixa, considerando os valores já recebidos através da operação de cessão.|
| 08 - Parcelas Pendentes|Arquivo gerado uma única vez no primeiro dia após o cliente optar pelo recebimento do extrato de Vendas com Plano Parcelado (arquivo 03).|<ul><li>0 - Header</li><li>1 - Detalhe RO</li><li>2 - Detalhe CV</li><li>9 – Trailer</li></ul>|Elaborar a previsão de fluxo de caixa das vendas realizadas anteriormente.|
|09 - Saldo em Aberto|<ul><li>Valores a receber com a Cielo, contemplando as transações realizadas, capturadas e processadas, vendas a débito, crédito e parceladas ainda não liquidadas no movimento fechado no mês anterior.</li><li>É apresentado um indicador de ARV e Cessão, para facilitar a conciliação dos ROs envolvidos nas operações de antecipação e que alteram os valores em aberto do cliente.</li><li>O arquivo é disponibilizado mensalmente.</li></ul>|<ul><li>0 - Header</li><li>1 - Detalhe RO</li><li>9 - Trailer</li></ul>|Atualizar a previsão futura de recebimentos com os devidos abatimentos de Antecipações, Cessões e Ajustes.|
