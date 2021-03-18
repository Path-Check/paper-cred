# **Mass AAMVA DL/ID specification** Payload

The **Massachusetts AAMVA Driver License/Identification** QR code is designated to contain information about a Massachusetts Driver's License V3 from a **HOLDER**.

Fields in the **serialization** order:
1. `IIN`: *Required.* Issuer Identification Number which uniquely identifies the issuing jurisdiction
1. `AAMVAVersion`: *Required.* 0=pre-specification, 1=2000, 2=2003, 3=2005, 4=2009, 5=2010, 6=2011, 7=2012, 8=2013, 9=2016
1. `jurisdictionVersion`: *Required.*  jurisdiction specific version number of the implementation
1. `DCA`: *Required.* Jurisdiction-specific vehicle class/group code
1. `DCB`: *Required.* Jurisdiction-specific codes that represent restrictions to driving privileges
1. `DCD`: *Required.* Jurisdiction-specific codes that represent additional privileges granted to the cardholder beyond the vehicle class
1. `DBA`: *Required.* **DATE**. Expiration Date
1. `DAC`: *Required.* First name
1. `DAD`: *Required.* Middle Name(s), separated by a comma
1. `DCS`: *Required.* Family name
1. `DCT`: *Required.* Given name (all names other than the family name)
1. `DBD`: *Required.* **DATE**. Date on which the document was issued.
1. `DBB`: *Required.* **DATE**. Date of Birth Date
1. `DBC`: *Required.* Gender of the cardholder 
    | Code | Description |
    | ---- | ----------- |
    | 0    | Unknown |
    | 1    | Male |
    | 2    | Female|
    | 9    | Not specified|
1. `DAY`: *Required.* ANSI D-20 eye color code (3 letters): 
    | Code | Description |
    | ---- | ----------- |
    | BLK  | Black |
    | BLU  | Blue |
    | BRO  | Brown |
    | GRY  | Gray |
    | GRN  | Green |
    | HAZ  | Hazel |
    | MAR  | Maroon |
    | PNK  | Pink |
    | DIC  | Dichromatic |
    | UNK  | Unknown |
1. `DAU`: *Required.* Height of cardholder.
1. `DAG`: *Required.* Street portion of the cardholder address.
1. `DAI`: *Required.* City portion of the cardholder address.
1. `DAJ`: *Required.* State portion of the cardholder address.
1. `DAK`: *Required.* Postal code portion of the cardholder address. A ZIP code is parsed as: ‘5digit’ : 5-digit postal address (ZIP), ‘9digit’ : 9-digit postal address (ZIP+4)
1. `DAQ`: *Required.* Customer ID
1. `DCF`: *Required.* Document Discriminator Number
1. `DCG`: *Required.* Country (‘USA’ or ‘CAN’)
1. `DCH`: *Required.* Federal Commercial Vehicle Codes
1. `DDE`: *Required.* Family name truncation. Truncated (‘T’), has not been truncated (‘N’), or unknown whether truncated (‘U’).
1. `DDF`: *Required.* First name truncation. Truncated (‘T’), has not been truncated (‘N’), or unknown whether truncated (‘U’).
1. `DDG`: *Required.* Middle name truncation. Truncated (‘T’), has not been truncated (‘N’), or unknown whether truncated (‘U’).
1. `DAH`: *Optional.* Second line of street portion of the cardholder address.
1. `DAZ`: *Optional.* Hair color. Can be written out or ANSI D-20 hair color code
    | Code | Description | 
    | ---- | ----------- |
    | BAL | Bald | 
    | BLK | Black | 
    | BLN | Blond | 
    | BRO | Brown | 
    | GRY | Grey | 
    | RED | Red/Auburn | 
    | SDY | Sandy | 
    | WHI | White | 
    | UNK | Unknown | 
