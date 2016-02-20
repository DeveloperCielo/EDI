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

|Product/Service|Batch authorization|Batch cancellation|IATA Submission Base2|IATA Cancellation|
|-------|-------|-------------------|------------------|---------------------|-----------------|
|Debt|No|Yes|No|Yes*|
|Credit View|Yes|Yes|Yes|Yes|
|Shop installments|Yes|Yes|Yes|Yes|

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

|SEQ|DESCRIPTION|POS|TAM|FOR|COMMENTS|ConsistenciesEDI|
|---|-----------|---|---|---|--------|----------------|
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

|SEQ|DESCRIPTION|POS|TAM|FOR|COMMENTS|Consistencies EDI|
|---|-----------|---|---|---|--------|-----------------|
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

|SEQ|DESCRIPTION|POS|TAM|FOR|COMMENTS|Consistencies EDI|
|---|-----------|---|---|---|--------|-----------------|
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

* Cancellations can be requested up to 365 days from the date of authorization.
*	For sales cancellations, the fields 004, 005, 006, 007 and 008 detail line must be filled with the same information the sale, respecting the time and date of authorization and capture of Brazil.
*	It allowed the cancellation Total or Partial sale.
*	Partial cancellations can be sent to the total value of the transaction, in compliance with the following conditions: 
*	Transactions Vista: they can be sent over a partial cancellation of the same transaction on the same day if they are sent in different Lots.
*	Transactions Installment Shop: does not allow partial cancellation of the same transaction on the same day.
* Cancellations of transactions authorized through the EDI channel can only be ordered upon receipt of the file the return authorization transaction (up to 48 hours after sending the file).

# Batch files and Cancellations Return
Remittance File layout and Cancellations Return (not PCI) used for transaction cancellations through the EDI channel, eg recurring transactions.

## Without PCI certification key:

*	Number of Dobby;
*	Card number;
*	Date,
*	Value
*	Authorization of the original transaction;

## Header registry

|SEQ|DESCRIPTION|POS|TAM|FOR|COMMENTS|Consistencies EDI|
|---|-----------|---|---|---|--------|-----------------|
|001|Record type|1|2|N|Header identifies the type of record	Must be "00"|
|002|Date of filing|3|8|N|Data generation and transmission of movement Required field.|Format should be YYYYMMDD|
|003|Operational summary of the number (RO)|11|7|N|Informed by the establishment, identifies the batch of transactions.	Required field.|Do not send the same lot in a 30-day period of the previous submission|
|004|Reserved for the establishment|18|10|X|Exclusive use of the property is not used by CIELO|No checks|
|005|Do not use|28|3|N|Constant = Zeros|Not read|
|006|Cielo establishment number|31|10|N|No. Dobby (CIELO contract)|Required field.|Rejected if <or = 0 or non-numeric|
|007|Currency code|41|3|N	"986" - R $. "840" - US $	Rejected if other than 986 or 840|
|008|Indicator process	44	1	X	"P" - Production	Rejects is other than "P"|
|009|Sales cancellation indicator	45	1	X	"C" - cancellation of sale Credit sight."P" - selling cancellation Installment Shop. "D" - selling cancellation Debit	If different field of "C" or "P" or "D" the lot will be rejected.|
|010|Establishing special statement	46	01	X	Constant = white	Only to be other than white so if the systems team determine.|011|Do not use|47|204|X|Constant = white|Not read|

## Registration Detail

