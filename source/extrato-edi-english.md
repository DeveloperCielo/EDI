---

title: Development Manual Extract EDI

search: true

toc_footers:
  - <a Href='index.html'> Integration Manual EDI </a>
---

# Introduction

The Electronic Statement (EDI) is a product offered by Cielo to customers requiring automation in conciliation. In it, the
information is transmitted in a standardized manner without manual intervention through the EDI channel, providing agility and security
in the traffic information.

![Fluxo](images/fluxo-edi.jpg)

## Benefits

* Allows the realization of accounting and financial reconciliation in an automated way;
* Greater agility and operational efficiency;
* Integration with the commercial sales automation;
* Security in receiving information;
* Specialized support.

## Attendance

### Support Center:

*+55 4002-9700 - *Capital and Metropolitan Regions*
* **Other Places**: 0800.570.1700
* From Monday to Saturday, from 8 to 22 hours.

### EDI - Specialized attendance

* 4002 5270 - capitals, metropolitan areas and other locations
* edi@cielo.com.br
* From Monday to Friday, from 9 to 18 hours.

### Internet

www.cielo.com.br

# Types of EDI extract

You can request to receive the following types of Electronic extract files:

|File Type|Information|Record Type|Reconciliation|
|---------------|----------|----------------|-----------|
|03 - Selling Installment Plan|<ul><li> completed sales (reported) yesterday rejected adjustments and transactions, with the forecast 
of payment </li><li> All installment plan sale held on previous day </ul><br/> <strong> Note: </strong> forecasting payment is sent only to
the first installment. If changed by tweaking the acceleration records and rescheduling of the involved parcels will be presented|<ul><li> 
0 - Header </li><li> 1 - RO Detail (with the first installment) </li><. li> 2 - Detail CV </ li> <li> 1 - Details RO 
(forecast of the second installment on) </li><li> 9 - trailer </li></ul>|Check that all sales They were received by Cielo.|
|04 - Payments|<ul><li> Amounts paid on current account on the day of submission of the Statement: Details the ROs and adjustments 
compensated the day </li><li> Transactions offset, as anticipated or transferred previously and payments of outstanding installments 
</li></ul>|<ul><li> 0 - Header </li><li> 1 - Details RO </li><li> 2 - Detail CV </li>< li> 9 - trailer </li></ul>|Check origin of the payment 
received (type of sale, flag and client/establishment that carried out the sale).Reconciliation of the current account.|
|06 - Prepayment of Receivables|ARV operation conducted by Cielo the day before sending the file and its detail of ROs and CVs that were 
anticipated|<ul><li> 0 - Header </li><li> 5 - detail of ARV </li><li> 6 - detail anticipated ROs </li><li> 2 - detail CV </li><li> 7 - ROs debts 
detail advance </li> <li> 2 - detail CV </li> <li> 9 - trailer </li></ul><br/> <strong> Note:. </strong> will be shown the record type 5 for
each transaction carried out by bank domicile.|Update the cash flow, considering the amounts already received by anticipation.|
|07 - Assignment of Receivables|<Ul><li>assignment operation performed at home bank (Bradesco and HSBC) the day before sending the file and its detail 
of the assigned ROs </li><li> It is shown the commercial condition negotiated or assigned portion </li></ul>|<ul><li> 0 - Header </li><li> 5 - receivables 
assignment information </li><li> 6 - Detail assigned ROs </li><li> 9 - trailer </li></ul><br/><strong> Note:. </strong> will be shown the record type 5 for each
transaction carried out by bank domicile.|Update the cash flow, considering the amounts already received by the transfer operation.|
|08 - Pending Plots|File generated only once on the first day after the customer choose to receive the Sales extract with Installment Plan (file 03)|
<ul> <li> 0 - Header </li> <li> 1 -. detail RO </li> <li> 2 - detail CV </li> <li> 9 - trailer </ li> </ ul>|Prepare a cash flow forecast of sales previously.|
|09 - Balance at Open|<ul> <li> Receivables with Cielo, contemplating transactions, captured and processed, sales debit, credit and installment not yet settled 
on the movement closed the previous month </ li> <li> an indicator of ARV and assignment is presented to facilitate the reconciliation of ROs involved in 
anticipation of operations that change the amounts outstanding from the customer.</ li><li> the file is available monthly. </li></ul>|<ul><li> 0 - Header </li><li> 
1 - RO Detail </ li> <li> 9 - trailer </ li> </ ul> | Refresh forward-receipts with due allowances of Advances, Disposals and Adjustments.|

# Layouts of Records Types

The information shown in the Electronic Statement - EDI (EDI) will be organized by the types of records that are described in sequence.

## Type 0 - Header

Identifies the header of each file electronically extract chain *.

