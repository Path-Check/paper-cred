# **Liberty** W3C VC Payload

The IBM **Liberty Health Passport** QR code contains a COVID-19 Vaccination Status of a **HOLDER** on a Regular W3C credential.

Fields in the **serialization** order:
1. `credentialSubject.subject.name.given`: *Required.* **STRING**. The first name of the **HOLDER**.
1. `credentialSubject.subject.name.family`: *Required.* **STRING**. The last name of the **HOLDER**.
1. `credentialSubject.subject.birthDate`: *Required.* **DATE**. The date of birth of the **HOLDER**.
1. `credentialSubject.display`: *Required.* **STRING**. The display code for the Status card.
1. `credentialSubject.passType`: *Required.* **STRING**. The display type of passport issued.
1. `issuanceDate`: *Required.* **DATE**. The date of issuance of the passport.
1. `expirationDate`: *Required.* **DATE**. The date of expiration of the passport.
1. `issuer`: *Required.* **STRING**. The issuer of the credential. If a DID, map from hex to Base32URL. 
1. `uuid`: *Required.* **STRING**. The ID of this credential.

## Fixed  W3C VC Fields

When converting the credential back to a JSON structure, verifiers must hardcode this JSON template, replacing `${field}` by the content of `field`
```
{
    "@context": ["https://www.w3.org/2018/credentials/v1"],
    "type": ["VerifiableCredential"],
    "id": "${ISSUER}#vc-${UUID}"
    ...
    "credentialSchema": {
        "id": "${ISSUER};id=libertyhealthpass;version=0.1"
        "type": "JsonSchemaValidator2018"
    },
    "credentialSubject": {
        ...
        "type": "Liberty HealthPass"
    },
    "proof": {
        ....
        "creator": "${ISSUER}#${KEYID}"
        "type": "EcdsaSecp256r1Signature2019"
    }
}
```

## Example:
```
CRED:LIBERTY:1:GBCQEIBB77JDNIVCH7TWFJZHFGWAKXSAGE5S6HRXIMOUI5P6UMJMPAXBC4BCCAGKO42UCH2PFCDTOY5554QJA
LCL5YXRNH5P6M6EIJ3YINAXYH4XI4:KEYS.PATHCHECK.ORG:JANE/DOE/19810101/%23999999E/COVID-19%20VACCINATION
/20210228/20210328/DID%3AHPASS%3AWOURRKQ6YKYPBVMKNSR6PSHLW2TDDS5SICFNL2HMVSU73NEO4S7Q%3AOIIAE2TJDDRS
KDA5CCKGG3AKRHGAMRUM6MBLZYRBNYQMOVOQECWA/SOME-LONG-UUID-THAT-LOOKS-LIKE-THIS
```