# **EU's Digital COVID Recovery Certificate** Payload

This Payload is [defined](https://ec.europa.eu/health/sites/health/files/ehealth/docs/digital-green-certificates_dt-specifications_en.pdf) by the EU and contains a COVID-19 Recovery certificate of a **HOLDER** on a JSON Schema.

Fields in the **serialization** order:
1. `nam.fn`: *Optional.* **STRING50**. The family or primary name(s) of the person addressed in the certificate;
1. `nam.gn`: *Optional.* **STRING50**. The given name(s) of the person addressed in the certificate;
1. `nam.fnt`: *Required.* **STRING50**. Standardised family name: The family name(s) of the person transliterated. Regex: `^[A-Z<]*$`
1. `nam.gnt`: *Optional.* **STRING50**. Standardised given name: The given name(s) of the person transliterated. Regex: `^[A-Z<]*$`
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
  "r": [
    {
      "tg": "${r.tg}",
      "fr": "${r.fr}",
      "co": "${r.co}",
      "is": "${r.is}",
      "df": "${r.df}",
      "du": "${r.du}",
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
  "r": [
    {
      "tg": "840539006",
      "fr": "2021-04-21",
      "co": "NL",
      "is": "Ministry of Public Health, Welfare and Sport",
      "df": "2021-05-01",
      "du": "2021-10-21",
      "ci": "urn:uvci:01:NL:LSP/REC/1289821"
    }
  ]
}
```

## Example:
```
CRED:DGC.RECV:1:GBCQEIBSJNA47B3L22FJUJLM7VWQI54AZXQVZ25OD7GOIHS2ATGCH6GT3MBCC
AFVWGPMKO5PIUSMO4CHHIXUOMQSN6IVU4XAVCW5DUW3BVWVERZAB4:1A9.PCF.PW:D'ARS%C3%98N
S%20-%20VAN%20HALEN/FRAN%C3%87OIS-JOAN/DARSONS%3CVAN%3CHALEN/FRANCOIS%3CJOAN/
20090227/840539006/20210420/20210430/20211020/NL/MINISTRY%20OF%20VWS/01%3ANL%
3ALSP%2FREC%2F1289821
``` 