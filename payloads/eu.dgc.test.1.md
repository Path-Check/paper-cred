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
1. `t.ma`: *Optional.* **STRING**. Test Manufacturer
    | Code | Description | 
    | ---- | ----------- |
    | 1232 | --------- | 
    | 1304 | --------- | 
    | 1065 | --------- | 
    | 1331 | --------- | 
    | 1484 | --------- | 
    | 1242 | --------- | 
    | 1223 | --------- | 
    | 1173 | --------- | 
    | 1244 | --------- | 
    | 1360 | --------- | 
    | 1363 | --------- | 
    | 1767 | --------- | 
    | 1333 | --------- | 
    | 1268 | --------- | 
    | 1180 | --------- | 
    | 1481 | --------- | 
    | 1162 | --------- | 
    | 1271 | --------- | 
    | 1341 | --------- | 
    | 1097 | --------- | 
    | 1489 | --------- | 
    | 344" | --------- | 
    | 345" | --------- | 
    | 1218 | --------- | 
    | 1278 | --------- | 
    | 1343 | --------- | 
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