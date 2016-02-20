---
title: Manual of EDI Integration

search: true

toc_footers:
  - <a href='extrato-edi.html'>EDI Manual Statement</a>
---

# Introduction

EDI - Electronic Data Interchange is a capture channel and processing large volumes of batch transactions through the exchange of files between the client, the file transmission partner (VANs) and Cielo.
This channel is indicated for:

* Authorize Credit and Installment transactions mainly recorrent transactions, ex .: Publishing, Insurance, Academies, etc. 
* Cancel transactions Credit, Debit Installment and independent of authorization channel: Machine / TEF / E-commerce / Mobile or EDI.
* Catch for processing in Base II, the transactions of air companies, sent by IATA (International Air Transport Association).
* Transmission of electronic statement (of specific integration manual)

# Audience

There is no restriction for using this channel. However, the client's business needs may or may not enable the use of the capture channel.
As described above, some features are necessary:

*	Transaction authorization: suitable for customers with the need to authorize large transaction volumes, especially recurring transactions (academies, insurance, subscriptions, etc.).
*	Transaction cancellation: suitable for guests in need to withdraw large volumes of transactions in an automated manner.
*	Transactions IATA: International Air Cias used by that part of the IATA and GDS.
*	Electronic Statement: suitable for clients with large volume of transactions, in order to facilitate financial reconciliation.

## EDI in 4 steps

To use the EDI capture channel, requires four steps:

1.	Accreditation: the customer must be accredited Cielo;
2.	Integration: carry out development according to the manual to integrate with EDI. Obs .: the development size will depend on the desired degree of integration with their management systems (ERP, Commercial Automation, etc.);
3.	Approval: a test file can be sent to the EDI Service Island to file structure validation;
4.	Production: send the file for processing.

![Steps](images/edi-passos.png)

## Technical support

The EDI capture solution includes a Service Island to support, responsible for:

1.	Integration support and approval by the client;
2.	1st level support at the production stage;
3.	Interface with internal areas Cielo and VAN's partners (TIVIT and GXS), for 2nd and 3rd level support.

## Goods and card issuers enabled EDI CHANNEL:

![Card issuers](images/edi-bandeiras.png)

|Product|Service|Batch authorization|Batch cancellation|IATA Submission|Base 2|IATA Cancellation|
|---------------|-------------------|--------------------|---------------------|---------------|
|Debt|No|Yes|No|Yes*|
|Credit View|Yes|Yes|Yes|Yes|
|Shop installments|Yes|Yes|Yes|

### Exceptions

*	ELO is not enabled for IATA mode.
* Card issuers in the VANs model (Amex, Vouchers, etc.) are not enabled for EDI channel.

# Concepts

## What is?

EDI - Electronic Data Interchange, it is a capture channel and batch transaction processing, through the exchange of files between the client and file transmission partner (VANs) and Cielo.

## What is it for?

*	Authorize card not present transactions, especially recurring transactions, ex .: Publishing, Insurance, Academies, etc.
*	Cancel card transactions, present or not present, independent of the channel where the commitment was made: Machine, TEF, E-Commerce or EDI.
*	Transactions IATA: International Air Cias used by that part of the IATA and GDS.
*	Electronic Statement: suitable for clients with large volume of transactions, in order to facilitate financial reconciliation.

## How it works?

*	The customer, after accreditation in Cielo, requests the integration of documents and the creation of a mailbox for sending EDI files.
*	Develops the integration of their management systems, back office and business automation to format transactions according to the layout EDI (transaction file).
*	The Island Care EDI and Cielo partner (VAN), providing the customer supports needed during the integration phase.
*	The client sends the transaction files in the format (layout) specific EDI for the electronic mailbox. Exception: IATA transactions have their own layout.
*	These transactions are sent to the systems of Cielo, which will hold the Monday to Saturday processing and sending of these transactions to their card brands, for approval or cancellation thereof.
*	Cielo returns the customer file within 2 days, with the result of processing all transactions (approved or denied with their reasons).

# General considerations