|SEQ|DESCRIPTION|POS|TAM|FOR|COMMENTS|Consistencies EDI|
|---|-----------|---|---|---|--------|-----------------|
|001|Record type|1|2|N|Identifies the detail record type	Only accepted if equal to "01"|
|002|No. sale receipt (CV)|3|7|N|It is a sequential number assigned by the establishment, identifies the transaction within the batch.	Required field; Must be numeric; It can not be duplicated on the lot.|
|003|card number|10|19|N|No. aligned right card, with leading zeros	Required field; must be between 16 and 19 digits.|
|004|Authorization Code|29|6|X|Fill with the authorization code from the sale.|Required field|
|005|Date of sale|5|8|N|DDMMYYYY|Required field only accept the specified format|
|006|Option sale|43|1|N|'0'- the sight, '2' - Installment shopkeeper, '4' -. Shopkeeper Installment Airline	Required Field. Within the same batch all transactions must have the same put option|
|007|Sale value*|44|15|N|Sale view => value sale, sale parceled => sum of installments and input value.	Required field. You have to respect the "put option"|
|008|Number of installments|59|3|N|Cash sale => fill with zeros, sale parceled => number of installments	Required field. You have to respect the "put option"|
|009|The amount financed*|62|15|N|Cash sale => fill with zeros, sale parceled => funded original value	Required field. You have to respect the "put option"|
|010|Input value*|77|15|N|Cash sale => fill with zeros, sale parceled => input value. Only Cias. Flight	Required field. You have to respect the "put option"|
|011|Departure Tax Value*|92|15|N|Fill only to Cias.Aeras.Otherwise fill with zeros	Rejected if nonzero or non-numeric|
|012|Value share*|107|15|N|Cash sale => fill with zeros, sale parceled => Value of the plot (not to rounding)	Required field. You have to respect the "put option"|
|013|No. summary operations (RO)|122|7|N|Same as No. RO informed the header record	Mandatory to use the same header number|
|014|Do not use|129|3|N|Constant = zeros|Not read|
|015|Number of the establishment|132|10|N|No. Dobby (CIELO contract)|It should be the same header|
|016|Reserved for the establishment|142|30|X|Exclusive use of the property is not used by CIELO	No checks|
|017|Status of sale (return code). Detail field 023	172	2	N	The delivery fill with zeros. In return, it will be formatted by CIELO containing status as table ANNEX I|No checks|
|018|Expected date of settlement|174|8|N|The delivery complete with zeros; In return, it will be formatted by CIELO with the expected settlement date.|Not read|
|019|Validity of the card|182|4|X|Format = AAMM.|It may be padded with zeros|No checks|
|020|Operational summary of the number (RO) Original|186|7|N|Inform the original RO in the number of sale	Required field. Rejected if <or = 0 or non-numeric|
|021|Reimbursement of the value/cancellation|193|15|N|Amount to be reimbursed	Required field. Rejected if <or = 0 or non-numeric|
|022|Do not use|208|03|X|Constant = white	No checks|
|023|Error code|211|04|X|The remittance fill with white.In return, if any error is returned by Cielo as ANNEX II	No checks|
|024|Reference number|215|11|X|Constant = white	Should be white|
|025|New card|226|19|X|The delivery fill with white. In return, if you covenant "renews easy" will come the card number aligned left to right white.|No checks|
|026|New Due Date|245|04|N|The delivery fill with white. In return, if you covenant "renews easy" will come to the Maturity Date. the new card.|No checks|
|027|Do not use|249|02|X|Constant = white|Not read|

## Trailler record

|SEQ|DESCRIPTION|POS|TAM|FOR|COMMENTS|Consistencies EDI|
|---|-----------|---|---|---|--------|-----------------|
|001|Record type|1|2|N|Identifies the type of record detail trailer	Mandatory use of the code "99"|
|002|Number of records|3|7|N|Including header and trailer	Rejection is <0 or non-numeric|
|003|Total gross amount (*)	10	15	N	Informed by the establishment - the total value of the lot, the sum of the field 007 Detail record	Rejection is <0 or non-numeric. Rejects different value if the sum of the field 07|
|004|Total value accept (*)	25	15	N	The delivery complete with zeros; In return, it will be formatted in Cielo a total amount accepted.|No checks|
|005|Net total amount (*)	40	15	N	The delivery complete with zeros; In return, will be formatted by Cielo with total net value.	|No checks|
|006|Expected date of credit	55	8	N	The delivery complete with zeros; In return, will be formatted by Cielo with the expected payment date.|No checks|
|007|Do not use	63	188	X	Constant = white|Not read|

(*) - These fields should be considered with two (2) decimal places, without commas, periods, or any other character to separate numbers.

