# **EU's Digital COVID Recovery Certificate** Payload

This Payload is [defined](https://ec.europa.eu/health/sites/health/files/ehealth/docs/digital-green-certificates_dt-specifications_en.pdf) by the EU and contains a COVID-19 Recovery certificate of a **HOLDER** on a JSON Schema.

Fields in the **serialization** order:
1. `nam`: *Required.* **STRING**. Surname(s) and forename(s), in that order;
1. `dob`: *Required.* **DATE**. Date of birth. 
1. `r.tg`: *Required.* **STRING**. Disease or Agent targeted
    | Code | Description | 
    | ---- | ----------- |
    | 840539006 | COVID19 |
1. `r.fr`: *Required.* **DATE**. Date of First Positive Test Result
1. `r.df`: *Required.* **DATE**. Certificate Valid From
1. `r.du`: *Required.* **DATE**. Certificate Valid Until
1. `r.co`: *Required.* **STRING10**. Country - Member State of vaccination
1. `r.is`: *Required.* **STRING50**. Issuer
1. `r.ci`: *Required.* **STRING50**. Unique certificate identifier.

## Types

1. DATE: Date type ISO 8601 - date part only, restricted to range 1900-2099. Regex: `[19|20][0-9][0-9]-(0[1-9]|1[0-2])-([0-2][1-9]|3[0|1])`
1. NUMERIC1: Single Digit Numeric 1-9. Regex: `[1-9]`
1. STRING10: String with 10 chars. Regex: `[A-Z]{1,10}`
1. STRING50: String with 50 chars. 

## Example:
```
CRED:EU.DGC.RECV:1:GBDAEIIAZNI25NOPTBTDHDFDXCS6NHCB27QKB5J6GU4NUVACFVWLXCH
FNTAQEIIA6QWV4FITWNWOQVUJ3H6HDY6WOAAUJ5I4KADHSD4ZZYPGPQFA6ECQ:1A9.PCF:JOHN
%20DOE/1982-01-01/840539006/2021-04-22/2021-04-20/2021-10-20/GB/GB%20ISSUE
R/SOME-UNIQUE-ID
``` 