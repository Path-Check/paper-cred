# **VIAL** Payload

The **VIAL** QR code is designated to contain information about a vaccine vial and printed to the label itself. 

Fields in the **serialization** order:
1. `manuf`: *Required.* **SHORTSTRING**. The name of the manufacturer of the vaccine
1. `product`: *Required.* **SHORTSTRING**. The name of the product of the vaccine.
1. `lot`: *Required.* **SHORTSTRING**. The lot number of bottle of the vaccine.
1. `ndc`: *Required.* **SHORTSTRING**. The national drug code number
1. `exp`: *Required.* **DATE**. The expiration date of the vial.
1. `bud`: *Required.* **DATE**. The befure use date of the vial.
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
1. `dose` *Optional.* **NUMERIC**. A dose size in μL (micro liters).
1. `doses` *Optional.* **NUMERIC**. Number of doses in the vial.
1. `serial` *Optional.* **NUMERIC**. Serial Number of the vial

## Example:


```
CRED:VIAL:1:GBCAEIDL34QEWV3E6EFNTBRSS3Q2QEE3MYMJXPDYMOW6OJWXEFRHDUWI34BCAGPQK7FWX7JCQQLO54222
JSAQH6JTIIAQTXK6O4GL44BRYSW6EID:1A9.PCF:PFIZER/COVID-19/EL9264/29267-1000-1/20210501/20210208
/C28161/500/10/701861
```

