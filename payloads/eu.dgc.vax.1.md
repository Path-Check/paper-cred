# **EU's Digital Green Vaccine Certificate** Payload

This Payload is [defined](https://ec.europa.eu/health/sites/health/files/ehealth/docs/digital-green-certificates_dt-specifications_en.pdf) by the EU and contains a COVID-19 Immunization certificate of a **HOLDER** on a JSON Schema.

Fields in the **serialization** order:
1. `nam`: *Required.* **STRING50**. Surname(s) and forename(s), in that order;
1. `dob`: *Required.* **DATE**. Date of birth;
1. `v.tg`: *Required.* **STRING50**. Disease or Agent targeted
    | Code | Description | 
    | ---- | ----------- |
    | 840539006 | COVID19 |
1. `v.vp`: *Required.* **STRING50**. Vaccine/prophylaxis
    | Code | Description | 
    | ---- | ----------- |
    | 1119305005 | SARS-CoV2 antigen |
    | 1119349007 | SARS-CoV2 mRNA |
    | J07BX03 | Covid-19 |
1. `v.mp`: *Required.* **STRING50**. Vaccine Medicinal Product
    | Code | Description | 
    | ---- | ----------- |
    | EU/1/20/1528 | Comirnaty |
    | EU/1/20/1507 | Moderna |
    | EU/1/21/1529 | Vaxzevria |
    | EU/1/20/1525 | Janssen |
    | CVnCoV | CVnCoV |
    | NVX-CoV2373 | NVXCoV2373 |
    | Sputnik-V | Sputnik V |
    | Convidecia | Convidecia |
    | EpiVacCorona | EpiVacCorona |
    | BBIBP-CorV | BBIBP-CorV |
    | Inactivated-SARS-CoV-2-Vero-Cell | Inactivated SARS-CoV-2 Vero Cell |
    | CoronaVac | CoronaVac |
    | Covaxin | Covaxin |
1. `v.ma`: *Required.* **STRING50**. Vaccine Marketing Authorization holder or Manufacturer
    | Code | Description | 
    | ---- | ----------- |
    | ORG-100001699 | AstraZeneca AB |
    | ORG-100030215 | Biontech Manufacturing GmbH |
    | ORG-100001417 | Janssen-Cilag International |
    | ORG-100031184 | Moderna Biotech Spain S.L. |
    | ORG-100006270 | Curevac AG |
    | ORG-100013793 | CanSino Biologics |
    | ORG-100020693 | China Sinopharm International Corp |
    | ORG-100010771 | Sinopharm Weiqida Europe Pharmaceutical s.r.o |
    | ORG-100024420 | Sinopharm Zhijun Pharmaceutical Co. Ltd |
    | ORG-100032020 | Novavax CZ AS |
    | Gamaleya-Research-Institute | Gamaleya Research Institute |
    | Vector-Institute | Vector Institute |
    | Sinovac-Biotech | Sinovac Biotech |
    | Bharat-Biotech | Bharat Biotech |
1. `v.dn`: *Required.* **NUMERIC1**. Dose Number
1. `v.sd`: *Required.* **STRING50**. Total Series of Doses
1. `v.dt`: *Required.* **DATE**. Date of Vaccination, indicating the date of the latest dose received
1. `v.co`: *Required.* **STRING10**. Country - Member State of vaccination: ISO 3166 Country Codes (2-letter codes)
1. `v.is`: *Required.* **STRING50**. Issuer
1. `v.ci`: *Required.* **STRING50**. Unique certificate identifier.

## Types

1. DATE: Date type ISO 8601 - date part only, restricted to range 1900-2099. Regex: `[19|20][0-9][0-9]-(0[1-9]|1[0-2])-([0-2][1-9]|3[0|1])`
1. NUMERIC1: Single Digit Numeric 1-9. Regex: `[1-9]`
1. STRING10: String with 10 chars. Regex: `[A-Z]{1,10}`
1. STRING50: String with 50 chars. 

## Example:
```
CRED:EU.DGC.VAX:1:GBDAEIIAZTKNS5MUFU3MZXYEHZRV77HSXMUFTHFPK477HDF6RWPYNMRYEBMAEIIARC
5F3HL3QBINBHKFYERUY6L5WPVMDHBHR4JPJ6PIYYVDRCXQTCOA:1A9.PCF:JOHN%20DOE/1982-01-01/840
539006/1119349007/EU%2F1%2F20%2F1507/ORG-100031184/1/2/2021-04-23/GB/GB%20ISSUER/SOM
E-UNIQUE-ID
``` 