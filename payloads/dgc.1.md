# **EU's Digital Green Certificate** Payload

This Payload is [defined](https://ec.europa.eu/health/sites/health/files/ehealth/docs/digital-green-certificates_dt-specifications_en.pdf) by the EU and contains a combined set of COVID-19 Vaccination, Test and Recovery certificates of a **HOLDER**.

Fields in the **serialization** order:
1. `nam.fn`: *Optional.* **STRING50**. The family or primary name(s) of the person addressed in the certificate;
1. `nam.gn`: *Optional.* **STRING50**. The given name(s) of the person addressed in the certificate;
1. `nam.fnt`: *Required.* **STRING50**. Standardised family name: The family name(s) of the person transliterated. Regex: `^[A-Z<]*$`
1. `nam.gnt`: *Optional.* **STRING50**. Standardised given name: The given name(s) of the person transliterated. Regex: `^[A-Z<]*$`
1. `dob`: *Required.* **DAYSFROMISSUING**. Date of birth
1. `iat`: *Required.* **TIMESTAMP**. Issued At
1. `exp`: *Required.* **SECONDSFROMISSUINGBASE32**. Expiration
1. `iss`: *Optional.* **STRING50**. Issuing Country

1. `nvs`: *Required.* **NUMERIC**. Number of Vaccinations in this record
    1. `v.tg`: *Required.* **BASE32STRING**. Disease or Agent targeted. Codes below are converted from Base10 to Base32
        Code | Description | 
        ---- | ----------- |
        840539006 | COVID19 |
    1. `v.vp`: *Required.* **STRING50**. Vaccine/prophylaxis
        Code | Description | 
        ---- | ----------- |
        1119305005 | SARS-CoV2 antigen |
        1119349007 | SARS-CoV2 mRNA |
        J07BX03 | Covid-19 |
    1. `v.mp`: *Required.* **STRING50**. Vaccine Medicinal Product
        Code | Description | 
        ---- | ----------- |
        EU/1/20/1528 | Comirnaty |
        EU/1/20/1507 | Moderna |
        EU/1/21/1529 | Vaxzevria |
        EU/1/20/1525 | Janssen |
        CVnCoV | CVnCoV |
        NVX-CoV2373 | NVXCoV2373 |
        Sputnik-V | Sputnik V |
        Convidecia | Convidecia |
        EpiVacCorona | EpiVacCorona |
        BBIBP-CorV | BBIBP-CorV |
        Inactivated-SARS-CoV-2-Vero-Cell | Inactivated SARS-CoV-2 Vero Cell |
        CoronaVac | CoronaVac |
        Covaxin | Covaxin |
    1. `v.ma`: *Required.* **STRING50**. Vaccine Marketing Authorization holder or Manufacturer
        Code | Description | 
        ---- | ----------- |
        ORG-100001699 | AstraZeneca AB |
        ORG-100030215 | Biontech Manufacturing GmbH |
        ORG-100001417 | Janssen-Cilag International |
        ORG-100031184 | Moderna Biotech Spain S.L. |
        ORG-100006270 | Curevac AG |
        ORG-100013793 | CanSino Biologics |
        ORG-100020693 | China Sinopharm International Corp |
        ORG-100010771 | Sinopharm Weiqida Europe Pharmaceutical s.r.o |
        ORG-100024420 | Sinopharm Zhijun Pharmaceutical Co. Ltd |
        ORG-100032020 | Novavax CZ AS |
        Gamaleya-Research-Institute | Gamaleya Research Institute |
        Vector-Institute | Vector Institute |
        Sinovac-Biotech | Sinovac Biotech |
        Bharat-Biotech | Bharat Biotech |
    1. `v.dn`: *Required.* **NUMERIC1**. Dose Number
    1. `v.sd`: *Required.* **STRING50**. Total Series of Doses
    1. `v.dt`: *Required.* **DAYSFROMISSUINGBASE32**. Date of Vaccination, indicating the date of the latest dose received
    1. `v.co`: *Required.* **STRING10**. Country - Member State of vaccination: ISO 3166 Country Codes (2-letter codes)
    1. `v.is`: *Required.* **STRING50**. Issuer
    1. `v.ci`: *Required.* **STRING50**. Unique certificate identifier.


1. `nts`: *Required.* **NUMERIC**. Number of Vaccinations in this record
    1. `t.tg`: *Required.* **BASE32STRING**. Disease or Agent targeted. Codes below are converted from Base10 to Base32
        Code | Description | 
        ---- | ----------- |
        840539006 | COVID19 |
    1. `t.tt`: *Required.* **STRING**. Type of Test 
        Code | Description | 
        LP6464-4 | Nucleic acid amplification with probe detection |
        LP217198-3 | Rapid immunoassay |
    1. `t.nm`: *Optional.* **STRING**. Test Name
    1. `t.ma`: *Optional.* **STRING**. [Test Manufacturer](https://github.com/ehn-digital-green-development/ehn-dgc-schema/blob/4ad15f7128236482c5f7bfe7d0271d94bce6a7af/Lookup-tables/DGC-RAT-lookup.json)
        Code | Description | 
        ---- | ----------- |
        1232 | Abbott Rapid Diagnostics, Panbio COVID-19 Ag Test | 
        1304 | AMEDA Labordiagnostik GmbH, AMP Rapid Test SARS-CoV-2 Ag | 
        1065 | Becton Dickinson, Veritor System Rapid Detection of SARS-CoV-2 | 
        1331 | Beijing Lepu Medical Technology Co., Ltd, SARS-CoV-2 Antigen Rapid Test Kit | 
        1484 | Beijing Wantai Biological Pharmacy Enterprise Co., Ltd, Wantai SARS-CoV-2 Ag Rapid Test (FIA) | 
        1242 | Bionote, Inc, NowCheck COVID-19 Ag Test | 
        1223 | BIOSYNEX SWISS SA, BIOSYNEX COVID-19 Ag BSS | 
        1173 | CerTest Biotec, S.L., CerTest SARS-CoV-2 Card test | 
        1244 | GenBody, Inc, Genbody COVID-19 Ag Test | 
        1360 | Guangdong Wesail Biotech Co., Ltd, COVID-19 Ag Test Kit | 
        1363 | Hangzhou Clongene Biotech Co., Ltd, Covid-19 Antigen Rapid Test Kit | 
        1767 | Healgen Scientific Limited Liability Company, Coronavirus Ag Rapid Test Cassette | 
        1333 | Joinstar Biomedical Technology Co., Ltd, COVID-19 Rapid Antigen Test (Colloidal Gold) | 
        1268 | LumiraDX UK Ltd, LumiraDx SARS-CoV-2 Ag Test | 
        1180 | MEDsan GmbH, MEDsan SARS-CoV-2 Antigen Rapid Test | 
        1481 | MP Biomedicals Germany GmbH, Rapid SARS-CoV-2 Antigen Test Card | 
        1162 | Nal von minden GmbH, NADAL COVID-19 Ag Test | 
        1271 | Precision Biosensor, Inc, Exdia COVID-19 Ag | 
        1341 | Qingdao Hightop Biotech Co., Ltd, SARS-CoV-2 Antigen Rapid Test (Immunochromatography) | 
        1097 | Quidel Corporation, Sofia SARS Antigen FIA | 
        1489 | Safecare Biotech (Hangzhou) Co. Ltd, COVID-19 Antigen Rapid Test Kit (Swab) | 
        344" | SD BIOSENSOR Inc, STANDARD F COVID-19 Ag FIA | 
        345" | SD BIOSENSOR Inc, STANDARD Q COVID-19 Ag Test | 
        1218 | Siemens Healthineers, CLINITEST Rapid Covid-19 Antigen Test | 
        1278 | Xiamen Boson Biotech Co. Ltd, Rapid SARS-CoV-2 Antigen Test Card | 
        1343 | Zhejiang Orient Gene Biotech, Coronavirus Ag Rapid Test Cassette (Swab) | 
    1. `t.sc`: *Required.* **SECONDSFROMISSUINGBASE32**. Date of Sample Collection
    1. `t.tr`: *Required.* **STRING**. Test Result
        Code | Description | 
        ---- | ----------- |
        260415000 | Not detected |
        260373001 | Detected  |
    1. `t.tc`: *Required.* **STRING**. Test Centre
    1. `t.co`: *Required.* **STRING**. Country - Member State of vaccination
    1. `t.is`: *Required.* **STRING**. Issuer
    1. `t.ci`: *Required.* **STRING**. Unique certificate identifier.

1. `nrs`: *Required.* **NUMERIC**. Number of Vaccinations in this record
    1. `r.tg`: *Required.* **BASE32STRING**. Disease or Agent targeted. Codes below are converted from Base10 to Base32
        Code | Description | 
        ---- | ----------- |
        840539006 | COVID19 |
    1. `r.fr`: *Required.* **DAYSFROMISSUINGBASE32**. Date of First Positive Test Result
    1. `r.df`: *Required.* **DAYSFROMISSUINGBASE32**. Certificate Valid From
    1. `r.du`: *Required.* **DAYSFROMISSUINGBASE32**. Certificate Valid Until
    1. `r.co`: *Required.* **STRING10**. Country - Member State of vaccination
    1. `r.is`: *Required.* **STRING50**. Issuer
    1. `r.ci`: *Required.* **STRING50**. Unique certificate identifier.

## Types

1. **NUMERIC1**: Single Digit Numeric 1-9. Regex: `[1-9]`
1. **STRING10**: String with 10 chars. Regex: `[A-Z]{1,10}`
1. **STRING50**: String with 50 chars. 
1. **BASE32STRING**: String with 10 chars. 
1. **TIMESTAMP**: Datetime in seconds since Epoch (January 1, 1970 00:00:00 UTC)
1. **SECONDSFROMISSUINGBASE32**: date as a difference from issuing date, in days, represented in Base32. 
1. **DAYSFROMISSUINGBASE32**: date as a difference from issuing date, in days, represented in Base32. 

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
  ], 
  "t": [
    {
      "tg": "${t.tg}",
      "tt": "${t.tt}",
      "tr": "${t.tr}",
      "ma": "${t.ma}",
      "sc": "${t.sc}",
      "tc": "${t.tc}",
      "co": "${t.co}",
      "is": "${t.is}",
      "ci": "urn:uvci:${t.ci}"
    }
  ], 
  "r": [
    {
      "tg": "${r.tg}",
      "fr": "${r.fr}",
      "co": "${r.co}",
      "is": "${r.is}",
      "df": "${r.df}",
      "du": "${r.du}",
      "ci": "urn:uvci:${r.ci}"
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
CRED:DGC:1:GBCAEIBFOUUISCLFFO6XGSS6AM7KZR7IYOSGFW7MGFFUFMHCTJRMIIWMSMBCANYGL
7N5BODF3T6ADIHOHACOXJYIKXPGCIMCHY7W3G7NGV5TZ3KV:1A9.PCF.PW:D'ARS%C3%98NS%20-
%20VAN%20HALEN/FRAN%C3%87OIS-JOAN/DARSONS%3CVAN%3CHALEN/FRANCOIS%3CJOAN/-4BS
/1GB9TP7/3OC7S0/NL/2/P1J6RU/1119349007/EU%2F1%2F20%2F1528/ORG-100030215/1/2/
-R/NL/MINISTRY%20OF%20VWS/01%3ANL%3APLA8UWS60Z4RZXVALL6GAZ/P1J6RU/1119349007
/EU%2F1%2F20%2F1528/ORG-100030215/2/2/-7/NL/MINISTRY%20OF%20VWS/01%3ANL%3AAT
S342XDYS358FDFH3GTK5/2/P1J6RU/LP217198-3/COVID%20PCR/1232/-8Q5ON/260415000/G
GD%20FRYSL%C3%82N%2C%20L-HELICONWEG/NL/MINISTRY%20OF%20VWS/01%3ANL%3AGGD%2F8
1AAH16AZ/P1J6RU/LP6464-4/NAAT%20TEST/1343/-4K59F/260373001/ALPHEN%20A%2FD%20
RIJN/NL/MINISTRY%20OF%20VWS/01%3ANL%3AGGD%2F23BBS36BC/1/P1J6RU/-19/-V/4E/NL/
MINISTRY%20OF%20VWS/01%3ANL%3ALSP%2FREC%2F1289821
``` 