*	The return file will be available to the merchant within 48 hours, using the same layout.
*	The Sales or Cancellation Remittance file submitted by 12:00 will be returned in D + 1.Files submitted after 12:00 will be considered as later day of the movement and his return will be on T + 2.
*	The movements are processed from Monday to Saturday. No processing on Sundays.
*	Banners credit card transactions can be sent: Visa, MasterCard, ELO (except IATA mode) and Diners.
*	Transaction Cias files. Via IATA airlines are not covered in this manual. They have specific layout.
*	**We have no homologation environment for this service.**

## Names of files returns:

*	By submitting Cancellation submission file, the system can return two types of files: "Retmov" or "Visa error".
*	"Retmov": return file with all transactions (approved or denied).
*	"Visa Error": transactions were not carried out due to the uploaded file structure problem, and all uncommitted transactions.
*	Possible typos, as well as the file structure, are the customer's responsibility.

# Tips for formatting EDI files

## Operating Summary (OS)

Identifies the batch number, sales transactions group.

## Sales Receipt (SR)

Each sale is recorded in a sales voucher.

* Each Operating Summary (RO) can contain up to 9,999 vouchers for sale (HP).
* Transactions should be grouped by type of transaction (cash or in installments store), and one should create a specific RO for each of them.
*	For each transaction group (batch / RO) must be generated header containing a movement, detail (s) and trailer.
*	For each number of dobby all the above rules should be applied.
* Must undergo header, detail and trailer for each contraption and each transaction summary (RO)
*	The file must be formatted with one single lot (RO) of installment sales for dobby and the first batch of sale (RO) cash for Dobby.

# Files for Shipping and/or Sales Return

File layout for Shipping and/or Sales Return

## Header record

|SEQ|DESCRIPTION|POS|TAM|FOR|COMMENTS|Consistencies|EDI|
|---|---------|---|---|---|-----------|-----------------|
|001|Record type|1|2|N|Header identifies the type of record|Necessarily equal to "00"|
|002|Date of filing|3|8|N|Data generation and transmission of movement|Required field format should be YYYYMMDD|
|003|Operational summary of the number (OS)|11|7|N|Informed by the establishment, identifies the batch of transactions|Required field. Do not send the same lot in a 30-day period of the previous submission|
|004|Reserved for the establishment|18|10|X|Exclusive use of the property is not used by CIELO|No checks|
|005|Do not use|28|3|N|Constant = Zeros|Not read|
|006|Cielo establishment number|31|10|N|The dobby (CIELO contract)|Required field. Rejection is <0 or non-numeric|
|007|Currency code|41|3|N|"986" - R $, "840" - US$|Rejected if other than 986 or 840|
|008|Indicator process|44|1|X|"P" - Production|Rejects is other than "P"|
|009|Sales Indicator|45|1|X|“V" - sale|Required field|
|010|Establishing special statement|46|01|X|Constant = white|Only to be other than white so if the systems team determine.|
|011|Do not use|47|204|X|Constant = white|not read|

### Subtitle

•	POS = Field Start position in file;
•	TAM = Field Size
•	FOR = valid formats for the field contents, as follows:
   * **N**= Numeric
   * **X** = Alphanumeric

Registration Detail