|Home|End|Size|Type|Description|Data Edition|
|------|---|-------|----|---------|----------------|
|001|001|1|a|Record Type|Constant "0": Header identifies the type of record (beginning of the file).|
|002|011|10|a|Establishment Matrix|establishment number matrix electronic statement chain.|
|012|019|8|a|Data processing|YYYYMMDD - date the file was created.|
|020|027|8|a|Initial period|YYYYMMDD - less data capture found in motion.|
|028|035|8|a|Period end|YYYYMMDD - most data capture found in motion.|
|036|042|7|a|Sequence|sequential file number. In cases of recovery this data will be sent to 9999999.|
|043|047|5|Alpha|Company acquirer|Constant Cielo.|
|048|049|2|a|Extract Option|See Table I.|
|050|050|1|Alpha|VAN|"I" - OpenText (formerly GXS), "P" - TIVIT|.
|051|070|20|alphanumeric|Mailbox| Information obtained in the registration form in VAN|
|071|073|3|a|Layout Version| Constant "001".|
|074|250|177|-|Use Cielo|Blank. Reserved for Cielo.|

(*) Register which allows the inclusion of all establishments controlled by the customer, regardless of the payment is centralized. 
All * establishment numbers * should be informed in the registration form.

## Type 1 - Detail Operations Summary (RO)

Group sales, adjustments or billing services. It allows you to identify the source of releases and maintenance actions.

|Home|End|Size|Type|Description|Data Edition|
|------|---|-------|----|---------|----------------|
|001|001|1|a|Record Type|Constant "1" - Identifies the detail record type RO|
|002|011|10|a|Establishment Submitter| Number of establishment and / or branch where the sale took place.|
|012|018|7|a|RO Number|Operation summary number. They contain information relating to a sales group performed on a certain date.|
|019|020|2|a|Portion|In the case of split sale, will be formatted with the number of the parcel which is being released in the file
submission date. In the case of cash sale, will be formatted with white.|
|021|021|1|Alpha|Filler|<ul><li> "/" - For installment sales </ li> <li> "a" - acceleration of installments </ li> <li> '. "-. . other 
situations </li></ ul>|
|022|023|2|alphanumeric|Plan| In the case of split sale, will be formatted with the largest number of parcels found that sales group. 
If the RO has sales in 03, 04 or 06 installments, will be filled with <br/> 06. If an acceleration of installments will be formatted 
with the most accelerated share <br/>. Example: (019 positions to 023) <br/> <ul> <li> 02A02. - indicates the acceleration of the 
parcel 02 to 02, or only a portion </li> <li> 03A08 - indicates the acceleration of the portion 03 to the portion 08 plan of sale, 
or were accelerated 06 installments. </ li></ul><br/> in the case of cash sale, will be formatted with white.|
|024|025|2|a|Transaction Type|Code identifying the transaction - see Table II.|
|026|031|6|a|Date of submission|YYMMDD - Date on which the RO is transmitted to Cielo.|
|032|037|6|a|Estimated date of payment|YYMMDD - Expected date of payment.. In recovery, it can be updated after processing the transaction 
or adjustment.|
|038|043|6|a|Date of dispatch to the bank|YYMMDD - Date on which payment file was sent to the bank.. In recovery, it can be updated 
after processing the transaction or adjustment.|
|044|044|1|Alpha|Sign gross|<ul> <li> "+" identifies value credit </li><li> "-" identifies value debt </ li> </ ul>..|
|045|057|13|a|Gross (*)|Sum of sales figures.|
|058|058|1|Alpha|Commission sign| <Ul> <li> "+" identifies value credit </li><li> "-" identifies value debt </li></ul>|
|059|071|13|a|Commission value (*)| commission value discounted on sales.|
|072|072|1| Alpha..|Rejected value of the sign|<Ul> <li> "+" value identifies the credit </li><li> "-" identifies value debt </li></ul>|
|073|085|13|a|Rejected Value (*)|If there is rejection, will be filled with the sum of rejected transactions./
|086|086|1|Alpha|Signal net|<Ul><li> "+" identifies value credit </ li> <li> "-" identifies value debt </li> </ul >|
|087|099|13|a|Net (*)|Sales value minus the amount of commission.|
|100|103|4|a|Bank|Bank code in which the amounts were deposited|
|104|108|5|a|Agency|Agency ID in which the values ​​were deposited|
|109|122|14|Alphanumeric|Current account|Current account code in which the values ​​were deposited|
|123|124|2|a|Payment Status |identifies the situation in which they are sent to the bank credits - see Table III. During recover, 
the status is updated according to the shipping and payment confirmation back by the bank.|
|125|130|6|a|CV Quantity accepted|Sales quantities accepted in RO.|
|131|132|2|a|Product Code| From 1.3.2014, the product identifier shall be sent in positions 233-235 with three characters. 
Disregard the information sent in this position.|
|133|138|6|a|CVs quantities rejected|quantity rejected in the RO.|
|139|139|1| Alpha|Identifier resale/acceleration|Identifies maintenance occurrences in installments transactions in store: <ul> <li> "R" - Resale </ li> <li> "A" - Acceleration < / li> <li> "" - White (no occurrence) </ li> </ ul> |
|140|145|6|a| transaction capture Date| YYMMDD - Date on which the transaction was captured by the Cielo. In recovery, it can be updated after processing the transaction or adjustment./
|146|147|2| alphanumeric|setting Source|Identifies the type of adjustment - see Table V. Fill in the type of transaction is: <ul> <li> 03 - Debt Adjustment </ li> <li> 02 - setting credit </ li> <li> 04 - rent adjustment </ li> </ ul> |
|148|160|13|a|Additional Value|withdrawal amount when the product is equal to "36" or value of Agro Electron for transactions of products "22", "23" or "25" shown in Table IV. |
|161|161|1|Alpha|Anticipation identifier|RO anticipation identifier: <ul> <li> "" - not anticipated; </ li> <li> "A" - Early in Cielo - ARV; </ . li> <li> "C" - Early in the bank - Assignment of Receivables </ li> </ ul> |
|162|170|9|a|Anticipation operation number|identifies the number of Anticipation of operation presented in the record type 5 - Field 12-20, associated with the anticipated RO/ Cielo given in or on the bench.. Contains zeros if the RO has not been anticipated./
|171|171|1|Alpha|value Signal anticipated Gross|<ul><li> "+" identifies value credit </ li> <li> "-" identifies value debt </ li> </.. ul>|
|172|184|13|a|Gross Value Early(*)|anticipated Gross, provided when the RO is anticipated/assigned. It will be filled with zeros when no anticipation. The anticipated gross value is the net value of the RO.|
|185|187|3|a|Issuer|Issuer Code - see Table VI|
|188|209|22|a|Single Number RO| RO identification Single Number formatted as follows:<Ul> <li> First part (fixed) - 15 fixed positions: Identifies the summary retaining its history in . Cielo; </ li> <li> Second part (variable) - 07 variable positions: Identifies changes made to the RO </ li> </ ul>|
|210|213|4|a|Fee Rate(*)|Commission Percentage applied on the transaction amount.|
|214|218|5|a|Tarifa|Tarifa charged per transaction.|
|219|222|4|a|Guarantee Fee|Discount Percentage applied on Electron Pre-Dating transactions.|
|223|224|2|a|Capture Media|See table VII. If the sale has been reprocessed, the system will send the means to capture 06: manual 
capture medium; in this case disregard the value set in the logical terminal number. Course not available for debt sales in the daily 
payment file and the second installment on the installment sales in the diary and recovered payment file.|
|225|232|8|a|Logical terminal number|terminal logical number which was made the sale. When the capture medium is equal to 06,
disregarding the logical terminal number as this is an internal issue of Cielo.|
|233|235|3|a|Product Code|Code that identifies the product - see Table IV.|
|236|245|10|a|Pay Matrix|Establishment array of centralized payment chain.|
|246|250|5|alphanumeric.|Use Cielo|Blank. Reserved for Cielo.|

