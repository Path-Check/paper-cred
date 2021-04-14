# **Liberty** Payload

The IBM **Liberty Health Passport** QR code contains a COVID-19 Vaccination Status of a **HOLDER** on a Regular W3C certificate.

Fields in the **serialization** order:
1. `credentialSubject.subject.name.given`: *Required.* **STRING**. The first name of the **HOLDER**, to be used when authenticating the **HOLDER**.
1. `credentialSubject.subject.name.family`: *Required.* **STRING**. The last name of the **HOLDER**, to be used when authenticating the **HOLDER**.
1. `credentialSubject.subject.birthDate`: *Required.* **DATE**. The date of birth of the **HOLDER**, to be used when authenticating the **HOLDER**.
1. `credentialSubject.display`: *Required.* **STRING**. The display code for the Status card.
1. `credentialSubject.passType`: *Required.* **STRING**. The display type of passport issued.
1. `issuanceDate`: *Required.* **DATE**. The date of issuance of the passport.
1. `expirationDate`: *Required.* **DATE**. The date of expiration of the passport.
1. `issuer`: *Required.* **STRING**. The issuer of the certificate. If a DID, map from hex to Base32URL. 
1. `id`: *Required.* **STRING**. The ID of this certificate.

## Fixed Fields

When converting the certificate back to a JSON structure, verifiers must hardcode this JSON template, replacing `${ISSUER}` by the content of `issuer`, `${ID}` for the `id` from the payload and `${KEYID}` is the key id in the URI
```
{
    "@context": ["https://www.w3.org/2018/credentials/v1"],
    "type": ["VerifiableCredential"],
    "id": "${ISSUER}#vc-${ID}"
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
CRED:LIBERTY:1:GBCQEIBXAP4BBH2OMC3FRXTKEUSXP4ZK6MUEMVA376UAG3KDRTIDOXW574BCCAF5O3VC77NO7T67FXK7TOVFKG7EECE36NMEINQ3VC4GIHAWZRJMFQ:KEYS.PATHCHECK.ORG:JANE/DOE/19810101/%23999999E/COVID-19%20VACCINATION/ISSUER%2B%3BID%3DLIBERTYHEALTHPASS%3BVERSION%3D0.1/20210228/20210328/DID%3AHPASS%3AWOURRKQ6YKYPBVMKNSR6PSHLW2TDDS5SICFNL2HMVSU73NEO4S7Q%3AOIIAE2TJDDRSKDA5CCKGG3AKRHGAMRUM6MBLZYRBNYQMOVOQECWA/ISSUER%2B%23VC-SOME-LONG-UUID-THAT-LOOKS-LIKE-THIS
```