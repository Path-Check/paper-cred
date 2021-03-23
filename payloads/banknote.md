# **BankNote** Payload

The **BankNote** QR code is designated to contain information about a bank note and printed to the note itself to serve as a form of authentication. 

Fields in the **serialization** order:

1. `country`: *Required.* **STRING**.
1. `issuer`: *Required.* **STRING**.
1. `date`: *Optional.*  **DATE**. The issuing date of the note
1. `lot-year`: *Required.* **SHORTSTRING**. The lot number (series year) of the note
1. `serial`: *Required.* **NUMERIC**. Serial Number of the note
1. `denomination`: *Required.* **NUMERIC**. The denomination of the note
1. `currency-name`: *Required.* **STRING**. The name of the currency
1. `indicator`: *Optional.* **SHORTSTRING**. A letter and number designation that corresponds to one of the 12 Federal Reserve Banks
1. `face-plate-number`: *Optional.* **SHORTSTRING**. Identify the printing plates used to print the face side of the note.
1. `back-plate-number`: *Optional.* **SHORTSTRING**. Identify the printing plates used to print the back side of the note. 
1. `position`: *Optional.* **SHORTSTRING**. Identify the position on a plate a note was printed

## Example:


```
CRED:BANKNOTE:1:GBCAEIDOHBJSAYKAI7C3CZ2ZXYVEU2FDXMMOXMCML3NJSC43KVGUP66NKYBCAEBJJB5RAMQB4FLQE3MO3R
5NBGCNILDJTUGR34GQYUTLNZDYGK4K:1A9.PCF:UNITED%20STATES%20OF%20AMERICA/FEDERAL%20RESERVE//2006/IE%2
025507922C/5.00/DOLLAR/E5/FWH103/437/H4
```