* <Sup>(1)</sup> - should be considered 2 decimal places, without commas, periods, or any other character.
* <Sup>(2)</sup> - Available only in daily files; recovery is not shown.
* <Sup>(3)</sup> - Available only in Balance extract Open. For other statements will be filled with blanks.
* <Sup>(4)</sup> - Field Not Set in Balance extract Open - Revolving and Installment.
* <Sup>(5)</sup> - Field Not Set in Balance extract Open - Installment.

## Type 2 - Sales Receipt Details (CV)

Detail of sales or ganged adjustments in an RO.

As security rules, all records that have card number will present the truncated number.

|Home|End|Size|Type|Description|Data Edition|
|------|---|-------|----|---------|----------------|
|001|001|1|A|Record Type|Constant "2" - identifies the type of Sales Receipt detail record (CV).|
|002|011|10|A|Establishment Submitter| Number of the establishment and/or branch where the sale took place.|
|012|018|7|A|RO Number| Operation summary of the number. They contain information relating to a sales group performed on a certain date.|
|019|037|19|Alphanumeric|truncated card number|Number of the truncated card: the card number that was purchased with truncated number.
Contain zeros for purchases via mobile payment and e-commerce, and for the last option.|
|038|045|8|A|Date of sale/set|YYYYMMDD - Date on which the sale or adjustment was made.|
|046|046|1|Alfa|the purchase value of the signal or value of the parcel| .. <Ul> <li> "+" value identifies the credit </ li> <li> "-" 
identifies value debt </li></ul>|
|047|059|13|A|the purchase price or value of the parcel | value of purchase or the portion that was released in the case of piecemeal 
sale in the shop.|
|060|061|2|A|share|In case of split sale, will be formatted with the number of the parcel being released. In the case of cash sale, 
will be formatted with zeros.|
|062|063|2|A|Total tranches|Total number of shares sale. In the case of cash sale, will be formatted with zero.|
|064|066|3|A|Reason for Rejection| See Table VIII, should not have rejected the field is blank formatted.|
|067|072|6|alphanumeric|authorization code|transaction authorization ode. This number is not unique and can be repeated. For 
reconciliation purposes should be combined with other keys.|
|073|092|20|alphanumeric|TID|transaction identification made in e-commerce and mobile payment.|
|093|098|6|alphanumeric|NSU/DOC|Sequence Number, also known as DOC (document number), which identifies the transaction in the day it 
was made. This number is not unique and can be repeated. If the sale has been reprocessed, the NSU can be changed.|
|099|111|13|a|Complementary Value|Serve with debit card transaction value or AgroElectron according RO product indicator.|
|112|113|02|Num|Total value of sale in the case of Instalment Store | Total value of the fractional sale in the shop is sent only in 
the sales file on all plots. For the remaining cases will be empty.|
|127|139|13|a|next installment value|The value of the next installments of the sale is sent only in the sales file. For customers 
without installment plan will be sent to all parts of the sale, with the exception of the last installment. For customers with 
installment plan will be sent on the first portion and captured in detail the first accelerated share.|
|140|148|9|a|number Invoice|Number of invoice for establishments that capture this information at POS. When not available will be 
formatted with zeros|
|149|152|4|A|Card indicator issued abroad|Identifies whether the card that made the purchase was sent abroad as follows: <ul><li> "0000" 
- Service not assigned </ li> <li> "0001" - issued card in Brazil </ li> <li> "0002" - issued card abroad </li></ul>|
|153|160|8|A|logical terminal number|logical terminal number which was made the sale. When the capture medium is 06, disregard this 
information.|
|161|162|2|Alpha|boarding rate identifier or input value|transaction ID referring to the departure tax or input value: <ul> <li> TX - 
boarding rate; </ li> <li .> VE - input value; </li> <li> White - for other types of transaction </ li> </ ul>|
|163|182|20|alphanumeric|Reference/code request|Reference or code request informed in a transaction mobile payment and e-commerce. When
not available, it will be formatted with white.|
|183|188|6|a|Transaction Time|Transaction Time presented in the format HHMMSS. This information will be generated only in the sales
records of the sales file with original CV. In other cases, the field is formatted with zeros.|
|189|217|29|a|Single transaction Number|Unique number that identifies each transaction|
|218|218|1|Alpha|Cielo Promo Display|Identifier Product Promo Cielo = "S". It identifies that the sale took part in the promotional 
campaign platform. Otherwise it will be formatted with white.|
|219|250|32|alphanumeric|Use Cielo|Blank. Reserved for Cielo.|