# Attachments

## ANNEX I - Return Codes Table

|Rejection code|Occurrence Description|
|--------------|----------------------|
|00|Transaction ok (transaction accepted)|
|01|Error in file|
|02|Invalid authorization code|
|03|Invalid property|
|04|Mixed batch|
|05|Invalid number of installments|
|06|Difference in value in RO|
|07|RO invalid number (BH record)|
|08|Invalid input value|
|09|Invalid shipping rate value|
|10|Invalid share value|
|11|Invalid transaction code|
|12|Invalid transaction|
|13|Invalid value|
|14|Not applicable|
|15|Invalid cancellation value|
|16|Not located the original transaction (for cancellation)|
|17|Number of items reported in the RO not compatible with CVs|
|18|Invalid number of reference|
|20|Cancellation for transaction installments already canceled|
|21|Cancellation of value greater than the value of the sale|
|22|Cancellation of value greater than allowed (heave)|
|23|Number of invalid original RO (I2 record)|
|42|Card canceled|
|54|You may not partial cancellation of an installment plan that is being disputed by the carrier.|
|56|Invalid card type of plan|
|59|Invalid card type 60 Invalid Data|
|71|Transaction rejected by the issuing bank|
|72|Transaction rejected by the issuing bank|
|73|Card problem - retain the card|
|74|Denied authorization|
|75|Error|
|76|Transaction rejected by the issuing bank|
|77|Syntax error - redo the transaction|
|78|There was no authorization issuer|
|79|Card canceled|
|80|Card canceled|
|81|Insufficient funds|
|82|Expired card or expiration date wrong|
|87|Card not allowed|
|88|Exceeded the number of transactions in the period|
|89|Message differs from the original message|
|92|Issuing bank without communication|
|93|Cancellation more than 365 days|
|94|Duplication of airlines|
|95|No outstanding balance|
|99|Other reasons|

## ANNEX II - Detailed error status for Sale Set in the countryside 23

