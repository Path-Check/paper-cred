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
        "id": "https://cowin.gov.in/vaccine/${evidence.id}",
        "feedbackUrl": "https://cowin.gov.in/?${evidence.id}",
        "infoUrl": "https://cowin.gov.in/?${evidence.id}",
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
CRED:COWIN:1:GBCQEIIAVJ5CAREXP7IQ3Z7FGMI5YIVQE6D26SB5VAJEPULJYHEGBQN7S7EAEICGF7RW2HUS77
WMXL62S4IKHSE4V5VYVLPSNXIVIVEDPVPIIQU4YA:KEYS.PATHCHECK.ORG:DID%3AIN.GOV.UIDAI.AADHAAR%
3A2342343334/12346/BHAYA%20MITRA/27/MALE/INDIAN/101-102%2C%20MANGAL%20ASHIRWAD/S%20V%20
ROAD/SANTACRUZ%20WEST/MUMBAI/400054/MAHARASHTRA/IN/20210416/COVPHARMA/COVAX/MB3428BX/20
201202/20251202/1/1/UNDEFINED/SOORAJ%20SINGH/ABC%20MEDICAL%20CENTER/123%2C%20KORAMANGAL
A//BENGALURU%20SOUTH/BENGALURU//KARNATAKA/IN/20210416
```