* <Sup>(1)</sup> - should be considered 2 decimal places, without commas, periods, or any other character.
* <Sup>(2)</sup> - Available only noas daily files; recovery is not shown.

## Type 5 - Detail of Prepayment of Receivables Operation

It presents the frontloading operation held in Cielo or assignment of receivables (for Bradesco and HSBC) held the day before the file
generation.

|Home|End|Size|Type|Description|Data Edition|
|------|---|-------|----|---------|----------------|
|001|001|1|A|Record Type|Constant "5" - identifies the type of record that presents the information in a frontloading operation.|
|002|011|10|A|Establishment of payment or submission|Number of the establishment and/or branch where the sale took place.|
|012|020|9|A|Anticipation operation number|Number of financial Anticipation operation, also presented in the record type 1 in the RO settlement date.|
|021|028|8|A| Operation credit date|YYYYMMDD - date of payment of the operation./
|029|029|1|Alfa|Sign of the gross anticipation of view|<Ul><li> "+" value identifies the credit </li><li> "-" identifies value debt 
</li></ul>|
|030|042|13|A|Gross value of the anticipation of view|gross value of the order to book in advance. The gross amount of anticipation is the sum of the original amounts net 
of anticipated ROs this agenda.|
|043|043|1|Alfa|Sign of the gross advance installments|<Ul><li> "+" value identifies the credit </ li> <li> "-" identifies value debt </ li></ul>|
|044|056|13|A|Gross value of advance installments|gross amount of advance installments of the agenda. The amount of the advance is the sum of the original amounts 
net of anticipated ROs this agenda.|
|057|057|1|Alfa|Sign of the gross anticipation Electron Pre-Dating|<ul><li> "+" value identifies the credit </ li> <li> "-" identifies value debt </ li> </ ul>.|.
|058|070|13|A|Gross value of anticipation Electron Pre-Dating|gross anticipation of Electron Pre-Dating agenda. The gross amount of anticipation is the sum of the original 
amounts net of anticipated ROs this agenda.|
|071|071|1|Alpha|Sign of the gross total advance|<Ul><li> "+" identifies value credit </ li> <li> "-" identifies value debt </ li> </ul>|
|072|084|13|a|Gross value of total advance|Gross anticipation of agendas in cash installments and Electron Pre-Dating. The gross amount of 
anticipation is the sum of the original amounts net of anticipated ROs.|
|085|085|1|Alpha|Sign the net value of the anticipation of view|<Ul> <li> "+" identifies value credit </li><li> "-" identifies value 
debt </li></ul>|
|086|098|13|a|in anticipating the Net| Net anticipation agenda in sight.|
|099|099|1|Alpha|Net sign of anticipation installments| <Ul> <li> "+" identifies value credit </ li> <li> "-" identifies value debt
</li></ul>|
|100|112|13|a|Net anticipation installments|Net value of advance installments of the agenda.|
|113|113|1|Alpha|Signal net value of Pre-Dating anticipation | <ul> <li> "+" identifies value credit </ li> <li> "-" identifies value 
debt. </li></ul>|
|114|126|13|a|Net Pre-Dating anticipation|Net anticipation of Electron Pre-Dating agenda.|
|127|127|1|Alpha|Sign the net value of the total advance| <Ul> <li> "+" identifies value credit </li><li> "-" identifies value debt 
</li></ul>|
|128|1140|13|a|net total anticipated value|Net anticipation of agendas in cash installments and Electron Pre-Dating.|
|141|145|5|a|Discount rate of anticipation|discount shopping anticipation fee.|
|146|149|4|a|comicílio the Bank Code|Bank in which the amounts were deposited|
|150|154|5|a|household agency code|Agency in which the values ​​were deposited|
|155|168|14|alphanumeric|current account Code of household | Current account in which the values ​​were deposited|
|169|169|1|Alpha|Signal net anticipation|<Ul><li> "+" identifies value credit </ li> <li> "-" identifies value debt </li></ul>|
|170|182|13|a|Net anticipation|Net Value of anticipation.|
|183|250|68|alphanumeric|Use Cielo|Blank Reserved for Cielo.|

