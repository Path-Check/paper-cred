# **VIAL** Payload

The **VIAL** QR code is designated to contain information about a vaccine vial and printed to the label itself. 

Fields in the **serialization** order:
1. `manuf`: *Required.* **SHORTSTRING**. The name of the manufacturer of the vaccine
1. `product`: *Required.* **SHORTSTRING**. The name of the product of the vaccine.
1. `lot`: *Required.* **SHORTSTRING**. The lot number of bottle of the vaccine.
1. `ndc`: *Required.* **SHORTSTRING**. The national drug code number
1. `exp`: *Required.* **DATE**. The lot number of bottle of the vaccine.
1. `bud`: *Required.* **DATE**. The lot number of bottle of the vaccine.

## Example:


```
CRED:VIAL:1:GBCAEIA6HBPNT2RJWHQS4KJ5TCFFSGV2EU3C6TFCDO3J43WZKBCULG4BH4BCASUMRX
24BLICO354B2FT7IUOOQDROFEX4I7D6ELWBMMYSPBHGZHJ:1A9.PCF:PFIZER/COVID-19/EL9264/
29267-1000-1/20210501/20210208
```