|CODE|Complement|Error|EC action|
|----|----------|-----|---------|
|E001|Lot exceeded number of records	Review file structure|
|E001|Structure of incorrect batch	Review file structure|
|E001|Invalid transmission medium	Contact Cielo|
|E002|Review card data	Redo the transaction with valid data|
|E005|Establishment number not found	Check the data of the Establishment Cielo|
|E005|Problem in your registration in Cielo	Contact Cielo|
|E010|Problem in your registration in Cielo	Contact Cielo|
|E023|Problem in your registration in Cielo	Contact Cielo|
|E024|Card Type not allowed in this channel	Contact your customer|
|E044|Transaction date does not give	Reviewing data from the original transaction|
|E045|Authorization code not found	Check the transaction data|
|E048|Incorrect card expiration date	Contact your customer|
|E054|Structure of incorrect batch	Review your file|
|E055|Installment amount invalidates	Check the amount of installments|
|E056|Transaction not allowed to Register	Not resubmit the transaction|
|E057|Card canceled	Contact your customer|
|E061|Review card data	Redo the transaction with valid data|
|E094|Transaction not permitted for International card	Not resubmit the transaction|
|E095|Transaction not permitted for Debit Card	Not resubmit the transaction|
|E097|Installments with less than R $ 5.00	Retrace adjusting the amount of parcels|
|E101|Transaction rejected by the Issuing bank	Contact your customer|
|E102|Transaction rejected by the Issuing bank	Contact your customer|
|E103|Problem in your registration in Cielo	Contact Cielo|
|E104|Card canceled	Contact your customer|
|E105|Transaction rejected by the Issuing bank	Contact your customer|
|E107|Transaction rejected by the Issuing bank	Contact your customer|
|E112|Transaction not allowed on this channel	Contact Cielo|
|E112|Transaction currency dollar not allowed.	Not resubmit the transaction|
|E112|Transaction without authorization code	Redo including the authorization code|
|E112|Transaction not allowed to Register Applicant	Contact Cielo|
|E112|Previously authorized transaction	Not resubmit the transaction|
|E112|Card not allowed to Registration Dollar	Not resubmit the transaction|
|E112|Transaction parceled Not Allowed	Not resubmit the transaction|
|E112|Problem in your registration in Cielo	Contact Cielo|
|E112|Transaction not allowed	Not resubmit the transaction|
|E112|Transaction not allowed to Register	Not resubmit the transaction|
|E112|International transaction card not allowed to Register	Not resubmit the transaction|
|E112|Invalid transaction data	Review transaction data|
|E114|Review card data	Redo the transaction with valid data|
|E141|Card canceled	Contact your customer|
|E143|Card canceled	Contact your customer|
|E151|Unauthorized transaction by the Issuer bank	Contact your customer|
|E154|Incorrect card expiration date	Contact your customer|
|E157|Card canceled	Contact your customer|
|E158|Card Type not allowed in this channel	Contact Cielo|
|E162|Card not allowed for this transaction	Contact your customer|
|E180|Transaction date does not give	Reviewing data from the original transaction|
|E182|Security code invalid card	Review transaction data/Contact your customer|
|E191|Issuing Bank without communication	Contact Cielo|
|E192|Applicable for air companies	Applicable for air companies|
|E205|Review card data	Redo the transaction with valid data|
|E207|Lot number invalid	Review the Lot number|
|E207|Batch field is not numeric	Review the Lot number|
|E207|Number of duplicate Lot	Review the Lot number|
|E209|Applicable for air companies	Review the value|
|E210|Installment amount invalidates	Field zeroed number of installments|
|E210|Installment amount invalidates	Number of plots exceeded limit|
|E210|Product not authorized for your property	Contact Cielo|
|E210|Larger amount of shares permitted	Contact Cielo to change registration installments|
|E211|Applicable for air companies	Review the value|
|E212|Applicable for air companies	Review the value|
|E213|Cancellation value not allowed	Review the transaction data|
|E214|Not located Transaction	Review the transaction data|
|E215|Transaction already canceled	Contact Cielo|
|E216|Transaction previously canceled	Not resubmit the transaction|
|E217|Cancellation value higher sales	Review transaction data|
|E218|Cancellation not allowed in this channel	Contact Cielo|
|E219|Not located RO Number	Review the transaction data|
|E220|Period exceeded to cancel transaction	Not resubmit the transaction|
|E900|Record Type Invalid	Review file structure|
|E901|Header date of the deposit Invalid	Review the transaction data|
|E902|Operations Summary Number (RO) of the Header Invalid	Operations Review Summary Number (RO)|
|E903|Header Establishment number Invalid	Review the number of Establishment / Contact Cielo|
|E904|Currency Header Invalid	Review the structure of Header|
|E905|Header Test Invalid Indicator	Review the structure of Header|
|E906|Selling indicator Invalid Header	Review the structure of Header|
|E911|Sales Receipt Number (CV) Invalid	Review the transaction data|
|E912|Date of Sale Invalid	Review the transaction data|
|E913|Option Sale Invalid	Review the transaction data|
|E914|Card Number Invalid	Review the transaction data/Contact Your Customer|
|E915|Invalid value Sale	Review the transaction data|
|E916|Invalid number of plots	Review the transaction data|
|E917|Invalid value Funded	Review the transaction data|
|E918|Input value Invalid	Review the transaction data|
|E919|Rate value Invalid Boarding	Applicable for air companies|
|E920|Portion of value Invalid	Review the transaction data|
|E921|Operations Summary Number (RO) Invalid	Operations Review Summary Number (RO)|
|E922|Invalid Establishment Number	Review the number of Establishment / Contact Cielo|
|E923|Validity of Invalid Card	Contact your customer|
|E924|Operations Summary Number (RO) Original Invalid	Operations Review Summary Number (RO)|
|E925|Invalid refund of value	Review the transaction data|
|E926|Number of Invalid Reference	Review the transaction data|
|E930|Trailer Records of quantity does not confer	Review the number of records|
|E931|Total gross amount does not confer	Review the transaction data|
