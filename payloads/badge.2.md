# **BADGE V2** Payload

The **BADGE** QR code is designated to contain information about a vaccination event received by the **HOLDER**. 

V2 takes the [V1](badge.1.md) and adds the option for the issuer to choose to add a Hash of the [PassKey](passkey.1.md) (more private) or the Full Name and DoB of the **Holder**

Fields in the **serialization** order:
1. `date`: *Required.* **DATE**. The date of vaccination of the **HOLDER**.
1. `manuf`: *Required.* **SHORTSTRING**. The name of the manufacturer of the vaccine
1. `product`: *Required.* **SHORTSTRING**. The name of the product of the vaccine.
1. `lot`: *Required.* **SHORTSTRING**. The lot number of bottle of the vaccine.
1. `boosts`: *Required.* An array of **SHORTNUMERIC** representing the distance in days from the first dose (e.g. Moderna's two doses: `[28]`, for JnJ's just one dose: `[]`)
1. `passkey`: *Optional.* **STRING**. The cryptographic hash of the data in the Passkey, as defined in the Passkey specification.
1. `route` *Optional.* **SHORTSTRING**. The route of application. Options are:
    | Route Code | Meaning |
    | ----- | ------- |
    | `C38238` | INTRADERMAL |
    | `C28161` | INTRAMUSCULAR
    | `C38276` | INTRAVENOUS |
    | `C38284` | NASAL |
    | `C38288` | ORAL |
    | `C38676` | PERCUTANEOUS |
    | `C38299` | SUBCUTANEOUS |
    | `C38305` | TRANSDERMAL |
1. `site` *Optional.* **SHORTSTRING**. The site of the application. Options are:
    | Site Code | Meaning |
    | --------- | ------- |
    | `LA` | Left Arm |
    | `LD` | Left Deltoid |
    | `LG` | Left Gluteus Medius |
    | `LLFA` | Left Lower Forearm |
    | `LT` | Left Thigh |
    | `LVL` | Left Vastus Lateralis |
    | `RA` | Right Arm |
    | `RD` | Right Deltoid |
    | `RG` | Right Gluteus Medius |
    | `RLFA` | Right Lower Forearm |
    | `RT` | Right Thigh |
    | `RVL` | Right Vastus Lateralis |
1. `dose` *Optional.* **NUMERIC**. A dose size in μL (micro liters).
1. `name`: *Optional.* **STRING**. The full name of the **HOLDER**, to be used when authenticating the **HOLDER**.
    1. In the event the name exceeds 255 bytes when encoded to UTF-8, the name should be truncated until its length does not exceed 255 bytes.
1. `dob`: *Optional.* **DATE**. The date of birth of the **HOLDER**, to be used when authenticating the **HOLDER**.

Although `passkey` and `name`+`dob` are optional, issuers must include one of them. 

## Boosts Field Serialization
When serializing `boosts` data, the array should be represented as decimal
strings joined with plus (`+`) characters. Hence, `[28, 14]` would serialize to
the string `28+14`.

## Example:

Badge with PII becomes: 

```
CRED:BADGE:1:GBCQEIIARTQ5EJ3JUI5DBIFBDLOBXSBJR7E5UBGPF4KIVNYYLFFRDLUEMVKQEICFSGO76HXUZGFVRWV2
SXZAMJ64IRWNR3B4XHUK6D2NXZLHEWQJSY:KEYS.PATHCHECK.ORG:20210308/MODERNA/COVID19/012L20A/28//
C28161/RA/500/JANE%20DOE/19820321
```

Badge without PII: 

```
CRED:BADGE:1:GBCQEID3LMNJK5DA2T5BON33CZD4JDPTUUEFDWNTRFONSLULW3G55PKI5MBCCAERZSGL2VDXGC5VIOIE
CMXESOQK2OHF6NQO5ABV5V3EB6Z265MAI4:KEYS.PATHCHECK.ORG:20210308/MODERNA/COVID19/012L20A/28/IFF
FME37IFZSUOHWQQEIHCLTNN43OUCBFYLHBS7RRJTON7RNLJBQ/C28161/RA/500//
```