1. `DCI`: *Optional.* Place of birth
1. `DCJ`: *Optional.* Audit information
1. `DCK`: *Optional.* Inventory control number
1. `DBN`: *Optional.* Alias/AKA Family Name
1. `DBG`: *Optional.* Alias/AKA Given Name
1. `DBS`: *Optional.* Alias/AKA Suffix Name
1. `DCU`: *Optional.* Name Suffix (can be ‘JR’, ‘SR’, ‘1ST’, ‘2ND’, ‘3RD’, ‘4TH’, ‘5TH’, ‘6TH’, ‘7TH’, ‘8TH’, ‘9TH’, ‘I’, ‘II’, ‘III’, ‘IV’, ‘V’, ‘VI’, ‘VII’, ‘VIII’ or ‘IX’)
1. `DCE` Weight Range
    | Code | Description |
    | ---- | ----------- |
    | 0 | up to 31 kg (up to 70 lbs)|
    | 1 | 32 – 45 kg (71 – 100 lbs)|
    | 2 | 46 - 59 kg (101 – 130 lbs)|
    | 3 | 60 - 70 kg (131 – 160 lbs)|
    | 4 | 71 - 86 kg (161 – 190 lbs)|
    | 5 | 87 - 100 kg (191 – 220 lbs)|
    | 6 | 101 - 113 kg (221 – 250 lbs)|
    | 7 | 114 - 127 kg (251 – 280 lbs)|
    | 8 | 128 – 145 kg (281 – 320 lbs) |
    | 9 | 146+ kg (321+ lbs) |
1. `DCL`: *Optional.* D-20 Code for Race/Ethnicity
    | Code | Description |
    | ---- | ----------- |
    | AI   | Alaskan or American Indian  | 
    | AP   | Asian or Pacific Islander   | 
    | BK   | Black  | 
    | W    | White   | 
    | H    | Hispanic Origin  | 
    | O    | Not of Hispanic Origin  | 
    | U    | Unknown    | 
1. `DCM`: *Optional.* Standard vehicle classification
1. `DCN`: *Optional.* Standard endorsement code
1. `DCO`: *Optional.* Standard restriction code
1. `DCP`: *Optional.* Jurisdiction-specific vehicle classification description
1. `DCQ`: *Optional.* Jurisdiction-specific endorsement code description
1. `DCR`: *Optional.* Jurisdiction-specific restriction code description
1. `DDA`: *Optional.* Compliance Type, ‘F’ = fully compliant and ‘N’ = non-compliant.
1. `DDB`: *Optional.* Card Revision Date
1. `DDC`: *Optional.* Date on which the hazardous material endorsement granted by the document is no longer valid
1. `DDD`: *Optional.* Indicator that the cardholder has temporary lawful status, can be 1 or 0.
1. `DAW`: *Optional.* Weight in pounds
1. `DAX`: *Optional.* Weight in kilograms
1. `DDH`: *Optional.* **DATE**. Date on which the cardholder turns 18
1. `DDI`: *Optional.* **DATE**. Date on which the cardholder turns 19
1. `DDJ`: *Optional.* **DATE**. Date on which the cardholder turns 21
1. `DDK`: *Optional.* Indicator that the cardholder is an organ donor, can be ‘1’ or ‘0’
1. `DDL`: *Optional.* Indicator that the cardholder is a veteran, can be ‘1’ or ‘0’
1. `ZVA`: *Optional.* Indicator that the cardholder is a veteran, can be ‘1’ or ‘0’



## Example:
```
CRED:US.MA.ID:3:GBDAEIIAWILNF3EPL2RFVJVUJYCHTVHCDOISXS3IM7I2ZYSVEXJVJ4LITG7QEIIAROBU
XXJ3PV5ZNVKRDG45XNMQ3MNF4E6PRUV5R367EU6LVPRP3BOA:1A9.PCF:636000/9/0/D/K/PH/20241210/
JOHN//SAMPLE//20160606/19860606/1/BRO/173/2300%20WEST%20BROAD%20STREET/RICHMOND/VA/2
32690000/T64235789/2424244747474786102204/USA//APT%2022/WHI/BOSTON%2C%20MA//12345678
9////JR/4/BK///////F/20080606/20090606/1///20040606/20050606/20070606/1/0/01
```

