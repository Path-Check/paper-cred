# **COWIN** W3C VC Payload

DIVOC's **Cowin** QR code contains a COVID-19 Immunization Record of a **HOLDER** on a Regular W3C credential.

Fields in the **serialization** order:
1. `credentialSubject.id`: *Required.*. **STRING**.The DID of the **HOLDER**.
1. `credentialSubject.refId`: *Required.*. **STRING**.The reference id of the **HOLDER**.
1. `credentialSubject.name`: *Required.* **STRING**. The name of the **HOLDER**.
1. `credentialSubject.age`: *Required.*  **NUMERIC**. The age of the **HOLDER**.
1. `credentialSubject.gender`: *Required.*  **SHORTSTRING**. The gender of the **HOLDER**.
1. `credentialSubject.nationality`: *Required.* **STRING**. The nationality of the **HOLDER**.
1. `credentialSubject.address.streetAddress`: *Optional.* **STRING**.Address 1 line portion of the **HOLDER** address.
1. `credentialSubject.address.streetAddress2`: *Optional.* **STRING**.Address 2 line portion of the **HOLDER** address.
1. `credentialSubject.address.district`: *Optional.* **STRING**.The District portion of the **HOLDER** address.
1. `credentialSubject.address.city`: *Optional.* **STRING**.The city portion of the **HOLDER** address.
1. `credentialSubject.address.addressRegion`: *Optional.* **STRING**.The region (state) portion of the **HOLDER** address.
1. `credentialSubject.address.addressCountry`: *Optional.* **SHORTSTRING**. The country portion of the **HOLDER** address.
1. `credentialSubject.address.postalCode`: *Optional.* **SHORTSTRING**. The Postal Code portion of the **HOLDER** address.
1. `evidence.date`: *Required.* **DATE**. The date of vaccination of the **HOLDER**.
1. `evidence.manufacturer`: *Required.* **SHORTSTRING**. The name of the manufacturer of the vaccine
1. `evidence.vaccine`: *Required.* **SHORTSTRING**. The name of the product of the vaccine.
1. `evidence.batch`: *Required.* **SHORTSTRING**. The lot number of bottle of the vaccine.
1. `evidence.effectiveStart`: *Required.* **DATE**. The date the vaccine starts to be effective.
1. `evidence.effectiveUntil`: *Required.* **DATE**. The date the vaccine ends its effectiveness.
1. `evidence.dose`: *Optional.* **NUMERIC**. The dose number of this shot
1. `evidence.totalDoses`: *Optional.* **NUMERIC**. The number of doses required by this vaccine. 
1. `evidence.id`: *Required.* **STRING**.  The ID of this evidence
1. `evidence.verifier.name`: *Required.* **STRING**. The name of the verifier.
1. `evidence.facility.name`: *Required.* **STRING**. The name of the facility.
1. `evidence.facility.address.streetAddress`: *Required.* **STRING**. Address 1 line portion of the facility address.
1. `evidence.facility.address.streetAddress2`: *Required.* **STRING**. Address 2 line portion of the facility address.
1. `evidence.facility.address.district`: *Required.* **STRING**. The District portion of the facility address.
1. `evidence.facility.address.city`: *Required.* **STRING**. The city portion of the facility address.
1. `evidence.facility.address.addressRegion`: *Required.* **STRING**. The region (state) portion of the facility address.
1. `evidence.facility.address.addressCountry`: *Required.* **SHORTSTRING**. The country portion of the facility address.
1. `evidence.facility.address.postalCode`: *Required.* **SHORTSTRING**. The Postal Code portion of the facility address.
1. `issuanceDate`: *Required.* **DATE**. The date of issuance of the passport.

## Fixed W3C VC Fields

When converting the certificate back to a JSON structure, verifiers must hardcode this JSON template, replacing `${field}` by the content of `field`
```
{
    "@context": ["https://www.w3.org/2018/credentials/v1", "https://cowin.gov.in/credentials/vaccination/v1"],
    "type": ["VerifiableCredential", "ProofOfVaccinationCredential"],
    "issuer": "https://cowin.gov.in/",
    "nonTransferable": "true",
    .... 
    "credentialSubject": {
        "type": "Person",
        ...
        "address": {
            ...
        }
    },
    "evidence": [{
        "type": ["Vaccination"],
        "id": "https://cowin.gov.in/vaccine/${EVID}",
        "feedbackUrl": "https://cowin.gov.in/?${EVID}",
        "infoUrl": "https://cowin.gov.in/?${EVID}",
        ...
        "verifier": {
            ...
        },
        "facility": {
            ...
            "address": {
                ...
            }
        }
    }],
    "proof": {
        "type": "RsaSignature2018",
        "proofPurpose": "assertionMethod",
        ...
    }
}
```

## Example:
```
CRED:COWIN:1:GBCAEID5RBWCXQB5I3MX2AGFVNTSJINWCPPNUJ3SIYLJXCEQJZI3TRSJYIBCAJZVBU4PGICIRC
7S5OXNKTBFX2A3X7EUU3BTYZJC776ARFDGFA2C:KEYS.PATHCHECK.ORG:PERSON/DID%3AIN.GOV.UIDAI.AAD
HAAR%3A2342343334/12346/BHAYA%20MITRA/27/MALE/INDIAN/101-102%2C%20MANGAL%20ASHIRWAD/S%2
0V%20ROAD/SANTACRUZ%20WEST/MUMBAI/400054/MAHARASHTRA/IN/VACCINATION/20210413/COVPHARMA/
COVAX/MB3428BX/20201202/20251202/1/1/HTTPS%3A%2F%2FCOWIN.GOV.IN%2FVACCINE%2FUNDEFINED/H
TTPS%3A%2F%2FCOWIN.GOV.IN%2F%3FUNDEFINED/HTTPS%3A%2F%2FCOWIN.GOV.IN%2F%3FUNDEFINED/SOOR
AJ%20SINGH/ABC%20MEDICAL%20CENTER/123%2C%20KORAMANGALA//BENGALURU%20SOUTH/BENGALURU//KA
RNATAKA/IN/HTTPS%3A%2F%2FWWW.W3.ORG%2F2018%2FCREDENTIALS%2FV1%2C%20HTTPS%3A%2F%2FCOWIN.
GOV.IN%2FCREDENTIALS%2FVACCINATION%2FV1/20210413/VERIFIABLECREDENTIAL%2C%20PROOFOFVACCI
NATIONCREDENTIAL/HTTPS%3A%2F%2FCOWIN.GOV.IN%2F/TRUE
```