## Type 9 - Trailer

Indicates the end of the file.

|Home|End|Size|Type|Description|Data Edition|
|------|---|-------|----|---------|----------------|
|001|001|1|A|Record Type|Constant "9" - Identifies the type of detail trailer record (end of file).|
|002|012|11|A|Total record|Total number of records, which do not include header and trailer.|
|013|250|238|Alphanumeric.|Use Cielo|Reserved for Cielo.|

<Aside class = "notice"> <strong> Note: </strong> fields reserved for Cielo can be used for adding new information. 
It may also be necessary to include new types of different registers 0, 1, 2, 3, 4, 5, 6, 7 or 9. Because of this, we suggest that the reconciliation solution despise records not listed in this specification. </ Aside>

# Tables

## Table I - Extract Option

|Code|Description|
|------|---------|
|03|Selling Installment Plan|
|04|payments|
|06|Prepayment of Receivables|
|07|Assignment of Receivables|
|08|Outstanding installments|
|09|Balance Open|

## Table II - Transaction Type

|Code|Description|
|------|---------|
|01|sale|
|02|Set Credit|
|03|Adjust Debit|
|04|Cielo plan|
|05|rescheduling|

## Table III - Payment Status

|Code|Description|
|------|---------|
|00|Scheduled: identifies the capture of a transaction and informs the forecast of payment. The schedule may be changed|
|01|Pay: identifies that the payment was made by bank domicile |
|02|Sent to the Bank: it identifies the Cielo requested payment / collection for the home bank, but there was no confirmation|
|03|The Confirm: identifies the Cielo requested payment / collection for the home bank, but still there was no confirmation|

<Aside class = "notice"> <strong> Note: </ strong> is important that all payment status are considered </aside>.

* When a credit value is clearing process with a value to debt, both will be sent in the payment file in the clearing date later than 
scheduled payment date. The status sent to the bank to be displayed again in the payment statement when the bank domicile return a 
credit order.

## Table IV - Product Code

|Code|Description|
|------|---------|
|001|AGIPLAN view credit|
|002| AGIPLAN installments shop|
|003|Banescard view credit|
|004|Banescard installments shop|
|005|view credit Esplanade|
|006|Credz view credit|
|007|Terrace installments shop|
|008|Credz installments shop|
|009|Link Crediário|
|010|Mastercard credit view|
|011|Maestro|
|012|Mastercard installments shop|
|013|Link Construcard|
|014|Link Agro Debit|
|015|Link Agro Costing|
|016|Link Agro Investment|
|039|Credsystem installments shop|
|040|Visa Credit sight|
|041|Visa Electron Debit sight|
|042|Visa Toll|
|043|Visa Installment Store|
|044|Visa Electron Pre-Dating|
|045|Allele Meal (Visa issuer/Elo)|
|046|Allele Power (Visa issuer/Elo)|
|058|Link Culture|
|059|Allele Auto|
|061|Sorocred Mortgage view|
|062|Sorocred Installment Store|
|064|Visa Crediário|
|065|Allele meal (Elo Flag)|
|066|Allele Power (Elo Flag)|
|067|Visa Working Capital|
|068|Visa Mortgages|
|017|Link Agro Costing + Debit|
|018|Link Agro Investment + Debit|
|019|Discover credit view|
|020|Diners credit view|
|021|Diners installments shop|
|022|Agro Costing + Electron|
|023|Agro Investment + Electron|
|024|FCO Investment|
|025|Agro Electron|
|026|Agro Costing|
|027|Agro Investment|
|028|FCO Giro|
|033|JCB|
|036|Serve with VISA Debit Card|
|037|Flex Car Visa Vale|
|038|Credsystem view credit|
|069|Culture Visa Vale|
|070|Link Credit|
|071|Link Debit sight|
|072|Link Installment shop|
|079|Carnet payment Visa Electron|
|080|Visa Credit Currency Converter|
|081|Mastercard Specialised Credit (*)|
|089|Link Mortgages|
|091|Link Specialised Credit (*)|
|094|Banescard Debit|
|096|Cabal Credit sight|
|097|Cabal Debit|
|098|Cabal Installment Store|
|342|Master Toll|
|377|Link Carnet|
|378|Master Carnet|
|380|Mastercard Credit Currency Converter|

