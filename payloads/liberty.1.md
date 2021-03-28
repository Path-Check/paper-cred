# **Liberty** Payload

The IBM **Liberty Health Passport** QR code contains a COVID-19 Vaccination Status of a **HOLDER** on a Regular W3C certificate.

Fields in the **serialization** order:
1. `firstname`: *Required.* **STRING**. The first name of the **HOLDER**, to be used when authenticating the **HOLDER**.
1. `lastname`: *Required.* **STRING**. The last name of the **HOLDER**, to be used when authenticating the **HOLDER**.
1. `dob`: *Required.* **DATE**. The date of birth of the **HOLDER**, to be used when authenticating the **HOLDER**.
1. `display`: *Required.* **STRING**. The display code for the Status card.
1. `passType`: *Required.* **STRING**. The display type of passport issued.
1. `schemaType`: *Required.* **STRING**. The type of the schema used in the certificate.
1. `schemaID`: *Required.* **STRING**. Unique ID of the Schema Type
1. `w3cContext`: *Required.* **STRING**. The type of the schema used in the certificate.
1. `date`: *Required.* **DATE**. The date of issuance of the passport.
1. `w3cType`: *Required.* **STRING**. The type of the schema used in the certificate.
1. `expiration`: *Required.* **DATE**. The date of expiration of the passport.
1. `issuer`: *Required.* **STRING**. The issuer of the certificate. If a DID, map from hex to Base32URL. 
1. `id`: *Required.* **STRING**. The ID of this certificate.

## Example:
```
CRED:PASSKEY:1:GBCAEICBWCIVX2FSRW4PQ3QVSC7FJ7HOMVEPZSTQMKTHWRLBAOFAZNKGUYBCAALORNEN4XTVYGFX4X
ZZYBOITFVVGGCYTHUCKVSSTP5VITSAJTEE:KEYS.PATHCHECK.ORG:JANE%20DOE/19820321/633PBY127H/16173
332345
```