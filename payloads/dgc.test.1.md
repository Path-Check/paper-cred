# **EU's Digital COVID Test Certificate** Payload

This Payload is [defined](https://ec.europa.eu/health/sites/health/files/ehealth/docs/digital-green-certificates_dt-specifications_en.pdf) by the EU and contains a COVID-19 Test certificate of a **HOLDER** on a JSON Schema.

Fields in the **serialization** order:
1. `nam.fn`: *Optional.* **STRING50**. The family or primary name(s) of the person addressed in the certificate;
1. `nam.gn`: *Optional.* **STRING50**. The given name(s) of the person addressed in the certificate;
1. `nam.fnt`: *Required.* **STRING50**. Standardised family name: The family name(s) of the person transliterated. Regex: `^[A-Z<]*$`
1. `nam.gnt`: *Optional.* **STRING50**. Standardised given name: The given name(s) of the person transliterated. Regex: `^[A-Z<]*$`
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
1. `t.sc`: *Required.* **TIMESTAMP**. Date of Sample Collection
1. `t.dr`: *Optional.* **TIMESTAMP**. "Date/Time of Test Result
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
  "t": [
    {
      "tg": "${t.tg}",
      "tt": "${t.tt}",
      "tr": "${t.tr}",
      "ma": "${t.ma}",
      "sc": "${t.sc}",
      "dr": "${t.dr}",
      "tc": "${t.tc}",
      "co": "${t.co}",
      "is": "${t.is}",
      "ci": "urn:uvci:${t.ci}"
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
  "t": [
    {
      "tg": "840539006",
      "tt": "LP217198-3",
      "tr": "260415000",
      "ma": "1232",
      "sc": "2021-04-13T14:20:00+00:00",
      "dr": "2021-04-13T14:40:01+00:00",
      "tc": "GGD Fryslân, L-Heliconweg",
      "co": "NL",
      "is": "Ministry of Public Health, Welfare and Sport",
      "ci": "urn:uvci:01:NL:GGD/81AAH16AZ"
    }
  ]
}
```

## Example:
```
CRED:DGC.TEST:1:GBCAEIAN6XEFUY3EGOQET4EQSKBWDEXCURWCDMTYTFEIBFPKEC5MWHNUFUB
CAN4V6KQGF7QWFZW45KKATORMOOUTUD6ZRG4M66EUXUZ64WIXNIYP:1A9.PCF.PW:D'ARS%C3%9
8NS%20-%20VAN%20HALEN/FRAN%C3%87OIS-JOAN/DARSONS%3CVAN%3CHALEN/FRANCOIS%3CJ
OAN/20090227/840539006/LP217198-3/COVID%20PCR/1232/1618323600/1618324801/26
0415000/GGD%20FRYSL%C3%82N%2C%20L-HELICONWEG/NL/MINISTRY%20OF%20VWS/01%3ANL
%3AGGD%2F81AAH16AZ
``` 