<Aside class = "notice"> <strong> * </strong> codes concerning Construcard cards, My House Best, Producard and Moveiscard </ aside>

## Table V - Set Source

|Code|Description|Adjustment Type|
|----|-----------|---------------|
|1|restatement Setting|Setting|
|2|payment date Setting|Setting|
|3|Adjustment commission rate|Hit|
|4|Setting values ​​unprocessed|Hit|
|5|Settlement of amounts not received|Hit|
|6|Setting values ​​unrecognized|Hit|
|7|Hit traded values ​​|Hit|
|8|Setting values ​​processed improperly|Hit|
|9|Release Hit uncompensated current account|Hit|
|10|Hit regarding disputed values ​​|Hit|
|11|Temporary Settlement of contested values|Hit|
|12|many Hits|Hit|
|13|Billing Agreement|Hit|
|14|Legal Agreement|Hit|
|15|fine Application Program Monitoring Chargeback|Hit|
|16|value Lock by court order|Lock|
|17|of sale Cancellation|Cancellation|
|18|Operating fare collection|collection|
|19|Monthly Billing Lynx Trade|collection|
|20|Recovery Plan Cielo|collection|
|21|escrow contract|Hit|
|22|cancellation of the return of credit - issuing bank|Hit|
|23|Credit EC - related dispute carrier|Hit|
|24|Credit for cancellation rejected - Cielo|Hit|
|25|duplicate debit processing - Visa Toll|Hit|
|26|Debt held by sale without reading the chip|Hit|
|27|Debit for sale rejected in the system - Cielo|Hit|
|28|Debit regarding the carrier's dispute|Chargeback|
|29|Reversal of legal agreement|Hit|
|30|escrow contract Reversal|Hit|
|31|Reversal collection agreement|Hit|
|32|value lock Reversal by court order|Hit|
|33|selling cancellation Reversal|Hit|
|34|Operating fare collection Reversal|Hit|
|35|Reversal of monthly charge Lynx Trade|Hit|
|36|Reversal collection Cielo Plan|Hit|
|37|sale debit Reversal without reading the Chip|Hit|
|38|Reversal of commercial incentive|Hit|

## Table VI - Issuer

|Code|Description|
|------|---------|
|001|VISA|
|002|Mastercard|
|006|Sorocred|
|007|ELO|
|009|Diners|
|011|AGIPLAN|
|015|Banescard|
|023|Cabal|
|029|Credsystem|
|035|Terrace|
|064|Credz|

## Table VII - Capture Media

|Code|Description|
|------|---------|
|01|POS (Point of Sale)|
|02|POS (Point of Sale) or EFT (Electronic Funds Transfer)|
|03|e-Commerce (E-Commerce)|
|04|EDI (Electronic Data Exchange)|
|05|ADP/BSP (Company video capture)|
|06|Manual|
|07|URA/CVA|
|08|Mobile|
|09|Purse Electronic Network|

## Table VIII - Rejection Reason

|Code|Description|
|------|---------|
|002|Invalid Card|
|023|Other errors|
|031|withdrawal Transaction with card Electron zero value|
|039|Invalid issuing Bank|
|044|Date of transaction invalid|
|02|Invalid authorization code|
|05|Invalid number of installments|
|056|Transaction funded to establish unauthorized|
|057|Card in Protective newsletter|
|061|invalid card number|
|066|unauthorized Transaction|
|067|unauthorized Transaction|
|069|unauthorized Transaction|
|070|unauthorized Transaction|
|92|Issuing bank without communication|
|093|Imbalance in installments plan|
|094|Sale installments for cards issued abroad|
|097|smaller share than the permitted value|
|099|Invalid issuing Bank|
|100|unauthorized Transaction|
|101|Duplicate Transaction|
|102|duplicate Transaction|
|071|unauthorized Transaction|
|072|unauthorized Transaction|
|12|Invalid Transaction|
|074|Invalid transaction value|
|075|invalid card number|
|077|unauthorized Transaction|
|078|unauthorized Transaction|
|079|unauthorized Transaction|
|080|unauthorized Transaction|
|081|expired Card|
|082|unauthorized Transaction|
|083|unauthorized Transaction|
|084|unauthorized Transaction|
|086|unauthorized Transaction|
|124|BIN not registered|
|126|withdrawal transaction with invalid Electron card|
|128|withdrawal transaction with invalid Electron card|
|129|withdrawal transaction with invalid Electron card|
|130|withdrawal transaction with invalid Electron card|
|133|withdrawal transaction with invalid Electron card|
|134|withdrawal transaction with invalid Electron card|
|145|invalid Establishment for distribution|