|SEQ|DESCRIPTION|POS|TAM|FOR|COMMENTS|Consistencies|EDI|
|---|---------|---|---|---|-----------|-----------------|
|001|Record type|1|2|N|Identifies the detail record type|Consistencies EDI|Only accepted if equal to "01"|
|002|N. of the sales receipt (SR)|3|7|N|It is a sequential number assigned by the establishment, identifies the transaction within the batch.|Required field; Must be numeric; It can not be duplicated on the lot.|
|003|Card number|10|19|N|N. aligned right card, with leading zeros.|Required field; Must be between 16 and 19 digits.|
|004|Authorization Code|29|6|X|The delivery, fill with zeros. In return, return with the authorization code.|No consistency in the time of shipment.
|005|Date of sale|35|8|N|DDMMYYYY format|Only accept the specified format required field|
|006|Option sale|43|1|N|'0' - the sight, '2' - Installment shopkeeper, '4' - shopkeeper Installment - Cia Aerea|Within the same batch all transactions must have the same put option.|
|007|Sale value *|44|15|N|Cash sale => the sale. Sale parceled => total amount of installments + value + shipment entry fee|Required field - has to adhere to the "put option" - can not be less than zero / non-numeric|
|008|Number of installments|59|3|N|Cash sale => fill with zeros. Sale parceled => number of installments|Required field - has to adhere to the "put option" - can not be less than zero\nonnumeric.|
|009|Funded Value *|62|15|N|Cash sale => fill with zeros. Sale parceled => Original Value funded|Can't be less than zero/nonnumeric|
|010|Input value *|77|15|N|Cash sale => fill with zeros. Sale parceled => input value. **Only Airlines Cias.**|Can't be less than zero/nonnumeric|
|011|Boarding Fee *|92|15|N|**Fill only to Airlines Cias. Aeras.** Otherwise fill with zeros|Can't be less than zero/nonnumeric|
|012|Value share *|107|15|N|Cash sale => fill with zeros.Sale parceled => value share (without rounding)|Required Field.It can not be <0 or non-numeric|
|013|In summary operations (SO)|122|7|N|Same as in the RO informed the header record|Mandatory to use the same header number|
|014|Do not use|129|3|N|Constant = zeros|Not read|
|015|Number of the establishment|132|10|N|The dobby (CIELO contract)|It should be the same header|
|016|Reserved for the establishment|142|30|X|Exclusive use of the property is not used by CIELO|No checks|
|017|Status of sale (return code). Detail field 023|172|2|N|The delivery complete with zeros, in return, will be formatted by CIELO containing status as table ANNEX I|No checks|
|018|Expected date of settlement|174|8|N|The delivery complete with zeros; In return, it will be formatted by CIELO with the expected settlement date.|Not read|
|019|Card due date|182|4|X|Format = AAMM|Mandatory field|
|020|Do not use|186|7|N|Constant = zero|Not read|
|021|Do not use|193|15|N|Constant = zero|Not read|
|022|Do not use|208|03|X|Inform white spaces|No checks|
|023|Error code|211|04|X|The delivery fill with white. In return, if any error is returned by Cielo as ANNEX II|No checks|
|024|Reference|215|11|X|Constant = zero|Should be white|
|025|New card|226|19|X|Na remessa preencher com brancos. No retorno, caso tenha convênio “renova fácil” virá o número cartão alinhado a esquerda com brancos direita.|No checks|
|026|New Due date|245|04|N|The delivery fill with white. In return, if you covenant "renews easy" will come to the New Due Date. New card|No checks|
|027|Do not use|249|02|X|Constant = zero|Not read|

## Trailler register

|SEQ|DESCRIPTION|POS|TAM|FOR|COMMENTS|Consistencies|EDI|
|---|---------|---|---|---|-----------|-----------------|
|001|Record type|1|2|N|Identifies the type of record detail trailer|Consistencies EDI|Mandatory use of the code "99"|
|002|Number of records|3|7|N|Including header and trailer|Rejeição se <0 ou não numérico|
|003|Total gross amount (*)|10|15|N|Informed by the establishment - Total value of the lot, the sum of the field 007 Detail record.|Rejection is <0 or non-numeric. Rejects different value if the sum of the field 07|
|004|Total value accept (*)|25|15|N|The delivery complete with zeros; In return, will be formatted by Cielo with the amount accepted.|No checks|
|005|Total net value(*)|40|15|N|The delivery complete with zeros; In return, will be formatted by Cielo with total net value.|No checks|
|006|Expected date of credit|55|8|N|In return, will be formatted by Cielo with the expected payment date.|No checks|
|007|Do not use|63|188|X|Constant = white|Not read|

<aside class="notice">( * )These fields should be considered with two (2) decimal places, without commas, periods, or any other character to separate numbers.</aside>

* Other sales on cancellation rules

The cancellation of any transaction will only be accepted if there is available balance on the Financial Schedule Commercial Establishment to cover the value of the requested cancellation.




