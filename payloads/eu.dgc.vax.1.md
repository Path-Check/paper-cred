# **EU's Digital Green Vaccine Certificate** Payload

This Payload is [defined](https://ec.europa.eu/health/sites/health/files/ehealth/docs/digital-green-certificates_dt-specifications_en.pdf) by the EU and contains a COVID-19 Immunization certificate of a **HOLDER** on a JSON Schema.

Fields in the **serialization** order:
1. `nam.fn`: *Optional.* **STRING50**. The family or primary name(s) of the person addressed in the certificate;
1. `nam.gn`: *Optional.* **STRING50**. The given name(s) of the person addressed in the certificate;
1. `nam.fnt`: *Required.* **STRING50**. Standardised family name: The family name(s) of the person transliterated. Regex: `^[A-Z<]*$`
1. `nam.gnt`: *Optional.* **STRING50**. Standardised given name: The given name(s) of the person transliterated. Regex: `^[A-Z<]*$`
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

1. **DATE**: Date type ISO 8601, in [ISO 8601 (YYYYMMDD) Basic Notation]
1. **TIMESTAMP**: Datetime in seconds since Epoch
1. **NUMERIC1**: Single Digit Numeric 1-9. Regex: `[1-9]`
1. **STRING10**: String with 10 chars. Regex: `[A-Z]{1,10}`
1. **STRING50**: String with 50 chars. 

## JSON Payload
When converting the credential back to a JSON structure, verifiers must hardcode this JSON template, replacing `${field}` by the content of `field`
```
{
  "ver": "1.0.0",
  "nam": {
    "fn": "${nam.fn}",
    "gn": "${nam.gn}",
    "fnt": "${nam.fnt}",
    "gnt": "${nam.gnt}"
  },
  "dob": "${dob}",
  "v": [
    {
      "tg": "${v.tg}",
      "vp": "${v.vp}",
      "mp": "${v.mp}",
      "ma": "${v.ma}",
      "dn": "${v.dn}",
      "sd": "${v.sd}",
      "dt": "${v.dt}",
      "co": "${v.co}",
      "is": "${v.is}",
      "ci": "urn:uvci:${v.ci}"
    }
  ]
}
```

### JSON Example:
```
{
  "ver": "1.0.0",
  "nam": {
    "fn": "d'Arsøns - van Halen",
    "gn": "François-Joan",
    "fnt": "DARSONS<VAN<HALEN",
    "gnt": "FRANCOIS<JOAN"
  },
  "dob": "2009-02-28",
  "v": [
    {
      "tg": "840539006",
      "vp": "1119349007",
      "mp": "EU/1/20/1528",
      "ma": "ORG-100030215",
      "dn": 2,
      "sd": 2,
      "dt": "2021-04-21",
      "co": "NL",
      "is": "Ministry of Public Health, Welfare and Sport",
      "ci": "01:NL:PlA8UWS60Z4RZXVALl6GAZ"
    }
  ]
}
```

## Example:
```
CRED:EU.DGC.VAX:1:GBCQEIIAZ24WO7SM36K4J6ZKXOMMNHNIJ5L72D2WOIYPIMU3RJG36SVIQWTQEIA7Z
6WMLK3TFPCWL6O2M7NH2ZNDHDIL7M73NIXEVGQ3D467JABILU:1A9.PCF:D'ARS%C3%98NS%20-%20VAN%2
0HALEN/FRAN%C3%87OIS-JOAN/DARSONS%3CVAN%3CHALEN/FRANCOIS%3CJOAN/2009-02-28/84053900
6/1119349007/EU%2F1%2F20%2F1528/ORG-100030215/2/2/2021-04-27/NL/MINISTRY%20OF%20VWS
/01%3ANL%3APLA8UWS60Z4RZXVALL6GAZ
``` 