# Partners

## VAN

Access to Electronic Statement will be made through a mailbox created in the VAN website (company hired by Cielo to manage the sending
and receiving of extracts) or via the receiving system, according to the best option for the client. For, change or problems with the mailbox, contact the respective VAN:

### TIVIT

sup.neg.cielo@tivit.com.br
Contact: (11) 3626 6666
Help Desk: 0800 55 0769 or (11) 3246 8511

### OpenText

suporte.platinum@opentext.com
Contact: (11) 2167 2620

### INTERVAN

Company hired by the client to manage the sending and receiving of files. In this case, the transmission costs of INTERVAN are
the customer's responsibility.

### Conciliator

Company conciliation contracted by the client to carry out the processing of information of files, according to the default layout provided by Cielo, developing and / or providing a reading tool file. In this case, costs are the customer's responsibility.

# Transmission and file forwarding

## File Transmission

To receive the Electronic Statement (EDI) requires that the customer contact specialist care (EDI Island) and complete the purchase 
of the service. The files are available in the mailbox daily except the balance of open file which will be sent monthly.

When there is no movement, the file will be sent only to the "Header" and "Trailer". If there is any inconsistency in the transmission
(s) file (s), the customer must inform the Cielo by contacting EDI Island (edi@cielo.com.br).

## Forward Files

In case of loss of the file or not receiving the VAN provide the mailbox the same file sent daily by Cielo (backup file). The customer
can contact their van to request the court.

## File Recovery

* Enables recovery of a previous movement, updating the status of the releases

* Available for the movements of the last 30 days

* The application must be submitted held for EDI Island.

* Files are available separately from the journal file

# Financial Concepts / Glossary

## Portion of Acceleration

Occurs when maintenance is performed on the installment plan, as cancellation, chargeback or change in the amount of the sale 
installments. The same movement is to advance debts.

## Prepayment of Receivables (ARV)

Service offered to the customer affiliated with Cielo you want to anticipate receipt of your credit sales in cash and installments.

## Adjustment

financial posting to the credit or debit conducted by Cielo to rectify improper collection, cancel a sale or transfer one chargeback 
to the client.

## Extract Chain

This chain has no relation to the chain of payments of the company, so it is important to ask the EDI Island including all the stores 
that should be included in the extract files.

## Chain Centralized Payment

It is a group of customers with the same root CNPJ, bank address and the same trading conditions commission and run. A chain has only 
one mother, chosen by the customer, which are registered all goods and payments imposed in branches. The rules locks and antecipaçãosão the same for all clients in a centralized chain.

## Change Installment Plan

It occurs when the carrier calls the customer a change in the installment plan contracted at first, as a total or partial cancellation
of the sale and change in the amount or value of land.

## Date of Sale

Day on which the sale is conducted at Cielo client. For customers who operate e-commerce solutions, this is the date on which the 
carrier made the purchase on the site and not the date on which it was made to confirm the sale by Cielo client.

## Presentation of Data

Day the sale was submitted for processing in Cielo, being a Saturday, Sunday or holiday. This data is the basis for calculating the 
pay programming and may be different from the date of sale, according to the type of capture solution used.

## Data Capture

Date on which the transaction was captured on Cielo system. Cielo for customers who operate e-commerce solutions, this is the date on 
which the transaction was confirmed by the customer.

## Pay day

payment amount effective the day the customer's current account, considering the period of deferred payment. If the calculated date 
is not a business day, the payment will be made on the first working day. Initially, after the capture of the sale, it informed the 
scheduled date of payment, which may be delayed if the value is used to offset the collection of any debt arising from cancellation of sale, chargeback and billing services. Payment can also be anticipated if the customer performs a preparatory operation.

## Data Submission to the Bank

Date on which the payment file (credit or debit) was sent to the customer's home bank

## Discount Rate / Commission Rate

The discount is always calculated on the Operating Summary (RO) and in the case of installments store sales, the discount is on the 
RO that the parcel is located. The sum of the discount of the total value is demonstrated in the RO.

## Single Number of Transaction

