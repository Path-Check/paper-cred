# **EU's Digital COVID Test Certificate** Payload

This Payload is [defined](https://ec.europa.eu/health/sites/health/files/ehealth/docs/digital-green-certificates_dt-specifications_en.pdf) by the EU and contains a COVID-19 Test certificate of a **HOLDER** on a JSON Schema.

Fields in the **serialization** order:
1. `nam`: *Required.* **STRING**. Surname(s) and forename(s), in that order
1. `dob`: *Required.* **DATE**. Date of birth
1. `t.tg`: *Required.* **STRING**. Disease or Agent targeted
    | Code | Description | 
    | ---- | ----------- |
    | 840539006 | COVID19 |
1. `t.tt`: *Required.* **STRING**. Type of Test 
    | Code | Description | 
    | LP6464-4 | Nucleic acid amplification with probe detection |
    | LP217198-3 | Rapid immunoassay |
1. `t.nm`: *Optional.* **STRING**. Test Name
1. `t.ma`: *Optional.* **STRING**. [Test Manufacturer](https://github.com/ehn-digital-green-development/ehn-dgc-schema/blob/4ad15f7128236482c5f7bfe7d0271d94bce6a7af/Lookup-tables/DGC-RAT-lookup.json)
    | Code | Description | 
    | ---- | ----------- |
    | 1232 | Abbott Rapid Diagnostics, Panbio COVID-19 Ag Test | 
    | 1304 | AMEDA Labordiagnostik GmbH, AMP Rapid Test SARS-CoV-2 Ag | 
    | 1065 | Becton Dickinson, Veritor System Rapid Detection of SARS-CoV-2 | 
    | 1331 | Beijing Lepu Medical Technology Co., Ltd, SARS-CoV-2 Antigen Rapid Test Kit | 
    | 1484 | Beijing Wantai Biological Pharmacy Enterprise Co., Ltd, Wantai SARS-CoV-2 Ag Rapid Test (FIA) | 
    | 1242 | Bionote, Inc, NowCheck COVID-19 Ag Test | 
    | 1223 | BIOSYNEX SWISS SA, BIOSYNEX COVID-19 Ag BSS | 
    | 1173 | CerTest Biotec, S.L., CerTest SARS-CoV-2 Card test | 
    | 1244 | GenBody, Inc, Genbody COVID-19 Ag Test | 
    | 1360 | Guangdong Wesail Biotech Co., Ltd, COVID-19 Ag Test Kit | 
    | 1363 | Hangzhou Clongene Biotech Co., Ltd, Covid-19 Antigen Rapid Test Kit | 
    | 1767 | Healgen Scientific Limited Liability Company, Coronavirus Ag Rapid Test Cassette | 
    | 1333 | Joinstar Biomedical Technology Co., Ltd, COVID-19 Rapid Antigen Test (Colloidal Gold) | 
    | 1268 | LumiraDX UK Ltd, LumiraDx SARS-CoV-2 Ag Test | 
    | 1180 | MEDsan GmbH, MEDsan SARS-CoV-2 Antigen Rapid Test | 
    | 1481 | MP Biomedicals Germany GmbH, Rapid SARS-CoV-2 Antigen Test Card | 
    | 1162 | Nal von minden GmbH, NADAL COVID-19 Ag Test | 
    | 1271 | Precision Biosensor, Inc, Exdia COVID-19 Ag | 
    | 1341 | Qingdao Hightop Biotech Co., Ltd, SARS-CoV-2 Antigen Rapid Test (Immunochromatography) | 
    | 1097 | Quidel Corporation, Sofia SARS Antigen FIA | 
    | 1489 | Safecare Biotech (Hangzhou) Co. Ltd, COVID-19 Antigen Rapid Test Kit (Swab) | 
    | 344" | SD BIOSENSOR Inc, STANDARD F COVID-19 Ag FIA | 
    | 345" | SD BIOSENSOR Inc, STANDARD Q COVID-19 Ag Test | 
    | 1218 | Siemens Healthineers, CLINITEST Rapid Covid-19 Antigen Test | 
    | 1278 | Xiamen Boson Biotech Co. Ltd, Rapid SARS-CoV-2 Antigen Test Card | 
    | 1343 | Zhejiang Orient Gene Biotech, Coronavirus Ag Rapid Test Cassette (Swab) | 
1. `t.sc`: *Required.* **DATE**. Date of Sample Collection
1. `t.dr`: *Optional.* **DATE**. "Date/Time of Test Result
1. `t.tr`: *Required.* **STRING**. Test Result
    | Code | Description | 
    | ---- | ----------- |
    | 260415000 | Not detected |
    | 260373001 | Detected  |
1. `t.tc`: *Required.* **STRING**. Test Centre
1. `t.co`: *Required.* **STRING**. Country - Member State of vaccination
1. `t.is`: *Required.* **STRING**. Issuer
1. `t.ci`: *Required.* **STRING**. Unique certificate identifier.

## Types

1. DATE: Date type ISO 8601 - date part only, restricted to range 1900-2099. Regex: `[19|20][0-9][0-9]-(0[1-9]|1[0-2])-([0-2][1-9]|3[0|1])`
1. NUMERIC1: Single Digit Numeric 1-9. Regex: `[1-9]`
1. STRING10: String with 10 chars. Regex: `[A-Z]{1,10}`
1. STRING50: String with 50 chars. 

## Example:
```
CRED:EU.DGC.TEST:1:GBCQEIBSLX2TBQAEG4DZOHXLWXFZWP32EBAQIJUCUOE4KRX4IYGB6E5EPUBCCAHZ7
T5K74OKAU4SB5YCPYVJ24OGPXEJJQHQJ6QXYARGBCBRGBFTBY:1A9.PCF:JOHN%20DOE/1982-01-01/8405
39006/LP6464-4/COVID%20PCR/1232/2021-04-20/2021-04-22/260373001/TEST%20CENTER/GB/GB%
20ISSUER/SOME-UNIQUE-ID
``` 