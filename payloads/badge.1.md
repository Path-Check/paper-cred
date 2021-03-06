# **BADGE** Payload

Fields in the **serialization** order:
1. `date`: *Required.* **DATE**. The date of vaccination of the **HOLDER**.
1. `manuf`: *Required.* **SHORTSTRING**. The name of the manufacturer of the vaccine
1. `product`: *Required.* **SHORTSTRING**. The name of the product of the vaccine.
1. `lot`: *Required.* **SHORTSTRING**. The lot number of bottle of the vaccine.
1. `boosts`: *Required.* An array of **SHORTNUMERIC** representing the distance in days from the first dose (e.g. Moderna's two doses: `[28]`, for JnJ's just one dose: `[]`)
1. `passkey`: *Required.* **STRING**. The cryptographic hash of the data in the Passkey, as defined in the Passkey specification.
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

## Boosts Field Serialization
When serializing `boosts` data, the array should be represented as decimal
strings joined with plus (`+`) characters. Hence, `[28, 14]` would serialize to
the string `28+14`.

## JSON example:
```json
{
  "type": "badge",
  "version": 1,
  "data": {
    "date": 20210102,
    "manuf" : "Moderna",
    "product" : "Covid19",
    "lot": ":23092",
    "boosts" : [],
    "passkey": "d9116bbdf7e33414b23ce81b2d4b9079a111d7119be010a5dcde68a1e5414d2d",
    "route": "C28161",
    "site": "RA",
    "dose": 500
  },
  "signature": {
    "keyId": "1a9.cdc",
    "base32": "GBDAEIIAVA3PD5GI7GAMSMDJOF3YCN4PS6D4VFD3LODHVFJ5SFUPSOLIFH7QEIIA6JZW6O2WFCBGYCILP6H6Z4FE5FXOQPF2NNIC46BTPJFEXMASNUGA"
  }
}
```