It is awarded by Cielo and identifies each uniquely transaction, allowing the reconciliation of the stock adjustments, anticipation 
and assignment of receivables held both the RO and in the CV to be reconciled by this key To do this, the reconciliation solution 
should use only fixed parts of the single number following this composition.

|Shares|Composition|
|------|----------|
|1|15 fixed positions that identify the summary of operations (RO) in a unique way, keeping the history of maintenance performed on 
Cielo.|
|2|07 variable positions. Identify changes made to the RO.|
|3|04 fixed positions that identify the Sales Receipt (DR) in a RO maintaining its track record in Cielo.|
|4|03 variable positions. They identify changes made to the CV.|

## Installment Shop - Term

Sales file with future installments option, all parcels will be sent with the original date of submission. However, the payment file 
will be shown the date of release of its share.For customers without this option the system will always demonstrate the date of 
submission of the tranche payment for the month. The calculation of the date of payment of all installments is based on the date 
on which the first portion and has a different logic for transactions of each of the flags, as follows:

### VISA, ELO and DINERS

The installments will be released monthly on the same day of the first part, whether the day is useful or not. Thus a sale in 04 
installments presented on 01.10.2015 will have the following release plan (payment term: 30 days):

|Portion|Date of transmission|Date of deposit (Portion of Liberation)| Date of payment|
|-------|--------------------|---------------------------------------|-----------------|
|01/04|10/01/2015|10/01/2015|09/02/2015|
|02/04|10/01/2015|10/02/2015|12/03/2015|
|03/04|10/01/2015|10/03/2015|09/04/2015|
|04/04|10/01/2015|10/04/2015|11/05/2015|

The only exception to the above rule occurs when the day does not exist in the month of release of the plot. Example: Sales whose 
first installment was presented on 01/31/2015. As there is no 31 in February, the share of this month will be presented on the last 
day of the month, ie 02.28.2015.

### MASTERCARD

The filing date of the first installment will also be the base date for the release of all the flat portions, however, the future 
installments will always be 30 days after presentation of the first installment, keeping this logic to the completion of the plan. In the case of sale presented in 10/1/2015 previously used, we note that the installments will be released in 30, 60, 90 after the presentation of the first installment:

|Portion|Date of transmission|Date of deposit (Portion of Liberation)|Date of payment|
|-------|--------------------|---------------------------------------|-----------------|
|01/04| 10/01/2015| 10/01/2015| 09/02/2015|
|02/04| 10/01/2015| 09/02/2015| 11/03/2015|
|03/04| 10/01/2015| 11/03/2015| 10/04/2015|
|04/04| 10/01/2015| 10/04/2015| 11/05/2015|

## Installment shop - Share of Value Rounding

It is always performed in the first portion and occurs where the result of the sale value of the division by the number of portions 
is a periodic tithe. In this case the first installment will be higher than the others.

## Adjustment

one debt adjustment will be generated in the value of the requested cancellation.

## Acceleration

an RO acceleration is generated for each maintenance performed with the amount of shares that were pending payment and were 
accelerated due to the total cancellation of the transaction. For partial cancellation, will accelerate the amount of parcels needed
to compensate for the amount canceled.

## Rescheduling

If the customer has the Sales extract option Installment Plan (03) enabled and there is a partial cancellation of a transaction, the 
system will generate the rescheduling records of parts which are not accelerated. This is because the original payment forecast sent 
has been changed and no accelerated installments assume the maturity of the plots that were accelerated.

Portion ## Rescheduling

Changing the due date of payment of the portion of the installment plan. It occurs when there is a partial cancellation of a plan with 
outstanding installments payment to the customer.

## Summary of Operations (RO)

The RO number identifies a grouping of sales in certain date. It has seven positions and will be formatted as follows. For example, 
a split transaction in the store held on 01/12/2015 4,151,201 will have the RO.

|Location|Description|Data Edition|
|-------|---------|---------------|
|1|Product Type|<ul> <li> 0 - open credit </li> <li> 3 - Outbound installments </ li> <li> 4 - installments store </ li> <li> 5 - card debit </ li> <li> 6 - postdated electron </ li> </ ul> |
|2:03|Year that the transaction was carried out|AA|
|4:05|month the transaction was carried out|MM|
|6:07|Day the transaction was carried out |DD|

## Transaction Rejection

It occurs when the client or the sale does not have the attributes necessary for the proper processing and scheduling of payment. 
The sale is reprocessed manually and sent for further payment.

## Sale

It occurs when there is a change in the payment schedule of a piecemeal sale, either in number of installments or the total value
of the transaction.

## Outstanding balance

It comprises all releases to receive from following the date of calculation. the Cielo payments not made in the past due to 
cancellations and objections should be considered.

To read the balance of extract Open (09), consider the following fields (Type 1 record - RO detail):

* Revolving Balance (due in 30 days)
* Date Sent to the Bank (position 038-043): filter different data 000000.
* Consider the sum of the Net Asset Value (position 087-099)
* Installment Balance (due after 30 days)